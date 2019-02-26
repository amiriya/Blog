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

创建执行上下文有两个阶段：1）创建阶段，2）执行阶段。

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

每个词汇环境都有三个组件：
1. Environment Record
2. Reference to the outer environment
3. This binding

#### Environment Record  
Environment record 是一个地方






---
**重点概念抽离**
- LexicalEnvironment
- VariableEnvironment


---
翻译原文：https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0


