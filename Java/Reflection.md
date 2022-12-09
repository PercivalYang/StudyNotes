## Reflection API
- Reflection 是用来**在runtime阶段**检测和修改methods, class和interface的API。以一个示例来解释常见的用法
- 假设我们创建了class `Test`如下：
```java
class Test {
    // creating a private field
    private String s;
  
    // Constructor 1
    // Public constructor
    public Test() { s = "GeeksforGeeks"; }
  
    // no arguments
    public void method1()
    {
        System.out.println("The string is " + s);
    }
  
    // int as argument
    public void method2(int n)
    {
        System.out.println("The number is " + n);
    }
  
    // Private method
    private void method3()
    {
        System.out.println("Private method invoked");
    }
}
```
- 我们希望在runtime阶段，输入不同的参数来测试`Test`，首先Instantiate一个`Test`的对象
```java
Test obj = new Test();
```
- 可以通过`.getclass()`获取对象的Class type, `.getName()`查看Class Type的名称
```java
Class cls = obj.getClass();
System.out.println("The name of class is "
                           + cls.getName());
```
- 获取到Class 对象后可以进一步获取Class内的Constructor对象，通过`.getConstructor()`
```java
Constructor constructor = cls.getConstructor();
System.out.println("The name of constructor is "
                           + constructor.getName());
```
- 同样可以获取Class中的method
    ```java
    Method[] methods = cls.getMethods(); // 加s是一堆
    Method methodcall1= cls.getDeclaredMethod("method2", int.class); // 不加s代表指定Method
    ```  
    - `.getMthods()`: 只返回`public`的methods,但同样会返回父类的`public` methods;
    - `.getDeclaredMethods()`: 不涉及父类,只返回自身定义的，但能返回private protect等的method
- 上方code block获取了`Test` class中的`method2`方法，可以通过`.invoke()`来调用该方法，并传入method's args:
```java
// .invoke(Method method, arguments): arguments是传入method的参数
methodcall1.invoke(obj, 19);
```
- 同样可以获取Class中的Field，并重新设置访问权限，以及Field的value
```java
Field field = cls.getDeclaredField("s"); // 获取"s"字段对象
field.setAccessible(true); // 因为是private,设置访问权限为true以供访问
field.set(obj, "JAVA"); // 重新设置obj对象中field字段的value
```

### Dynamic Proxy
#### Proxy pattern
- 在Design Pattern中的Structural Pattern中存在Proxy Pattern，其思路和Decorator Pattern类似，都是在Interface与Implementations之间加入一个“中介”
- Proxy Pattern


## Class API
- `Class Class<T>`: 通过`Class`对象可以得到对象的class type，例如：`String.class`的type就是`Class<String>`
- 如果不清楚class的类型，可以用`Class<?>`(类似C里面的`void pointer`的道理)
