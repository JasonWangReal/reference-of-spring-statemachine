## 10.10 Configuring Model

StateMachineModelFactory is a hook to configure statemachine model without using a manual configuration. Essentially it is a third party integration to integrate into a configuration model. StateMachineModelFactory can be hooked into a configuration model by using a StateMachineModelConfigurer as shown above.

```java
@Configuration
@EnableStateMachine
public static class Config1 extends StateMachineConfigurerAdapter<String, String> {

    @Override
    public void configure(StateMachineModelConfigurer<String, String> model) throws Exception {
        model
            .withModel()
                .factory(modelFactory());
    }

    @Bean
    public StateMachineModelFactory<String, String> modelFactory() {
        return new CustomStateMachineModelFactory();
    }
}
```

As a custom example CustomStateMachineModelFactory would simply define two states, S1 and S2 and an event E1 between those states.

```java
public static class CustomStateMachineModelFactory implements StateMachineModelFactory<String, String> {

    @Override
    public StateMachineModel<String, String> build() {
        ConfigurationData<String, String> configurationData = new ConfigurationData<>();
        Collection<StateData<String, String>> stateData = new ArrayList<>();
        stateData.add(new StateData<String, String>("S1", true));
        stateData.add(new StateData<String, String>("S2"));
        StatesData<String, String> statesData = new StatesData<>(stateData);
        Collection<TransitionData<String, String>> transitionData = new ArrayList<>();
        transitionData.add(new TransitionData<String, String>("S1", "S2", "E1"));
        TransitionsData<String, String> transitionsData = new TransitionsData<>(transitionData);
        StateMachineModel<String, String> stateMachineModel = new DefaultStateMachineModel<String, String>(configurationData,
                statesData, transitionsData);
        return stateMachineModel;
    }

    @Override
    public StateMachineModel<String, String> build(String machineId) {
        return build();
    }
}
```

> Defining a custom model is usually not what end user is looking for, although it is possible, however it is a central concept of allowing external access to this configuration model.

Example of using this model factory integration can be found from Chapter 32, Eclipse Modeling Support. More generic info about custom model integration can be found from Chapter 54, Developer Documentation.