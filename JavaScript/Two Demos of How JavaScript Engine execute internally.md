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
