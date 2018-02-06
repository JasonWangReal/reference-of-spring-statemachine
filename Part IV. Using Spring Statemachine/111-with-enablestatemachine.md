## 11.1 With @EnableStateMachine

Setting machineId via JavaConfig as mymachine then exposes that for logs as shown above. This same machineId is also available via method StateMachine.getId().

```java
@Override
public void configure(StateMachineConfigurationConfigurer<String, String> config)
        throws Exception {
    config
        .withConfiguration()
            .machineId("mymachine");
}
```

```
11:23:54,509  INFO main support.LifecycleObjectSupport [main] -
started S2 S1  / S1 / uuid=8fe53d34-8c85-49fd-a6ba-773da15fcaf1 / id=mymachine
```

> Manual builder Section 12.2, “State Machine via Builder” uses same config interface meaning behaviour would be equivalent.
