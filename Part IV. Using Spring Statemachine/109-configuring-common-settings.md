## 10.9 Configuring Common Settings

Some of a common state machine configuration can be set via a ConfigurationConfigurer. This allows to set BeanFactory, TaskExecutor, TaskScheduler, autostart flag for a state machine and register StateMachineListener instances.

```java
@Configuration
@EnableStateMachine
public class Config17
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineConfigurationConfigurer<States, Events> config)
            throws Exception {
        config
            .withConfiguration()
                .autoStartup(true)
                .machineId("myMachineId")
                .beanFactory(new StaticListableBeanFactory())
                .taskExecutor(new SyncTaskExecutor())
                .taskScheduler(new ConcurrentTaskScheduler())
                .listener(new StateMachineListenerAdapter<States, Events>())
                .transitionConflictPolicy(TransitionConflictPolicy.CHILD);
    }
}
```

State machine autoStartup flag is disabled by default because all instances handling sub-states are controlled by a state machine itself and cannot be started automatically. Also it is much safer to leave this decision to a user whether a machine should be started automatically or not. This flag will only control an autostart of a top-level state machine.

Setting machineId within a configuration is simply a convenience if user wants or needs to do it here.

Setting a BeanFactory, TaskExecutor or TaskScheduler exist for convenience for a user and are also use within a framework itself.

Registering StateMachineListener instances is also partly for convenience but is required if user wants to catch callback during a state machine lifecycle like getting notified of a state machine start/stop events. Naturally it is not possible to listen a state machine start events if autoStartup is enabled unless listener can be registered during a configuration phase.

transitionConflictPolicy can be used in cases where multiple transition paths could be selected. One usual use case for this is if machine contains anonymous transitions leading out from a sub-state and a parent state and user want to define a policy which one will be selected. This is a global setting within a machine instance and default to CHILD.

DistributedStateMachine is configured via withDistributed() which allows to set a StateMachineEnsemble which if exists automatically wraps created StateMachine with DistributedStateMachine and enables distributed mode.

```java
@Configuration
@EnableStateMachine
public class Config18
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineConfigurationConfigurer<States, Events> config)
            throws Exception {
        config
            .withDistributed()
                .ensemble(stateMachineEnsemble());
    }

    @Bean
    public StateMachineEnsemble<States, Events> stateMachineEnsemble()
            throws Exception {
        // naturally not null but should return ensemble instance
        return null;
    }
}
```

More about distributed states, refer to section Chapter 30, Using Distributed States.

StateMachineModelVerifier is an interface what is used internally to do some sanity checks for a state machine structure. Its purpose is to fail fast early instead of letting common configuration errors into a state machine itself. On default verifier is automatically enabled and DefaultStateMachineModelVerifier implementation is used.

With withVerifier() user can disable verifier or set a custom one if needed.

```java
@Configuration
@EnableStateMachine
public class Config19
        extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineConfigurationConfigurer<States, Events> config)
            throws Exception {
        config
            .withVerifier()
                .enabled(true)
                .verifier(verifier());
    }

    @Bean
    public StateMachineModelVerifier<States, Events> verifier() {
        return new StateMachineModelVerifier<States, Events>() {

            @Override
            public void verify(StateMachineModel<States, Events> model) {
                // throw exception indicating malformed model
            }
        };
    }
}
```

More about config model, refer to section Section 54.1, “StateMachine Config Model”.

> Config methods withSecurity, withMonitoring and withPersistence are documented in sections Chapter 24, State Machine Security, Chapter 29, Monitoring State Machine and Section 27.4, “Using StateMachineRuntimePersister” respectively.