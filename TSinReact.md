# TS

1.ts+react知识点

2.项目

3.学习代码

![image-20230511105815070](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20230511105815070.png)



[接口 · TypeScript中文网 · TypeScript——JavaScript的超集 (tslang.cn)](https://www.tslang.cn/docs/handbook/interfaces.html)

## type

```js
type Person = {
  name:string;
  age:number;
}

let person : Person = {
  name:"fatdove",
  age:21
}//如果person缺少type中定义的变量会报错
```



### 可选属性 ？

```
type Person = {
  name:string;
  age?:number;  //加上?.就算person缺少age就不会报错
}

let person : Person = {
  name:"fatdove",
}//如果person缺少type中定义的变量会报错
```



### 定义对象数组

type只对应单一元素，如果想要数组就加个[]

```js
type Person = {
  name:string;
  age:number;
}
let person : Person = {
  name:"fatdove",
  age:21
}

let persons:Person[] = [
  {
    name:"1",
    age:2
  }
]

```



## union

```
let age:string|number;
age = "xxx";✅
age = 1; ✅
```





## interface

```js
interface LabelledValue {
  label: string;
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

这里传入的myObj是有size的，但是labelledObj是接口定义的，只有label一个参数。

我发现这样将没有接口定义的myObj直接复制给有接口定义的labelledObj，就算多一个size变量，也是可以的，因为对于labelledObj来说只有label这个alias是必需的。

### 只读属性

```js
interface Point {
    readonly x: number;
    readonly y: number;
}
```

```js
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!❌
```

### 函数类型

接口定义函数的参数的数据类型和返回值的数据类型，结构如下

```
interface SearchFunc {
  (source: string, subString: string): boolean;
}
```

```js
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```

function的参数名字不一定非要和接口定义一样，函数内声明的数据类型也可以省略，ts会自动进行判断，如下：

```js
let mySearch: SearchFunc;
mySearch = function(src, sub) {
    let result = src.search(sub);
    return result > -1;
}
```





## 函数

### 函数类型

#### 为函数定义类型

```js
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { return x + y; };
```

匿名函数的定义：

```ts
const xxx = (xx:string):number=>{
	
}

```

***我们可以给每个参数添加类型之后再为函数本身添加返回值类型。 TypeScript能够根据返回语句自动推断出返回值类型，因此我们通常省略它。***

注意，如果函数体内并没有返回任何值，那么就不能省略返回值类型。必须显式指定返回值类型为`void`。



###  可选参数和默认参数

JavaScript里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是undefined。 在TypeScript里我们可以在参数名旁使用 `?`实现可选参数的功能。 比如，我们想让last name是可选的：

```js
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

let result1 = buildName("Bob");  // works correctly now
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");  // ah, just right
```

**可选参数必须跟在必须参数后面。**



### 剩余参数

```ts
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```





## 泛型

**泛型的意义就是，有时候我们传入的参数固定，但是参数的类型不固定，使用泛型可以提高函数的复用性和灵活性。**

我们定义一个identity函数，传入什么参数就返回什么参数

因此，我们需要一种方法使返回值的类型与传入参数的类型是相同的。 这里，我们使用了 *类型变量*，它是一种特殊的变量，只用于表示类型而不是值。

```js
function identity<T>(arg: T): T {
    return arg;
}
```

*我们给identity添加了类型变量`T`。 **`T`帮助我们捕获用户传入的类型**（比如：`number`），之后我们就可以使用这个类型。 之后我们再次使用了 `T`当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。 这允许我们跟踪函数里使用的类型的信息。*

使用时，第一种传入所有参数，包括类型参数

```ts
let output = identity<string>("myString");  // type of output will be 'string'
```

第二种，利用了类型推论，编辑器自动确定T的类型

```ts
let output = identity("myString");  // type of output will be 'string'
```



### 使用泛型变量

如果我们想同时打印出`arg`的长度。 我们很可能会这样做：

```ts
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  // Error: T doesn't have .length
    return arg;
}
```

*如果这么做，编译器会报错说我们使用了`arg`的`.length`属性，但是没有地方指明`arg`具有这个属性。 记住，这些类型变量代表的是任意类型，所以使用这个函数的人可能传入的是个数字，而数字是没有 `.length`属性的。*

*现在假设我们想操作`T`类型的数组而不直接是`T`。由于我们操作的是数组，所以`.length`属性是应该存在的。 我们可以像创建其它数组一样创建这个数组：*

```ts
function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}
//or~
function loggingIdentity<T>(arg: Array<T>): Array<T> {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}
```

第一种相当于泛型是基础的变量类型，number string等 第二种就是数组类型的泛型变量



### 泛型约束

```
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  // Now we know it has a .length property, so no more error
    return arg;
}
```

//如此继承之后，传入的变量必须包含lenth这个属性

```tsx
loggingIdentity({length: 10, value: 3});
```





# react&ts



## 声明函数式组件

```js
const App:React.FC = ()=> {
  return (
    <div className="App">
    </div>
  );
}
```



```js
export function getContract(
  address: string, // 
  ABI: any,
  library: any,
  account?: string,
): Contract {
  const AddressZero: string = '0x0000000000000000000000000000000000000000';
  if (!isAddress(address, true) || address === AddressZero) {
    throw Error(`Invalid 'address' parameter '${address}'.`);
  }

  return new ethers.Contract(
    address,
    ABI,
    getProviderOrSigner(library, account),
  );
}
```



## 变量的类型

```
const [todo,setTodo] = useState<string>();
```



## 函数式组件的泛型接口定义

React.FC<BaseInputType>用来定义props的数据结构，之所以和上文写的不一样是因为React.FC<BaseInputType>已经自动规定了props，不要再次声明

```js
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  // Error: T doesn't have .length
    return arg;
}
```



```ts
export interface BaseInputType {
  leftview?: any;
  rightview?: any;
  children?: ReactNode;
}

const BaseInputNumber: React.FC<BaseInputType> = (props) => {
	return (
    	<>xxx</>
    )
}
```



## import 接口要转换成type

```ts
export interface useWeb3Type {
  provider: any;
  web3: any;
  account: string;
  active: boolean;
  connect: (chain_id: number, wallet_type: string) => any;
  disconnect: () => void; //接口中定义函数
  loading: boolean;
  chainId: number;
}

import  { type useWeb3Type } from '@/store/Web3Provider';
```

这是因为interface只是声明了一个数据类型并没有生成一个数据类型，所以无法直接进行

```ts
const user:useWeb3Type
```

类似这种的赋值

所以我们引用的时候需要type转化

那我们声明interface直接使用的场景就是在定义泛型数据类型的时候，一般都是用interface这样更加灵活