## 10.8.4 选择状态

选择需要同时在状态和转换中定义以正常工作。使用`choice()`方法标记指定状态为选中。当在转换中配置这个状态时，该状态需要匹配源状态。

使用`withChoice()`定义转换的源状态和`first/then/last`语句块，这些语句块类似于普通的`if/elseif/else`。就像你在`if/elseif`声明中使用条件一样，你可以在`first`和`then`中指定guard。

确保使用`last`来使转换可退出。否则配置将不正确。

```java
@Configuration
@EnableStateMachine
public class Config13
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States.SI)
                .choice(States.S1)
                .end(States.SF)
                .states(EnumSet.allOf(States.class));
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<States, Events> transitions)
            throws Exception {
        transitions
            .withChoice()
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

Actions can be executed with both incoming and outgoing transitions of a choice pseudostate. As seeing from below example, one dummy lambda action is defined leading into a choice state and one similar dummy lambda action defined for one outgoing transition where it also define an error action.
Actions可以通过选择伪状态的传入和传出转换来执行。如以下例子所示，一个匿名lambda action

```java
@Configuration
@EnableStateMachine
public class Config23
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States.SI)
                .choice(States.S1)
                .end(States.SF)
                .states(EnumSet.allOf(States.class));
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<States, Events> transitions)
            throws Exception {
        transitions
            .withExternal()
                .source(States.SI)
                .action(c -> {
                        // action with SI-S1
                    })
                .target(States.S1)
                .and()
            .withChoice()
                .source(States.S1)
                .first(States.S2, c -> {
                        return true;
                    })
                .last(States.S3, c -> {
                        // action with S1-S3
                    }, c -> {
                        // error callback for action S1-S3
                    });
    }
}
```

> Junction have same api format meaning actions can be defined similarly.