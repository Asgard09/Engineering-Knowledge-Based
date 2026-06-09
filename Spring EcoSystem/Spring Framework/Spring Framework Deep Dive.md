# 1. Spring Bean Lifecycle
## 1.1. Lifecycle Phases
```
Container Start
    → Bean Definition Loading
        → Bean Instantiation
            → Dependency Injection
                → @PostConstruct / InitializingBean.afterPropertiesSet()
                    → Custom init-method
                        → Bean Ready for Use
                            → @PreDestroy / DisposableBean.destroy()
                                → Custom destroy-method
                                    → Bean Destroyed
```
## 1.2. Lifecycle Callbacks

| Callback                                | Mechanism             | When It Runs                           |
| --------------------------------------- | --------------------- | -------------------------------------- |
| `@PostConstruct`                        | Annotation            | After dependency injection is complete |
| `InitializingBean.afterPropertiesSet()` | Interface             | After all properties are set           |
| Custom `init-method`                    | XML/annotation config | After `afterPropertiesSet()`           |
| `@PreDestroy`                           | Annotation            | Before bean is removed from container  |
| `DisposableBean.destroy()`              | Interface             | During container shutdown              |
| Custom `destroy-method`                 | XML/annotation config | After `destroy()`                      |
