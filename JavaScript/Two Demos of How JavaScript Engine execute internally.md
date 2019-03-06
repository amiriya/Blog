### Demo 1
```JavaScript
var message = ‘Hello there’;
function foo(message) {
  bar(message);
}
function bar(message) {
  console.log(message);
}
foo(message);
```

### 1.Create GlobalExecutionContext:
```
GlobalExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            foo: <func>,
            bar: <func>
        },
        outer: <null>,
        ThisBinding: <GlobalObject>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            message: 'undefined'
        },
        outer: <null>,
        ThisBinding: <GlobalObject>
    }
}
```

### 2.var message = 'Hello World'
```
GlobalExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            foo: <func>,
            bar: <func>
        },
        outer: <null>,
        ThisBinding: <GlobalObject>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            message: 'Hello World'
        },
        outer: <null>,
        ThisBinding: <GlobalObject>
    }
}
```
 
### 3.foo(message)
```JavaScript
FucntionExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Decalrative',
            Arguments: {0: message, length: 1}
        },
        outer: <GlobalExecutionContext>,
        ThisBinding: <Global Object or undefined>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative'
        },
        outer: <GlobalExecutionContext>,
        ThisBinding: <Global Object or undefined>
    }
}
```

### 4.bar(message);
```
FunctionExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            Arguments: {0: message, length: 1}
        },
        outer: <FunctionExecutionContext>,
        ThisBinding: <Global Object or undefined>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative'
        },
        outer: <FunctionExecutionContext>,
        ThisBinding: <Global Object or undefined>
    }
}
```

### 5.console.log(message);
```
console.log(message); 
````

#### 6.Jump out of bar FunctionExecutionContext, then go into foo FunctionExecutionContext

#### 7.Jump out of foo FunctionExecutionContexct, then go into GlobalExecutionContext

#### 8.Jump out of GlobalExecutionContext

---

### Demo 2
```JavaScript
var a = ‘a’;
function foo() {
  var b = ‘b’;
  function bar() {
    var c = ‘c’;
    console.log(c); // You can access me here.   
    console.log(b); // You can access me too..
    console.log(a); // You can also access me..
  }
  bar();
}
foo();
```
