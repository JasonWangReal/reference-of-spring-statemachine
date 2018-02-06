## 12.2 State Machine via Builder

Using adapters shown above has a limitation imposed by its requirement to work via Spring @Configuration classes and application context. While this is a very clear model to configure a state machine instances it will limit configuration at a compile time which is not always what a user wants to do. If there is a requirement to build more dynamic state machines, a simple builder pattern can be used to construct similar instances. Using strings as states and events this builder pattern can be used to build fully dynamic state machines outside of a Spring application context as shown above.

```java
StateMachine<String, String> buildMachine1() throws Exception {
    Builder<String, String> builder = StateMachineBuilder.builder();
    builder.configureStates()
        .withStates()
            .initial("S1")
            .end("SF")
            .states(new HashSet<String>(Arrays.asList("S1","S2","S3","S4")));
    return builder.build();
}
```

Builder is using same configuration interfaces behind the scenes that the @Configuration model using adapter classes. Same model goes to configuring transitions, states and common configuration via builder’s methods. This simply means that whatever you can use with a normal EnumStateMachineConfigurerAdapter or StateMachineConfigurerAdapter can be used dynamically via a builder.

> Currently builder.configureStates(), builder.configureTransitions() and builder.configureConfiguration() interface methods cannot be chained together meaning builder methods needs to be called individually.

```java
StateMachine<String, String> buildMachine2() throws Exception {
    Builder<String, String> builder = StateMachineBuilder.builder();
    builder.configureConfiguration()
        .withConfiguration()
            .autoStartup(false)
            .beanFactory(null)
            .taskExecutor(null)
            .taskScheduler(null)
            .listener(null);
    return builder.build();
}
```

It is important to understand on what cases common configuration needs to be used with a machines instantiated from a builder. Configurer returned from a withConfiguration() can be used to setup autoStart, TaskScheduler, TaskExecutor, BeanFactory and additionally register a StateMachineListener. If StateMachine instance returned from a builder is registered as a bean via @Bean, i.e. BeanFactory is attached automatically and then a default TaskExecutor can be found from there. If instances are used outside of a spring application context these methods must be used to setup needed facilities.