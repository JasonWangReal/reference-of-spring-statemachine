## 10.8.7 Join State

Join needs to be defined in both states and transitions to work properly. Mark particular state as choice state by using join() method. This state doesn’t need to match either source states or target state in a transition configuration.

Select a target state where transition goes when all source states has been joined. If you use state hosting regions as source, end states of a regions are used as joins. Otherwise you can pick any states from a regions.

```java
@Configuration
@EnableStateMachine
public class Config15
        extends EnumStateMachineConfigurerAdapter<States2, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States2, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States2.S1)
                .state(States2.S3)
                .join(States2.S4)
                .state(States2.S5)
                .and()
                .withStates()
                    .parent(States2.S3)
                    .initial(States2.S2I)
                    .state(States2.S21)
                    .state(States2.S22)
                    .end(States2.S2F)
                    .and()
                .withStates()
                    .parent(States2.S3)
                    .initial(States2.S3I)
                    .state(States2.S31)
                    .state(States2.S32)
                    .end(States2.S3F);
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<States2, Events> transitions)
            throws Exception {
        transitions
            .withJoin()
                .source(States2.S2F)
                .source(States2.S3F)
                .target(States2.S4)
                .and()
            .withExternal()
                .source(States2.S4)
                .target(States2.S5);
    }
}
```

It is also possible to have multiple transitions originating from a join state. It this case it is advised to use guards and define those so that only one guard evaluates TRUE at any given time as otherwise transition behaviour is not predicted. This is shown above where guard simply checks if extended state has variables.

```java
@Configuration
@EnableStateMachine
public class Config22
        extends EnumStateMachineConfigurerAdapter<States2, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States2, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States2.S1)
                .state(States2.S3)
                .join(States2.S4)
                .state(States2.S5)
                .end(States2.SF)
                .and()
                .withStates()
                    .parent(States2.S3)
                    .initial(States2.S2I)
                    .state(States2.S21)
                    .state(States2.S22)
                    .end(States2.S2F)
                    .and()
                .withStates()
                    .parent(States2.S3)
                    .initial(States2.S3I)
                    .state(States2.S31)
                    .state(States2.S32)
                    .end(States2.S3F);
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<States2, Events> transitions)
            throws Exception {
        transitions
            .withJoin()
                .source(States2.S2F)
                .source(States2.S3F)
                .target(States2.S4)
                .and()
            .withExternal()
                .source(States2.S4)
                .target(States2.S5)
                .guardExpression("!extendedState.variables.isEmpty()")
                .and()
            .withExternal()
                .source(States2.S4)
                .target(States2.SF)
                .guardExpression("extendedState.variables.isEmpty()");
    }
}
```