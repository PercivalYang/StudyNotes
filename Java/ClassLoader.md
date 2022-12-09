## JAVA类加载
- 自带的3个加载器，加载顺序自上而下：
    - Bootstrap Classloader：顶层加载器，通过`System.getProperty("sun.boot.class.path")`得到;
    - Extension Classloader：扩展加载器，通过`System.getProperty("java.ext.dirs")`得到;
    - AppClass loader(SystemAppClass)：加载当前应用的classpath的所有类，通过`System.getProperty("java.class.path")`

