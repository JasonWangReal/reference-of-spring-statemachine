## 10.8.3 History State

History state can be defined once for each individual state machine. You need to choose its state identifier and History.SHALLOW or History.DEEP respectively.

```java
@Configuration
@EnableStateMachine
public class Config12
        extends EnumStateMachineConfigurerAdapter<States3, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States3, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States3.S1)
                .state(States3.S2)
                .and()
                .withStates()
                    .parent(States3.S2)
                    .initial(States3.S2I)
                    .state(States3.S21)
                    .state(States3.S22)
                    .history(States3.SH, History.SHALLOW);
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<States3, Events> transitions)
            throws Exception {
        transitions
            .withHistory()
                .source(States3.SH)
                .target(States3.S22);
    }

}
```

Also as shown above, optionally it is possible to define a default transition from a history state into a state vertex in a same machine. This transition takes place as a default if for example machine has never been entered, thus no history would be available. If default state transition is not defined, then normal entry into a region is done. This default transition is also used if machine’s history is a final state.