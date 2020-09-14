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

# Spring事务处理

### 事务的传播

| **事务传播行为类型**          | **说明**                                                     |
| ----------------------------- | ------------------------------------------------------------ |
| **PROPAGATION_REQUIRED**      | **如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。这是 最常见的选择。** |
| **PROPAGATION_SUPPORTS**      | **支持当前事务，如果当前没有事务，就以非事务方式执行。**     |
| **PROPAGATION_MANDATORY**     | **使用当前的事务，如果当前没有事务，就抛出异常。**           |
| **PROPAGATION_REQUIRES_NEW**  | **新建事务，如果当前存在事务，把当前事务挂起。**             |
| **PROPAGATION_NOT_SUPPORTED** | **以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。** |
| **PROPAGATION_NEVER**         | **以非事务方式执行，如果当前存在事务，则抛出异常。**         |
| **PROPAGATION_NESTED**        | **如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与** **PROPAGATION_REQUIRED 类似的操作。** |

### Spring事务处理原理

待续。。