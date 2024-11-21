# TypeScript

## 系统类型

TypeScript继承了Js的类型

- boolean （true false）

- string （模版字符串 字符串）

- number（整数，浮点数、非十进制数）

-  bigint （大整数）js版本不能低于es2020

  - ##### bigInt是什么？

     js在非常大的整数会失去精度 Number类型只能表示在-9007199254740991
    (-(253-1))和 9007199254740991 (253-1)之间的整数

  - js获取安全范围的最大最小数值

    Number.MAX_SAFE_INTEGER   Number.MIN_SAFE_INTEGER

  - ```js
    //创建bigInt 只需要在整数末尾+n
    console.log(9007199254740995n)
    //直接调用BigInt构造函数 
    BigInt("9007199254740995")
    //字面量可以写成二进制、八进制、十六进制
    //不可以使用严格的相等运算符来比较BigInt和普通数字
    console.log(10n===10)
    console.log(typeof 10n) //bigint
    console.log(typeof 10) //number
    //可以使用相等运算符 进行隐式转换
    console.log(10n==10)//true
    //可以使用所有算数运算符 除了一元+运算符
    （因为加号会产生Number类型的值，或抛出异常，也会破坏asm.js 代码）
    //BigInt运算符操作后返回的也是一个BigInt值
    25n /10n 结果会自动四舍五入到最接近的整数
    //不支持number和bigint混合使用 但是比较关系运算符不遵守此规则
    ```

  ```tsx
  
  //bigint 与number 类型不兼容
  const x:bigint =123;
  const y:bigint =3.14;
  
  ```

- symbol 

  - symbol是什么？

    用来解决命名冲突问题的类型 不能进行运算 不能for in 遍历

  - 语法

  - ```js
    let sym1=Symbol();
    let sym2=Symbol('foo');
    let sym3=Symbol('foo');
    //每次都会创建一个新的symbol类型
    Symbol('foo')==Symbol('foo')//false
    sym2===sym3//false
    //symbol定义的对象的属性不能使用for in 循环遍历 
    //可以使用Reflect.ownKeys来获取对象的所有键名
    let age=Symbol('age');
    let sex=Symbol('sex');
    let person={
    	name='zhangsan',
    	[age]：23，
    	[sex]:'male',
    	[sumbol('test')]:'test'
    }
    //使用Reflect.ownKeys来获取对象的所有键名
    let keys = Reflect.ownKeys(person)
    //取出所有的值
    keys.forEach(element=>{
    	console.info(person[element])
    })
    //重新使用同一个Symbol值 Symbol.for()
    //她接受一个字符串作为参数，搜索有没有同名字的symbol值 
    有则使用，没有则新建 并且登记到全家环境中以供搜索
    let s1=Symbol.for('foo');
    let s2=Symbol.for('foo')
    s1===s2//true
    //Symbol.keyFor 返回已登记的Symbol类型值的key
    let s1=Symbol.for("foo")
    symbol.keyFor(s1)//'foo'
    let s2 =Symbol('foo');
    Symbol.keyFor(s2)//undefined
    ```

    - const x:symbol =Symbol();

    

- object 

- undefined  该类型只有一个值

  let x:undefined =undefined（值）;

-  null 该类型只有一个值

  const x:null = null 

- 如果没用声明类型的变量 被赋值为undefined 或者null ；再关闭编译设置noImplicitAny和strictNullChecks 会被推断为any

  ```
  let a=undefined //any
  const b=undefined//any
  let c=null //any
  let d=null//any
  //打开编译选项 strictNullChecks 就不会出现上面的情况
  ```

  #### 包装对象类型

- 8种类型之中，undefined,null属于特殊值，object是属于复合类型）剩下五种不可拆分的属于原始类型 boolean、string、number、bigint、symbol

- 上面的五种类型会产生对应的包装对象 
- 概念：包装对象：这五种原始类型的值在需要时会自动产生的对象

```js
'hello'.charAt(1)
//js种只有对象有方法，但是这个代码可以运行
//因为调用方法的时候字符串会自动转为包装对象
//省去了字符串的处理
```

- Boolean()、String()、Number() 可以直接获取某个原始类型值的包装对象

  ```js
  //需要带new（当作构造函数使用时） 才能是包装对象 
  const s=new String('hello');
  typeof s //Object
  s.charAt(1)//e
  ```

  ![image-20240223173647083](C:\Users\grt-linyina\AppData\Roaming\Typora\typora-user-images\image-20240223173647083.png)

#### Object类型与object类型

- Object类型

  - 合法的Object类型 ：原始类型值、对象、数组、函数（undefined和null不行，其他的任何值都可以赋值给Object类型）
  - 简写

  ```
  let obj:{}
  ```

- object类型

  - js的狭义对象：可以用字面量表示的对象。只包含对象数组和函数 

    ![image-20240223175241796](C:\Users\grt-linyina\AppData\Roaming\Typora\typora-user-images\image-20240223175241796.png)					

- 大写小写object都只包含js内置对象原生的属性和方法 （自定义的不存在其中）

  ```typescript
  const o1:Object = {foo:0};
  const o2:object = {foo:0};
  o1.String()//正确
  o2.String()//正确
  o1.foo//报错
  o2.foo//报错 调用自定义属性报错
  
  ```

## undefined和null的特殊性

- 既是值也是类型
  - 任何其他类型的变量可以赋值为undefined和null
  - 打开strctNullChecks之后 undefined和null只能赋值给自身或者any和unknown类型

### 值类型

- 单个值也是一种类型，称为值类型

  ```tsx
  let x:'hello';
  x = 'hello';//正确
  x = 'world';//报错
  //变量x的类型是字符串hello，他只能赋值这个字符串，赋值其他的字符串会报错
  ```

- TS 遇到const命令声明的变量，如果代码里面没用注明类型，默认判断是值类型

  - ```typescript
    //x的类型 是https
    const x = 'https';
    //y的类型是 string
    const y:string = 'https';
    //如果const命令声明的变量并且赋值为对象就不会被推断为值类型
    //会推断属性foo的类型是number，因为const赋值为对象时，属性值是可以改变的。
    const x = {foo:1};//x的类型是 {foo:1}
    //左边判断的类型是值类型5；右边判断的类型是number类型 
    //父类型不能赋值给子类型所以报错
    const x:5 =4+1;//报错
    //反过来，子类型可以赋值给父类型
    let x:5 = 5;
    let y:number =4+1;
    x=y;//报错
    y=x;//正确
    ```

### 联合类型

- 指的是多个类型租车一个新类型，使用符号 |表示

- A|B 任何类型只有属于A或者B就属于联合类型A|B

  ```typescript
  let x:string|number
  x = 123; //正确
  x = 'abc'; //正确
  //联合类型可以和值类型相结合
  let setting:true|fase;
  //变量可能包含空值
  let name:string|null;
  name = 'John';
  name = null;
  //也可以这样写
  let x:
  	|'one'
      |'two'
      |'three'
      |'four';
  //传递的参数也可以是联合类型，多种类型：要对类型进行判断处理
  function printId(
    id:number|string
  ) {
    if (typeof id === 'string') {
      console.log(id.toUpperCase());
    } else {
      console.log(id);
    }
  }
  ```

### 交叉类型

- let X:A&B  任何一个类型必须也同时属于A和B才能满足

- 交叉类型主要用于对象的合成  【经常用于为对象类型添加新属性】

  ```typescript
  let obj:
  	{ foo: string } &
  	{ bar: string };
  obj = {
  	foo: 'hello',
  	bar: 'world'
  }	
  //这样obj同事具有foo和bar属性
  //为对象类型添加新属性
  type A = { foo：number };
  typr B = A & { bar: number };
  ```

### type命令

- 用来定义一个类型的别名【不允许重名】

  ```typescript
  //为number类型定义了一个别名 Age
  type Age = number;
  //可以使用像number一样使用age
  let age:Age = 55;
  ```

- 别名的作用域是块级作用域【代码块内部定义的别名影响不到外部】

  ```tsx
  type Color = 'red';
  if(Math.random() < 0.5){
  	type Color = 'blue';
  }
  //可以使用表达式
  type World = 'world';
  type Greeting = `hello ${World}`;
  ```

- type 命令属于类型相关代码，编译成js的时候会被全部删除

### typeof 运算符

- JS中：typeof是一个一元运算符，返回一个字符串；代表操作数的类型

  ```tsx
  typeof 'foo';// 'string'
  ```

  ![image-20240226152207663](C:\Users\grt-linyina\AppData\Roaming\Typora\typora-user-images\image-20240226152207663.png)

- TS中 他返回的是一个值（该值的TS类型）

  - ```ts
    const a = { x:0 };
    //表示返回变量a的ts类型
    type T0 = typeof a;//{x:number}
    //返回属性x的类型 number
    type T1 = typeof a.x;//number  
    ```

- ​    编译时不会进行js的值运算；所以， TS的typeof的参数只能是标识符，不能是需要运算的表达式

  ```typescript
  type T = typeof Date();//报错 不能是表达式
  //typeof 命令的参数不能是类型
  type Age = number;
  type MyAge = typeof Age;//报错
  //Age是个类型 所以会报错
  ```

### 块级类型声明

- 类型可以声明在代码块里面，只在当前代码块有效

### 类型的兼容

- TS中存在兼容 

- 如果类型A的值可以赋值给类型B；A就为B类型的子类型

- 凡是可以使用父类型的地方都可以使用子类型；反之不行

- ```
  type T = number|string
  let a:number = 1;
  let b:T=a;
  //number为number|string的子类型
  //子可以赋值给父
  //父不可以赋值给子
  ```

## TS的数组类型

- 根本特征：所有数组成员的类型必须相同，成员数量不限

- 写法

  - ```tsx
    //写法1
    let arr:number[] = [1,2,3];
    //类型复杂的情况下
    let arr:(number|string)[];
    //写法2
    let arr:Array<number> = [1,2,3];
    let arr:Array<number|string>;
    ```

- 数组的成员数量是可以动态变化的

- 越界访问数组不会报错

  - ```tsx
    let arr:number[]=[1,2,3];
    let foo = arr[3];//正确
    //使用方括号读取数组成员的类型
    type Names =string[];
    type Names = Names[0];//string
    //上面示例中，Names[number]表示数组Names所有数值索引的成员类型，所以返回string。
    type Names= Names[number];
    ```

### 数组的类型判断

- 数组没有声明类型，TS自行推断数组成员的类型【只发生在初始值是空数组的

  情况】

  - ```tsx
    const arr = [];//推断为any[]
    //如果后续赋值，会自动更新类型推断
    arr.push(123);//number[]
    arr.push('abc');//(string|number)[]
    
    ```

  - ```tsx
    const arr=[123];//number[]
    arr.push('abc');//报错 因为初始值为number
    ```

### 只读数组，const断言

- JS中const 声明的数组变量是可以改变成员的

  ```js
  const arr =[0,1];
  arr[0] = 2;
  ```

- TS中只读数组不改变成员【在数组类型前加上readonly】

  ```tsx
  const arr:readonly number[] = [0,1];
  arr[1] = 2;//报错
  arr.push(3)//报错
  delete arr[0];//报错
  ```

- number[] 是readonly  number[]的子类型

  ![image-20240226170345141](C:\Users\grt-linyina\AppData\Roaming\Typora\typora-user-images\image-20240226170345141.png)

- readonly不可以与数组的泛型写法一起用

  ```tsx
  const arr:readonly Array<number> = [0,1];//报错
  //有专门的生成只读数组类型的泛型写法
  const a1:ReadonlyArray<number> = [0,1];
  const a2:Readonly<number> = [0,1];
  //使用const断言声明只读数组
  const arr=[0,1] as const;
  arr[0]=[2];//报错
  ```

### 多维数组

- ```ts
  var multi:number[][]=
  [[1,2,3],[4,5,6]]
  //最底层的数组类型为number
  ```

## TS的元组类型

- 可以有不同类型的成员的数组

- 由于成员类型不同，所以必须声明每个成员的类型(不可以省略否则自动推断为数组)

  ```TS
  const s:[string,string,boolean]=['a','b',true];
  //数组
  let a:number[]=[1];
  //元组
  let t:[number]=[1];
  //在类型加上？表示成员是可选的
  let a:[number,number?]=[1];
  //可选成员必须在必须成员的后面
  type myTuple= [
      number,
      number,
      number?,
      string?,
  ]
  ```

- 越界会报错，但是使用扩展运算符（...）可以表述不限制元组的成员数量

  ```TS
  let x:[string.string] = ['a','b'];
  	x[2]='c';//报错
  type NamedNums =[
      string,
      ...number[]
  ];
  const a:NamedNums = ['A',1,2];
  const b:NamedNums = ['B',1,2,3];
  //...数组、...元组
  type t1 = [string, number, ...boolean[]];
  type t2 = [string,...boolean,number]
  //元祖可以添加成员的说明
  type Color = [
      red:number,
      green:number,
      blue:number
  ]
  const c:Color =[225,225,225]
  //读取成员类型
  type Tuple = [string , number];
  type Age = Tuple[1];//number
  //获取所有数值索引的成员类型
  type TupleEl = Tuple[number];//string|number
  
  ```

- 只读元组

  ```
  //写法1
  type t = readonly [number,string]
  type t = Readonly<[number,string]>
  //只读元组是元组的父类型 元组可以替代只读元组
  
  ```

- 成员数量的推断

  ```tsx
  type point:[number,number]
  if(point.length === 3)//报错 因为长度为2，判断无意义
  {
  
  }
  //如果使用了 扩展运算符，TS就无法推断成员的数量
  const myTuple:[...string[]] = ['a','b','c'];
  if(myTuple.length===4){//正确
      
  }
  //  上面的myTuple只有三个成员，但是ts推断不出他的数量，因为他用到了扩展运算符。TS把他看作是数组，数组的成员数是不确定的
  ```

- 扩展运算符与成员数量【扩展运算符传入函数参数，会出现参数数量与数组长度不匹配的报错】

  ```tsx
  //解决办法：把不确定的数组先写成确定的成员数量元组，再使用扩展运算符
  const arr:[number,number] =[1,2];
  function add(x:number,y:number){
      
  }
  add(...arr)//正确
  
  //使用as const断言
  const arr = [1,2] as const
  ```

### TS的symbol类型

- Symbol 是 ES2015 新引入的一种原始类型的值。它类似于字符串，但是每一个 Symbol 值都是独一无二的，与其他任何值都不相

- unique symbol 这是一个symbol的子类型，表示单个的、某个具体的Symbol值（symbol包含所有symbol值，无法表示某一个具体的值）

- 这个类型的变量是不可修改的只能用const声明不能用let

  ```ts
  const x:unique symbol =Symbol();
  //const 命令为变量赋值Symbol值时，变量类型默认是unique symbol
  const x= Symbol();
  //每个声明为unique symbol 类型的变量，他都值不同 始于两个值类型
  const a:unique symbol = Symbol();
  const b:unque symbol =Symbol();
  a===b;//报错
  
  //如果要写成与变量同一个unique值类型 只能写成类型为typeof a
  const b:typeof a=a;//正确
  //Symbol.for()会返回相同的Symbol值 ,可能会出现多个unique symbol 类型的变量等同于同一个Symbol
  //值的情况
  const a:unqiue symbol =Symbol.for('foo')
  const b:unqiue symbol =Symbol.for('foo')
  //以上变量a和b是两个不同的值类型，但是值相等
  ```

- unique symbol 用作属性名 ；可以保证不会跟其他属性产生属性名冲突

  ```ts
  const x:unique symbol =Symbol();
  const y:symbol = Symbol();
  //如果要把一个特定的Symbol值当作属性名 那么他的类型只能是unique symbol
  interface Foo {
      [x]:string;//正确
      [y]:string;//报错
  }
  //也可以作类的属性值 但是只能赋值给类的readonly static属性
  class C {
      static readonly foo:unique symbol = Symbol();
  }
  ```

- 类型推断【如果声明变量是没有给出类型，TS会推断某个Symbol值变量的类型】

  ```ts
  let x = Symbol();//推断变量为symbol
  const y =Symbol();//推断为 unique symbol
  //let 命令声明的变量，如果赋值为另外一个unique symbol类型的变量 推断的类型还是symbol  
  const a=x;//类型为symbol
  ```

  #### 函数
  
- 类型声明

  ```ts
  //需要在声明函数时 给出参数的类型和返回值的类型
  //返回值的类型通常可以不写，因为 TypeScript 自己会推断出来。
  function hello( txt:string):void{//返回值的类型
  	console.log('hello'+txt)
  }
  
  //如果变量被赋值为一个函数 有两种写法
  //通过等号推断类型
  const hello = function(txt:string){
      console.log('hello'+txt);
  }
  //写法二 使用箭头形式 
  //要点：函数的参数必须要放在圆括号里面 
  
  const hello:
   (txt:string)=>void
  = function(txt){
    console.log('hello'+txt);  
  }
  
  //用type 为函数类型定义一个别名，便于指定给其他变量
  type MyFunc = (txt:string) => void
  const hello:MyFunc = function (txt){
        console.log('hello'+txt); 
  }
  //函数的实际参数个数可以少于类型指定的参数个数，但是不能多于
  
  let myFunc:
  (a:number,b:number)=>number;
  myFunc = (a:number)=>a
  //报错，参数多了
  myFunc =(a:number,b:number,c:number)=>a+b+c
  
  //不同传参数量多，的不能赋值给少的
  let x = (a:number)=>0;
  let y=(b:number,s:string)=>0;
  y=x;//错误
  x=y;//正确
  
  //一个变量套用另一个函数的类型 使用typeof运算符
  function add(
  	x:number,
       y:number,
  ){
          return x+y;
  }
  const myAdd:type add =function(x,y){
      return x+y;
  }
  //函数名add本身不是类型而是一个值，所以要用typeof运算符返回她的类型
  
  //函数类型采用对象的写法
  let add:{
      //(参数列表)：返回值
    (x:number,y:number):number  
  };
  add = function(x,y){
      return x+y;
  }
  
  //函数类型可以使用Interface 来声明 
  interface myfn{
      (a:number,b:number):number;
  }
  var add:myfn = (a,b)=>a+b;
  ```

  

##### Function类型

- 任何函数都属于Function类型

```ts
function doSomething(f:Function){
	return f(1,2,3);
}
//1.Function 类型的函数可以接受任意数量的参数，每个参数的类型都是any （不建议使用：最好用有声明的）
```



##### 箭头函数

- 是一种普通函数的简化写法

  ```ts
  const repeat =(str:srting,times:number):string => str.reapeat(times);
  const xx =(变量：类型)：返回值类型=>返回值
  
  //如果函数的类型是在箭头函数里面定义
  function greet(
  fn:(a:string)=>void  //返回值类型要写在箭头右边
  ):void{
      fn('world');
  }
  type Person ={name:string};
  //people是map的返回值
  const people=['a','b','c'].map(
      //参数是一个箭头函数 name的参数类型被省略了  参数的返回类型是Person
  	(name):Person=>({name})//表示返回一个对象 属性是name 圆括号是必须的
      //函数不会返回任何值
  )
  //people 的类型是Person[]
  ```

##### 可选参数

- 如果函数的某个参数可以省略，则在参数后面加问号表示

- 参数名带有问号，表示该参数类型实际时  原始类型|undefined  

- 函数的可选参数只能在参数列表的尾部跟在必选参数的后面

  ```ts
  function f(x?:number){
      
  }
  f();
  f(10)
  f(undifined)//ok 可以赋值为undefined
  
  let myFunc:
  (b:numbewr,a?number)=>number;//报错
  
  //如果前部参数有可能为空，只能显示注明参数的可能
  let myFunc:(
  	a:number|undefined,
      b:number
  )=>number;
  //用到可选参数时需要判断是否时undefined
  myFunc = function(x,y){
      if(y===undefined){
          return x;
      }
      return x+y;
  }
  ```

- 如果显示设立为undefined的参数 就不能省略

  ```ts
  function f(x:number|undefined){
  	return x;
  }
  f();//错
  f(10);//ok 
  f(undefined)//ok
  ```

##### 参数默认值

- 设置了默认值代表传参可选，不传就等于默认值
- 可选参数和默认值不能同时使用

```ts
function createPoint(
	y:number=0,
	x:number=0
)[number,number]{
	return [x,y]
}
createPoint();//[0,0]
//报错 可选参数和默认值不能同时使用
function f(x?:number =0){
    
}
//默认值不位于参数列表的末尾就，调用时不能省略 想要触发默认值必须显示传入undefined
function add(
	x:number=0,
 	y:number
){
    return x+y;
}
add(1)//err
add(undefined,1)//ok
```



##### 参数解构

```ts
function f(
	[x:y]:[number，number]
){
        
 }
function sum(
{a,b,c}:{
    a:number;
    b:number;
    c:number
}
)

//简介写法 使用type
type ABC = {a:number;b:number;c:number};
function sum({a,b,c}:ABC){
   
}
```

##### rest参数  表示函数剩余的所有参数

```ts
//rest参数为数组
function joinNumber(...nums:number[]){}
//rest 参数为元组  元组需要声明每一个剩余参数的类型
function f(...args:[boolean,numbers,srting?]){}

//与变量解构结合使用
function repeat(
	...[str,times]:[string,number]
):string{
        return str.repeat(times);
    }
//等同于
function repeat（
	str:string,
    times:number
):string{
    return str.repeate(times)
}
```

#### readonly 只读参数

```
function arraySum(
arr:readonly number[]
){
	arr[0] =0;//报错
}
```

#### void 类型

```tsx
//表示没有返回值  类型要写成void 如果返回了其他值 就会报错
function f():void{
	console.log('hello')
}
//void 类型允许返回undefined或null
function f():void{
	return undefined;//ok
	return null;//ok
	
	//如果打开了strictNullChecks
		return undefined;//ok
		return null;//err
}

//如果一个变量，对象方法，参数类型是一个void 它可以被赋值为有返回值的函数
type =()=>void;
const f:voidFunc=()=>{
    return 123;  //因为该函数的返回值没有利用价值，只要不用到返回值就不会报错 
}
f()*2 //引用了就会报错

//除了以上三种，其他的，函数字面量声明了返回值是void 那么还是不能有返回值
```

##### never 类型

```
//表示肯定不会出现的值  用在函数表示这个函数不会有返回值
//抛出错误的函数，不会正常退出 所以返回的值是never
function fail(msg:string):never {
	throw neww Error(msg);
}
//无限执行的函数
const sing = function():never{
    while(true){
		console.log('sing')//sing会永远被执行不会返回，所以返回值类型是never
    }
}
```

- never与void的不同 never表示没有执行结束，不可能有返回值，void表示不返回值

##### 局部类型

- 函数内部允许声明其他类型 该类型只在函数内部有效

```
function hello(txt:string){
	type message =string;//message在hello内部定义 只能在函数内部使用
	let newTxt:message ='hello'+txt;
	return newTxt;
}
外部使用就会报错
```

##### 高阶函数

- 一个函数的返回值还是一个函数 ，前一个函数就叫高阶函数

  ```
  (someVlaue:number)=>(mult:number)=>someValue*mult
  ```

##### 函数重载

- 根据参数类型不同，执行不同逻辑行为

- 函数重载的声明方法

  ```
  function reverse(srt:string):string;
  function reverse(arr:any[]):any[];
  //后面海必须对函数给予完整的类型声明
  function reverse(string|any[]){
  	if(typeof stringOrArray ='string')
  		return stringOrArray.split('').reverse().join('');
  	else
      	return stringOrArray.split('').reverse()
  }
  ```

- 重载函数的每个类型声明和函数实现之间不能有冲突

- 最宽的声明应该放在最后，以防覆盖其他声明

- 对象的方法也可以进行重载

  ```
  class SringBuilder{
  	#data =''
  	add(num:number):this;
  	add(bool:boolean):this;
  	add(str:string):this;
  	add(value:any):this{
  		this.#data +=String(value);
  		return this;
  	}
  	toString(){
  		return  this.#data
  	}
  }
  ```

- 优先使用联合类型代替函数重载

  ```
  //写法1 
  function len(s:string):number;
  function len(arr:any[]):number;
  function len(x:any):nmuber{
  	return x.length
  }
  
  //写法二
  function len(x:any[]|string):number{
  	return x.length;
  }
  ```

  ##### 构造函数

```ts
//必须使用new命令调用
const d= new Date(); //返回date对象的实例

class Animal{
	numLegs:number =4；
}
type AnimalConstructor =new()=>Animal;//是一个构造函数

//另一种写法 对象形式
type F ={
	new (s:string):Object;
}
//既是普通函数又是构造函数
type F={
    new（s:string）：Object;
    （n?number）:number;
}
```

##### Ts的对象类型

```ts
//声明方法
const obj:{
	x:number; 属性：方法的类型
	y:number;
}={x:1,y:1};
//可以用分号或者逗号结尾
type Myobj={
    y:number;
    
    x:number,
}
//一旦声明了类型，对象赋值时就不能有缺少或者多余的属性
const o1:MyObj={x:1};//err
const o2:MyObj={x:1,y:1,z:1};//报错
//读写不存在的属性也会报错
console.info(obj.z)//err
obj.z=1;//err
//不能删除类型声明中存在的属性，但是可以修改属性值
const myUser={
    name:"Sabrina",
}
delete myUser.name//报错
myUser.name="Cynthia"//正确

//对象的方法使用函数类型描述
const obj:{
    x:number;
    y:number;
    add(x:number,y:number):number;
    //或者
    add:(x:number,y:number)=>number;
} ={
    x:1,y:1,add(x，y){
        return x+y;
    }
}
//读取对象类型
type User ={
    name:string,
    age:number
};
type Name=User['name'];//string

//使用interface 把对象类型提炼为一个接口
interface MyObj{
    x:number;y:number;
}
const obj:MyObj={x:1,y:1}

//TS不区分对象自身的属性和继承的属性，都视为对象的属性
interface MyInterface{
    toString();string;//继承的属性
    prop:number;//自身的属性
}
const obj:MyInterface ={ //ok
    prop:123
}
```

##### 可选属性

如果某个属性时可选的，需要在属性名后面加个问号

```ts
const obj:{
	x:number;
	y?:number;
}={x:1}
//可选属性等同于允许赋值为undefined 
type User ={
    firstName：string;
    lastName?:string;
}
//等同
type User ={
    firstName：string;
    lastName?:string|undefined;
}
//读取一个没有赋值的可选属性时，返回undefined

type MyObj={
    x:string,y?:string
}
const obj:MyObj= {x:'hello'}
obj.y.toLowerCase()//err 因为返回的时undefined 无法对其调用toLowerCase（）
//读取可选属性之前，必须检查是否为undefined
if(obj.y!==undefined){
    obj.y.toLowerCase()
}
//写法一
let firstName = (user.firstName === undefined)?"foo":user.firstName
//写法二  使用Null运算符？？
let firstName = user.firstName ?? 'Foo'
```

##### 只读属性

```ts
//在属性名前面加上readonly 关键字，表示这个属性时只读属性，不能修改
interface MyInterface{
    readonly prop：number；
}
const person:{
    readonly age:number
}={age:20}
person.age=21//报错 因为不能修改只读属性
//如果只读属性值是一个对象，reaonly 修饰符表示不禁止修改该对象的属性，但是不可以完全替换该对象
interface Home{
    readonly resident:{
        name:string;
        age:number
    }
}
const h:Home ={
    resident:{
        name:'Viky',age:42
    }
}
h.resident.age=32//ok
h.resident ={ name:'Kate',age:23}//报错


interface Person{
    name：string;
    age:number;
}
interface ReadonlyPerson{
    readonly name:string;
    readonly age:number;
}
let w:Person ={
    name:'Vicxky',
    age:42
}
let r:ReadonlyPerson=w
w.age  +=1；
r.age //43
//w和r指向同一个对象，w可写，r时只读，对w属性的修改会影响到r
//希望属性值是只读的还可以 对象后面加上只读断言 as const
const myUser ={
    name:"Sabrina",
}as const;  
myUser.name="asdasd"//err
//明确声明变量的类型，类型会以声明的为准
const myUser:{name:string}={
    name:"Sabrina"
}as const;
myUser.name="asdsad"//ok
//由于myUser的类型声明，name不是只读属性，但是又使用了只读断言as const 这时候会以声明的类型为准
```

##### 属性名的索引类型

- Ts允许采用属性名的表达式的写法来描述类型

  ```ts
  type MyObj={
  	[property:string]:string
  }
  //不管对象有多少属性，只要属性名为字符串，属性值也是字符串，就符合这个类型声明
  const obj:MyObj ={
  	foo:'a',
      bar:'b',
      baz:'c'
  }
  ```

- 对象的属性名的类型又string ，number，symbol三种类型

- ```ts
  type MyArr ={
  	[n:number]:number;
  }
  const arr:MyArr =[1,2,3]
  //或者
  const arr:MyArr ={
      0:1,
      1:2,
      2:3
  }
  ```

- 对象可以同时又多种类型的属性名索引

```ts
type MyType ={
	[x:number]:boolean;//报错  属性名的值类型需要跟字符属性名一样才不会报错
	[x:string]:string; 
}
//数组索引不能于字符串索引发生冲突 必须服从后者，因为所有数值属性名会i东转为字符串属性名

//可以既声明属性名索引，也可以声明具体的单个属性名 
type MyType ={
    foo：boolean；//报错   符合属性名字符串索引，但是属性值类型不一样
    [x:string]:string;
}
```

- 属性名的数值索引不能用来声明数组，因为使用这种方式就不可以使用length

  ```ts
  type MyArr ={
  	[n:number]:number;
  }
  const arr:MyArr= [1,2,3]
  arr.length//报错
  ```

##### 解构赋值

- 用于直接从对象中提取属性

```ts
const {id,name,price} = product;
//从对象product 提取了三个属性
//相当于
const {id,name,price}:{
    id:string;
    name:string:
    price:number
} = product;

let {x:foo,y:bar}=obj;
//==  冒号表示为两个属性指定新的变量名
let foo = obj.x;
let bar =obj.y;
//如果要为x和y指定类型 
let {x:foo,y:bar}:{x:srting;y:number} = obj;

```

##### 结构类型原则

- 只要对象B满足对象A的结构特征，Ts认为对象B兼容对象A的类型 

```ts
type A={
	x:number;
}
type B={
	x:number;y:number;
}
//只要可以用A的地方，就可以使用B
const B={
    x:1,
    y:1
}
const A:{x:number}=B;//正确  B可以赋值给A
```

- B类型赋值给A TS认为B是A的子类型，子类型满足父类型的所有解构特征，还拥有自己的特征（子类型兼容父类型）

##### 严格字面量检查

- 如果对象使用字面量表示，会触发严格模式

  ```ts
  const point:{
  	x:number;
  	y:number;
  }={
    x:1,
    y:1,
    z:1,//报错
  }
  //因为等号右边是一个对象的字面量 会触发严格模式，z在类型声明中不存在，导致报错
  
  //如果等号右边是一个变量，根据结构类型原则就不会报错
  const myPoint ={
      x:1,y:1,z:1
  }
  const point:{
      x:number,
      y:number;
  }= myPoint;//正确
  ```

- 规避严格字面量的检查

  ```ts
  //1.使用中间变量
  let myOptions ={
      title:'我的网页'，
      darkmode：true，
  }
  const obj:Options =myOptions;
  
  //2.如果确认字面量没有错误，使用类型断言规避检查
  const obj:Options={
      title:'我的网页'，
      darkmode：true，
  } as Options;  //告诉编译器，字面量符合Options类型 
  ```

##### 最小可选属性规则

- 如果一个对象的所有属性都是可选的，那么其他对象跟他都是类似的

  ```
  type Options={
  	a?:number;
  	b?:number;
  	c?:number;
  }
  const opts ={d:123};
  const obj:Options = opts;//err
  //opts与类型Options没有共同属性 d所以，就报错
  如果某个类型的所有属性是可选的，那么该类型的对象opts至少要存在一个可选属性。（最小可选属性规则）
  ```

- ```ts
  规避该规则：[propName:string]:someType  或者使用断言 opts as Options
  ```

##### 空对象

- 是Ts的一种特殊值 

  ```
  cost obj={};
  obj.prop =123//报错   因为空对象没有自定义属性，所以会报错
  TS推断obj的类型为空对象执行的是下面的代码
  const obj:{}={};
  //只能使用继承的属性 继承至Object.prototype的属性
  obj.toString()//正确
  ```

- TS不能动态添加属性，必须要一次性声明所有属性

  ```
  const pt={};
  pt.x=3;
  pt.y=4;//err
  const pt ={
  	x:3,
  	y:4
  }
  ```

  - 分布声明的写法-》使用扩展运算符（...）合成一个新对象

    ```
    const pt0 ={};
    const pt1 ={x:3};
    const pt2 ={y:4};
    const pt ={
    	...pt0,...pt1,...pt2
    }
    ```

- 空对象作为类型 其实是Object类型的简写形式

  ```ts
  let d:{};//等同于 let d:Object;
  //除了null和undefined 都可以给空对象类型赋值
  d={};
  d={x:1};
  d='hello';
  d=2;
  //不会有严格字面量检查 赋值的时候允许多余的属性 
  interface Empty {}
  const b:Empty= {myProp:1,anotherProp:2}//正确
  b.myProp//err  但是读取的时候不能读取这些属性
  ```

#### TS的interface接口

- 简介，相当于是一种类型的约定，使用了某个接口，就拥有了指定的类型结构

  ```ts
  interface Person{
  	firstName:string;
  	lastName:string;
  	age:Number;
  }
  //任何实现这个接口的对象，必须部署这三个属性，并且必须符合规定的类型
  //实现接口
  const p:Person ={
      firstName:'John',
      lastName:'Smith',
      age:25
  }
  //方括号运算符可以取出interface某个属性的类型
  interface Foo{
      a:string;
  }
  type A = Foo['a']; //A的类型就是string
  ```

- interface 可以表示对象的各种语法 它的成员有五种形式 

  - 对象属性、对象的属性索引、对象方法、函数、构造函数

  - 对象属性

    ```ts
    interface Point{
    	x:number;
    	y:number;//x和y都是对象的属性，分别使用冒号指定每个属性的类型
        z?:string;//如果属性是可选的，就在属性名后面加一个问号
        readonly a:string;//如果属性是只读的，需要加上readonly修饰符
    }
    ```

  - 对象的属性索引

    ```ts
    interface A{
        [prop:string]:number;//属性的字符串索引，表示属性名只要是字符串，都符合类型要求
        //属性索引有string、number、symbol三种类型
        //一个接口中最多只能定义一个字符串的索引。字符串索引回约束该类型所以名字为字符串的属性
        a:boolean;//编译错误
    }
    //属性的数值索引，其实是指定数组的类型
    interface A{
        [prop:number]:string;
    }
    //[prop:number]表示属性名的类型是数值，所以可以用数组对变量obj赋值
    const obj:A =['a','b','c'];
    
    //如果一个interface 同时定义了字符串索引和数值索引，那么数值索引必须服从于字符串索引
    interface A{
        [prop:string]:number;
        [prop:number]:string;//报错 数值的索引与字符串索引不一致就会报错
        //数值索引必须兼容字符串索引的类型声明
    }
    interface B{
        [prop:string]:number;
        [prop:number]:number;//正确
    }
    ```

  - 对象的方法 （有三种写法）

    ```tsx
    //写法一
    interface A{
        f(x:boolean):string;
    }
    //写法二
    interface B{
        f:(x：boolean)=>string;
    }
    //写法三
    interface C{
        f:{(x:boolean):string};
    }
    //属性名可以采用表达式 以下写法也是可以的
    const f='f';
    interface A{
        [f](x:boolean):string;
    }
    //类型方法可以重载
    interface A{
        f():number;
        f(x:boolean):boolean;
        f(x:string,y:string):string;
    }
    //需要额外定义一个函数MyFunc()实现重载，然后部署A的对象的a属性f等于MyFunc()就可以了
    function MyFunc():number;
    function MyFunc(x:boolean):boolean;
    function MyFunc(x:string,y:string):string;
    function MyFunc(x?:boolean|string,y?:string):number|boolean|string{
        if(x === undefined && y===undefined ) return 1;
        if(typeof x === 'boolean' && y==undefined) return 2;
        if(typeof x === 'string' && typeof y==='string') return 3;
        throw new Error('wrong parameters');
    }
    const a:A ={
        f:MyFunc
    }
    ```

  - 函数 interface 也可以用来声明独立的函数

    ```ts
    interface Add{
    	(x:number,y:number):number;
    }
    const myAdd:Add = (x,y) => x + y;
    ```

  - 构造函数 （interface 内部可以使用new关键字，表示构造函数）

    ```ts
    interface ErroConstructor {
    	new (message?:string):Error
    }
    ```

#### interface的继承

- interface继承interface

  ```ts
  interface Shape{
  	name:string;
  }
  interface Circle extends Shape{
  	radius:number;
  }
  //Circle 继承了Shape 所以Circle有两个属性，name和radius
  
  //interface 允许多重继承
  interface Style {
      color：string;
  }
  interface Shape{
      name：string;
  }
  interface Shape extends Style，Shape{
      radius:number;
  }
  //如果子接口和父接口存在同名属性 子接口的属性会覆盖父接口的属性
  //!子接口与父接口的同名属性必须是类型兼容的，不能有冲突，否会报错
  interface Foo{
      id：string;
  }
  interface Bar extends Foo{
      id:number;//报错
  }
  //多重继承是，如果多个父接口存在同名属性，那么这些同名属性不能有类型冲突，要是兼容的
  interface Foo{
      id：string;
  }
  interface Bar{
      id:number;
  }
  //报错
  interface Baz extends Foo，Bar{
      type：string；
  }
  ```

- interface 继承 type

  ```ts
  //interface 可以继承type命令定义的对象类型
  type Country ={
      name:string;
      capital:string;
  }
  interface CountryWithPop extends Country{
      population:number;  //此处继承了type定义的对象并且新增了一个population属性
  }
  //!如果type命令定义的类型不是对象，interface就无法继承
  ```

- interface 继承class

  ```ts
  class A{
  	x:string='';
  	y():boolean{
  		return true;
  	}
  }
  interface B extends A{
      z:number//B继承了A就要x和y（），z属性
  }
  //实现B接口的对象 需要具备这三个属性
  const b:B ={
      x:'',
      y:function(){return true},
      z:123
  }
  //某些类拥有私有成员和保护成员，interface可以继承 但是意义不大
  class A{
      private x:string = '';
      protected y:string ='';
  }
  interface B extends A{
  	z:number
  }
  //B继承了A，但是无法用于对象，因为对象不能实现这些成员 
  ```

- 接口合并 （多个同名接口回合并成一个接口）

  ```ts
  interface Box{
  	height：number;
  	width:number;
  }
  interface Box{
  	length:number
  }
  //上面的两个接口会合并成一个接口并且同时拥有三个属性
  //这是为了兼容JS的行为，因为开发者经常对全局对象或者外部库添加自己的属性和方法只要使用interface就可以让这些自定义的方法自动更原始的interface合并
  
  interface Document{
      foo:string;
  }
  document.foo='hello';//接口Document增加了一个自定义属性，就可以在document对象上使用自定义属性；如果原始定义没有这些属性，直接添加会报错
  
  //同名接口合并时，同一个属性如果有多个类型声明，就不可以有类型冲突
  interface A{
      a:number;
  }
  interface A{
      a:string;//报错
  }
  
  //接口合并时，如果有同名的方法有不同的类型声明，就会出现函数重载；后面定义的比前面的优先级更高
  interface Cloner{
      clone(animal:Animal):Animal;
  }
  interface Cloner{
      clone(animal:Sheep):Sheep;
  }
  interface Cloner{
      clone(animal:Dog):Dog;
      clone(animal:Cat):Cat;
  }
  //等同于
  interface Cloner{
      clone(animal:Dog):Dog;//先执行最后的定义的
      clone(animal:Cat):Cat;
       clone(animal:Sheep):Sheep;
    	 clone(animal:Sheep):Sheep;
  }
  //例外 ：同名方法中有一个参数时字面量类型，字面量类型有更高优先级
  interface A {
      f(x:'foo'):boolean;
  }
  interface A{
       f(x:any): void;
  }
  //等同于
  interface A{
      f(x:'foo'):boolean; //这个是有‘’的字面量类型，重载时会放在最前面
      f(x:any): void;
  }
  //如果两个interface的联合类型存在同名属性，那么该属性的类型也是联合类型
  interface Circle{
      area：bigint
  }
  interface Rectangle {
    area: number;
  }
  declare const s:Circle|Rectangle
  s.area;//bigint|number
  ```

- interface 与type的异同

  - 都能为对象类型起名 

    ```ts
    type Country ={
    	name:string;
    	capital:srting;
    }
    interface Country {
        name:srting;
        capital:string;
    }
    ```

  - interface与type的区别

    1. type能够表示非对象类型，interface只能表示对象类型（包括数组，函数）

    2. interface可以继承其他类型，type不支持继承

       继承的主要作用是添加属性 

       ```ts
       //type添加属性只能通过&运算符重新定义一个类型
       type Animal ={
           name:string
       }
       type Bear=Animal&{
           honey:boolean
       }
       //&运算符表示同时具备两个类型的特征 
       
       //interface添加属性采用继承的写法
       interface Animal{
            name:string
       }
       interface Bear extends Animal{
            honey:boolean
       }
       
       //interfce可以继承type
       type Foo ={x:number;}
       interface Bar extends Foo{
           y:number;
       }
       ```

    3. 同名的interface 会自动合并，但是type则会报错 (interface是开放的可以添加属性，type是封闭的，不能添加属性只能重新定义type)

       ```ts
       type A ={foo:number};//报错
       type A ={bar：number}//报错 同名了
       
       interface A{foo:number};
       interface A{bar:number};
       const obj:A={
           foo:1,
           bar:1,
       }
       ```

    4. interface不能包含属性映射（mapping）type可以

       ```ts
       interface Point{
       	x:number;y:number;
       }
       //正确
       type PointCopy1 ={
           [key in keyof Point]:Point[key]
       }
       //报错
       type PointCopy2{
           [key in keyof Point]:Point[key]
       }
       ```

    5. this关键字只能用在interface

       ```ts
       //ok
       interface Foo{
       	add(num:number):this;
       }
       //err
       type Foo={
           add(num:number):this;//type不能用this
       }
       ```

    6. type 可以扩展原始数据类型 type不可以

       ```
       //ok
       type MyStr = string&{
       	type:'new'
       }
       //err
       interface MyStr extends string{
       	type:'new'
       }//string是原始数据类型
       ```

    7. interface 不能表达某些复杂类型（交叉类型和联合类型），type可以

       ```ts
       type A = { /* ... */ };
       type B = { /* ... */ };
       
       type AorB = A|B;
       type AorBwithName =AorB&{
           name:string
       }
       ```

  #### TS的class类型

- 封装属性和方法

- 属性的类型【可以在顶层声明，也可以在构造方法内部声明】

  ```ts
  //顶层声明 在声明是同时给出类型
  class Point{
      x:number;
      y:number;
  }
  //不给出类型，ts会任务x和y都是any
  class Point{
      x;
      y;
  }
  //声明给出初始值可以不写类型 ，会自行推断出类型
  class Point{
      x = 0;
      y = 0;//会被推断为number
  }
  //ts配置项strictPropertyInitialization (默认打开)，打开了就会检查是否有初始值，没有就报错
  class Point{
       x:number;//err
      y:number;
  }
  //类的顶层属性不赋值，就会出现报错，不希望报错可以使用非空断言
  class Point{
      x!:number;//表示这两个属性一点不会为空
      y!:number;
  }
  
  ```

- readonly修饰符 表示该属性是只读的，实例对象不能修改这个属性

  ```ts
  class A{
  	readonly id='foo';
  }
  const a = new A();
  a.id = 'bar'; //报错 因为id是只读属性
  //readonly的初始值可以写在顶层属性，也可以写在构造方法里面
  class A{
      readonly id：string；
      constructor(){
          this.id = 'bar';//正确
      }
  }
  //如果两个地方都设置了只读属性的值，以构造方法的为准
  class A{
      readonly id：string='foo'；
      constructor(){
          this.id = 'bar';//正确
      }
  }
  ```

- 方法的类型 （就是普通函数，声明方式与函数一致）

  ```ts
  class Point{
  	y:number;x:number;
  	constructor(x:number,y:number){
  		this.x = x;
  		this.y = y;
  	}
      constructor(x=0,y=0){
          this.x=x;
          this.y=y;
      }
      //新建实例时，不提供属性x和y的值，都等于默认值0
      add(point:Point){
          return new Point(
              this.x + point.x,//省略了返回值
              this.y + point.y
          );
      }
  }
  //构造方法可以接受一个参数也可以接受两个参数，采用函数重载进行类型声明
  class Point{
      constructor(x:number,y:string);
      constructor(s:string);
      constructor(xs:number|string,y?:srting){
          //...
      }
  }
  //构造方法不能声明返回值类型，否则会报错，因为他返回的是实例对象
  class B{
      cosntructor():object{//报错
          
      }
  }
  ```

- 存取器方法   

  - 存取器（accessor）是特殊的类方法，包括取值器（getter）和存取值（setter）两种方法

  ```ts
  class C{
  	_name ='';
  	get name(){//取值器     get是关键词，name是属性名 外部读取时实例对象会调用
  		return this._name;
  	}
  	set name(value){  //外部写入name属性时，实例对象会自动调用这个方法
  		this._name = value;
  	}
  }
  ```

  - ts对存取器的规则

    1. 如果某个属性只有get没有set 那么该属性自动为只读属性

       ```ts
       class C{
       	_name ='foo';
       	get name(){
       		return this._name;
       	}
       }
       const c =new C();
       c.name = 'bar'//报错
       ```

    2. ts5.1版本之前，set 方法的参数类型，必须兼容get方法的返回值,否则报错

       ```ts
       class C{
           _name="";
           get name():string{//err  
               return this._name;
           }
           set name(value:number){
               this._name =String(value);
           }
       }
       //上面get方法与set方法的参数类型不兼容 导致报错 应该改成下面这样
       class C{
           _name = '';
           get name():string{
               return this._name;
           }
            set name(value:number|string){
               this._name =String(value);
           }
       }
       ```

    3. get 和set的可访问性必须一致，要么都公开，要么都私有

- 属性索引 类允许定义属性索引

  ```ts
  class MyClass {//[s:string]表示所有属性名类型为字符串的属性
  	[s:string]:boolean | 
  		((s:string)=>boolean);//他们的属性值要么是布尔值，要么是返回布尔值的函数
  	get(s:string){
  		return this[s] as boolean;
  	}	
  }
  //类的方法也是一种特殊属性（属性值为函数的属性），所以属性所以的类型定义也覆盖了方法
  class MyClass{
      [s:string]:boolean;//因为属性索引的类型类目不包含方法所以f()定义报错
      f(){//err
          return true;
      }
  }
  //正确写法
  class MyClass {
      [s:string]:boolean | (()=>boolean);//要定义函数的类型
      f(){
          return true;
      }
  } 
  //但是属性的读取器get set视为同属性
  class MyClass {
      [s:string]:boolean;
      get insInstance(){ 
          return true;
      }
  }
  ```

  ##### 类的interface接口

- interface 接口或者type,可以用对象的形式，为class 指定一组检查条件

  ```ts
  interface Country{
  	name:string;
  	capital:string;
  }
  //or
  type Country ={
      name:string;
      capital:string;
  }
  class MyCountry implements Country{
      name='';
      capital='';
  }//使用implement关键字表示该类的实例对象满足这个外部类型
  
  //interface 只是指定检查条件 不满足就会报错，不能代替class自身的类型声明
  //下面例子中，B实现了接口A 但是不能代替B的类型声明，因为B的get参数是any 不是string B依旧需要声明s的类型
  interface A{
      get(name:string):boolean;
  }
  class B implements A{
      get(s){ //s的类型是any
          return true;
      }
  }
  
  interface A{
      x:number;
      y?:number;
  }
  //因为A有可选属性B没有声明，所以不会报错
  class B implements A{
      x=0;
  }
  const b= new B();
  b.y = 10//err 但是要给b实例对象的属性y赋值，就会报错
  //所以B类型还是需要声明可选属性y
  class B implements A{
      x=0;
      y?:number;
  }
  //类还可以定义接口没有声明的方法和属性
  interface Point{
      x:number;
      y:number;
  }
  class MyPoint inplements Points{
      x=1;
      y=1;
      z:number=1;//不仅实现了Point接口，还内部定义了额外的属性z
  }
  
  //implements 后面可以是接口也可以是类 这是后面的类会被当做接口
  class Car {
      id:number =1;
      move():void{};
  }
  class MyCar implements Car{
      id=2;//不可省略
      move:void{};//不可省略
  }
  //interfce 描述的是类的对外接口，只能有公开属性和公开方法
  ```

- 实现多个接口

  ```ts
  //每个接口之间使用逗号分隔
  class Car implements MotorVehicle,Flyable,Swimmable{
  	//需要满足这三个接口声明的所有属性和方法
  }
  //优化的写法
  //1.类的继承
  class Car implements MotorVehicle{
      
  }
  class SecretCar extends Car implements Flyable,Swimmable{}
  //2.接口的继承
  interface A{
      a:number;
  }
  interface B extends A{
      b:number;
  }//只要实现接口B就相当于实现了A和B两个接口
  //不同接口不能有相互冲突的属性
  ```

- 类与接口的合并

  - TS不允许同名的类，如果一个类和一个接口同名，接口会被合并到类

    ```ts
    class A{
    	x:number =1;
    }
    interface A{
        y:number;
    }
    let a =new A();
    a.y =10;
    a.x//1
    a.y//10
    //合并进类的非空属性 y 如果在赋值之前读取，就会返回undefined
    ```

#### Class类型

- 实例类型

  ```ts
  //Ts的类本身是一种类型，代表类的实例类型，而不是class的自身类型
  Class Color {
      name:string;
      constructor(name:string){
          this.name = name;
      }
  }
  const green：Color = new Color('green');
  
  interface MotorVehicle{
  }
  class Car implements MotorVehicle{
  }
  //写法一
  const c1:Car = new Car();
  //写法二
  const c2:MotorVehicle = new Car('green');
  //变量的类型可以写成类Car 也可以写成接口MotorVehicle
  
  //类型使用时，类名只能表示实例的类型，不能表示类的自身类型
  class Point{
      x:number;
      y:number;
      constructor(x:number,t:number){
          this.x=x;
          this.y=y;
      }
  }
  //err
  function createPoint(
  	PointClass:Point,//因为Point是实例类型，不是类的自身类型，所以传入会报错
       x:number,
       y:number
  ){
          return new PointClass(x,y)
      }
  ```

- TS 为对象类型起名的方法：type、interface、class

- ##### 类的自身类型

  - typeof 获得一个类的自身类型

    ```ts
    function createPoint(
    	PointClass:typeof Point,//使用typeof返回类的类型
    	x:number,
    	y:number
    ):Point{
    	return new PointClass(x,y);//这里返回的值类型是Point 代表实例类型
    }
    ```

  - JS中类是构造函数的语法糖，本质是构造函数的另一种写法

    ```ts
    //类的自身类型可以写成构造函数的形式
    function createPoint(
    	PointClass:new(x:number,y:number)=>Point,//将PointClass写成了构造函数这样就可以把Point类传入
        x:number,
        y:number    
    ):Point{
        return new PointClass(x,y);
    }
    
    //构造函数可以形成对象形式，参数PointClass的另一种写法
    function createPoint(
    	PointClass:{
            new (x:number,y:number):Point
        },
        x:number,
        y:number    
    ):Point{
        return new PointClass(x,y);
    }
    //还可以把构造函数提取出来，单独定义一个接口，可以提高代码通用性
    interface PointConstructor {
        new(x:number,y:number):Point;
    }
    function createPoint(
    	PointClass:PointConstructor,
        x:number,
        y:number    
    ):Point{
        return new PointClass(x,y);
    }
    ```

- 结构类型原则

  - 一个对象只要满足Class的实例结构，就跟该Class属于同一个类型

    【只要 A 类具有 B 类的结构，哪怕还有额外的属性和方法，TypeScript 也认为 A 兼容 B 的类型。】

    ```ts
    class Foo{
        id!:number;
    }
    function fn(arg:Foo){
        //...
    }
    const bar ={
        id:10,
        amount:100,
    };
    fn(bar);//正确  bar满足Foo的实例结构，只是多了一个属性，所以可以当作参数传入
    
    //如果两个类的实例结构相同，那么这两个类就是兼容的，可以用在对方的使用场合
    class Person{
        name:string;
    }
    class Customer{
        name:string;
    }
    //ok
    const cust:Customer = new Person();  //TS将这两个类视为相同的类型，因此可以这样写
    
    //给Person多添加一个属性
    class Person{
        name:string;
        age: number;
    }
    class Customer{
         name:string;
    }
    const cust:Customer = new Person();
    //此处类的结构不相同但是可以这样写，因为Person属于Customers类型
    //根据结构类型原则，只要Person类有那么属性，就满足Customer类型的实例结构
    
    class Person{
        name:string;
        
    }
    class Customer{
         name:string;
        age: number;
    }
    const cust:Customer = new Person();//报错
    //因为Customer比Person多一个类，Person不满足c的实例结构
    
    //如果某个对象跟某个class的实例结构相同，Ts也认为两者的类型相同
    class Person{
        name:string;
    }
    const obj ={name:'John'};
    const p:Person = obj;//ok obj不是Person的实例，打死你hi赋值给p没有报错
    //这种情况instanceof 不适用判断某个对象是否属于用一个类型
    obj instanceof Person //false    实际上两者的类型是相同的
    ```

  - 空类不包含任何成员，任何其他类都可以看作与空类结构相同；

    ```ts
    //凡是类型为空类的地方，所有类（对象）都可以使用
    class Empty{}
    function fn(x:Empty){
        //...
    }
    f({})
    fn(window);
    fn(fn);  //fn的参数是一个空类，意味这任何对象都可以用作fn()的参数
    ```

  - 确定两个类的兼容关系，只检查实例成员，不考虑静态成员和构造方法

    ```ts
    class Point{
    	x:number;
    	y:number;
    	static t:number;
    	constructor(x:number){}
    }
    class Position{
    	x:number;
    	y:number;
    	z:umber;
    	constructor(x:string){}
    }
    const point:Point = new Position('');
    //这里的静态属性和构造方法不一样，但是因为实例的成员x,y相同，所以ps兼容po
    ```

  - 如果类有private和protected成员，确认兼容关系时，TS要求私有成员和保护成员来自同一个类，两个类要有继承关系

    ```tsx
    //情况一
    class A{
        private name='a';
    }
    class B extends A{
    }
    const a:A = new B();
    //情况二
    class A{
        protected name ='a';
    }
    class B extends A{
         protected name ='b';
    }
    const a:A = new B();
    ```

- 类的继承

  - 子类可以使用extends关键字继承另一个类（基类)的所有属性和方法

    ```tsx
    class A{
    	greet(){
    		console.log('hello world');
    	}
    }
    class B extends A{
    }
    const b = new B();
    b.greet() //'hello,world'
    
    //根据结构类型原则，子类可以用于类型为基类的场合
    const a:A = b;
    a.greet()//a的类型是基类，但是可以赋值为子类的实例
    
    //子类可以覆盖基类的同名方法
    class B extends A{
        greet(name?:string){
            if(name === undefined){
                super.greet();//代表调用基类的greet()
            }else{
                console.log('Hello')
            }
        }
    }
    
    //子类的同名方法不能与基类的的类型定义相冲突
    class A{
        greet(){
            console.log('sadsad')
        }
    }
    class B extends A{
        //err 与基类定义的类型冲突
        greet(name:string){
            console.log()
        }
    }
    
    //如果基类有保护成员，子类可以将该成员设成public的 也可以保持不变，但是不能改成private
    class A{
         protected x:string = "";
         protected y:string = "";
         protected z:string = "";
    }
    class B extends A{
        public x:string = '';
        protected y:string = '';
        //err
        private z:string = '';
    }
    
    //extends 后面不一定是类名，也可以是表达式，只要她的类型是构造函数
    class MyArray extends Array<number>{}
    class  MyError extends Error{}
    
    class A {
        greeting(){
            return 'Hello from A'
        }
    }
    class B {
        greeting(){
            return 'Hello from B'
        }
    }
    interface GreeterConstructor{
        new():Greeter;
    }
    function getGreetBase():GreeterConstructor{
        return Math.random() >=0.5 ? A:B;
    }
    class Test extends getGreeterBase(){//这里的extends关键字是一个表达式
        sayHello(){
            console.log(this.greeting())
        }
    }
    ```

  - 类型、没有初值的顶层属性【需要注意的细节】

    ```tsx
    interface Animal{
    	animalStuff:any;
    }
    interface Dog extends Animal{
        dogStuff:any;
    }
    class AnimalHouse{
        resident:Animal;
        
        constructor(animal:Animal){
            this.resident = animal;
        }
    }
    class DogHouse extends AnimalHouse{
        resident:Dog;//这里只设置了类型Dog，没有设置初值
        constructor(dog:Dog){
            super(dog);
        }
    }
    //如果编译设置的target设成大于等于ES2022，或者useDefineForClassFields设成true，那么下面代码的执行结果是不一样的
    const dog ={
        animalStuff:'animal',
        dagStuff:'dog'
    }
    const dogHouse = new DogHouse(dog);
    console.log(dogHouse.resident)//undefined
    //上面示例中，DogHouse实例的属性resident输出的是undefined，而不是预料的dog。原因在于 ES2022 标准的 Class Fields 部分，与早期的 TypeScript 实现不一致，导致子类的那些只设置类型、没有设置初值的顶层成员在基类中被赋值后，会在子类被重置为undefined
    
    //解决办法 使用declare命令去声明顶层成员的类型，告诉Ts这些成员的赋值由基类实现
    class DogHouse extends AnimalHouse {
        declare resident:Dog;
    	constructor(dog:Dog){
            super(dog)
        }
    }
    ```

    

- 可访问性修饰符  public、private、protected （修饰方法，构造函数，成员）

  - private 

    ```tsx
    //子类不能定义父类私有成员的同名成员
    class A{
        private x =0;
        f(obj:A){
            console.log(obj.x);
        }
    }
    class B extends A{
        x=1;//err
    }
    const a = new A();
    a.f(a);//10 A的实例对象可以获取私有成员x
    
    
    //严格地说，private定义的私有成员，并不是真正意义的私有成员。一方面，编译成 JavaScript 后，private关键字就被剥离了，这时外部访问该成员就不会报错。另一方面，由于前一个原因，TypeScript 对于访问private成员没有严格禁止，使用方括号写法（[]）或者in运算符，实例对象就能访问该成员。
    
    class A{
        private x = 1;
    }
    const a =new A();
    //使用方括号写法（[]）或者in运算符，实例对象就能访问该成员。
    a['x']//1，通过这种形式获取私有变量
    if('x' in a){
        //ok
    }
    //不建议使用private 用新的写法
    class A{
        #x = 1;
    }
    const a = new A();
    //a['x'] ES2022无法这样写
    ```

- protected 

  - 表示该成员是保护成员，只能在类内部使用，实例无法使用，子类内部可以使用

    ```tsx
    class A{
     protected x = 1；
    }
    class B extends A{
        getX(){
            return this.x;
        }
    }
    const a =new A();
    const b = new B();
    a.x//实例无法使用err
    b.getX()//1 子类内部可以使用
    ```

  - 子类可以拿到父类的保护成员，也可以定义同名成员
  
    ```ts
    class A{
        protected x = 1;
    }
    class B extends A{
        x = 2;
    }
    //实例对象a由于x是a的保护成员就无法直接获取
    class A{
        protected x =1;
        f(obj:A){
            console.log(obj.x)
        }
    }
    const a = new A();
    a.x //err   
    a.f(a)//1 这样可以获取
    ```
    
  
- 实例属性的简写形式

  ```ts
  //简写前 通过构造方法的参数传入
  class Point{
      y:number;
      x:number;
      constructor(x:number,y:number){
          this.x=x;
          this.y=y;
      }
  }
  //简写后 会自动声明对应修饰符的实例属性
  class Point{
      construnctor(
      	public x:number,//public不能省略
       	public y:number
      ){}
  }
  const p = new Point(10,10);
  p.x //10
  p.y //10
  ```

- 静态成员

  - 使用static定义静态成员

    ```ts
    class MyClass{
    	static x =0 ;
        static printX(){
            console.info(Myclass.x);
        }
        private static x =0 ;//前面可以加修饰符
    }
    MyClass.x //0 
    MyClass.printX()//0  静态方法和静态属性必须通过MyClass获取不能用实例调用
    //ES6的写法
    class MyClass{
        static #x =0;
    }
    ```

    

  #### 泛型类

  ```ts
  class Box<Type>{
      contents:Type;
      constructor(value:Type){
          this.constents = value;
      }
      //静态成员不能使用泛型的类型参数
      static defa:Type;、、err
  }
  const b:Box<string> = new Box('hello')
  ```

- 抽象类，抽象成员 abstract （表示该类不能被实例化，只能当作其他类的模板）

  ```tsx
  abstract class A{
      id = 1;
  }
  const a = new A();//err 抽象类不能被实例化
  
  //抽象类只能当作基类使用，用来在他的基础上定义子类
  abstract class A{
      id=1;
  }
  class B extends A{
      amount = 100;
  }
  const b = new B();
  b.id //1
  b.amount //100
  
  //抽象类的子类也可以是抽象类
  abstract class A{
      foo:number;
  }
  
  abstract class B extends A{
      bar:string;
  }
  ```

  - 抽象成员，表示未实现的属性名和方法，需要在子类实现

  ```ts
  abstract class A{
  	abstract foo:string;//必须要实现这个属性，否则会报错
      bar:string = '';
      abstract execute():string;
  }
  class B extends A{
      foo = 'b';
       execute(){
           return 'Bxxx'
       }
  }
  ```

  - 注意要点
    - 抽象成员只能存在于抽象类，不能在普通类
    - 抽象成员不能有具体的实现代码
    - 抽象成员不能有private修饰符
    - 一个子类最多继承一个抽象类


- ​	this问题

  ```ts
  class A{
  	name='A';
      getName(){
          return this.name;//this表示该方法当前所在的对象
      }
  }
  const a = new A();
  a.getName()//'A'
  const b = {
      name:'b',
      getName:a.getName
  }
  b.getName()//'b' 在变量a运行，this指向a，在b运行，this指向b
  ```

  ```ts
  function fn(//编译前
  	this:SomeType,//this用来声明内部的this的类型
  	x:number
  ){
  
  }
  //编译后
  function fn(x){}//编译后会去除这个this参数
  ```

  ```ts
  //this参数的类型可以声明为各种对象
  function foo(
  	this:{name:string}
  ){
          this.name='jack';
          
      }
  ```

  - 类的内部他是可以当作类型使用，表示当前类的实例对象

    ```ts
    class Box{
    	contents:string = '';
        set(value:string):this{
            this.contents =value;
            return this;//表示当前的实例对象
        }
    }
    ```

  - this类型不允许使用于静态成员

    ```
    class A{
    	static a:this;//errr
    }
    ```

  - this is Type 判断this是否属于某种类型  

- 泛型   实现输入类型与输出类型的一一对应关系

  ```ts
  function getFirst<T>(arr:T[]):T{ 
      return arr[0]; //这里返回的类型就是T的类型
  }
  //函数调用时，需要提供类型参数
  getFirst<number>([1,2,3])
  //可以省略类型参数的值，让Ts自己判断
  getFirst([1,2,3])
  //复杂的使用场景，ts无法推断的必须显示给出
  
  //多个类型参数的写法
  function map<T,U>(
  	arr:T[],
       f:(arg:T)=>U
  ):U[]{
          return arr.map(f)
      }
  map<string,number>(
  ['1,'2','3'],
   (n) => parseInt(n)
  );//[1,2,3] 得到的是U类型的数组
  ```

- 泛型的写法 【函数，接口，类，别名】

  - 函数写法

    ```ts
    function id<T>(arg:T){
        return arg;
    }
    let myId:{<T>(arg:T):T}=id
    ```

  - 接口写法

    ```ts
    interface Box<Type>{
    	contents:Type;
    }
    //使用泛型接口，需要给出类型参数的值
    let box:Box<string>;
    //写法二
    interface Fn{
        <Type>(arg:Type):Type;
    }
    function id<Type>(arg:Type):Type{
        return arg;
    }
    let myId:Fn = id;//Fn的类型参数，需要在id使用时提供
    ```

  - 类的泛型写法

    ```ts
    //类型参数写在类名后面
    class Pair<K,V>{
        key:K;
        value:V;
    }
    //继承泛型类
    class A<T>{
        value:T;
    }
    class B extends A<any>{ //继承时必须要给出T的类型
    }
    
    //泛型可以用在类表达式
    const Container = class<T>{
        constructor(private readonly data:T){}
    }
    //新建实例时要同时给出T和参数data1的值
    const a = new Container<boolean>(true);
    const b = new Container<number>(0);
    ```

  - 可以把泛型类写成构造函数

    ```ts
    type MyClass<T> = new (...args:any[])=>T;
    //or
    interface MyClass<T>{
        new(..args:any[]):T;
    }
    
    //用法
    function createInstance<T>(
    	AnyClass: MyClass<T>,//第一个参数是一个构造函数（类）
         ..args: any[]
    ):T{
        return new AnyClass(..args);
    }
    ```

  - 类型别名的泛型写法

    ```ts
    type Nullable<T> = T | undefined |null;//只要传入一个类型，就可以得到这个类型与undefined和null的一个联合类型
    
    type Container<T> = {value:T};
    const a:Container<number> = {value:0
    const b:Container<string>={value:'b'};  
    //定义树结构
    type Tree<T> ={
        value:T;
        left:Tree<T> |null;
        right:Three<T> |null;
    }
    ```

- 类型参数的默认值

  ```ts
  function getFirst<T= srting>(//T=string 表示默认值是string
   arr:T[]
  ):T{
     return arr[0]
  }
  //调用getFirst时，如果不给T值默认是string
  
  
  class Generic<T = string>{
      list:T[] = []
      add(t:T){
          this.list.push(t)
      }
  }
  const g = new Generic();//这里新建实例时没有传入T的类型 所以是默认类型
  g.add(4)//err 所以报错
  g.add('hello')//ok
  
  //一旦类型参数有默认值就默认他是可选参数
  //可选参数必须在必选参数之后
  <T,U = boolean>//ok
  ```

- 数组的泛型表示

  ```ts
  let arr:Array<number> = [1,2,3] //相当于number[],string[]
  ```

  ```ts
  //Ts内部 Array是一个泛型接口
  interface Array<Type>{
      length:number;
      pop():Type|undefined;
      push(...items:Type[]):number;//只能添加同类型的成员
  }
  ```

  ```ts
  //ReadonlyArray<T> 表示只读
  function doStuff(
  	values:ReadonlyArray<string
  ){
          values.push('hello')；//报错
      }
  
  ```

- 类型参数的约束条件  

  ```ts
  function comp<Type>(a:Type,b:Type){
  	if(a.length >= b.length){ //这里隐含了一个必须存在length的属性的条件
          return a;
      }
      return b;
  }
  
  //在类型参数上写明约束条件，不满足会报错
  function comp<T extends {length:number}>(//表示类型必须满足
  	a:T,
      b:T
  ){
     if(a.length >=b.length){
        return a;
     }
     return b;     
   }
  //参数类型的约束条件采用以下形式
  <TypeParamter extends ConstrainType>
      
  //类型参数可以同时设置约束条件和默认值，默认值必须满足约束条件
   type Fn<A extends string , B extends string = 'world'>=[A,B]
  type Result =Fn<'hello'>//['hello','world'] 可以只给A的值不给出B的值
  
  //如果有多个类型参数，一个类型参数的约束条件，可以引用其他参数
  <T,U extends T>
  <T extends U ,U>
  //但是约束条件不能引用类型参数自身
  <T extends T>//err
  //多个类型参数也不可以互相约束
  <T extends U ,U extends T>//err
  ```

- 使用要点

  - 尽量少用泛型 =》因为复杂性会变大

  - 类型参数越少越好

  - 类型参数需要出现两次以上

  - 泛型可以嵌套【类型参数可以是另一个泛型】

    ```ts
    type orNull<Type> = Type|null;
    ```

#### Enum类型

- Enum结构用来将相关的常量放在一个容器里，方便使用

  ```ts
  enum Color{
  	Red,//0
      Green,//1
      Blue//2
  }
  //使用  
  let c= Color.Green;//1
  let c =Color['Green'];//1
  //Enum结构本身也是一种类型  所以可以是number或者Color（更好）
  let c:Color = Color.Green;//ok
  let c:number= Color.Green;//ok
  
  //编译前
  enum Color{
      Red，
      Green,
      Blue
  }
  //编译后
  let Color ={
      Red:0,
      Green:1,
      Blue:2
  }
  ```

  - TS5.0之前 的bug【Enum类型的变量可以赋值为任何值】

    ```ts
    enum Bool{
    	NO，
    	Yes
    }
    function foo(noYes：Bool){
        
    }
    foo(33)//5.0之前任何数值作为函数foo的参数，编译都不会报错
    ```

  - Enum编译后是一个对象，不能有与她同名的变量

  - Enum结构可以被对象的as const 断言替代

    ```ts
    enum Foo{A,B,C}
    const Bar ={
    	A:0,
        b:1,
        c:2
    } as const;
    if(x=== Foo.A){}
    //等同于
    if(x === Bar.A)
    ```

- Enum成员默认不用赋值，默认从0开始逐一递增，也可以显示赋值

  ```ts
  enum Color {
    Red,
    Green,
    Blue
  }
  enum Color {//可以任意赋值，但是不能是Bigint
    Red = 90,
    Green = 0.5,
    Blue = 7n // 报错
  }
  //成员的值可以相同
  //只设定第一个成员的值，后面成员的值会从这个值开始递增
  const enum Color{
      Red = 7,
      Green,//8
      Blue//9
  }
  //成员的值可以用计算式
  //Enum成员值都是只读的，不能重新赋值
  Color.Red = 4;//err
  ```

- 同名Enum的合并[多个同名的Enum结构会自动合并]

  ```ts
  enum Foo {
    A，//只允许其中一个的首成员省略初始值，否则报错
  }
  
  enum Foo {
    B = 1,
  }
  
  enum Foo {
    C = 2,
  }
  
  // 等同于
  enum Foo {
    A,
    B = 1，
    C = 2
  }
  //同名合并的限制是 要么都是const 要么都是非const枚举 不能混合使用
  //报错
  enum E{
      A,
  }
  const enum E{
      B=1
  }
  ```

- 字符串Enum 【可以用作一组相关字符串的集合】

  ```ts
  //必须显示设置 如果没有设置，默认为数值
  enum Foo{
      A,//0
      B='hello',
      C//err c前面没用数值成员，必须赋值
  }
  //字符串和数值可以混合赋值，但是不能用其他值
  
  ```

- keyof 运算符【取出Enum结构的所有成员名，作为联合类型返回】

  ```tsx
  enum MyEnum{
  	A = 'a'，
      B = 'b'
  }
  type Foo = keyof typeof MyEnum; //'A'|'B'
  // typeof必须写
  //返回Enum所有的成员值，in运算 {a:any,b:any}
  type Foo = {[key in MyEnum]:any}; 
  ```

- 反向映射【可以通过成员值获得成员名】

  ```ts
  enum Weekdays{
  	Money = 1，
      Tuesday,
      WendnesDay
  }
  console.log(Weekdays[3])// Wednesday
  //对于字符串Enum，不存在反向映射
  ```

##### TS的类型断言

- 语法

  ```ts
  //语法一：<类型>值
  <Type> value
  //语法二：值 as 类型  频繁
  value as Type
  ```
  
- ```ts
  //对象类型字面量检查比较严格，如果存在额外的属性会报错
  const p:{x:number}= {x:0,y:0};//err
  const p0:{x:number} = {x:0,y:0} as {x:number};//ok
  const p1:{x:number} = {x:0,y:0} as {x:numbe;y:number}//ok
  ```
  
  
  



