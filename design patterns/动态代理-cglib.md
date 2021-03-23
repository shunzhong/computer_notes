#### Cglib的动态代理类的机制继承

CGLIB（Code Generation Library），可以通过类继承的方式实现动态代理避免了真实类必须实现接口这一条件。cglib 创建某个类A的动态代理类主要过程：

1. 查找A上的所有非final 的public类型的方法定义；

2. 将这些方法的定义转换成字节码；

3. 将组成的字节码转换成相应的代理的class对象；

4. 实现 MethodInterceptor接口，用来处理 对代理类上所有方法的请求

```java
 Programmer programmer = new Programmer(); 
 Advice advice = new LogAdvice(); 
 Hacker hacker = new Hacker(advice); 
 //cglib 中加强器，用来创建动态代理 

 Enhancer enhancer = new Enhancer(); 
 // 置要创建动态代理的类 
 enhancer.setSuperclass(programmer.getClass()); 

 // 设置回调，这里相当于是对于代理类上所有方法的调用，都会调用CallBack，而Callback则需要实行intercept()方法进行拦截 

 enhancer.setCallback(hacker);
 Programmer proxy = (Programmer) enhancer.create(); 

 proxy.code();
```

