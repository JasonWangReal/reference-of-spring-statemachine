## 10.8.5 连接状态

连接需要同时在状态和转换中定义以正确工作。使用`junction()`方法标记某个状态选中状态。这个状态需要匹配转换配置中的源状态。

使用`withJunction()`定义转换的源状态和`first/then/last`语句块，这些语句块类似于普通的`if/elseif/else`。就像你在`if/elseif`声明中使用条件一样，你可以在`first`和`then`中指定guard。

确保使用`last`来使转换可退出。否则配置将不正确。

```java
@Configuration
@EnableStateMachine
public class Config20
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States.SI)
                .junction(States.S1)
                .end(States.SF)
                .states(EnumSet.allOf(States.class));
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<States, Events> transitions)
            throws Exception {
        transitions
            .withJunction()
                .source(States.S1)
                .first(States.S2, s2Guard())
                .then(States.S3, s3Guard())
                .last(States.S4);
    }

    @Bean
    public Guard<States, Events> s2Guard() {
        return new Guard<States, Events>() {

            @Override
            public boolean evaluate(StateContext<States, Events> context) {
                return false;
            }
        };
    }

    @Bean
    public Guard<States, Events> s3Guard() {
        return new Guard<States, Events>() {

            @Override
            public boolean evaluate(StateContext<States, Events> context) {
                return true;
            }
        };
    }

}
```

> Difference between choice and junction is purely academic as both are implemented with first/then/last structure. However in theory based on uml model, choice allows only one incoming transition while junction allows multiple incoming transitions. At a code level functionality is pretty much identical.
