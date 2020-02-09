# SpringBean的生命周期

> 1. Spring容器启动
>
> 2. 实例化Bean对象 *`调用bean的默认构造方法`*
>
> 3. 设置对象属性 *`对象的属性注入`*
>
> 4. 检查Aware相关接口并设置相关依赖
>
>    - 调用BeanNameAware的`setBeanName()`方法
>
>    - 调用BeanFactoryAware的`setBeanFactory()`方法
>
>    - 调用ApplicationContextAware的`setApplicationContext()`方法
>
> 5. 调用BeanPostProcessor的前置处理`postProcessBeforeInitialization`
>
> 6. 调用InitializingBean的`afterPropertiesSet()`
>
> 7. 调用自定义配置的`init-method`  *`初始化bean,一些自定义的操作`*
>
> 8. 调用BeanPostProcessor的后置处理`postProcessAfterInitialization`
>
> 9. scope为[^singleton]的缓存在springIOC容器中  
>
>    scope为[^prototype]的生命周期交给客户端
>
>    [^singleton]: 一个spring容器只会有一个Bean实例，此为Spring的默认配置
>    [^prototype]: 每次调用都会创建一个新的实例
>
> 12. Spring容器关闭
> 13. 调用DisposableBean的`afterProperties()`
> 14. 调用自定义配置的`destory-method`