### 10.8.2 结束状态

使用`end()`方法简单地将一个特定的状态标记为结束状态。可以在每个单独的子状态机或区域中最多执行一次。

```java
@Configuration
@EnableStateMachine
public class Config1Enums
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States.S1)
                .end(States.SF)
                .states(EnumSet.allOf(States.class));
    }

}
```