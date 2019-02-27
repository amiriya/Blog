了解JavaScript的内部执行机制  
![](https://cdn-images-1.medium.com/max/2600/0*qPD741uxGb8ldrYt)  
<p align="center"> Photo by Greg Rakozy on Unsplash </p>

如果你是一个JavaScript开发者，或者想成为JavaScript开发者，那么你必须知道JavaScript程序内部是如何执行的。理解执行上下文和执行栈是很重要的，对于理解JavaScript的其他概念，比如：前置、作用域和闭包。  

确切地说，理解执行上下文和执行栈的概念，会让你成为一个更好的JavaScript开发者。  

话不多说，我们开始学习吧:)

### 什么是执行上下文？
简单来说，执行上下文是对于JavaScript代码评估和执行所在环境的一个抽象。无论何时，只要有JavaScript代码运行，都会运行在一个上下文中。

### 执行上下文的类型？
在JavaScript中有三种类型的执行上下文。
- **全局执行上下文—** 这是一个默认的，基本的执行上下文。不在任何函数中的代码都在执行上下文中。它执行两件事情：创建一个全局对象，它是一个window对象（在浏览器场景下）；并将this设置成等于全局对象。一个程序中只有一个全局执行上下文。
- **函数执行上下文—** 每次调用函数时，都会为该函数创建一个新的执行上下文。每个函数都有自己的执行上下文，但是只有该函数被调用时才会创建。可以有任意数量的函数执行上下文。当创建一个新的执行上下文时，它会按照定义的顺序执行一系列步骤，这个我将在后文进行讨论。
- **Eval函数执行上下文—** 在eval函数中执行的代码，也有自己的执行上下文。但是因为JavaScirpt开发者并不经常使用eval，所以我就不在此讨论它。

### 执行栈
执行栈，在其他的编程语言中也称为“调用栈”，是一种具有LIFO（后进先出）结构的栈，用于存储代码执行期间创建的的所有执行上下文。

当JavaScript引擎第一次遇到脚本时，会创建一个全局执行上下文，并把它推入当前执行栈中。每当引擎碰到一个函数调用时，会为该函数创建一个新的执行上下文，并把它推入栈顶。

引擎执行其执行上下文位于栈顶部的函数。当这个函数完成时，它的执行栈会从栈中弹出，控件会到达当前栈中它下面的执行上下文。

我们通过下面的代码示例来理解一下：
```JavaScript
let a = 'Hello World';
 
function first(){
  console.log('Inside first Function');
  second();
  console.log('Again inside first function');
}

function second(){
  console.log('Inside second function');
}

first();
console.log('Inside Global Excution Context!');
```
![](https://cdn-images-1.medium.com/max/2000/1*ACtBy8CIepVTOSYcVwZ34Q.png)
<p align="center"> An Execution Context Stack for the above code </p>

当上面的代码在浏览器中加载时，JavaScript引擎会创建一个全局执行上下文，并把它推入当前执行栈中。当遇到first()的调用时，JavaScript引擎会为该函数创建一个新的执行上下文，并把它推入当前执行栈的顶部。

当first()函数调用second()函数时，JavaScript引擎会为该函数创建一个新的执行上下文，并把它推入当前执行栈的顶部。当second()函数执行完，它的执行上下文会从当前栈中弹出，控制到达它下方的执行上下文，就是first()函数执行上下文。

当first()执行完成，它的执行栈会从当前栈中移除，控制到达全局执行上下文。一旦所有的代码都执行完成，JavaScript引擎会从当前栈中移除全局执行上下文。

### 执行上下文是如何创建的？
至此，我们已经了解了JavaScript引擎是如何管理执行上下文的，现在我们了解一下JavaScript引擎是如何创建执行上下文的。

创建执行上下文有两个阶段：1）创建阶段； 2）执行阶段。

### 创建阶段
执行上下文是在创建阶段创建的。在创建阶段会做以下事情：

1. 创建 **LexicalEnvironment** 组件。  
2. 创建 **VariableEnvironment** 组件。

因此，执行上下文的概念可以表示如下：
```
executionContext = {
  LexicalEnvironment = <ref. to LexicalEnvironment in memory>,
  VariableEnvironment = <ref. to VariableEnvironment in memory>
}
```

#### LexicalEnvironment
官方ES6文档是这样定义Lexical Enviroment的：
> A Lexical Environment is a specification type used to define the association of Identifiers to specific variables and functions based upon the lexical nesting structure ECMAScript code. A Lexical Environmenet consists of an Environment Record and a possibly null reference to an outer Lexical  Environment.

简单地说，词汇环境是一个包含标识符-变量映射的结构。（标识符指的是变量/函数的名字，变量指的对实际对象（包括函数对象和数组对象）或者原始值的引用。）

例如，查看以下代码片段：
```JavaScript
var a = 20;
var b = 40;

function foo(){
  console.log('bar');
}
```

所以上面代码片段的词法环境是这样的：
```
lexicalEnvironment = {
  a: 20,
  b: 40,
  foo: <ref. to foo function>
}
```

每个Lexical Environment都有三个组件：
1. Environment Record
2. Reference to the outer environment
3. This binding

#### Environment Record  
Environment record是变量和函数声名存储在lexical environment中的位置。

有两种类型的lexical environment：
- **Declarative environment record——** 顾名思义，用来存储变量和函数声名。函数代码的词法环境包含一个declarative environment record。
- **Object environment record——** 全局代码的lexical environment包含一个object environment record。除了变量和函数声名，object environment record 也存储了一个全局绑定对象（浏览器中的window对象）。目前，对于每个绑定对象的属性（对于浏览器，包含浏览器提供给windwo对象的属性和方法），将会在记录中创建一个新的条目。

**Note——** 对于函数代码，environment record还包含一个arguments对象，包含传递给函数的索引和参数之间的映射，以及传给函数参数的长度（数字）。举例，下面函数的参数对象是这样的：
```JavaScript
function foo(a,b){
 var c = a + b;
}
foo(2, 3);

// argument Object
Arguments:{0:2, 1:3, length:2}
```
#### Reference to the Outer environment
外部环境的引用，意味着可以访问它的外部lexcial environment。也就是说JavaScript引擎可以查找外部环境的变量，如果他们不在当前的lexical environment。

#### This Binding
在这个组件中，this的值是确定的。

在全局执行上下文中，this的值指向全局对象。（在浏览器中，this指向Window对象。）

在函数执行上下文中，this的值取决于函数的调用方式。如果this被对象引用调用，那么它的值被设置为该对象；否则，this的值被设定为全局对象或者undefined（在严格模式中）。举例：

```JavaScript
const Person = {
 name: 'Peter',
 birthYear: 1994,
 calcAge: function(){
  console.log(2019 - this.birthYear);
 }
}
Person.calcAga(); // this refers to 'Person', because 'calc' was called by 'Person' object reference.

const calculateAge = person.calcAge();
calculateAge(); // this refers to the global window object, because no object reference was given.
```
抽象地说，lexical environment用伪代码表示是这样的：
```JavaScript
GlobalExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Object'
    },
    outer: null,
    this: <global object>
  }
}

FunctionExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative'
    },
    outer: <global or outer function environment reference>,
    this: <depends on how function is called>
  }
}
```

#### VariableEnvironment
这也是一个Lexical Environment，其EnvironmentRecord保存了在这个执行上下文中由VariableStatement创建的绑定。

如上所属，Variable Environment也是一个Lexical Environment，所以它有如上Lexical Environment定义的的所有属性和组件。

在ES6中，Lexical Environmet和Variable Environment之间的一个区别就是：Lexical Environment用来存储函数声明和变量（let 和 const）绑定，Variable Environment仅仅用来存储变量（var）绑定。

### 执行阶段
在这个阶段，所有这些变量的赋值都完成了，代码最终被执行。

### 举例：

```JavaScript
let a = 20;
const b = 30;
var c;

function multipy(e, f){
  var g = 20;
  return e * f * g;
}
c = multipy(20, 30);
```

上面的代码执行的时候，JavaScript引擎会创建了一个全局执行上下文来执行所有的代码。所以全局执行上下文看起来像创建阶段。

```
GlobalExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      a: <uninitialized>,
      b: <uninitialized>,
      multiply: <func>
    },
    outer: <null>,
    ThisBinding: <Global Object>
  },
  VariableEnvironmemt: {
    EnvironmentRecord: {
      Type: 'Object',
      c: undefined
    },
    outer: <null>,
    ThisBinding: <Global Object> 
  }
}
```

在执行阶段，变量分配完成。所以在执行阶段，全局执行上下文看起来是这样的：
```
GlobalExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Object',
      a: 20,
      b: 30,
      multiply: <func>
    },
    outer: <null>,
    ThisBinding: <Global Object>
  },
  VaraiableEnvironment: {
    EnvironmentRecord: {
      Type: <Object>,
      c: undefine
    },
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

当调用multiply(20, 30)函数的时候，将创建一个新的函数执行上下文来执行函数代码。所以在创建阶段，函数执行上下文看起来是这样的：
```
FunctionExecutionContext = {
  LexicalEnvironment:{
    EnvironmentRecord: {
      Type: 'Declarative',
      Arguments: {0: 20, 1: 30, length: 2}
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  },
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      g: undefined
    },
    outer: <GlobalLexicalEnviroment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

在此之后，执行上下文经历执行阶段，意味着对函数内部变量的分配已经完成。函数执行上下文在执行阶段是这样的：
```
FunctionExecutionContext  = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      Arguments: {0: 20, 1: 30, length: 2}
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  },
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: 'Declarative',
      g: 20
    },
    outer: <GlobalLexicalContext>,
    ThisBinding: <Global Object or undefined>
  }
}
```

函数执行完成之后，返回值存储在c。所以Global Lexical Environment更新了。然后全部代码执行完成，程序结束。

**Notes—** 你可能已经注意到了，在创建阶段，let和const定义的变量，没有任何与它们相关联的值；但是var定义的变量被设置成了undefined。

这是因为，在创建阶段，会扫描代码中的变量和函数声名，而函数声明将完整地存储在环境中，变量最初被定义为undefined（对于var）或者是保持未初始化（对于let、const）。

这就是为什么可以在变量声明之前访问var定义的变量（尽管是undefined），但是如果在let和const变量声明之前访问它们，会返回一个引用错误。

这就是，所谓的“前置”（Hoisting）。

**Notes—** 在执行阶段，如果JavaScript引擎无法在源代码声明的实际位置找到let变量的值，就会把它设置为undefined。

### 结论
我们已经讨论了JavaScript的内部运行机制。尽管了解所有这些概念，对你成为一个优秀的JavaScript开发者并不是必须的，但是对上面的概念有良好的理解，有助于你更轻松、更深入地理解其他概念。比如：前置、作用域、闭包。



---
**重点概念抽离**
- LexicalEnvironment
- VariableEnvironment

---
**翻译中一些惯用语**
- As its name suggests 顾名思义

---
翻译原文：https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0
