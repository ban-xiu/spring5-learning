# aop

Advice 接口定义了切面的增强方式，如：
前置增强 BeforeAdvice，后置增强 AfterAdvice

Pointcut 接口定义了切面匹配模式，如：
JdkRegexpMethodPointcut 和 NameMatchMethodPointcut，前者主要通过正则表达式对方法名进行匹配，后者则通过匹配方法名进行匹配

Advisor 接口将 Pointcut 和 Advice 有效地结合在一起，如：DefaultPointcutAdvisor，它定义了在哪些方法（Pointcut）上执行哪些动作（Advice）

以 ProxyFactoryBean 的实现为例，对 AOP 的实现原理进行分析：

ProxyFactoryBean 主要持有目标对象 target 的代理对象 aopProxy，和 Advisor 通知器，而 Advisor 持有 Advice 和 Pointcut，
这样就可以判断 aopProxy 中的方法 是否是某个指定的切面 Pointcut，然后根据其配置的织入方向（前置增强/后置增强），通过反射为其织入相应的增强行为 Advice

ProxyFactoryBean 的 getObject() 方法先对通知器链进行了初始化，然后根据被代理对象类型（单例/原型）的不同，生成代理对象

- 初始化 Advisor 链：通过对 ioc 容器的 getBean() 方法的调用来获取配置好名称的 advisor 通知器
- 生成代理对象（以单例为例）：如果目标类是接口，则使用 JDK 动态代理，否则使用 CGLIB 生成 AopProxy 代理对象
- 以 JDK 动态代理为例：JdkDynamicAopProxy implements AopProxy, InvocationHandler，这个类重写了 invoke() 方法，如果有拦截器链，则需要先调用拦截器链中的拦截器，再调用目标的对应方法
- 具体来说是走 ReflectiveMethodInvocation.proceed() 方法：通过 methodMatcher 和 Pointcut 做匹配，执行增强方法和原本方法
