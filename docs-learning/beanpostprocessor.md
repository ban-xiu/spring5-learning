# BeanPostProcessor
BeanPostProcessor 是 bean 的后置处理器，作用是在 bean 对象实例化和依赖注入完成后，在配置文件 bean 的 init-method() 等方法的前后做一些处理

BeanPostProcess 可以有多个，并且可以通过设置 order 属性来控制这些 BeanPostProcessor 实例的执行顺序，此时 BeanPostProcessor 必须要实现 Ordered/PriorityOrdered 接口

如果某个类实现了 BeanPostProcessor 则它会在 AbstractApplicationContext 中的 registerBeanPostProcessors(beanFactory) 方法中先创建 bean 而不是和普通的 bean 一样在 finishBeanFactoryInitialization(beanFactory) 中才被创建

AutowiredAnnotationBeanPostProcessor 是 BeanPostProcessor 的一个子类，是 @Autowired 和 @Value 的具体实现