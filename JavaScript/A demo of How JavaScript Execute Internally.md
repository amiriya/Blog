## Learn How JavaScript Execute Internally

Here is a demo:
```JavaScript
var a = 12;
const b = 20;
console.log(a, b);
let c = 24;

function add(a, b){
  let g = 12;
  console.log('Adding ...');
  console.log( a + b + g );
}

function step(aa){
  console.log(aa, 'Step Start!');
  var c = 20;
  var d = 30;
  add(20, 30);
  console.log('Step End!');
}

step(10);
console.log(a + b + c);
```

1. Create a Global Execution Context, In the createion phase:
```
GlobalExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      b: <uninitialized>,
      c: <uninitialized>,
      add: <func>,
      step: <func>
    },
    outer: <null>,
    ThisBinding: <GlobalObject>
  },
  VaraibleEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      a: <undefined>
    },
    outer: <null>,
    ThisBinding: <GlobalObject>
  }
}
``` 

2. This Global Execution Context, In the Execution phase:
### var a = 12; ###
```
GlobalExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      b: <uninitialized>,
      c: <uninitialized>,
      add: <func>,
      step: <func>
    },
    outer: <null>,
    ThisBinding: <GlobalObject>
  },
  VaraibleEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      a: 12
    },
    outer: <null>,
    ThisBinding: <GlobalObject>
  }
}
```

### const b = 20; ###
```
GlobalExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      b: 20,
      c: <uninitialized>,
      add: <func>,
      step: <func>
    },
    outer: <null>,
    ThisBinding: <GlobalObject>
  },
  VaraibleEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      a: 12
    },
    outer: <null>,
    ThisBinding: <GlobalObject>
  }
}
```

### console.log(a, b); ###
// 输出： 12 20

### let c = 24; ###
```
GlobalExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      b: 20,
      c: 24,
      add: <func>,
      step: <func>
    },
    outer: <null>,
    ThisBinding: <GlobalObject>
  },
  VaraibleEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      a: 12
    },
    outer: <null>,
    ThisBinding: <GlobalObject>
  }
}
```

### step(10); ###
3. When the step() is invoked, a new Execution Context is created for this function, In the createion phase:
```
FunctionExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      Arguments: {0:10, length:1}
    },
    outer: <GlobalExecutionContext>,
    ThisBinding: <Global Object or undefined>
  },
  VaraiableEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      c: <undefined>,
      d: <undefined>
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

4. Function Execution Context, In the execute Phase:
### console.log(aa, 'Step Start!'); ###
// 输出： 10 Step Start!

### var c = 20; ###
```
FunctionExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      Arguments: {0:10, length:1}
    },
    outer: <GlobalExecutionContext>,
    ThisBinding: <Global Object or undefined>
  },
  VaraiableEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      c: 20,
      d: <undefined>
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

### var d = 30; ###
```
FunctionExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      Arguments: {0:10, length:1}
    },
    outer: <GlobalExecutionContext>,
    ThisBinding: <Global Object or undefined>
  },
  VaraiableEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      c: 20,
      d: 30
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

### add(20, 30); ###
5. when the add() is called, a new Execution Context is created for this function, In the Creation Phase:
```
FunctionExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      Arguments: {0:20, 1: 30, length: 2},
      g: <uninitialized>
    },
    outer: <GlobalExecutionContext>,
    ThisBinding: <Global Object or undefined>
  },
  VariableEnvironment: {
    EnvironmentRecord: {},
    outer: <GlobalExecutionContext>,
    ThisBinding: <Global Object or undefined>
  }
}
```

6. Function Execution Context, In the execute Phase:
### let g = 12; ###
```
FunctionExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      Arguments: {0:20, 1: 30, length: 2},
      g: 12
    },
    outer: <GlobalExecutionContext>,
    ThisBinding: <Global Object or undefined>
  },
  VariableEnvironment: {
    EnvironmentRecord: {},
    outer: <GlobalExecutionContext>,
    ThisBinding: <Global Object or undefined>
  }
}
```

### console.log('Adding ...');  ###
// 输出： Adding ...

### console.log( a + b + g ); ###
// 输出： 62

7. Function Execution Context(**add()**) is popped off from the Execution Stack. The JavaScript Engine reaches the execution context below(**step()**).

### console.log('Step End!'); ###
// 输出： Step End！

8. Function Execution Context(**step()**) is popped off from the Execution Stack. The JavaScript Engine reaches the execution context below(**Gloabl Execution Context**).

### console.log(a + b + c); ###
// 输出： 56

9. All is code is executed, the JavaScript engine removes the global execution context from the current stack.