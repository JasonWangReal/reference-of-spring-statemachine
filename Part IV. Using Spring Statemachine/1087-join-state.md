### 10.8.7 联合状态

联合需要同时在状态和转换中定义以正确工作。使用`join()`方法标记某个状态选中状态。这个状态不需要匹配转换配置中的源状态或目标状态。

选择一个目标状态，当所有源状态已经连接时，转换进行。如果使用状态托管区域作为源，则区域的结束状态将用作联接。否则你可以选中区域中任意状态。

```java
@Configuration
@EnableStateMachine
public class Config15
        extends EnumStateMachineConfigurerAdapter<States2, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States2, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States2.S1)
                .state(States2.S3)
                .join(States2.S4)
                .state(States2.S5)
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
            .withJoin()
                .source(States2.S2F)
                .source(States2.S3F)
                .target(States2.S4)
                .and()
            .withExternal()
                .source(States2.S4)
                .target(States2.S5);
    }
}
```

可以有多个来自联接状态的转换。在这种情况下，建议使用guards并对其进行定义，以便在任何给定的时间内只有一个guard对其进行检测是否为TRUE，否则转换行为将不可预知。如下所示，guard只是检查扩展状态是否有变量。

```java
@Configuration
@EnableStateMachine
public class Config22
        extends EnumStateMachineConfigurerAdapter<States2, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States2, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States2.S1)
                .state(States2.S3)
                .join(States2.S4)
                .state(States2.S5)
                .end(States2.SF)
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
            .withJoin()
                .source(States2.S2F)
                .source(States2.S3F)
                .target(States2.S4)
                .and()
            .withExternal()
                .source(States2.S4)
                .target(States2.S5)
                .guardExpression("!extendedState.variables.isEmpty()")
                .and()
            .withExternal()
                .source(States2.S4)
                .target(States2.SF)
                .guardExpression("extendedState.variables.isEmpty()");
    }
}
```