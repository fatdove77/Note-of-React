## 遍历对象的方法

```js
const person = {
    //注意是逗号分隔属性
    firstName: 'first',
    lastName: 'last',
    age: 20,
    skills: [
        'JavaScript',
        'React',
        'Node'
    ],
    isMarried: false,

    getFullName: function () {
        return `${this.firstName},${this.lastName}`;
    },
};


```

```js
//console.log(Object.getOwnPropertyDescriptor(person, 'skills'));
//console.log(Object.getOwnPropertyDescriptor(person, 'getFullName'));
//console.log(Object.getOwnPropertyDescriptor(person.skills, 'length'));

//for in

for (let prop in person) {
    console.log("person." + prop + " = " + person[prop]);
}
//output is person.firstName = first
//person.lastName = last
//person.age = 20
//person.skills = JavaScript,React,Node
//person.isMarried = false
//person.getFullName = function () {
//        return `${this.firstName},${this.lastName}`;
//    }


//Object.keys()打印对象键值生成一个数组

console.log(Object.keys(person));
//output is 
//[
  'firstName',
  'lastName',
  'age',
  'skills',
  'isMarried',
  'getFullName'
//]

//生成json对象
console.log(JSON.stringify(person));
//output is {"firstName":"first","lastName":"last","age":20,"skills":["JavaScript","React","Node"],"isMarried":false}
```





## 对象的引用和复制

对象的引用是指向同一个内存，对象的复制是创建不同内存的副本，两个对象互不影响

一般我们会写出这样的代码

```js
String a = "123";
String b = a;
b = "345";
console.log(a,b); //output is "123" "345"
```

如果我们把数据类型换成对象

```js
const person = {
    firstName: 'first',
    lastName: 'last',
    age: 20,
};
//person、samePerson指向的相同对象，即都引用了一个对象
let samePerson = person;
samePerson.age = 21;
console.log(person.age); //output is 21
```

这是因为基本数据类型是复制变量，而对象是引用一个变量

这是一个很重要的特性





```js
//age拥有person.age的一个副本
let age = person.age;
age = 30;  //age是基本数据类型 Integer
console.log(person.age);

```



### Object.assgin

```js
//复制对象
let anotherPerson = Object.assign({}, person);
anotherPerson.age = 70;
console.log(anotherPerson.age);
console.log(person.age);

//复制合并多个对象
let newPerson = Object.assign(
    {}, person, { city: 'HangZhou' }, { firstName: 'Peng' });
console.log(newPerson);   
```



## 解构

```js
const person = {
    firstName: 'robin',
    lastName: 'peng',
    age: 20,
};
//解构赋值
let {firstName, age} = person;
firstName = "xuan";   //结构是复制操作 不对原来的对象属性进行改变
console.log(person.firstName);
console.log(firstName);
console.log(age);

```

## 对象展开

```js
const person = {
    firstName: 'robin',
    lastName: 'peng',
    age: 20,
};
//展开
let {firstName, age} = {...person};
console.log(firstName,age);


//展开用于复制对象  和对象的引用不同

let another = {...person};
another.age = 22;
console.log(person, another); //20、22


```





## THIS

执行时this指向调用函数的上下文对象

```js
const person = {
	name:"zjx",
	sayHello:function(){
		console.log(`Hello ${this.name}`);
	}
}

console.log(person.sayHello());  //output is zjx;
const ano = {
	name:"zs"
}
ano.sayHello = person.sayHello();
console.log(person.sayHello());  //output is zjx;
console.log(ano.sayHello()); //output is zs

//this指向调用函数的对象，是动态的，只有在运行的时候才能确定
```



### this的默认值

**tips: 在nodejs环境下全局变量是global，在浏览器环境下全局变量是window**

```js
//函数绑定全局对象，this变为指向全局对象
let freeSayHello = person.sayHello;
freeSayHello(); //undefined 全局对象中没有name属性  //指向global对象，但是global.name为空
global.name = "Global"; 
freeSayHello(); //Global 全局定义name属性
```



### 箭头函数的this

*箭头函数在this上的特殊性, 箭头函数没有this, 但是可以使用外层的this对象*****

```js
robot.arrowFunction = ()=>{console.log(this)};
robot.arrowFunction(); //{} 箭头函数没有this并且当前外层没有this  //output is {}
robot.anonymousFunction = function(){console.log(this)};
robot.anonymousFunction(); //robot对象 普通匿名函数有this  
robot.arrowFunctionInRobot = function(){
    (()=>{console.log(this)})();
};
robot.arrowFunctionInRobot(); //robot对象 箭头函数没有this但是外层有this


```

