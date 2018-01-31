10.7.3 状态Action错误处理

与转换action错误处理类似的逻辑，可以对状态action定义其行为和进入、退出action异常处理。
`StateConfigurer`的`stateEntry`，`stateDo`和`stateExit`这些方法用来定义当前`action`的`error`动作。

```java
@Configuration
@EnableStateMachine
public class Config55
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States.S1)
                .stateEntry(States.S2, action(), errorAction())
                .stateDo(States.S2, action(), errorAction())
                .stateExit(States.S2, action(), errorAction())
                .state(States.S3);
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