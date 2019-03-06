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
```JavaScript
GlobalExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            foo: <func>,
            bar: <func>
        },
        outer: <null>,
        ThisBinding: <Global Object>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            message: 'undefined'
        },
        outer: <null>,
        ThisBinding: <Global Object>
    }
}
```

### 2.var message = 'Hello World'
```JavaScript
GlobalExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            foo: <func>,
            bar: <func>
        },
        outer: <null>,
        ThisBinding: <Global Object>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            message: 'Hello World'
        },
        outer: <null>,
        ThisBinding: <Global Object>
    }
}
```
 
### 3.foo(message)
```JavaScript
FucntionExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            Arguments: {0: message, length: 1}
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative'
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    }
}
```

### 4.bar(message);
```JavaScript
FunctionExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            Arguments: {0: message, length: 1}
        },
        outer: <FunctionLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative'
        },
        outer: <FunctionLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    }
}
```

### 5.console.log(message)
```JavaScript
console.log(message);
```

### 6.Jump out of bar FunctionExecutionContext, then goes into foo FunctionExecutionContext

### 7.Jump out of foo FunctionExecutionContexct, then goes into GlobalExecutionContext

### 8.Jump out of GlobalExecutionContext

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

### 1.Create GlobalExecutionContext
```JavaScript
GlobalExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Object'
        },
        outer: <null>,
        ThisBinding: <Global Object>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            a: <undefined>
        },
        outer: <null>,
        ThisBinding: <Global Object>
    }
}
```

### 2.var a = 'a';
```JavaScript
GlobalExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Object'
        },
        outer: <null>,
        ThisBinding: <Global Object>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Object',
            a: 'a'
        },
        outer: <null>,
        ThisBinding: <Global Object>
    }
}
```

### 3.foo();
```JavaScript
FunctionExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            Arguments: {},
            bar: <func>
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            b: <undefined>
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    }
}
```

### 4.var b = 'b';
```JavaScript
FunctionExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            Arguments: {},
            bar: <func>
        },
        outer: <FunctionLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            b: 'b'
        },
        outer: <FunctionLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    }
}
```

### 5.bar();
```JavaScript
FunctionExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            Arguments: {}
        },
        outer: <FunctionLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            c: <undefined>
        },
        outer: <FucntionLexicalEnvironment>,
        ThisBinding: <Global Object or undefinede>
    }
}
```

### 6.var c = ‘c’;
```JavaScript
FunctionExecutionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            Arguments: {}
        },
        outer: <FunctionLexicalEnviroment>,
        ThisBinding: <Global Object or undefined>
    },
    VariableEnvironment: {
        EnvironmentRecord: {
            Type: 'Declarative',
            c: 'c'
        },
        outer: <FunctionLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
    }
}
```

### 7.console.log(c);
```JavaScript
console.log(c); // 'c', find in the current FunctionExecutionContext
```

### 8.console.log(b);
```JavaScript
console.log(b); // 'b' find in the outer of the current FunctionExecutionContext
```

### 9.console.log(a);
```JavaScript
console.log(a); // 'a', find in the GlobalExecutionContext <the outer of the outer of the current FunctionExecutionContext>
```

### 10.Jump out of bar FunctionExecutionContext, then goes into foo FunctoinExecutionContext

### 11.Jump out of foo FunctionExecutionContext, then goes into GlobalExecutionContext

### 12.Jump out of GlobalExecutionContext

