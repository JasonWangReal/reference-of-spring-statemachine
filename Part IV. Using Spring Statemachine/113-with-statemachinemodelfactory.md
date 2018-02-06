## 11.3 With StateMachineModelFactory

Behind a scenes all machine configurations are first translated into a StateMachineModel so that StateMachineFactory don’t need to know from where configuration originated as machine can be built from JavaConfig, UML or Repository. If user wants to go crazy a custom StateMachineModel can also be used which would be a lowest possible level to define configuration.

What all these has to do with a machineId? StateMachineModelFactory also have a method StateMachineModel<S, E> build(String machineId) which a StateMachineModelFactory implementation may choose to use.

RepositoryStateMachineModelFactory Chapter 33, Repository Support uses machineId to support different configurations in a persistent storage used via Spring Data Repository interfaces. For example both StateRepository and TransitionRepository have a method List<T> findByMachineId(String machineId) order to build different states and transitions by a machineId. With RepositoryStateMachineModelFactory if machineId is used as empty or NULL defaults to repository config(in a backing persistent model) without known machine id.

> UmlStateMachineModelFactory currently doesn’t distinguish between different machine id’s as uml source is always coming from a same file. Thought this may get changed in future releases.