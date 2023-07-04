dapp主要解决两个问题

1. 框架next 搭配ts https://juejin.cn/post/7021674818621669389

2. 状态管理工具 unstated-nex

3. 缓存和链上信息的暴露

4. 连接钱包 metamask  使用metamaskjshttps://docs.metamask.io/wallet/reference/rpc-api/

   连接方法

   断开方法

   切换链

   查询操作：查询余额 块高 链id等

5. 调用合约  使用ethers

   - 只读方法
   - 可写方法

6. svg使用

7. 国际化

8. antd组件

   

   

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

### 在自定义的hook中引入

```ts
import { createContainer } from 'unstated-next';
```

全局调用，不需要每次重新写provider

[React轻量状态管理库 unstated-next使用教程 - 简书 (jianshu.com)](https://www.jianshu.com/p/f5d0d777b523)

### 组件中引用

```js
import useWeb3Hook from '@/store/Web3Provider'
let {count} = useWeb3Hook.useContainer();
```



### 最外层统一封装自定义hooks的provider

把自定义hook引入放到models中

```ts
import React, { useEffect, useState } from "react";
import Header from "./Header";
import Footer from "./Footer";
import Head from 'next/head'
import useWeb3Hook from '@/store/Web3Provider'
import useStorage from '@/store/Web3Provider/storage'

// Pay attention to the sorting,
const models = {
  useStorage,
  useWeb3Hook,
};


function compose(containers: any) {
  return function Component(props: any) {
    return containers.reduceRight(
      (children: any, Container: any) => (
        <Container.Provider>{children}</Container.Provider>
      ),
      props.children,
    );
  };
}

const ComposedStore = compose(Object.values(models));


function Layout({ children }: any) {

  return (
    <>
      <Head>
        <meta name="viewport" content="initial-scale=1, width=device-width" />
      </Head>
      <div className="relative h-screen">
        <Header />
        <div id="content" className="w-full relative top-0 left-0">
          <ComposedStore>
            {children}
          </ComposedStore>
          <Footer />
        </div>
      </div>

    </>
  )
}

export default Layout;
```



# 网络信息、链信息以及存入缓存

## 配置可用的链信息

./netConfig

```ts
export const chains = [
  {
    name:'Fibo Chain',
    chainId: 12306,  // 1230  //12306 //十六进制 3012
    chainName: 'FIBO',
    rpcUrls: ['https://node.fibochain.org'],
    faucets: ["https://www.fibochain.org/drawdex"],
    explorers: [],
    infoURL: "https://www.fibochain.org/",
    network: "fibochain",
    networkId: 12306,  //12306
    nativeCurrency: {
      name: 'fibo',
      symbol: 'FIBO',
      decimals: 18,
      }
  
  }
]
```

## 导出链信息

index.ts

```ts
//引入定义好的支持链
import { chains } from "./netConfig";

interface BaseDataType{
  CHAIN_ID:number;
  Contract:string;
  NETWORK_URL:string;
}

const DataType:BaseDataType = {
  CHAIN_ID:12306,
  Contract:'',
  NETWORK_URL:'https://node.fibochain.org' //RPC
}


const {CHAIN_ID, Contract, NETWORK_URL } = DataType;



const config = {
  // env
  BaseLocale: 'zh-cn',  //默认语言
  // 
  DEFAULT_NETWORK_ID: 12306,  //默认网络id号（链id）
  DEFAULT_WALLET_TYPE: 'MetaMask', //默认连接钱包
  chains, // 支持链 
  WEBSITE: 'https://www.fibochain.org/',
  precision: 2,  //
  interestRate: 22.5 / 100,

  // 
  CHAIN_ID,
  NETWORK_URL,
  Contract,
};
export default config;

//暴露出 chainId 钱包type 以及rpcurl
```



## 设置缓存

storage

```ts
import { useState } from 'react';
import { createContainer } from 'unstated-next';
//链上网络配置
import config from '@/config'
const STORAGE_PREFIX = 'WEB_';  //加一个头部描述信息


//如果value不存在那么存入缓存，如果value存在那么取出key值对应的value
export function storage(key:string,value?:any){
  //传入value
  if(value!==undefined){
    localStorage.setItem(STORAGE_PREFIX+key,value);
    return ;
  }
  else {
    return localStorage.getItem(STORAGE_PREFIX+key);
  }
}


//设置默认值
const defaultStates:any = {
  //缓存中有信息读出来 没有的话使用config的默认值
  NETWORK_ID:storage('NETWORK_ID')??config.DEFAULT_NETWORK_ID,
  WALLET_TYPE: storage('WALLET_TYPE') ?? config.DEFAULT_WALLET_TYPE,
}


//hooks导出网络id，钱包种类，以及相应state的修改方法。
function useStorage(customInitialStates = {}){
  const initStates = Object.assign({},defaultStates,customInitialStates);
  const [networkId, setNetworkId] = useState<number>(initStates.NETWORK_ID);
  const [walletType, setWalletType] = useState(initStates.WALLET_TYPE);
  return {
    networkId: Number(networkId),
    walletType,
    setNetworkId: (payload: any) => {
      storage('NETWORK_ID', payload);
      setNetworkId(payload);
    },
    setWalletType: (payload: any) => {
      storage('WALLET_TYPE', payload);
      setWalletType(payload);
    },
  };
}

export default createContainer(useStorage);
```



# 连接钱包调用合约







# 封装合约调用







# 前置的验证（）