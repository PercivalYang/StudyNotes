# Define custom annotation
- Example:  
    ```java
    @Retention(RetentionPolicy.SOURCE)
    @Target(ElementType.TYPE)  // Applies to class, interface, or enum
    public @interface ConvertsTo {
      Class<?> targetClass();
      String setterPrefix() default "set";
    }
    ```

## Retention Policies
| Retention Policy   | Description    |
|--------------- | --------------- |
| `SOURCE`   | Annotation 只存在source code   |
| `RUNTIME`   | Annotation 可以存在`.class`格式的二进制文件，并且可以通过Reflection在runtime阶段use   |
| `CLASS`   | 存在于`.class`格式的二进制文件，但是在不存在于runtime阶段   |

## Annotation Target
- `.Target`决定程序中哪一部分可以使用特定的Annotation.  

| Element Type   | Description    |
|--------------- | --------------- |
| `ANNOTATION_TYPE`   | 对Annotation类别的声明   |
| `CONSTRUCTOR`   | Constructor 声明   |
| `FIELD`   | 字段的声明，包含enum constant   |
| `LOCAL_VARIABLE`   | 本地变量的声明   |
| `METHOD`  |  Method的声明  |
| `PACKAGE`  |  Package的声明  |
| `PARAMETER` | Method 参数的声明 |
| `TYPE`  |  Type的声明，例如class, interface, annotation type, enum declaration | 


