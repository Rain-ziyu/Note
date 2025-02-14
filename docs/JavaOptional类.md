# Optional类使用方式

> 身为一名Java程序员，大家可能都有这样的经历: 调用一个方法得到了返回值却不能直接将返回值作为参数去调用别的方法。我们首先要判断这个返回值是否为null，只有在非空的前提下才能将其作为其他方法的参数。这正是一些类似Guava的外部API试图解决的问题。一些JVM编程语言比如Scala、Ceylon等已经将对在核心API中解决了这个问题。

新版本的Java，比如Java 8引入了一个新的Optional类。Optional类的Javadoc描述如下:

> 这是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。

## Optional类包含的方法

### of

> 为非null的值创建一个Optional。

of方法通过工厂方法创建Optional类。需要注意的是，创建对象时传入的参数不能为null。如果传入参数为null，则抛出NullPointerException 。

```java
//调用工厂方法创建Optional实例
Optional<String> name = Optional.of("Sanaulla");
//传入参数为null，抛出NullPointerException.
Optional<String> someNull = Optional.of(null);
```

### ofNullable

> 为指定的值创建一个Optional，如果指定的值为null，则返回一个空的Optional。

ofNullable与of方法相似，唯一的区别是可以接受参数为null的情况。示例如下:
```java
//下面创建了一个不包含任何值的Optional实例
//例如，值为'null'
Optional empty = Optional.ofNullable(null);
```

### isPresent

> 如果值存在返回true，否则返回false。

```java
//isPresent方法用来检查Optional实例中是否包含值
if (name.isPresent()) {
  //在Optional实例内调用get()返回已存在的值
  System.out.println(name.get());//输出Sanaulla
}
```

### get

> 如果Optional有值则将其返回，否则抛出NoSuchElementException。

```java
//执行下面的代码会输出: No value present 
try {
  //在空的Optional实例上调用get()，抛出NoSuchElementException
  System.out.println(empty.get());
} catch (NoSuchElementException ex) {
  System.out.println(ex.getMessage());
}
```
### ifPresent

> 如果Optional实例有值则为其调用传入的consumer函数，否则不做处理

```java
//ifPresent方法接受lambda表达式作为参数。
//lambda表达式对Optional的值调用consumer进行处理。
name.ifPresent((value) -> {
  System.out.println("The length of the value is: " + value.length());
});
```
### orElse
> 如果有值则将其返回，否则返回指定的其它值。

如果Optional实例有值则将其返回，否则返回orElse方法传入的参数。示例如下:
```java
//如果值不为null，orElse方法返回Optional实例的值。
//如果为null，返回传入的消息。
//输出: There is no value present!
System.out.println(empty.orElse("There is no value present!"));
//输出: Sanaulla
System.out.println(name.orElse("There is some value!"));
```
### orElseGet
> orElseGet与orElse方法类似，区别在于得到的默认值。orElse方法将传入的字符串作为默认值，orElseGet方法可以接受Supplier接口的实现用来生成默认值。示例如下:

```java
//orElseGet与orElse方法类似，区别在于orElse传入的是默认值，
//orElseGet可以接受一个lambda表达式生成默认值。
//输出: Default Value
System.out.println(empty.orElseGet(() -> "Default Value"));
//输出: Sanaulla
System.out.println(name.orElseGet(() -> "Default Value"));
```
### orElseThrow

> 如果有值则将其返回，否则抛出supplier接口创建的异常。

在orElseGet方法中，我们传入一个Supplier接口。然而，在orElseThrow中我们可以传入一个lambda表达式或方法，如果值不存在来抛出异常。示例如下:
```java
try {
  //orElseThrow与orElse方法类似。与返回默认值不同，
  //orElseThrow会抛出lambda表达式或方法生成的异常 

  empty.orElseThrow(ValueAbsentException::new);
} catch (Throwable ex) {
  //输出: No value present in the Optional instance
  System.out.println(ex.getMessage());
}
```
ValueAbsentException定义如下:
```java
class ValueAbsentException extends Throwable {

  public ValueAbsentException() {
    super();
  }

  public ValueAbsentException(String msg) {
    super(msg);
  }

  @Override
  public String getMessage() {
    return "No value present in the Optional instance";
  }
}
```
### map

> 如果有值，则对其执行调用mapping函数得到返回值。如果返回值不为null，则创建包含mapping返回值的Optional作为map方法返回值，否则返回空Optional。

map方法用来对Optional实例的值执行一系列操作。通过一组实现了Function接口的lambda表达式传入操作map方法示例如下:
```java
//map方法执行传入的lambda表达式参数对Optional实例的值进行修改。
//为lambda表达式的返回值创建新的Optional实例作为map方法的返回值。
Optional<String> upperName = name.map((value) -> value.toUpperCase());
System.out.println(upperName.orElse("No value found"));
```

### flatMap
> 如果有值，为其执行mapping函数返回Optional类型返回值，否则返回空Optional。flatMap与map(Funtion)方法类似，区别在于flatMap中的mapper返回值必须是Optional。调用结束时，flatMap不会对结果用Optional封装。也就是说可以返回一个泛型不一样的Optional结果。

flatMap方法与map方法类似，区别在于mapping函数的返回值不同。map方法的mapping函数返回值可以是任何类型T，而flatMap方法的mapping函数必须是Optional。
```java
//flatMap与map(Function)非常类似，区别在于传入方法的lambda表达式的返回类型。
//map方法中的lambda表达式返回值可以是任意类型，在map函数返回之前会包装为Optional。 
//但flatMap方法中的lambda表达式返回值必须是Optionl实例。 
        Optional<List<String>> upperName1 = name.flatMap(
                (value) -> Optional.of(Arrays.asList(value.toUpperCase(), value.toLowerCase())));
        System.out.println(upperName1.orElse(Collections.singletonList("No value found")));//[SANAULLA, sanaulla]
```
### filter
> 如果有值并且满足断言条件返回包含该值的Optional，否则返回空Optional。

```java
//filter方法检查给定的Option值是否满足某些条件。
//如果满足则返回同一个Option实例，否则返回空Optional。
Optional<String> longName = name.filter((value) -> value.length() > 6);
System.out.println(longName.orElse("The name is less than 6 characters"));//输出Sanaulla

//另一个例子是Optional值不满足filter指定的条件。
Optional<String> anotherName = Optional.of("Sana");
Optional<String> shortName = anotherName.filter((value) -> value.length() > 6);
//输出: name长度不足6字符
System.out.println(shortName.orElse("The name is less than 6 characters"));
```