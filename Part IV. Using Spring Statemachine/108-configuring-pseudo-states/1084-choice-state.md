## 10.8.4 Choice State

Choice needs to be defined in both states and transitions to work properly. Mark particular state as choice state by using choice() method. This state needs to match source state when transition is configured for this choice.

Transition is configured using withChoice() where you define source state and first/then/last structure which is equivalent to normal if/elseif/else. With first and then you can specify a guard just like you’d use a condition with if/elseif clauses.

Transition needs to be able to exist so make sure last is used. Otherwise configuration is ill-formed.

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