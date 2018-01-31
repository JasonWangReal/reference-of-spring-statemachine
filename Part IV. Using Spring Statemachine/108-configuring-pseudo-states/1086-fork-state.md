## 10.8.6 Fork State

Fork needs to be defined in both states and transitions to work properly. Mark particular state as choice state by using fork() method. This state needs to match source state when transition is configured for this fork.

Target state needs to be a super state or immediate states in regions. Using a super state as target will take all regions into initial states. Targeting individual state give more controlled entry into regions.

```java
@Configuration
@EnableStateMachine
public class Config14
        extends EnumStateMachineConfigurerAdapter<States2, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States2, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States2.S1)
                .fork(States2.S2)
                .state(States2.S3)
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
            .withFork()
                .source(States2.S2)
                .target(States2.S22)
                .target(States2.S32);
    }

}
```