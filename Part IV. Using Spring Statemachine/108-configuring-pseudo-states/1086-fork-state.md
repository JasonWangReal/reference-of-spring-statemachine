## 10.8.6 交叉状态

交叉需要同时在状态和转换中定义以正确工作。使用`fork()`方法标记某个状态选中状态。这个状态需要匹配转换配置中的源状态。

目标状态必须是区域的父状态或直接状态。使用父状态为目标状态将导致所有区域成为初始化状态。分别针对单个状态定义目标可以获得更多进入区域的控制。

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