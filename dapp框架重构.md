dapp主要解决两个问题

1. 框架next 搭配ts https://juejin.cn/post/7021674818621669389

2. 状态管理工具 unstated-nex

3. 连接钱包 metamask  使用metamaskjshttps://docs.metamask.io/wallet/reference/rpc-api/

   连接方法

   断开方法

   切换链

   查询操作：查询余额 块高 链id等

4. 调用合约  使用ethers

   - 只读方法
   - 可写方法

5. svg使用

6. 国际化

7. antd组件

   

   

# 框架next 搭配ts 

https://juejin.cn/post/7021674818621669389

## 泛型的用法

`IProps` 是一个自定义的接口（或类型），用于定义 `Child1` 组件的 props 的类型。通常，`IProps` 是在组件所属的模块或文件中定义的。通过在组件声明中使用 `<IProps>`，可以指定 `Child1` 组件的 props 必须符合 `IProps` 接口所定义的结构。

```ts
interface IProps {
  name: string;
  age: number;
}
const Child1: React.FC<IProps> = (props) => {
  // 在组件中可以使用 props.name 和 props.age
  // ...
}
```

这样，当使用 `Child1` 组件时，传入的 props 必须符合 `IProps` 接口所定义的结构，即包含 `name` 属性（字符串类型）和 `age` 属性（数字类型）。

使用泛型参数可以提供静态类型检查的好处，可以在编译时捕获错误和类型不匹配的问题，提高代码的可维护性和可靠性

**React.FC定义了他是一个jsx的组件，没有任何返回值**



## 定义一般函数式组件

```ts
import React from 'react'

const Home:React.FC = ()=> {
  return (
    <div>
      home
    </div>
  )
}

export default Home
```



## hooks

### useState

```ts
const [count, setCount] = useState<number>(1)
```

```ts
const [count, setCount] = useState<number | null>(null); 
```







## type or interface

https://juejin.cn/post/6978417573050187784

```ts
export interface IWeb3ProviderProps {
  count:number;
  provider:any;
  web3:any;
  account:string;
  active:boolean;
  chainId:number;
  connect:(chainId:number,wallerType:string)=>any;
  disconnect:()=>void;
  loading:boolean;
}
```



## nextjs 路由传递参数以及子路由的实现

传递参数

```ts
<Link href="/Blog/22222">
	跳转blog2
</Link>
//上面这种传递方式 需要写子组件[id].js用来接受参数 不然跳转的路径是空的
or
<Link href={`Blog?id=${23}`}>
	跳转blog2
</Link>
```

接受

```ts
import React from 'react'
import {useRouter} from 'next/router'
import BlogLayout from './BlogLayout';
  const router = useRouter();
  const {id} = router.query;

```



文件结构树

```
Blog
	--BlogLayout.tsx
	--index.tsx
	--[id].tsx
```





# 状态管理工具 unstated-next

```ts
npm install --save unstated-next
```

**为什么手写的hook不行，因为每次引入手写的hook都会重新初始化，而使用状态管理的不会，所以这就是必须要使用状态管理的原因**

引入

```
import { createContainer } from 'unstated-next';
```

全局调用，不需要每次重新写provider

[React轻量状态管理库 unstated-next使用教程 - 简书 (jianshu.com)](https://www.jianshu.com/p/f5d0d777b523)



# 缓存

storage是存储区块链基本信息的缓存hooks

```
import web3Storage from './Web3Provider/storage';

// Pay attention to the sorting,
const models = {
  web3Storage,
  Web3,
};

```

