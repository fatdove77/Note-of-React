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





