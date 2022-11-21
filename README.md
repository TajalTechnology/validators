### Client usage

```js
// Call it in app level. & use common validation rules.
const validate = new Validator();
App.use(validate);

// Set customize rules & mapping it with request endpoint
const rules = {
    name: "int|min:100",
    location: "int|min:100",
};
```

```mermaid
classDiagram

    class Validator {
        %%Take decition which validator can be use
        %%Default GrootsValidator
        +constructor(lib-name?:name);
    }

    class Context {
        +private strategy: IGrootsValidation;
        +setStrategy(IGrootsValidation: strategy) void;
        +executeStrategy(property:any) validateProperty;
    }

    Context o--|> IGrootsValidation
    class IGrootsValidation{
        <<interface>>
        +validateProperty(property:any) Promise~obj~;
    }

    ConcreteValidation .. Validator
    ConcreteValidation "1" *-- IsNulable : composition
    ConcreteValidation "1" *-- MinChar : composition
    ConcreteValidation "1" *-- MaxChar : composition
    ConcreteValidation "1" *-- Required : composition
    class ConcreteValidation{
        +public context object;
        +property:any;
        +constractor(this.parseProperty);%% Take decition which class will be execute(switch)
        +parseProperty(property:any) Promise~array~;
    }

    Context --|> GrootsValidator
    Context --|> YupValidator
    Context --|> JoiValidator
    GrootsValidator <|.. Validator : Inheritance
    GrootsValidator <|.. ConcreteValidation : composition
    class GrootsValidator{
        +concateRules(rules:json): void;
    }

    JoiValidator <|.. Validator : Inheritance
    JoiValidator <|.. ConcreteValidation : composition
    class JoiValidator{
        +concateRules(rules:json);
    }

    YupValidator <|.. Validator : Inheritance
    YupValidator <|.. ConcreteValidation : composition
    class YupValidator{
        +concateRules(rules:json);
    }

    IGrootsValidation <|.. MinChar : implements
    class MinChar {
        +property:any;
        +number:int;
        +constructor(property:any,number:int);
        +validateProperty(property:any) Promise~obj~;
    }

    IGrootsValidation <|.. MaxChar : implements
    class MaxChar {
        +property:string;
        +number:int;
        +constructor(property:any,number:int);
        +validateProperty(property:any) Promise~obj~;
    }

    IGrootsValidation <|.. Required : implements
    class Required {
        +property:any;
        +constructor(property:any);
        +validateProperty(property:any) Promise~obj~;
    }

    IGrootsValidation <|.. IsNulable : implements
    class IsNulable {
        +property:any;
        +constructor(property:any);
        +validateProperty(property:any) Promise~obj~;
    }
```
