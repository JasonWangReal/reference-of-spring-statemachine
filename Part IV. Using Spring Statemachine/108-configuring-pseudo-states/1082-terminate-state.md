## 10.8.2 Terminate State

Simply mark a particular state as end state by using end() method. This can be done max one time per individual sub-machine or region.

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