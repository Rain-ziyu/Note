# SpringBoot

虽然Spring的组件代码是轻量级的，但它的配置却是重量级的。一开始，Spring用XML配置，而且是很多XML配置。Spring 2.5引入了基于注解的组件扫描，这消除了大量针对应用程序自身组件的显式XML配置。Spring 3.0引入了基于Java的配置，这是一种类型安全的可重构配置方式，可以代替XML。

但是以上模式都会带来开发时的损耗。因为在思考Spring特性配置和解决业务问题之间需要进行思维切换，编写配置挤占了编写应用程序逻辑的时间。和所有框架一样，Spring实用，但与此同时它要求的配置也不少。

除此之外，项目的依赖管理也是一件耗时耗力的事情。在环境搭建时，需要分析要导入哪些库的坐标，而且还需要分析导入与之有依赖关系的其他库的坐标，一旦选错了依赖的版本，随之而来的不兼容问题就会严重阻碍项目的开发进度。
1. jsp中要写很多代码、控制器过于灵活,缺少一个公用控制器 
2. Spring不支持分布式,这也是EJB仍然在用的原因之一。

因此SpringBoot对上述Spring的缺点进行了改善和优化，基于约定优于配置的思想，可以让开发人员不必在配置与逻辑业务之间进行思维的切换，全身心地投入到逻辑业务的代码编写中，从而大大提高了开发的效率，一定程度上缩短了项目周期。

## 特点
1. 为基于Spring的开发提供更快的入门体验
2. 开箱即用，没有代码生成，也无需XML配置。同时也可以修改默认值来满足特定的需求
3. 提供了一些大型项目中常见的非功能性特性，如嵌入式服务器、安全、指标，健康检测、外部配置等

SpringBoot不是对Spring功能上的增强，而是提供了一种快速使用Spring的方式
## 核心功能
### 起步依赖 
起步依赖本质上是一个Maven项目对象模型（Project Object Model，POM），定义了对其他库的传递依赖，这些东西加在一起即支持某项功能。简单的说，起步依赖就是将具备某种功能的坐标打包到一起，并提供一些默认的功能。
### 自动配置
Spring Boot的自动配置是一个运行时（更准确地说，是应用程序启动时）的过程，考虑了众多因素，才决定Spring配置应该用哪个，不该用哪个。该过程是Spring自动完成的。

# 拓展使用

## 日志框架拓展

### 配置要点
> 在配置日志时需要考虑哪些因素？

* 支持日志路径，日志level等配置
* 日志控制能够通过application.yml控制
* 按天生成日志，当天的日志>50MB进行切割存放等
* 最多保存10天日志
* 设置自定义Pattern，生成满足自己要求的格式
* 可以根据不同环境设置不同的日志配置，比如生产环境仅设置到INFO级别、某些核心类关闭生产环境的打印
* 在生产上尽量关闭Location等消耗资源的信息
* 对于落盘的日志采用按小时、按日志级别等来分别存储
* 控制一些三方包的日志级别及输出路径方便查找
* 针对一些复杂配置可以翻阅对应日志实现的官方文档，其实官方已经提供了比较完善的功能，比如集成消息队列、异步Http告警等。

## 开发式SpringBoot常用注解快速解析

### @SpringBootApplication
定义在main方法入口类处，用于启动Spring Boot应用项目，同时引入ComponentScan、Configuration、自动注入（EnableAutoConfiguration）等。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Configuration
@EnableAutoConfiguration
@ComponentScan
public @interface SpringBootApplication {

	/**
	 * Exclude specific auto-configuration classes such that they will never be applied.
	 * @return the classes to exclude
	 */
	Class<?>[] exclude() default {};

}
```
### @EnableAutoConfiguration

让Spring Boot根据当前项目类路径中的jar包具有的依赖进行自动配置一些基础组件

自动注入的所有相关Configuration在src/main/resources的META-INF/spring.factories中
```shell
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration

若有多个自动配置，用“，”隔开
```
### @ImportResource

指定额外的xml配置，一般放到启动main类上
```java
@ImportResource("classpath*:/spring/*.xml")  //单个

@ImportResource({"classpath*:/spring/1.xml","classpath*:/spring/2.xml"})   //多个

```

### @Value

读入配置文件中指定的配置项的值
```java
public class A{
	 @Value("${push.start:0}")    //如果缺失，默认值为0
     private Long  id;
}
```

### @ConfigurationProperties(prefix="person")

读取指定配置文件中指定层级之后的配置项作为类中的属性值，prefix指定properties的配置的前缀，通过location指定properties文件的位置。需要注意的是该注解不会将类生成bean注册到ioc容器中，需要你自己添加@Component、@EnableConfigurationProperties等方式显式启用配置属性绑定和 Bean 注册。
```java
@ConfigurationProperties(prefix="person")
public class PersonProperties {
	
	private String name ;
	private int age;
}
```

### @EnableConfigurationProperties

用 @EnableConfigurationProperties注解使 @ConfigurationProperties生效，并将对应的配置类注册为bean。

### @RestController

组合@Controller和@ResponseBody，当你开发一个和页面交互数据的Controller时，可以使用此注解。
用来映射web请求(访问路径和参数)、处理类和方法，可以注解在类或方法上。注解在方法上的路径会继承注解在类上的路径。

### @RequestMapping
用来映射web请求(访问路径和参数)、处理类和方法，可以注解在类或方法上。

注解在方法上的路径会继承注解在类上的路径。

produces属性: 定制返回的response的媒体类型和字符集，或需返回值是json对象
```java
@RequestMapping(value="/api2/copper",produces="application/json;charset=UTF-8",method = RequestMethod.POST)
```

### @RequestParam
获取request请求url后的参数值
```java
 public List<CopperVO> getOpList(HttpServletRequest request,
                                    @RequestParam(value = "pageIndex", required = false) Integer pageIndex,
                                    @RequestParam(value = "pageSize", required = false) Integer pageSize) {
 
  }
```
### @ResponseBody

持将返回值放在response体内，而不是返回一个页面。比如Ajax接口，可以用此注解返回数据而不是页面。此注解可以放置在返回值前或方法前。
另一个玩法，可以不用@ResponseBody。
继承FastJsonHttpMessageConverter类并对writeInternal方法扩展，在spring响应结果时，再次拦截、加工结果
```java

// stringResult: json返回结果
//HttpOutputMessage outputMessage

 byte[] payload = stringResult.getBytes();
 outputMessage.getHeaders().setContentType(META_TYPE);
 outputMessage.getHeaders().setContentLength(payload.length);
 outputMessage.getBody().write(payload);
 outputMessage.getBody().flush();
```

### @Bean
@Bean(name="bean的名字",initMethod="初始化时调用方法名字",destroyMethod="close")
定义在方法上，在容器内初始化一个bean实例类。
```java
@Bean(destroyMethod="close")
@ConditionalOnMissingBean
public PersonService registryService() {
		return new PersonService();
}
```
### @Service
用于标注业务层组件

### @Controller

用于标注控制层组件(如struts中的action)
### @Repository

用于标注数据访问组件，即DAO组件
### @Component

泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。
### @PostConstruct
使得该方法在 Bean 初始化完成之后、依赖注入完成之后 自动执行。通常用于执行一些初始化逻辑。
### @PathVariable
用来获得请求url中的动态参数
```java
@Controller  
public class TestController {  

     @RequestMapping(value="/user/{userId}/roles/{roleId}",method = RequestMethod.GET)  
     public String getLogin(@PathVariable("userId") String userId,  
         @PathVariable("roleId") String roleId){
           
         System.out.println("User Id : " + userId);  
         System.out.println("Role Id : " + roleId);  
         return "hello";  
     
     }  
}
```
### @ComponentScan

告知Spring需要扫描哪些包下的bean定义

### @Autowired
在默认情况下使用 @Autowired 注释进行自动注入时，Spring 容器中匹配的候选 Bean 数目必须有且仅有一个。当找不到一个匹配的 Bean 时，Spring 容器将抛出 BeanCreationException 异常，并指出必须至少拥有一个匹配的 Bean。
当存在多个相同实现类时，spring会通过 Bean 名称自动匹配

    如果字段名称与某个 Bean 的名称一致，Spring 会自动匹配并注入该 Bean。

    这种方式依赖于字段命名，不够直观，不推荐使用。
当不能确定 Spring 容器中一定拥有某个类的 Bean 时，可以在需要自动注入该类 Bean 的地方可以使用 @Autowired(required = false)，这等于告诉 Spring: 在找不到匹配 Bean 时也不报错
### @Configuration
配置类，需要注意的是在configuration中的@bean方法如果被类内其他方法调用，那么实际上只会被调用一次，因为@Configuration进行了AOP增强。
```java
@Configuration("name")//表示这是一个配置信息类,可以给这个配置类也起一个名称
@ComponentScan("spring4")//类似于xml中的<context:component-scan base-package="spring4"/>
public class Config {

    @Autowired//自动注入，如果容器中有多个符合的bean时，需要进一步明确
    @Qualifier("compent")//进一步指明注入bean名称为compent的bean
    private Compent compent;

    @Bean//类似于xml中的<bean id="newbean" class="spring4.Compent"/>
    public Compent newbean(){
        return new Compent();
    }   
}
```
### EnableZuulProxy
路由网关的主要目的是为了让所有的微服务对外只有一个接口，我们只需访问一个网关地址，即可由网关将所有的请求代理到不同的服务中。Spring Cloud是通过Zuul来实现的，支持自动路由映射到在Eureka Server上注册的服务。Spring Cloud提供了注解@EnableZuulProxy来启用路由代理。
### @Import(Config1.class)

即向IOC容器中注入指定的类，可以是@Configuration, ImportSelector, ImportBeanDefinitionRegistrar, or regular component 标注的需要导入的类
### @Order
@Order(1)，值越小优先级超高，越先运行
### @ConditionalOnExpression
当指定配置项为true时才创建对应的Bean
```java
@Configuration
@ConditionalOnExpression("${enabled:false}")
public class BigpipeConfiguration {
    @Bean
    public OrderMessageMonitor orderMessageMonitor(ConfigContext configContext) {
        return new OrderMessageMonitor(configContext);
    }
}
```
### @ConditionalOnProperty
通过判断其某几个属性name以及havingValue（对应的值）来判断是否注入对应Bean，其中name用来从application.properties中读取某个属性值，如果该值为空，则返回false;如果值不为空，则将该值与havingValue指定的值进行比较，如果一样则返回true;否则返回false。如果返回值为false，则该configuration不生效；为true则生效。

### @ConditionalOnClass

该注解的参数对应的类必须存在，否则不解析该注解修饰的配置类
```java
@Configuration
@ConditionalOnClass({Gson.class})
public class GsonAutoConfiguration {
    public GsonAutoConfiguration() {
    }

    @Bean
    @ConditionalOnMissingBean
    public Gson gson() {
        return new Gson();
    }
}
```
### @ConditionalOnMisssingClass({ApplicationManager.class})

如果存在它修饰的类的bean，则不需要再创建这个bean；

### @ConditionOnMissingBean(name = "example")
表示如果name为“example”的bean存在，该注解修饰的代码块不执行。

### @ControllerAdvice
众所周知@ControllerAdvice是用来配合@ExceptionHandler进行Controller层的全局统一异常处理的，但是其实@ControllerAdvice还有其他用途：

#### 结合@InitBinder注解
用于请求中注册自定义参数的解析，从而达到自定义请求参数格式的目的；

比如，在@ControllerAdvice注解的类中添加如下方法，来统一处理日期格式的格式化
@InitBinder注解
主要用于处理请求参数（如查询参数、表单数据）的绑定，而不是直接处理请求体中的 JSON 数据。因此，在 @InitBinder 中注册的 CustomDateEditor 不会自动处理 JSON 请求体中的时间字段。

以下是将URL上的非标准时间转换为Date
```java
@InitBinder
public void handleInitBinder(WebDataBinder dataBinder){
    dataBinder.registerCustomEditor(Date.class,
            new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), false));
}

@GetMapping("testDate")
public Date processApi(Date date) {
    return date;
}
```
#### 结合@ModelAttribute注解

用来预设全局参数，比如最典型的使用Spring Security时将添加当前登录的用户信息（UserDetails)作为参数。

然后所有controller类中requestMapping方法都可以直接获取并使用currentUser。
示例如下：
```java
@ModelAttribute("currentUser")
public UserDetails modelAttribute() {
    return (UserDetails) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
}
@PostMapping("saveSomething")
public ResponseEntity<String> saveSomeObj(@ModelAttribute("currentUser") UserDetails operator) {
    // 保存操作，并设置当前操作人员的ID（从UserDetails中获得）
    return ResponseEntity.success("ok");
}
```
#### @ControllerAdvice是如何起作用的（原理）？

DispatcherServlet中onRefresh方法是初始化ApplicationContext后的回调方法，它会调用initStrategies方法，主要更新一些servlet需要使用的对象，包括国际化处理，requestMapping，视图解析等等。

而在DispatcherServlet中onRefresh中会调用一下内容：
```java
	/**
	 * This implementation calls {@link #initStrategies}.
	 */
	@Override
	protected void onRefresh(ApplicationContext context) {
		initStrategies(context);
	}

	/**
	 * Initialize the strategy objects that this servlet uses.
	 * <p>May be overridden in subclasses in order to initialize further strategy objects.
	 */
	protected void initStrategies(ApplicationContext context) {
		initMultipartResolver(context);
		initLocaleResolver(context);
		initThemeResolver(context);
		initHandlerMappings(context);
		initHandlerAdapters(context);
		initHandlerExceptionResolvers(context);
		initRequestToViewNameTranslator(context);
		initViewResolvers(context);
		initFlashMapManager(context);
	}
```
从上述代码看，如果要提供@ControllerAdvice提供的三种注解功能，从设计和实现的角度肯定是实现的代码需要放在initStrategies方法中。

##### @ModelAttribute和@InitBinder处理
具体来看，如果你是设计者，很显然容易想到：对于@ModelAttribute提供的参数预置和@InitBinder注解提供的预处理方法应该是放在一个方法中的，因为它们都是在进入requestMapping方法前做的操作。

如下方法是获取所有的HandlerAdapter，无非就是从BeanFactory中获取。
```java
	/**
	 * Initialize the HandlerAdapters used by this class.
	 * <p>If no HandlerAdapter beans are defined in the BeanFactory for this namespace,
	 * we default to SimpleControllerHandlerAdapter.
	 */
	private void initHandlerAdapters(ApplicationContext context) {
		this.handlerAdapters = null;

		if (this.detectAllHandlerAdapters) {
			// Find all HandlerAdapters in the ApplicationContext, including ancestor contexts.
			Map<String, HandlerAdapter> matchingBeans =
					BeanFactoryUtils.beansOfTypeIncludingAncestors(context, HandlerAdapter.class, true, false);
			if (!matchingBeans.isEmpty()) {
				this.handlerAdapters = new ArrayList<>(matchingBeans.values());
				// We keep HandlerAdapters in sorted order.
				AnnotationAwareOrderComparator.sort(this.handlerAdapters);
			}
		}
		else {
			try {
				HandlerAdapter ha = context.getBean(HANDLER_ADAPTER_BEAN_NAME, HandlerAdapter.class);
				this.handlerAdapters = Collections.singletonList(ha);
			}
			catch (NoSuchBeanDefinitionException ex) {
				// Ignore, we'll add a default HandlerAdapter later.
			}
		}

		// Ensure we have at least some HandlerAdapters, by registering
		// default HandlerAdapters if no other adapters are found.
		if (this.handlerAdapters == null) {
			this.handlerAdapters = getDefaultStrategies(context, HandlerAdapter.class);
			if (logger.isTraceEnabled()) {
				logger.trace("No HandlerAdapters declared for servlet '" + getServletName() +
						"': using default strategies from DispatcherServlet.properties");
			}
		}
	}
```
我们要处理的是requestMapping的handlerResolver，作为设计者，就很容易出如下的结构

在RequestMappingHandlerAdapter中的afterPropertiesSet去处理advice
```java
	@Override
	public void afterPropertiesSet() {
		// Do this first, it may add ResponseBody advice beans
		initControllerAdviceCache();

		if (this.argumentResolvers == null) {
			List<HandlerMethodArgumentResolver> resolvers = getDefaultArgumentResolvers();
			this.argumentResolvers = new HandlerMethodArgumentResolverComposite().addResolvers(resolvers);
		}
		if (this.initBinderArgumentResolvers == null) {
			List<HandlerMethodArgumentResolver> resolvers = getDefaultInitBinderArgumentResolvers();
			this.initBinderArgumentResolvers = new HandlerMethodArgumentResolverComposite().addResolvers(resolvers);
		}
		if (this.returnValueHandlers == null) {
			List<HandlerMethodReturnValueHandler> handlers = getDefaultReturnValueHandlers();
			this.returnValueHandlers = new HandlerMethodReturnValueHandlerComposite().addHandlers(handlers);
		}
	}
```