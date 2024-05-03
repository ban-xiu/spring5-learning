# BeanFactoryPostProcessor
在 BeanDefinition 读取完元数据，还未实例化为 bean 之前，使用 BeanFactoryPostProcessor 可以对 bean 进行一些数据修改

在 ApplicationContext 的 refresh() 方法 -> invokeBeanFactoryPostProcessors() 方法中开始创建 BeanFactoryPostProcessor

根据 BeanFactoryPostProcessor 的不同类型按一定顺序执行 invoke，实际上就是遍历所有的 BeanFactoryPostProcessor 执行对应的 postProcessBeanFactory() 方法
