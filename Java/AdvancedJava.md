[toc]
# Advanced Java
## Lambda Expression
```java
BinaryOperator<Integer> add =
   (Integer a, Integer b) -> { return a + b; };

System.out.println(add.apply(1, 2));
```
- `()`内定义参数，`->`指向implementation`{}`
- `.apply()`执行Lambda Expression
- `()`内的参数类型可以省略，complier会自动推导
- `{}`内的`return`可以省略`
### Shortcomings
- 只能用来implement `Functional Interface`, not classes;
- Lambdas cannot implement any interface that has multiple abstract methods.
- Lambdas cannot throw checked exceptions (any subclass of Exception, such as IOException).

## Functional Interfaces
```java
@FunctionalInterface
public interface Predicate<T> {
  boolean test(T t);
  default Predicate<T> negate() { return (t) -> !test(t); }

  // Other methods left out of this example
}
```
- `Functional Interface`只能有一个`Abstract method`，但是可以有多个`static`, `default`等其它类型的methods；
- `Functional Interface`里的`Abstract method`可以通过Lambda表达式实现；
- `@FunctionalInterface`的作用：
    - 查错，如果不符合规范，如出现多个`Abstract method`时complier会报错;
    - 代码标注清晰

## Anonymous Subclasses  
- 和Lambda区别：  
    |Anonymous|Lambda|
    |---------|------|
    |Class generated at compile-time|Class generated at runtime|
    |Can override `equals()`/`hashCode()`|Cannot override them|
    |`this` refers to the anonymous class|`this` refers to the enclosing class|

## Stream
### Conceptions
- 流程: Create a stream -> Aplly **intermediate** operations -> Apply one **terminal** operation
- `Single-use`: Intermediate operations always return a brand new `Stream`, never the original;
- *lazily evaluated*: No computation happens until the terminal operation is called.

### Methods
- `.stream()`: create a stream;
#### Intermediate operations
- `.filter()`: 按指定trick过滤elements,例如：
    - `students.stream.filter(Objects::nonNull)`: 过滤掉Null;
    - 也可通过Lambda表达式过滤，例如：
        ```java
        List names = Arrays.asList("Reflection","Collection","Stream");
        List result = names.stream().filter(s->s.startsWith("S")).collect(Collectors.toList());
        ```

- `.map()`: 对stream中的elements进行映射，例如：
    - `.map(String::toUpperCase)`

- `.mapToInt()`: 根据括号内指定的元素，将stream转换为`IntStream`，例如：
    - `students.stream.mapToInt(Student::getScore)`：将students的stream转换为以`Student::getScore`为标识的`IntStream`

- `.sorted()`: 排序，可自定义排序方式，例如：
    - `.sorted(Comparator.reverseOrder())`

- 
#### Terminal operations
- `.collect()`: 转换为指定的Collection格式，例如
    - `.collect(Collectors.toSet())`
    - `.collect(Collectors.groupingBy(K,V))`

- `.forEach()`: 用来遍历stream中的每个elements并执行对应操作，例如：
    - `.forEach(y->System.out.println(y))`

- `.reduce()`: 将`BinaryOperator`作为参数之一，对stream中的elements进行操作如下：
    - `.reduce(0,(ans,i)-> ans+i)`：初始值为0,通过正则表达式与stream中的elements(即上述`i`)相加，返回single value(不是list value)

### Collectors
- `.collect()`是一个termianl的operation，将stream的elements转换为熟知的数据结构，如set,map,list，转换方式如下：
    - Set:  
    `Set<String> s = stringList.stream().collect(Collectors.toSet());`
    - Map:  
        ```java
        Map<Year, Long> graduatingClassSizes = studentList.stream()
        .collect(Collectors.groupingBy(
            Student::getGraduationYear, Collectors.counting());
        ```

### Optional Type
- `java.util.Optional`: a container object that may or may not contain a single, non-null value.
- Optional is an alternative to using `null` to represent the absence of a value.
- 来看个例子：
    ```java
    int getTopScore(List<Student> students) {
     return students.stream()
     .filter(Objects::nonNull)
     .mapToInt(Student::getScore)
     .max()
     .orElse(0);
    }
    ```
    - `.max()`是terminal operation,他会返回一个`OptionalInt`，如果student这个List是空的，`.max()`会返回`OptionalInt.empty()`，然后就会继续call `.orElse(0)`以返回`0`
- 为什么要用`Optinal Type`？
    - 相比`null`，`Optional Tpye`可以避免`NullPointerException`，因为`Optional Type`有可以唤醒保证程序继续执行的method例如`OptianlInt.empty()`
- 缺点：`Optional Type`必须要call`.get()`，才能获取其内部的value，增加code verbose

## File & Path
- 文件的读取步骤：  
![FileRead](./imgs/FileRead.png =400x300) 
### File

#### Readers/Writers
- `Readers`:
    ```java
    Reader reader =
       Files.newBufferedReader(Path.of("test"), StandardCharsets.UTF_8);
    while (reader.read(data) != -1) {
     useData(data); // useData是抽象的,此处举例用
    }
    reader.close();
    ```
- `Writers` & `BufferedWriter`:
    ```java
    Writer writer =
       Files.newBufferedWriter(Path.of("test"),StandardCharsets.UTF_8);
    writer.write("hello, world");
    writer.write("Hello, ");
    writer.write("world!"); // 虽然调用了两次write,但实际只想output stream write了一次，因为BufferedWriter在内存中的缓存大小是600bytes
    writer.flush();
    writer.close();
    ```
    - `.flush()`的作用是立即empty out `BufferedWriter`在内存中的缓存


#### Methods & Types
- `InputStream`
    - `.read(data)`: 读取数据到`data`中
- `OutputStream`
    - `.write(data)`：将数据`data`写到`OutputStream`中，即File中
- 下面假设`in`->`InputStream`, `out`->`OutputStream`:
    - `in.transferTo(out)`
- `Files.copy(Path.of(arg1), Path.of(arg2))`

#### Type

### Unicode encodings
#### UTF-8
- At least 8 bits to store each character. Up to 24 bits each for non-ASCII characters
#### UTF-16
- At least 16 bits, including lower-order ASCII and higher-oder non-ASCII

### Path
- `Path.of(String path1, String path2)`: 拼接路径`path1`, `path2`

### Prevent resource leaks
- 为什么要关闭通过stream打开的文件：
    - 避免资源浪费，占用系统资源;
    - 操作系统会限制打开的文件个数；
    - 确保程序运行后文件保持最新版本，而非因出错导致`Writer`没有关闭，文件无法更新
#### `try-catch-finally`
```java
Writer writer;
try {
  writer = Files.newBufferedWriter(Path.of("test"));
  writer.write("Hello, world!");
} catch (IOException e) {
  e.printStackTrace();
} finally {
  writer.close();
}
```
- `finally`语句保证即使在程序出错下，创建的`BufferedWriter`实例仍然可以关闭
#### `try-with-resources`
- 相比`finally`目前更常用的方法
```java
// Copy the contents of "foo" to "bar"
try (InputStream in   = Files.newInputStream(Path.of("foo"));
     OutputStream out = Files.newOutputStream(Path.of("bar"))) {
  out.write(in.readAllBytes());
}
```
- 在try的body block前的圆括号内进行Initialize,保证文件关闭与`try`退出同步进行
#### `Closeable` and `AutoCloseable`
- 只有`Closeable` and `AutoCloseable`的变量才能使用`try-with-resources`
- `Stream`，`Reader`, `Writer`, `InputStream`, `OutputStream`都implements了`Closeable`，即implements了`close()`(could throw an `IOException`)
- `AutoCloseable` 不会throw `IOException`
### Further Reading
- [Files JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/nio/file/Files.html)
- [Path JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/nio/file/Path.html)
- [Paths JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/nio/file/Paths.html)
- [StandardOpenOption JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/nio/file/StandardOpenOption.html)
- [Reader JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/io/Reader.html) 
- [Writer JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/io/Writer.html) 
- [Unicode](https://unicode.org/standard/standard.html) 
- [BufferedReader JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/io/BufferedReader.htm) 
- [BufferedWriter JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/io/BufferedWriter.htm) 
- [Closeable JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/io/Closeable.html) 
- [AutoCloseable JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/io/AutoCloseable.html) 
- [tryResourceClose JavaDoc](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 
- [finally JavaDoc](https://docs.oracle.com/javase/tutorial/essential/exceptions/finally.html) 

## Data Serialization(数据序列化)
### JSON
- Library: `Jackson`
    - serialize: `ObjectMapper.writeObject()`
    - deserialize: `ObjectMapper.readValue()`

### XML
- Library: `JAXB`
    - serialize: `Marshaller.marshal`
    - deserialize: `Unmarshaller.unmarshal`

