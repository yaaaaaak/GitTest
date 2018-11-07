1、Spring4中的@Profile有经过重构，实际上是通过@Conditional实现的。



2、@Scope有4种而不是2种，除了Singleton，Prototype，还有Session和Request（仅对web应用）。后两者在声明时需要注意设置代理模式，以Session域为例，bean的声明如下（注入不变）：

```java
@Bean
@Scope(value="Session",ProxyMode="自行查api，interface或者targetClass")

```

配置了代理后，实际注入的是一个代理，详见p87的图。



3、Spring AOP只支持方法级别的切面，其余如构造器切面需要通过其他手段去实现。本质还是代理，而原生AspectJ是一门独立的语言编写的，Spring AOP编写时也只是借鉴其部分语法。



4、Spring AOP同一个方法被切多次，可以通过以下三种形式定义先后顺序：

```java
//其一
@AspectJ
@Order(1)
public class AspectDemo1{}

//其二
public class AspectDemo2 implements Ordered{
    
    @Override
    public int getOrder(){
        return 2;
    }
}

//其三 xml在aop命名空间配置 order="3"，不多赘述

```



5、Spring AOP还可以给代理的类通过增加接口实现扩展方法，达到无侵入扩展的目的，特别适合对他人写的底层或者无源码的外部包的切入。不过调用新的方法需要强转成扩展接口的类型，某种程度上让表层代码变得结构混乱了，个人用着不是太爽。具体搜关键字@DeclareParents，这里不细讲。



