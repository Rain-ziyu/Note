# 前言

为了统一Dubbo层的上下游服务间的参数校验，并且不侵入我们的业务逻辑，我们希望能够实现在Dubbo调用时可以像在Controller层一样进行参数的校验。

即：借助`hibernate-validator`完成注解式的参数判断。

当然Dubbo官网表示自己支持JSR303标准的校验，但是因为Dubbo相比SpringBootWeb那样简单的引入即可完成，因为Dubbo的**版本变化问题**以及**序列化问题**等。

[Demo参考地址](https://github.com/Rain-ziyu/dubbo-validation)

# 依赖选择

因为Dubbo项目往往是三个模块，一个提供接口的Common模块，一个提供实际接口的Provider模块，一个调用方的Consumer模块。

![](https://prod.huayu.asia:9000/picgo/2023/08/15/20230815171652.png)

按照项目中简化的模型，我们进行各个模块所需要的依赖解释。

## Common

首先是common，作为提供接口的模块，我们接口上作为参数的类也存在于这个模块中，因此跟我们Controller层校验时一样，我们需要在实体类中进行注解标记。

```java
package com.platform.ahj.dubbocommon.entity;


import com.platform.ahj.dubbocommon.anno.Phone;
import com.platform.ahj.dubbocommon.service.UserService;
import lombok.Data;

import javax.validation.constraints.NotNull;
import java.io.Serializable;


/**
 * @Description: 用户
 * @Author: ZiYu
 * @Created on: 2023/08/08 16:08
 * @Since:
 */
@Data
public class User implements Serializable {
    private static final long serialVersionUID = 7158903718568000392L;
    // 仅GetUser方法验证
    @NotNull(groups = UserService.GetUser.class)
    private String name;
    // UserService内的方法都验证
    @NotNull(groups = UserService.class)
    private String password;
    // 仅VerifyUserPhoneNumber方法验证
    @Phone(groups = UserService.VerifyUserPhoneNumber.class)
    private String phone;
}

```

那么我们就需要注解对应的依赖：

```xml
        <!--Dubbo层参数校验，需要放在common层的东西，需要同时提供给consumer和provider-->
        <!--        为了兼容JDK17中javax被被删除因此jdk11以上的SpringBoot2.x项目使用必须使用该依赖-->
        <dependency>
            <groupId>javax.validation</groupId>
            <artifactId>validation-api</artifactId>
            <version>2.0.1.Final</version>
        </dependency>
```

因为我们的场景是SpringBoot2.x+Dubbo3.x+JDK17，因此JEE包被删除的问题，我们需要手动引入javax，并且因为Jakarta版本的`validation-api`是面向JDK17以及SpringBoot3.x的因此我们此处是引入`javax.validation`。除此之外Common中的依赖与正常Dubbo项目相同。

## 注解使用

基本上原生注解的效果与我们在Controller层使用时效果一样，但是对于分组情况时存在一定区别：

我们需要在接口中需要分组的方法上使用如下形式标记：

```java
package com.platform.ahj.dubbocommon.service;


import com.platform.ahj.dubbocommon.entity.User;
import org.apache.dubbo.common.stream.StreamObserver;

/**
 * @Description: UserService接口
 * @Author: ZiYu
 * @Created on: 2023/08/03 9:13
 * @Since:
 */
// 缺省可按服务接口区分验证场景，如：@NotNull(groups = ValidationService.class)
public interface UserService {

    @interface GetUser{} // 与方法同名接口，首字母大写，用于区分验证场景，如：@NotNull(groups = ValidationService.Save.class)，可选
    /**
     * 方法getUser作用为：
     * 校验用户的名字不能为空
     *
     * @param
     * @return java.lang.String
     * @throws
     * @author RainZiYu
     */
    String getUser(User user);
    
    String setUser(User user);
    @interface VerifyUserPhoneNumber{}
    /**
     * 方法verifyUserPhoneNumber作用为：
     * 校验用户手机号
     * @author RainZiYu
     * @param user
     * @throws
     * @return java.lang.String
     */
    String verifyUserPhoneNumber(User user);
}

```

通过在方法上新建一个同名的大写接口，然后在**注解的groups属性中填写该class**

```java
    // 仅GetUser方法验证
    @NotNull(groups = UserService.GetUser.class)
    private String name;
    // UserService内的方法都验证
    @NotNull(groups = UserService.class)
    private String password;
    // 仅VerifyUserPhoneNumber方法验证
    @Phone(groups = UserService.VerifyUserPhoneNumber.class)
    private String phone;
```

配置完成之后即可实现**按照分组进行方法参数校验**。

对于**关联式的参数校验**则使用，以下形式

```java
import javax.validation.GroupSequence;
 
public interface ValidationService {   
    @GroupSequence(Update.class) // 同时验证Update组规则
    @interface Save{}
    void save(ValidationParameter parameter);
 
    @interface Update{} 
    void update(ValidationParameter parameter);
}
```

同时也支持**方法上的直接参数验证**

```java
import javax.validation.constraints.Min;
import javax.validation.constraints.NotNull;
 
public interface ValidationService {
    void save(@NotNull ValidationParameter parameter); // 验证参数不为空
    void delete(@Min(1) int id); // 直接对基本类型参数验证
}
```

**同时支持自定义注解进行校验**，具体方式见代码，可以与Controller一样也可使用Hibernate Validator的SPI机制进行发现。

**同样支持集合验证**。

## Consumer

因为Dubbo实际提供了两种参数校验方式，即Consumer方校验与Provider方校验。

理论上我们应该选择Consumer方校验，原因很简单，因为减少进入下游服务的调用，来减小服务压力。

而开启参数校验的方式也非常简单，我们不需要再借助Controller层时的@Validated注解来开启，而是在我们注入Provider的地方，在注解中添加`validation = "true"`。

```java
    // 进行调用方参数校验   提供方参数校验其参数异常无法正常序列化至调用方
    @DubboReference(version = "2.0",validation = "true")
    private UserService userService;
```

使用该注解即可开启调用方参数校验。

## Provider

对于Provider方的参数校验开启方式也很简单，在实现接口的地方，增加`validation = "true"`即可，并引入依赖。

```xml
        <dependency>
            <groupId>org.hibernate.validator</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>6.1.0.Final</version>
        </dependency>
```



```java

import com.platform.ahj.dubbocommon.entity.User;
import com.platform.ahj.dubbocommon.exception.BizException;
import com.platform.ahj.dubbocommon.exception.NotifyCode;
import com.platform.ahj.dubbocommon.service.UserService;
import lombok.extern.slf4j.Slf4j;
import org.apache.dubbo.common.stream.StreamObserver;
import org.apache.dubbo.config.annotation.DubboService;

@Slf4j
/**
 * @Description: 提供User的相关服务
 * @Author: ZiYu
 * @Created on: 2023/08/03 9:10
 * @Since:
 */
@DubboService(version = "2.0"
         ,validation = "true"   //服务端序校验出现的异常无法传递至客户端
)
public class UserServiceImpl implements UserService {
    public String getUser(User user) {
        return "wwl1";
    }
    public String setUser(User user) {
        return "wwl1";
    }
}
```

# 异常传递（`ConstraintViolationException` 异常）

如果我们使用Dubbo的hibernate-validator进行参数校验，当我们参数出现问题时，会爆出`ConstraintViolationException` 异常，该异常中包含我们的所有的不满足条件条件的字段。

但是实际上因为该异常中`ConstraintViolations`类实际上也就是`ConstraintViolationImpl`无法序列化（无论是fastjson2还是hs2方式，因为该类最短的构造函数序列化时很多参数无法赋值，比如其中messageParameters等）。

调用方在被调用方出现异常之后会尝试反序列化该异常，但是会出现反序列化失败，从而导致我们的调用方并没有办法准确捕获不满足参数的异常。那么此时不仅会导致Dubbo调用认为是我们被调用方服务调用过程中出现问题（认为建立RPC调用失败），从而开启三次重试，但是也毫无意义，反而增加被调用方的压力。

因此我们需要一个方法将无法传递的异常信息转换为其他信息，正确的传递给调用方。

按照官方文档我们一般有两种选择：

## 1.**自定义Dubbo过滤器** 

为了能够正确的处理`ConstraintViolationException` 异常，我们也可以借助Dubbo的SPI机制创建我们自己的`ValidationFilter`。

我们参考JValidation的源码实现我们自己的`CustomValidationFilter`

最终实现结果如下：

```java
package com.platform.ahj.dubbocommon.validator.dubbo;

import com.alibaba.dubbo.rpc.RpcException;
import com.platform.ahj.dubbocommon.entity.CommonResult;
import com.platform.ahj.dubbocommon.exception.BusinessEnum;
import com.platform.ahj.dubbocommon.exception.NotifyCode;
import org.apache.dubbo.common.extension.Activate;
import org.apache.dubbo.common.utils.CollectionUtils;
import org.apache.dubbo.common.utils.ConfigUtils;
import org.apache.dubbo.rpc.*;
import org.apache.dubbo.validation.Validation;
import org.apache.dubbo.validation.Validator;

import javax.validation.ConstraintViolation;
import javax.validation.ConstraintViolationException;
import javax.validation.ValidationException;

import java.util.Set;

import static org.apache.dubbo.common.constants.CommonConstants.CONSUMER;
import static org.apache.dubbo.common.constants.CommonConstants.PROVIDER;
import static org.apache.dubbo.common.constants.FilterConstants.VALIDATION_KEY;

/**
 * @Description: 自定义Dubbo的Filter   用于解决ConstraintViolationException异常无法反序列化的问题
 * @Author: ZiYu
 * @Created on: 2023/08/15 15:57
 * @Since:
 */
@Activate(group = {CONSUMER, PROVIDER}, value = "customValidationFilter", order = 10000)
public class CustomValidationFilter implements Filter {

    private Validation validation;

    public void setValidation(Validation validation) { this.validation = validation; }

    public Result invoke(Invoker<?> invoker, Invocation invocation) throws RpcException {
        if (validation != null && !invocation.getMethodName().startsWith("$")
                && ConfigUtils.isNotEmpty(invoker.getUrl().getMethodParameter(invocation.getMethodName(), VALIDATION_KEY))) {
            try {
                Validator validator = validation.getValidator(invoker.getUrl());
                if (validator != null) {
                    validator.validate(invocation.getMethodName(), invocation.getParameterTypes(), invocation.getArguments());
                }
            } catch (RpcException e) {
                throw e;
            } catch (ConstraintViolationException e) {
                // 与Dubbo默认的filter不同 这边细化了异常类型的处理
                Set<ConstraintViolation<?>> violations = e.getConstraintViolations();
                if (CollectionUtils.isNotEmpty(violations)) {
                    ConstraintViolation<?> violation = violations.iterator().next();
                    // 取第一个进行提示,并封装进统一的Result
                    CommonResult facadeResult = CommonResult.fail(Integer.parseInt(NotifyCode.INVALID_PARAM.getCode()), violation.getMessage());
                    return AsyncRpcResult.newDefaultAsyncResult(facadeResult, invocation);
                }
                return AsyncRpcResult.newDefaultAsyncResult(new ValidationException(e.getMessage()), invocation);
            } catch (Throwable t) {
                return AsyncRpcResult.newDefaultAsyncResult(t, invocation);
            }
        }
        return invoker.invoke(invocation);
    }
}
```

重写完我们自定义的CustomValidationFilter后需要向Dubbo注册该Filter，借助Dubbo的SPI扩展

在resources/META-INF/dubbo/org.apache.dubbo.rpc.Filter文件中配置

![](https://prod.huayu.asia:9000/picgo/2023/08/16/20230816091204.png)

```
customValidationFilter=com.platform.ahj.dubbocommon.validator.dubbo.CustomValidationFilter
```



## 2.**进行验证扩展**

根据官方文档的指示，官方默认实现的`org.apache.dubbo.validation.support.jvalidation.JValidation`就是

[官网验证扩展文档](https://cn.dubbo.apache.org/zh-cn/overview/mannual/java-sdk/reference-manual/spi/description/validation/)

![](https://prod.huayu.asia:9000/picgo/2023/08/16/20230816084745.png)



# Dubbo层统一返回结果

当我们在Controller层时我们可以通过全局异常处理，然后统一出现异常时返回给http请求的Result。

但是在Dubbo层我们往往没有该要求，但是如果你想统一Dubbo层出现异常时的返回内容，而不是直接将异常抛至调用方，我们仍然可以借助我们自定义的CustomValidationFilter来实现。

![](https://prod.huayu.asia:9000/picgo/2023/08/16/20230816090407.png)



通过捕获我们想要处理的自定义异常，然后借助`return AsyncRpcResult.newDefaultAsyncResult(t, invocation);`来将我们的异常封装成返回结果给调用方，而不是异常。

最终实现效果：

![image-20230816093718723](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230816093718723.png)

![image-20230816093626536](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230816093626536.png)

# 项目上使用

以`platform-tenant`为例

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817091631.png)

platform-tenant作为Dubbo的被调用方，其中`api`模块用于调用方进行依赖导入。

## 1.在`api`模块引入依赖：

```xml
        <!--Dubbo层参数校验，需要放在common层的东西，需要同时提供给consumer和provider-->
        <!--        为了兼容JDK17中javax被被删除因此jdk11以上的SpringBoot2.x项目使用必须使用该依赖-->
        <dependency>
            <groupId>javax.validation</groupId>
            <artifactId>validation-api</artifactId>
            <version>2.0.1.Final</version>
        </dependency>
```

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817092028.png)

## 2.在需要进行参数校验的`entity`中进行相关注解的添加

也可以在**provider包中的接口参数**上进行注解侵入

如果需要使用**分组校验、自定义注解校验器**等，都建议编写在api包中，方便调用方直接引入

## 3.自定义CustomValidationFilter编写

每一个服务可以自定义自己的Dubbo层Filter，主要是为了解决ConstraintViolationException异常无法反序列化的问题

也可以用于通一项目中的返回结果

对于最基础的CustomValidationFilter编写请参考上边的**自定义Dubbo过滤器** ，**记得使用SPI进行服务注册至Dubbo**

## 4.参数校验开启

Dubbo参数校验可以是调用方也可以是被调用方

哪方进行参数校验就需要引入以下依赖：

```xml
    <dependency>
        <groupId>org.hibernate.validator</groupId>
        <artifactId>hibernate-validator</artifactId>
        <version>6.1.0.Final</version>
    </dependency>
```

然后在对应的注解（`@DubboService`或者是`@DubboReference`）注解中添加`validation = "true"`来开启参数校验即可。

以`platform-tenant`为例，如果你想进行被调用方的参数校验，需要在**`platform-tenant-service`**中引入以上依赖，并在`@DubboService`注解中添加`validation = "true"`。

如果你想进行调用方（如`platform-ahtc`项目）的提前参数校验，你需要在`platform-ahtc`项目的**`platform-ahtc-service`**模块引入以上依赖，并在`@DubboReference`注解中添加`validation = "true"`。