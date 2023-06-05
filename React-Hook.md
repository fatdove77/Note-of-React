# React-Hook

hook-router-redux

## 简介

`React Hooks`就是用函数的形式代替原来的继承类的形式，并且使用预函数的形式管理`state`，有Hooks可以**不再使用类的形式定义组件**了。这时候你的认知也要发生变化了，原来把组件分为**有状态组件**和**无状态组件**，有状态组件用类的形式声明，无状态组件用函数的形式声明。那现在所有的组件都可以用**函数**来声明了。





### react hooks 编写形式对比

原生写法：

```react
import React, { Component } from 'react';

class Example extends Component {
    constructor(props) {
        super(props);
        this.state = { count:0 }
    }
    render() { 
        return (
            <div>
                <p>You clicked {this.state.count} times</p>
                <button onClick={this.addCount.bind(this)}>Chlick me</button>
            </div>
        );
    }
    addCount(){
        this.setState({count:this.state.count+1})
    }
}

export default Example;
```



hook写法：

```react
import { useState } from "react";

function HookDemo() {  //必须大写
  const [count,setCount] = useState(0);
  return ( 
    <div>
      <p>you clicked {count} times</p>
      <button  onClick = {()=>{setCount(count+1)}}>click me</button>
    </div>

   );
}

export default HookDemo;
```

hook 就是让你不再写class extent  用函数一统江湖







## Hook的使用说明

- Hook不能在class组件中使用（可以在package中进行修改）
  也就是说组件用了class声明就不能再用useState等函数

- 只能在函数最外层调用Hook。不要在循环，条件判断或者子函数中调用

  不能在判断中使用useEffect等（因为钩子函数是根据创建顺序对应，执行顺序不能发生变化）

- 只能在React的组件函数中调用Hook,不要在其他JS函数中调用





## useState

### useState的介绍

useState是一个自带的hook函数，他的作用是用来声明状态变量

那我们从三个方面来看`useState`的用法，分别是声明、读取、使用（修改）。这三个方面掌握了，你基本也就会使用`useState`了.

使用前要引用

```react
import { useState } from "react";
```



#### 声明

```react
const [count,setCount] = useState(0);
```

useState接受的参数是状态的初始值，他返回一个数组，这个数组第0位是当前的状态值，第1位是可以改变状态值的函数，所以上面的代码就是声明了一个状态变量count，以及可以改变count的状态值的函数**（解构的思想）**

```react
const [person,setPerson] = useState({
    name:"zjx",
    age:18,
  });
```





#### 读取

```react
<p>You clicked {count} times</p>
```

读取很简单，因为是一个变量，在JSX中直接用{}读取



#### 修改

直接调用setCount,传入参数就是对应修改过状态值，剩下的React会重新渲染组件

```react
<button onClick={()=>{setCount(count+1)}}>click me</button>  //count = count+1 
```

对于复杂的更新逻辑使用函数式更新

```react
const handleClick1= ()=>{
    setCount(()=>{
       if(count<3){
       	return count+1;  //这里不能写count = count+1 
      }
    	else {
        return count+2
    	}
    })
  }
<button  onClick = {()=>{handleClick()}}>click me</button>
```

必须有返回值而且返回值必须是一个常量

```react
 const handleClick2 = ()=>{
    setPerson(()=>{
      return {
        ...person,
        name:'zs'
      }
    })
 }
```



### 多状态声明的注意事项

比如我们现在要声明多个状态

```react
import React,{ useState } from "react";
function Demo2() {
  const [age,setAge] = useState(10);
  const [sex,setSex] = useState('MALE');
  const [work,setWork] =  useState('engineer');
  return ( 
    <div>
      <p>zjx is {age} years old</p>
      <p>zjx's gender is {sex}</p>
      <p>zjx's work is {work}</p>
    </div>
   );
}

export default Demo2;
```

react是如何把useState中的值绑定到前面的状态值当中呢

答案是按顺序绑定，所以useState不能写在 if else 等判断性语句中





### useState惰性初始化

```react
<HookDemo init = {100}/>
```

```react
//function HookDemo(props) {
  const [count,setCount] = useState(()=>{
    if(props.init<100){
      return props.init+1;
    }
    else {
      return props.init+100;
    }
  });  //通过函数式调用进行props的判断，再返回给count的值，注意返回的是一个常量
```

**如果状态值没有更新，那么react将跳过组件的渲染和effect的执行**







## useContext

```react
import React,{useContext}  from 'react';
import './App.css';
//创建context
const numberContext = React.createContext();
//它返回一个具有两个值的对象
//{Provider ， Consumer}
function App(){
  //使用Provider为所有子孙提供value值
  return (
    <numberContext.Provider value={520}>
        <div>
        <ShowAn />
        </div>
    </numberContext.Provider>
  )
}

function ShowAn(){
  //使用Consumer从上下文获取value
//调用useContext，传入从React.createContext获取的上下文对象。
  const value = useContext(numberContext);
  return(
    // <numberContext.Consumer>
      // {value=><div>the answer is {value}</div>}
    // </numberContext.Consumer>
    <div>
      the answer is {value}
    </div>

  )
}
export default App;
```





带有参数的createContext声明

context.js

```react
import React from 'react'


export const themes = {
  light:{
    color:"red",
    backgroundColor:"#ffcc33",
  },
  dark:{
    color:"blue",
    backgroundColor:"#222222",
  }
}
export const ThemeContext =  React.createContext(themes);// 这个参数加不加意义不大  
```

father.js

```react
import React from 'react'
import {ThemeContext,themes} from './context'
import Child from './child'
function Father(){
  // console.log(test);
  return (
    <ThemeContext.Provider value = {themes.light}> //这里一定要引入两个变量一个是createContext创建的变量,还有一个是传入的参数,具体是为什么我也不知道
      <Child/>
  </ThemeContext.Provider>
  )
  

}
export default Father ;
```

child.js

```react
import React,{useContext} from 'react'
import {ThemeContext} from './context'
function Child() {
  // const value = useContext(ThemeContext);
  // console.log(themes);
  const value = useContext(ThemeContext)//接受的参数是带有provider的标签，接收到的是value值
  console.log(value)
  return ( 
    <button style = {value}>
      点击 
    </button>
  );
}

export default Child;
```



## useEffect

在react完成对DOM的更新后会执行，默认情况下， react会在渲染后调用副作用函数，包括第一次渲染的时候（didmounted didupdated）

使用前进行引入

```react
import { useEffect} from "react";
```



```react
const [count,setCount] = useState(0);
useEffect(()=>{
    setCount(1);
    console.log(count);
  })
//output is 0 1  0是因为第一次页面渲染count的初始值是0，同时把setcount放到循环队列中，这是一个异步的过程，然后调用setcount改变count值为1，页面重新渲染，再次进入useEffect中，打印出1
```



```react
import React, { useState, useEffect } from 'react';

function Demo3 () {
  const [ count , setCount ] = useState(0);
  useEffect(() =>{
    console.log(`useEffect you clicked ${count} times`);
  })
  return ( 
    <div>
      <p>You clicked {count} times</p>
            <button onClick={()=>{setCount(count+1)}}>click me</button>
    </div>
  );
}

export default Demo3;
```

每次函数mount渲染之后都会执行useEffect,而之前我们要componentDidMount和componentDidUpdate两个生命周期函数来实现



### useEffect的两个参数

第一个参数是副作用的处理函数，第二个参数是与该副作用关联的状态或属性依赖数组

完整demo代码如下

```react
import { useState ,useEffect} from "react";

function HookDemo(props) {
  const [count,setCount] = useState(0);
  // console.log("update")
  useEffect(()=>{
    console.log('useEffect');
  },[])
  const handleClick1 = ()=>{
    setCount(()=>{
       if(count<3){
       	return  count+1;
      }
    	else {
        return count+2
    	}
    })
  }

  return ( 
    <div>
      <p>you clicked {count} times</p>
      <button  onClick = {()=>{handleClick1();}}>click me</button>
      {/* <button  onClick = {()=>{handleClick2()}}>click me</button> */}
    </div>

   );
}

export default HookDemo;
```

如果我们要useEffect函数只执行一次，也就是只在第一次渲染的时候进行更新，那么传递进去的参数就是空



### useEffect中的return

```react
useEffect(()=>{
	//开始监听代码
    console.log('开始监听');
    return()=>{
    //结束监听代码
      console.log("取消监听")
    }
  })
```

页面首次渲染 output is 开始监听

当页面Dom改变,the output is 取消监听 开始监听

可得知第一次渲染不执行useEffect函数但是后面页面的更新是先执行return中的函数的

可以用于监听函数，这一次点击事件需要取消上一次遗留下来的信息，就可以用return函数清除上次的信息





### 使用useEffect函数实现生命周期函数

ccomponentDidUpdate

```react
useEffect(()=>{},[需要监听的变量])
```



### 如何绕过 Capture Value

```react
function Demo3 () {
  const [count, setCount] = useState(0);
  const latestCount = useRef(count);

  useEffect(() => {
    // Set the mutable latest value
    latestCount.current = count;
    setTimeout(() => {
      // Read the mutable latest value
      console.log(`You clicked ${count} times`);
    }, 3000);
  });
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );

}
```

获取的是点击时的值





利用 `useRef` 就可以绕过 Capture Value 的特性。**可以认为 `ref` 在所有 Render 过程中保持着唯一引用，因此所有对 `ref` 的赋值或取值，拿到的都只有一个最终状态**，而不会在每个 Render 间存在隔离。

```react
function Example() {
  const [count, setCount] = useState(0);
  const latestCount = useRef(count);

  useEffect(() => {
    // Set the mutable latest value
    latestCount.current = count;
    setTimeout(() => {
      // Read the mutable latest value
      console.log(`You clicked ${latestCount.current} times`);
    }, 3000);
  });
  // ...
}
```

获取点击之后的终值







### 回收机制

在组件被销毁时，通过 `useEffect` 注册的监听需要被销毁，这一点可以通过 `useEffect` 的返回值做到：

```scss
useEffect(() => {
  ChatAPI.subscribeToFriendStatus(props.id, handleStatusChange);
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.id, handleStatusChange);
  };
});
```

在组件被销毁时，会执行返回值函数内回调函数。同样，由于 Capture Value 特性，每次 “注册” “回收” 拿到的都是成对的固定值。







## useReducer

*The `useReducer` hook is similar to `useState`, but gives us a more structured approach for updating complex values.*

*We typically use `useReducer` when our state has multiple sub-values, e.g. an object containing keys that we want to update independently.*

useReducer接受三个参数，第一个参数是处理状态更新的reducer（处理状态更新都要把参数返回），第二个参数是状态初始值，但三个函数是状态初始化函数

```react
import { useState ,useEffect, useReducer} from "react";

function HookDemo(props) {
  function init(initialCount){
    return {count:initialCount};
  }
  function reducer(state,action){
    switch(action.type){
      case 'increment':return {count:state.count+1};
      case 'decrement':return {count:state.count-1};
      case 'reset':return init(action.payload);
      default:throw new Error();
    }
  }
  const [state,dispatch] = useReducer(reducer,1,init);  
  return ( 
    <div>
      <p>you clicked {state.count} times</p>
      <button  onClick = {()=>{dispatch({type:'reset',payload:1})}}>click me to reset</button>
      <button  onClick = {()=>{dispatch({type:'decrement'})}}>click me to decrement one</button>
      <button  onClick = {()=>{dispatch({type:'increment'})}}>click me add one </button>
      {/* <button  onClick = {()=>{handleClick2()}}>click me</button> */}
    </div>

   );
}

export default HookDemo;
```

dispatch会调用reducer函数，同时传进去的参数就是reducer中的action对象

state是个对象状态值，可以存储很多数据

init是初始化状态对象count为1



## useCallback

x更新，y也会随之更新

```react
function Demo3(){
  const [x,setX] = useState(0);
  const y = 2*x+1
  const changeX = ()=>{
    setX(x+1);
  }
  return (
    <ul onClick={changeX}>
      <li>x是{x}</li>
      <li>y是{y}</li>
    </ul>
  )
}
```

```react
const y = useMemo(()=>2*x+1,[x]);  //定义变量的改变，使用useMemo返回一个表达式，第二个参数是绑定的因变量的值
const changeX = useCallback(()=>setX(x+1),[x]);  //返回体是一个函数，第二个参数是绑定值

```

这两种方法可以提高性能





### 基本写法

第一个参数是**处理函数**，第二个参数是一个**数组**，用于制定被记忆函数更新所依赖的值，**返回的是一个函数**

```react
const memoizedCallback = useCallback(()=>{
	doSomething(a,b);
},[a,b])
```

### 为什么要使用useCallback

函数式组件中，定义在组件内函数会随着状态值的更新而重新渲染，函数中定义的函数会被频繁定义，在父子组件通信中这是非常消耗性能的，使用useCallback和memo减少子组件更新频率，提高效率



如下，每次点击都会重新渲染子组件，每次都会重新打印

我们使用memo（react的高阶方法），就不会重新打印了

```react
 import React , {useCallback,useState} from 'react';
 //父组件
 function Memoized() {
  const [count,setCount] = useState(1);
  const handleClick = ()=>{
    setCount(count+1);
  }
  // const handleClick = useCallback(()=>{setCount(1);})
  return ( 
    <div>
      <p>{count}</p>
      <button onClick={handleClick}>点击</button>
      <Child></Child>
    </div>
   );
 }
 

const Child = React.memo(function Child(){
  console.log("子组件被触发")
  return(
    <div>
      <p>hello world </p>
    </div>
  )
}) //传入组件，如果这个组件props的值没有变化，那就被缓存，不会更新


 export default Memoized ;
 
```



如果我们需要子组件调用父组件的函数，那么父组件每次重新渲染函数都会跟着一起更新，那么子组件的props就会改变，又会触发打印，所以我们使用useCallback 用于子组件调用父组件的方法，同时父组件是频繁改变的状态：

```react
 import React , {useCallback,useState} from 'react';
 //父组件
 function Memoized() {
  const [count,setCount] = useState(1);
  // const handleClick = ()=>{
  //   setCount(count+1);
  // }
  const handleClick = useCallback(()=>{setCount(count+1);},[count])
  const childClick = ()=>{

  }
  const childClick = useCallback(()=>{}.[]);  //不会打印了
  return ( 
    <div>
      <p>{count}</p>
      <button onClick={handleClick}>点击</button>
      <Child childClick={childClick}></Child>
    </div>
   );
 }
 


 const Child = React.memo(function Child(){
  console.log("子组件被触发")
  return(
    <div>
      <p>hello world </p>
    </div>
  )
})


 export default Memoized ;
 
```





## useMemo

第一个参数用于计算并返回需要记录的值，第二个参数为数组，用于记忆，**返回一个值**

```react
const memoizedValue = useMemo(()=>{computedValue(a,b)},[a,b]);
```

useMemo和useCallback的区别

1. useMemo传入的函数内部需要有返回值
2. useMemo只能声明在函数式组件内部   类比react.memo

点击事件的bug：

[(6条消息) 解决“Error: Too many re-renders. React limits the number of renders to prevent an infinite loop.”_蛞蝓不孤寡的博客-CSDN博客](https://blog.csdn.net/fish_skyyyy/article/details/119137987)





### 计算属性的监听作用

```react
import React,{useState,useMemo} from 'react'
function MemoizeValue() {
  const [a,setA] = useState(0);
  const [b,setB] = useState(0);
  const [d,setD] = useState(0);
  const c = useMemo(()=>a+b,[a,b]);
  const handleClick = (type)=>{
    switch(type){
      case 'a':
        setA(a+1)
        break;
      case 'b':
        setB(b+1)
        break;
      default:
        return false;
    }
  }
  return ( 
    <div>
      <p>{a}</p>
      <p>{b}</p>
      <p>{c}</p>
      <button onClick={()=>handleClick('a')}>+a</button>
      <button   onClick={()=>handleClick('b')}>+b</button>

    </div>
    
   );
}

export default MemoizeValue;
```



也可以返回DOM值

```react
import React,{useState,useMemo} from 'react'
function MemoizeValue() {
  const [a,setA] = useState(0);
  const [b,setB] = useState(0);
  const [d,setD] = useState(0);
  const c = useMemo(()=>{
    return (
      <div>
        dom:
        {a+b}
      </div>
    )
  },[a,b]);
  const handleClick = (type)=>{
    switch(type){
      case 'a':
        setA(a+1)
        break;
      case 'b':
        setB(b+1)
        break;
      case 'd':
        setD(d+1)
        break;
      default:
        return false;
    }
  }
  return ( 
    <div>
      <p>{a}</p>
      <p>{b}</p>
      <p>{d}</p>
      <p>{c}</p>
      <button onClick={()=>handleClick('a')}>+a</button>
      <button   onClick={()=>handleClick('b')}>+b</button>
      <button   onClick={()=>handleClick('d')}>+d</button>

    </div>
    
   );
}

export default MemoizeValue;
```





## useRef(不受控组件)

正常我们要取到一个值我们会用到useState这个方法

```js
import React,{useState} from 'react';
function GetRef() {
  const [val,setVal] = useState('asd');
  const inputChange = (e)=>{
    console.log(e.target.value);
    setVal(e.target.value);
  }
  return ( 
    <div>
      <input type="text" name="" value={val} onChange={inputChange}/>
      <button > 获取input的值</button>
    </div>
   );
}

export default GetRef;
```





所谓的受控组件和不受控组件只存在于表单元素内，需要用useState来定义

不受控组件意味着 表单元素的value，只能用useRef来获取

```js
import React,{useState,useRef} from 'react';
function GetRef() {
  const element = useRef(null);
  return ( 
    <>
    <input type="text" ref = {element} />
    <button  onClick={()=>console.log(element.current.value)} >获取input的值 </button>
    </>
   );
}

export default GetRef;
```



