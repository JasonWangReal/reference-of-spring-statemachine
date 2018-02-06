## 10.11 Things to Remember

When defining actions, guards or any other references from a configuration there are things to remember how Spring Framework works with beans. In below we have defined a normal configuration with states S1 and S2 and 4 transitions between those. All transitions are either guarded by guard1 or guard2. Pay attention that guard1 is created as a real bean because it’s annotated with a @Bean, while guard2 is not.

What this mean is that event E3 would get guard2 condition as TRUE and E4 would get guard2 condition as FALSE as those are simply coming from a plain method calls to those functions.

However because guard1 is defined as a @Bean, it is proxied by a Spring Framework, thus additional calls to its method will result only one instantiation of that instance. Event E1 would get first proxied instance with condition TRUE while event E2 would get same instance with TRUE condition while method call was defined with FALSE. This is not a Spring State Machine specific behaviour, it’s just how Spring Framework works with Beans.

```java
@Configuration
@EnableStateMachine
public class Config1
        extends StateMachineConfigurerAdapter<String, String> {

    @Override
    public void configure(StateMachineStateConfigurer<String, String> states)
            throws Exception {
        states
            .withStates()
                .initial("S1")
                .state("S2");
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<String, String> transitions)
            throws Exception {
        transitions
            .withExternal()
                .source("S1").target("S2").event("E1").guard(guard1(true))
                .and()
            .withExternal()
                .source("S1").target("S2").event("E2").guard(guard1(false))
                .and()
            .withExternal()
                .source("S1").target("S2").event("E3").guard(guard2(true))
                .and()
            .withExternal()
                .source("S1").target("S2").event("E4").guard(guard2(false));
    }

    @Bean
    public Guard<String, String> guard1(final boolean value) {
        return new Guard<String, String>() {
            @Override
            public boolean evaluate(StateContext<String, String> context) {
                return value;
            }
        };
    }

    public Guard<String, String> guard2(final boolean value) {
        return new Guard<String, String>() {
            @Override
            public boolean evaluate(StateContext<String, String> context) {
                return value;
            }
        };
    }
}
```
