## 10.8.5 Junction State

Junction needs to be defined in both states and transitions to work properly. Mark particular state as choice state by using junction() method. This state needs to match source state when transition is configured for this choice.

Transition is configured using withJunction() where you define source state and first/then/last structure which is equivalent to normal if/elseif/else. With first and then you can specify a guard just like you’d use a condition with if/elseif clauses.

Transition needs to be able to exist so make sure last is used. Otherwise configuration is ill-formed.

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