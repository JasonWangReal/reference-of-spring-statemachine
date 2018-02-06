## 11.2 With @EnableStateMachineFactory

You’ll see same machineId getting configured if you use a StateMachineFactory and request a new machine using id.

```
StateMachineFactory<String, String> factory = context.getBean(StateMachineFactory.class);
StateMachine<String, String> machine = factory.getStateMachine("mymachine");
```