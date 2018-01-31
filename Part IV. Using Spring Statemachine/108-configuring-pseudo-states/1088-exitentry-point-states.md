## 10.8.8 Exit/Entry Point States

Exit and Entry Points can be used to do more controlled exit and entry from and into a submachines.

```java
@Configuration
@EnableStateMachine
static class Config21 extends StateMachineConfigurerAdapter<String, String> {

    @Override
    public void configure(StateMachineStateConfigurer<String, String> states)
            throws Exception {
        states
        .withStates()
            .initial("S1")
            .state("S2")
            .state("S3")
            .and()
            .withStates()
                .parent("S2")
                .initial("S21")
                .entry("S2ENTRY")
                .exit("S2EXIT")
                .state("S22");
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<String, String> transitions)
            throws Exception {
        transitions
        .withExternal()
            .source("S1").target("S2")
            .event("E1")
            .and()
        .withExternal()
            .source("S1").target("S2ENTRY")
            .event("ENTRY")
            .and()
        .withExternal()
            .source("S22").target("S2EXIT")
            .event("EXIT")
            .and()
        .withEntry()
            .source("S2ENTRY").target("S22")
            .and()
        .withExit()
            .source("S2EXIT").target("S3");
    }
}
```

As shown above you need to mark particular states as exit and entry states. Then you create a normal transitions into those states and also specify withExit() and withEntry() where those states will exit and entry respectively.