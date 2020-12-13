this指向大致可以分为4种情况
* 对象方法调用
* 普通函数调用（总是指向全局window)
* 构造器调用
* call、apply、bind修改
````html
<body>
   <div id="div1">我是一个div</div>
</body>
<script>
   document.getElementById('div1').onclick = function(){
      alert(this.id) //输出：'div1'
      var callback = function(){
          alert(this) //输出：window
      }
      callback()
   }
</script>
````
````js
// function作为构造器
function myClass(){
   this.name = 'jack'
}

var obj = new myClass();
console.log(obj.name) // 'jack'

// 有种特殊情况，如果构造器显式地返回一个对象，new一个实例就是这个对象
function myClass(){
   this.name = 'jack'
   return {
      name: 'mike'
   }
}
var obj = new myClass();
console.log(obj.name) // 'mike'
````
## 丢失的this
````js
var obj = {
   name: 'jack',
   getName: function(){
      return this.name
   }
}
console.log(obj.getName()) // 'jack'

var getName2 = obj.getName
console.log(getName2()) // undefined
````
>obj.getName时，getName方法是作为对象属性调用，此时this指向obj对象，没啥疑问；**但是当用另外一个变量getName2来引用obj.getName并调用时，此时是普通函数调用方式，this指向全局window，输出undefined**
## call、apply
````js
var obj = {
   name: 'augus'
}
window.name = 'haha'
var getName =  function(){
   console.log(this.name)
}
getName() // 'haha'
getName.call(obj) // 'augus'
````
## 借用apply实现类似继承的效果
杜鹃既不会筑巢，也不会孵雏，而是把自己的蛋寄托给云雀等其他鸟类
````js
var A = function(name){
   this.name = name
}
var B = function(){
   A.apply(this,arguments)
}
B.prototype.getName = function(){
   return this.name
}
var b = new B('qgq')
console.log(b.getName())  // 'qgq'
````