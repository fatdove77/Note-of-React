# lab2

## 题目1

写一个JavaScript函数solveEquation ，求解一元二次方程ax2 + bx + c = 0. 

solveEquation( ) //解视为：0

solveEquation(1, 4, 4) // 解：-2

solveEquation(1, -1, -2) //解：{2, -1}



要求使用let、const

要求使用函数表达式或者箭头函数



## 题解

```js
const solveEquation = (a,b,c)=>{
  	//	1.如果空参
   
    console.log(a,b,c); ////undefined undefined undefined
  
  const s = b*b-4*a*c;
  let x = [];
  if(a==undefined){
    x[0] = 0;
    return x;
  }
  if(s>0){
    x[0] = (-b+Math.sqrt(s))/2*a;
    x[1] = (-b-Math.sqrt(s))/2*a;
  }
  else if(s==0){
    x[0] = (-b+Math.sqrt(s))/2*a;
  }
  else {
    x[0] = "无解";
  }
  return x;
} 

//1
solveEquation().forEach((item)=>{
  console.log(item);  //output is 0
});

//2
solveEquation(1，4，4).forEach((item)=>{
  console.log(item);  //output is -2
});

//3
solveEquation(1，-1，-2).forEach((item)=>{
  console.log(item);  //output is 2,-1
});

//4
solveEquation(1,0,1).forEach((item)=>{
  console.log(item);  //output is 无解
});

```

后两种情况比较好写，参数都是固定的，第一种如果不传参数，变量类型为undefined，以此来判断空参



## 题目2



针对一个数组

const ages = [52, 44, 76, 7, 17, 36, 32, 32, 26, 28, 27, 49, 32, 33, 32, 33, 27, 25, 26, 38, 37, 31, 38, 38, 49, 29, 85, 32, 64, 32, 78]

计算平均值、中位数、 范围、方差

*方差是每个样本值与全体样本值的平均数之差的平方值的平均数

•要求使用Array中方法进行求解

•要求使用高阶函数（不能使用for、while等循环语句）

•要求某个步骤中使用reduce方法（自己选择即可，不唯一）

•所需方法（不限于PPT中的几个方法）在下面参考中寻找

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array



## 题解

```js
const ages = [52, 44, 76, 7, 17, 36, 32, 32, 26, 28, 27, 49, 32, 33, 32, 33, 27, 25, 26, 38, 37, 31, 38, 38, 49, 29, 85, 32, 64, 32, 78]
//avg
let avg = 0;
let sum = ages.reduce((prev,cur)=>{
  prev+=cur;
  return prev;
},0)
avg = sum/ages.length;
console.log("平均值为"+avg);//output is 平均值为38.225806451612904
console.log("------------"); 
													

ages.sort((a,b)=>{
  return a-b;
});
// console.log(ages);n
// console.log(ages.length);
let mid = 0;
if(ages.length%2===0){
  const pos = ages.length/2;
  mid = (ages[pos]+ages[pos-1])/2;
}
else {
  const pos = (ages.length-1)/2;
  mid = ages[pos];

}

console.log("中位数"+mid);  //output is 中位数32
console.log("------------");

console.log("范围["+ages[0]+","+ages[ages.length-1]+"]");//output is 范围[7,85]
console.log("------------");
const cVariance = (ages,avg)=>{
  let sum = 0;
  ages.forEach((val)=>{
    sum+=Math.pow(val-avg,2);
  })
  return sum/ages.length;
}

console.log("方差为"+cVariance(ages,avg)); //output is 方差为290.4328824141519


```



## reduce函数

### 语法

```
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```



### 高级用法

#### 统计数组元素出现次数

```js
const names = ["zjx","zs","cg","zb","xg","zjx"];
const nameCnt = names.reduce((prev,cur)=>{
  if(cur in prev){
    prev[cur]++;
  }
  else {
    prev[cur] =1;
  }
  return prev;
},{});
console.log(nameCnt);
```



### 数组去重

```js
const arr = [1,2,3,3,3,4,2,1];
const newArr = arr.reduce((prev,cur)=>{
  if(!prev.includes(cur)){
    prev.push(cur);
  }
  return prev;
},[]);
console.log(newArr);
```



[(24条消息) js reduce函数_如花菇凉的博客-CSDN博客_js reduce函数](https://blog.csdn.net/u012174809/article/details/122343996?ops_request_misc=%7B%22request%5Fid%22%3A%22166411904016782391881134%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=166411904016782391881134&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-122343996-null-null.142^v50^control,201^v3^control_2&utm_term=js  reduce函数&spm=1018.2226.3001.4187)





## 函数式编程 

函数式编程就是可以把函数以变量的形式进行传递和调用，例如回调函数。可以更加简便地进行函数的组合



#### 函数是一等公民(头等函数)

- 函数可以存储在变量中
- 函数作为参数
- 函数作为返回值





