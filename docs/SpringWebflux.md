# Spring Webflux

[Spring官网](https://docs.spring.io/spring-framework/reference/web/webflux.html)

## Spring Web 与 Webflux组件对比

| API功能       | Servlet-阻塞式Web          | WebFlux-响应式Web            |
|---------------|----------------------------|------------------------------|
| 前端控制器    | DispatcherServlet          | DispatcherHandler            |
| 处理器        | Controller                 | WebHandler/Controller        |
| 请求、响应    | ServletRequest、ServletResponse | ServerWebExchange：ServerHttpRequest、ServerHttpResponse |
| 过滤器        | Filter（HttpFilter）       | WebFilter                    |
| 异常处理器    | HandlerExceptionResolver   | DispatchExceptionHandler     |
| Web配置       | @EnableWebMvc              | @EnableWebFlux               |
| 自定义配置    | WebMvcConfigurer           | WebFluxConfigurer            |
| 返回结果      | 任意                       | Mono、Flux、任意             |
| 发送REST请求  | RestTemplate               | WebClient                    |

Mono： 返回0|1 数据流
Flux：返回N数据流
WebFlux：底层完全基于netty+reactor+springweb 完成一个全异步非阻塞的web响应式框架
底层：异步 + 消息队列(内存) + 事件回调机制 = 整套系统
优点：能使用少量资源处理大量请求；

## 响应式的实现核心原理

### 1. HttpHandler    
HttpHandler使用非阻塞输入/输出和背压机制进行HTTP请求的基本处理 ，以及使用相应的响应式网络的适配器兼容Netty、Undertow、Tomcat、Jetty和任何Servlet容器。

HttpHandler 是一个简单的实现，只有一个方法来处理请求和响应。它是 故意最小化，它的主要和唯一目的是成为最小抽象用于处理不同的HTTP服务端API。

受支持的Servlet容器列表：

| Server name      | Server API used                                           | Reactive Streams support                                 |
|------------------|-----------------------------------------------------------|----------------------------------------------------------|
| Netty            | Netty API                                                 | Reactor Netty                                            |
| Undertow         | Undertow API                                              | spring-web: Undertow to Reactive Streams bridge          |
| Tomcat           | Servlet non-blocking I/O; Tomcat API to read and write ByteBuffers vs byte[] | spring-web: Servlet non-blocking I/O to Reactive Streams bridge |
| Jetty            | Servlet non-blocking I/O; Jetty API to write ByteBuffers vs byte[] | spring-web: Servlet non-blocking I/O to Reactive Streams bridge |
| Servlet container| Servlet non-blocking I/O                                  | spring-web: Servlet non-blocking I/O to Reactive Streams bridge |

HttpHandler使用示例：
```java
    public static void main(String[] args) throws IOException {
        //快速自己编写一个能处理请求的服务器

        //1、创建一个能处理Http请求的处理器。 参数：请求、响应； 返回值：Mono<Void>：代表处理完成的信号
        HttpHandler handler = (ServerHttpRequest request,
                                   ServerHttpResponse response)->{
            URI uri = request.getURI();
            System.out.println(Thread.currentThread()+"请求进来："+uri);
            //编写请求处理的业务,给浏览器写一个内容 URL + "Hello~!"
//            response.getHeaders(); //获取响应头
//            response.getCookies(); //获取Cookie
//            response.getStatusCode(); //获取响应状态码；
//            response.bufferFactory(); //buffer工厂
//            response.writeWith() //把xxx写出去
//            response.setComplete(); //响应结束

            //数据的发布者：Mono<DataBuffer>、Flux<DataBuffer>

            //创建 响应数据的 DataBuffer
            DataBufferFactory factory = response.bufferFactory();

            //数据Buffer
            DataBuffer buffer = factory.wrap(new String(uri.toString() + " ==> Hello!").getBytes());


            // 需要一个 DataBuffer 的发布者
            return response.writeWith(Mono.just(buffer));
        };

        //2、启动一个服务器，监听8080端口，接受数据，拿到数据交给 HttpHandler 进行请求处理
        ReactorHttpHandlerAdapter adapter = new ReactorHttpHandlerAdapter(handler);


        //3、启动Netty服务器
        HttpServer.create()
                .host("localhost")
                .port(8080)
                .handle(adapter) //用指定的处理器处理请求
                .bindNow(); //现在就绑定

        System.out.println("服务器启动完成....监听8080，接受请求");
        System.in.read();
        System.out.println("服务器停止....");


    }
```

### 2.WebHandler
org.springframework.web.server包建立在HttpHandler基础之上，通过链式调用 多个WebExceptionHandler、多个WebFilter、以及一个WebHandler进行Web API请求处理。责任链中的这些组件通过WebHttpHandlerBuilder可以简单的与Spring ApplicationContext中的自动检测到后组合在一起，或者也可以通过向Builder中注册一个component。

虽然HttpHandler的目标是简单的抽象不同HTTP服务器的使用，但 WebHandlerAPI旨在提供Web应用程序中常用的更广泛的功能集。 
例如：
在attributes中维护用户会话。
维护请求attributes。
维护请求的Locale或Principal。
访问已解析和缓存的表单数据。
大部分数据的抽象。

特殊的BEAN类型列表：
下表列出了WebHttpHandlerBuilder可以在 Spring Application ationContext中自动检测的components，或者可以主动注册的：

| Bean名称                   | Bean类型                | 数量  | 描述                                                                                     |
|----------------------------|-------------------------|-------|------------------------------------------------------------------------------------------|
| <any>                      | WebExceptionHandler     | 0…N   | 提供对来自WebFilter实例链和目标WebHandler的异常的处理。有关详细信息，请参阅异常。          |
| <any>                      | WebFilter               | 0…N   | 在滤波器链的其余部分之前和之后应用拦截样式逻辑目标WebHandler。有关详细信息，请参阅过滤器。 |
| webHandler                 | WebHandler              | 1     | 请求的处理程序。                                                                         |
| webSessionManager          | WebSessionManager       | 0..1  | 用于通过ServerWebExchange上的方法公开的WebSession。DefaultWebSessionManager默认。         |
| serverCodecConfigurer      | ServerCodecConfigurer   | 0..1  | 用于访问HttpMessageReader实例以解析表单数据和多部分数据。ServerCodecConfigurer.create()默认。|
| localeContextResolver      | LocaleContextResolver   | 0..1  | 通过ServerWebExchange上的方法公开的LocaleContext。AcceptHeaderLocaleContextResolver默认。  |
| forwardedHeaderTransformer | ForwardedHeaderTransformer | 0..1  | 对于处理转发的类型标头，可以通过提取和删除它们或仅删除它们。默认不使用。                 |

#### DispatcherHandler

Spring WebFlux类似于Spring MVC，是围绕前端控制器模式设计的， 其中核心的WebHandler，即DispatcherHandler，做为 实际工作的请求处理器她是可配置的。 这个模型是灵活的，用于支持不同的工作流程。

DispatcherHandler可以从Spring配置中发现它需要的组件。 它也被设计为Spring bean本身并实现ApplicationContextAware 用于访问它运行的上下文。如果DispatcherHandler是用bean声明的 名称为webHandler，它反过来可以为 WebHttpHandlerBuilder发现， 它将请求处理链组合在一起，如WebHandlerAPI中所述的那样。

WebFlux应用程序中的Spring配置通常包含：
- DispatcherHandler，Bean名称为webHandler
- WebFilter和WebExceptionHandler的Bean
- 一些特殊的DispatcherHandler的Bean

将以上内容配置给WebHttpHandlerBuilder去构建处理链即可，示例如下：
```java
ApplicationContext context = ...
HttpHandler handler = WebHttpHandlerBuilder.applicationContext(context).build();
```

> SpringMVC： DispatcherServlet；
> SpringWebFlux： DispatcherHandler
##### 1、请求处理流程
● HandlerMapping：请求映射处理器； 保存每个请求由哪个方法进行处理
● HandlerAdapter：处理器适配器；反射执行目标方法
● HandlerResultHandler：处理器结果处理器；

SpringMVC： DispatcherServlet 有一个 doDispatch() 方法，来处理所有请求；
WebFlux： DispatcherHandler 有一个 handle() 方法，来处理所有请求；
handler处理流程：
```java
public Mono<Void> handle(ServerWebExchange exchange) { 
		if (this.handlerMappings == null) {
			return createNotFoundError();
		}
		if (CorsUtils.isPreFlightRequest(exchange.getRequest())) {
			return handlePreFlight(exchange);
		}
		return Flux.fromIterable(this.handlerMappings) //拿到所有的 handlerMappings
        //concatMap： 先挨个元素变，然后把变的结果按照之前元素的顺序拼接成一个完整流
				.concatMap(mapping -> mapping.getHandler(exchange)) //找每一个mapping看谁能处理请求
				.next() //直接触发获取元素； 拿到流的第一个元素； 找到第一个能处理这个请求的handlerAdapter
				.switchIfEmpty(createNotFoundError()) //如果没拿到这个元素，则响应404错误；
				.onErrorResume(ex -> handleDispatchError(exchange, ex)) //异常处理，一旦前面发生异常，调用处理异常
				.flatMap(handler -> handleRequestWith(exchange, handler)); //调用方法处理请求，得到响应结果
	}
```
● 1、首先请求和响应都封装在 ServerWebExchange 对象中，由handle方法进行处理
● 2、如果没有任何的请求映射器； 直接返回一个： 创建一个未找到的错误； 404； 返回Mono.error；结束流
● 3、跨域工具，是否跨域请求，跨域请求检查是否复杂跨域，需要预检请求；
● 4、最后是Flux流式操作，先找到HandlerMapping，再获取handlerAdapter，再用Adapter处理请求，期间的错误由onErrorResume触发回调进行处理；

实际上正常请求主要依赖于
handleRequestWith： 编写了handlerAdapter怎么处理请求
在内部请求处理完成之后使用handleResult：处理我们的结果，如： String、User、ServerSendEvent、Mono、Flux ...

## 注解开发
### 1. 方法参数
首先基本上原本WEB时使用的注解现在基本上没有变化，只是在底层实现上的方式可能有所变化。还有就是取消了原本的request与response，统一放入ServerWebExchange。
| Controller method argument                          | Description                                                                 |
|-----------------------------------------------------|-----------------------------------------------------------------------------|
| ServerWebExchange                                   | 封装了请求和响应对象的对象; 自定义获取数据、自定义响应                      |
| ServerHttpRequest, ServerHttpResponse               | 请求、响应                                                                  |
| WebSession                                          | 访问Session对象                                                             |
| java.security.Principal                             |                                                                             |
| org.springframework.http.HttpMethod                 | 请求方式                                                                    |
| java.util.Locale                                    | 国际化                                                                      |
| java.util.TimeZone + java.time.ZoneId               | 时区                                                                        |
| @PathVariable                                       | 路径变量                                                                    |
| @MatrixVariable                                     | 矩阵变量                                                                    |
| @RequestParam                                       | 请求参数                                                                    |
| @RequestHeader                                      | 请求头                                                                      |
| @CookieValue                                        | 获取Cookie                                                                  |
| @RequestBody                                        | 获取请求体，Post、文件上传                                                  |
| HttpEntity<B>                                       | 封装后的请求对象                                                            |
| @RequestPart                                        | 获取文件上传的数据 multipart/form-data.                                     |
| java.util.Map, org.springframework.ui.Model, and org.springframework.ui.ModelMap | Map、Model、ModelMap                                                        |
| @ModelAttribute                                     |                                                                             |
| Errors, BindingResult                               | 数据校验，封装错误                                                          |
| SessionStatus + class-level @SessionAttributes      |                                                                             |
| UriComponentsBuilder                                | 用于准备相对于当前请求的主机、端口、方案和上下文路径的URL。参见URI链接。     |
| @SessionAttribute                                   |                                                                             |
| @RequestAttribute                                   | 转发请求的请求域数据                                                        |
| Any other argument                                  | 所有对象都能作为参数：<br>1、基本类型 ，等于标注@RequestParam <br>2、对象类型，等于标注 @ModelAttribute |

### 2.返回响应
| Controller method return value                                      | Description                                                                 |
|---------------------------------------------------------------------|-----------------------------------------------------------------------------|
| @ResponseBody                                                       | 把响应数据写出去，如果是对象，可以自动转为json                              |
| HttpEntity<B>, ResponseEntity<B>                                    | ResponseEntity：支持快捷自定义响应内容                                      |
| HttpHeaders                                                         | 没有响应内容，只有响应头                                                    |
| ErrorResponse                                                       | 快速构建错误响应                                                            |
| ProblemDetail                                                       | SpringBoot3；                                                               |
| String                                                              | 就是和以前的使用规则一样；                                                 |
| forward:                                                            | 转发到一个地址                                                             |
| redirect:                                                           | 重定向到一个地址                                                           |
| 配合模板引擎                                                        |                                                                             |
| View                                                                | 直接返回视图对象                                                           |
| java.util.Map, org.springframework.ui.Model                          | 以前一样                                                                   |
| @ModelAttribute                                                     | 以前一样                                                                   |
| Rendering                                                           | 新版的页面跳转API； 不能标注 @ResponseBody 注解                             |
| void                                                                | 仅代表响应完成信号                                                         |
| Flux<ServerSentEvent>, Observable<ServerSentEvent>, or other reactive type | 使用  text/event-stream 完成SSE效果                                        |
| Other return values                                                 | 未在上述列表的其他返回值，都会当成给页面的数据；                             |

