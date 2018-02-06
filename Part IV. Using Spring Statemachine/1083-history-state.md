## 10.8.3 历史状态

每个状态机可以定义一次历史状态。你需要分别选择自己的标识符和`History.SHALLOW`或者`History.DEEP`

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
正如上面示例，在同一个状态机中，可选的可以定义从历史状态到某个状态的默认转换。如果状态机从未被进入过导致没有可用的历史，则该转换将默认发生。如果未定义缺省状态转换，则执行该区域的正常进入。如果状态机的历史是最终状态，那么这个缺省的转换也会被使用。
