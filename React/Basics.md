* 组件并不是真实的DOM节点，而是存在于内存中的一种数据结构，叫做虚拟DOM。只有当它插入文档之后，才会变成真实的DOM。根据React的设计，所有的变动，都会现在虚拟DOM上发生，然后再将实际发生变动的部分，反映在真实DOM上，这种算法叫做DOM diff，可以极大提高页面的性能。

* 有时候需要从组件获取真实的DOM节点，就要使用到ref属性。
由于this.refs[refName]属性获取的是真实DOM，所以必须等到虚拟DOM插入文档之后，才能使用这个属性，否则会报错。

* 组件的style属性设置方式不能写成style='opacity: {this.state.opacity}'，而要写成style={{opacity: this.state.opacity}}。这是因为React组件样式是一个对象，所以第一重大括号表示这是JavaScript语法，第二重大括号博鳌是样式对象。
