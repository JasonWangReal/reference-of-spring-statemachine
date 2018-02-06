### 10.8.1 初始状态

通过使用`initial()`方法标记指定状态为初始化状态。有两个初始化方法，其中之一接受额外的参数来定义初始化action。这个初始化action可以很好的，例如，用来初始化扩展状态变量。

```java
@Configuration
@EnableStateMachine
public class Config11
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States.S1, initialAction())
                .end(States.SF)
                .states(EnumSet.allOf(States.class));
    }

    @Bean
    public Action<States, Events> initialAction() {
        return new Action<States, Events>() {

            @Override
            public void execute(StateContext<States, Events> context) {
                // do something initially
            }
        };
    }

}
```
