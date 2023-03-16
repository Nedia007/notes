#### 原始值与引用值

原始值：

- 简单数据类型（Undefined，Null，Boolean，Number，String，Symbol）

- 按值访问
- 动态属性：原始值不能有属性，但是给原始值添加属性不会报错
- 复制值：a复制给b两个值互相独立互不影响

引用值：多个值构成的对象

- 按引用访问

- js不能直接访问内存的位置，只能操作对象的引用

- 动态属性：只有引用值可以添加、删除修改他的属性和方法

  ```js
  let person = new Object();
  person.name = "Nicholas";
  console.log(person.name)//"Nicholas"
  ```

- 复制值：a复制给b 如果b改变了属性的值，a也会被改变

  - 原理 ：a复制给b的时候，是让b的引用指向了和a同样的位置，所以会互相影响

    ```js
    let obj1 = new Object();
    let obj2 = obj1;
    obj1.name ="lina"
    console.log(obj2.name);//lina
    ```

    ![image](https://user-images.githubusercontent.com/64974747/225506133-ec9e828e-a8bf-4062-b76d-cd2f4e873652.png)

#### 传递参数

##### *ECMAScript中所有函数的参数**都是**按值传递

按值传递时，会被复制到num这个局部变量

按引用传递参数的话：内存的位置会传给num这个局部变量，当内部修改了的话，会反映到函数外部（在js传参没有这个操作）

```js
function addTen(num){
	num+=10;
    return num;
}
let count = 20;
let result = addTen(count)
//count作为参数传入时，这个值会复制给num ；在里面加了10 如果时按引用传递的话，外部的话count会变成30
console.log(count);//20
console.log(result);//30
```

#### 确定类型

typeof 确定原始值是什么数据类型

instanceof  判断是否是引用值 （Object）

- 如果变量是引用类型（对象） intanceof返回true，如果是原始类型则返回false

#### 执行上下文

变量和函数的上下文决定他们可以访问哪些数据，以及行为

**全局上下文**：

- 在浏览器中是window对象，var定义的全局变量和函数会成为window对象的属性和方法，let和const的声明不会被定义在全局上下文中。

- 全局上下文会在关闭程序的时候才被销毁

**函数上下文**：

- 上下文在这个代码都执行完毕时会被销毁（包括变量和函数）

#### 作用域链

- 可以决定各级上下文的代码访问变量和函数的顺序
- 正在执行的上下文变量对象在作用域链的最前端
- 内部作用域可以访问上一级的作用域，以此类推找到最底层作用域
- 外部不能访问下一级也就是内部的作用域

#### 变量声明

##### var 函数作用域声明

- 使用var声明睡觉哦，变量会自动添加到最接近的上下文（函数的局部上下文）

- **变量未经声明就被初始化**，会自动添加到全局上下文

- 变量提升：

  ![preview](https://segmentfault.com/img/bVcO0XS/view)

https://segmentfault.com/a/1190000039288278

https://www.jianshu.com/p/cbd224250579

##### 使用let 的块级作用域声明

- 他的作用域由{}界定（if块，while块，function块，单独的块）

- 同一个作用域内不能声明两次

- 在循环中迭代声明变量 可以解决var泄露到外部的问题

  ```js
  for(var i=0;i<10;==1){}
  console.log(i)//10
  for(let j = 0; j < 10; ++j){}
  console.log(j)//error：j没有定义
  ```

##### 使用const的常量声明

- 使用const声明变量时必须同事初始化  

- 一旦声明，在这个生命周期内不能再重新赋予新的值

- 其他方面与let声明一样（块作用域）

- const只应用到顶级原语 [const 变量不能再被重新赋值，但是他的属性和键不受限制]

  ```js
  const o1={};
  o1 ={};//error:给常量赋值
  const o2={}
  o2.name ='jack'
  console.log(o2.name)
  ```

- 让整个对象不可修改  Object.freeze(), 此时再给属性赋值不会报错但是会失败

#### 内存管理

##### 优化系统内存的手段

- 执行时只保存比较数据，数据不再必要时要赋值为null
- 使用const和let声明可以提升性能

##### 内存泄漏

- 没有用关键字声明变量，会直接在window对象上创建属性（只要本身不清理就不会消失）

- 定时器产生内存泄漏   (定时器一只运行，回调函数的name会一直占用内存)

  ```js
  //定时器的回调通过闭包引用了外部变量
  let name = 'Jack';
  setInterval(() => {
  	console.log(name);
  },100)
  ```

- 使用闭包也会产生内存泄漏[闭包就是函数内部嵌套一个函数，内部函数可以访问外部的变量，但是外部不行，保护了内部函的变量不被访问]

  ```js
  let outer = function(){
  	let name = "Jack";
  	return function(){
  	 return name;//闭包一直引用name，无法清理name 
  	}
  }
  ```

  


