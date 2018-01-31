## 10.7.2 转换Action错误处理

用户可以手工捕获异常，但通过定义转换action我们可以定义错误action，当发生异常的时候被自动调用。异常将传给`StateContext`并在其中可用。

```java
@Configuration
@EnableStateMachine
public class Config53
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineTransitionConfigurer<States, Events> transitions)
            throws Exception {
        transitions
            .withExternal()
                .source(States.S1)
                .target(States.S2)
                .event(Events.E1)
                .action(action(), errorAction());
    }

    @Bean
    public Action<States, Events> action() {
        return new Action<States, Events>() {

            @Override
            public void execute(StateContext<States, Events> context) {
                throw new RuntimeException("MyError");
            }
        };
    }

    @Bean
    public Action<States, Events> errorAction() {
        return new Action<States, Events>() {

            @Override
            public void execute(StateContext<States, Events> context) {
                // RuntimeException("MyError") added to context
                Exception exception = context.getException();
                exception.getMessage();
            }
        };
    }

}
```

相同的逻辑可以对每一个action手工执行

```java
@Override
public void configure(StateMachineTransitionConfigurer<States, Events> transitions)
        throws Exception {
    transitions
        .withExternal()
            .source(States.S1)
            .target(States.S2)
            .event(Events.E1)
            .action(Actions.errorCallingAction(action(), errorAction()));
}
```
