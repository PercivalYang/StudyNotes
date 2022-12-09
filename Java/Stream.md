[toc]
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

- `.flatMap(MapFunc)`: 将原stream中所有元素flat,然后根据定义的MapFunc,将每个元素Map成一个新元素并生成新的stream返回，例如：
    - `.flatMap(l->Arrays.stream(l.split(" ")))`: 适用于读取String的文件stream,将File stream返回为Array Stream 
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
