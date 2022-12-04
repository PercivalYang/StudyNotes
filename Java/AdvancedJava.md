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
#### Methods & Types
- `InputStream`
    - `.read(data)`: 读取数据到`data`中
- `OutputStream`
    - `.write(data)`：将数据`data`写到`OutputStream`中，即File中
- 下面假设`in`->`InputStream`, `out`->`OutputStream`:
    - `in.transferTo(out)`
- `Files.copy(Path.of(arg1), Path.of(arg2))`

#### Type

### Path
- `Path.of()`
### Further Reading
- [Files JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/nio/file/Files.html)
- [Path JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/nio/file/Path.html)
- [Paths JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/nio/file/Paths.html)
- [StandardOpenOption JavaDoc](https://docs.oracle.com/javase/10/docs/api/java/nio/file/StandardOpenOption.html)
