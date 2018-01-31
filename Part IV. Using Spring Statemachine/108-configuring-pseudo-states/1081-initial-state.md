## 10.8.1 Initial State

Simply mark a particular state as initial state by using initial() method. There are two methods where one takes extra argument to define an initial action. This initial action is good for example initialize extended state variables.

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
