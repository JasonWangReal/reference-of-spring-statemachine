### 10.8.8 状态退出/进入点

状态退出/进入点可以用来进行更多从子状态机退出和进入的控制。


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

如上所示，你需要标记特定状态为退出和进入的状态。接着创建普通的状态转换，分别用`withExit()`和`withEntry()`定义退出和进入动作。