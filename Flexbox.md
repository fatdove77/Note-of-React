### Flexbox

#### 概念

采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"

![img](https:////upload-images.jianshu.io/upload_images/12937961-586b94eb70389949.png?imageMogr2/auto-orient/strip|imageView2/2/w/563/format/webp)


 容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`



项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`

#### Flexbox属性

###### flex-direction

- `flex-direction`在RN中属性名称为`flexDirection`
   `flexDirection`属性决定主轴的方向（即项目的排列方向）

  ![img](https:////upload-images.jianshu.io/upload_images/12937961-e8b935e254ebde46.png?imageMogr2/auto-orient/strip|imageView2/2/w/796/format/webp)

  

  - row(默认值)
     主轴为水平方向，起点在左端
  - row-reverse
     主轴为水平方向，起点在右端
  - column
     主轴为垂直方向，起点在上沿
  - column-reverse
     主轴为垂直方向，起点在下沿

###### flex-wrap

- `flex-wrap`在RN中属性名称为`flexWrap`
   `flexWrap`属性定义，如果一条轴线排不下，如何换行。
   默认情况下，项目都排在一条线（又称"轴线"）上。

  ![img](https:////upload-images.jianshu.io/upload_images/12937961-c49c9288bbc94eeb.png?imageMogr2/auto-orient/strip|imageView2/2/w/798/format/webp)

  

  - nowrap(默认)
     不换行，超过轴线内容不显示
  - wrap
     换行，第一行在上方
  - wrap-reverse
     换行，第一行在下方

###### flex-flow

- ```
  flex-flow
  ```

  在RN中属性名称为

  ```
  flexFlow
  ```

  ```
  flexFlow
  ```

  属性是

  ```
  flex-direction
  ```

  属性和

  ```
  flex-wrap
  ```

  属性的简写形式，默认值为

  ```
  row|nowrap
  ```

  如:

  

  ```js
  flexFlow:"row|wrap"
  ```

###### justify-content

- `justify-content`在RN中属性为`justifyContent`
   `justify-content`属性定义了项目在主轴上的对齐方式

  ![img](https:////upload-images.jianshu.io/upload_images/12937961-977866ffa3bc0a4d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1066/format/webp)

  

  - flex-start(默认值)
     左对齐
  - flex-end
     右对齐
  - center
     居中
  - space-between
     两端对齐，项目之间的间隔都相等
  - space-evenly
     均匀排列每个元素,每个元素之间的间隔相等
  - space-around
     每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍

###### align-items

- `align-items`在RN中属性名称为`alignItems`
   `align-items`属性定义项目在交叉轴上如何对齐，具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下

  ![img](https:////upload-images.jianshu.io/upload_images/12937961-dfcee11625885b61.png?imageMogr2/auto-orient/strip|imageView2/2/w/617/format/webp)

  

  - stretch(默认值)
     如果项目未设置高度或设为auto，将占满整个容器的高度。
  - flex-start
     交叉轴的起点对齐
  - flex-end
     交叉轴的终点对齐
  - center
     交叉轴的中点对齐
  - baseline:
     项目的第一行文字的基线对齐

###### align-content

- `align-content`在RN中属性为`alignContent`

  ![img](https:////upload-images.jianshu.io/upload_images/12937961-4887c116b8eb9682.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp)

  

  - stretch(默认值)
     轴线占满整个交叉轴
  - flex-start
     与交叉轴的起点对齐
  - flex-end
     与交叉轴的终点对齐
  - center
     与交叉轴的中点对齐
  - space-between
     与交叉轴两端对齐，轴线之间的间隔平均分布
  - space-around
     每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍

###### flex-grow

- ```
  flex-grow
  ```

  在RN中属性为

  ```
  flexGrow
  ```

  定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大

  ![img](https:////upload-images.jianshu.io/upload_images/12937961-ecf496850b1c1df6.png?imageMogr2/auto-orient/strip|imageView2/2/w/802/format/webp)

如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

###### align-self

- align-self在RN中属性为`alignSelf`
   `align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。如果没有父元素，则等同于`stretch`

  ![img](https:////upload-images.jianshu.io/upload_images/12937961-c73ee71aecd90ae0.png?imageMogr2/auto-orient/strip|imageView2/2/w/743/format/webp)

  

  - auto(默认值)
     表示继承父元素的`align-items`属性
  - flex-start
     交叉轴的起点对齐
  - flex-end
     交叉轴的终点对齐
  - center
     交叉轴的中点对齐
  - baseline:
     项目的第一行文字的基线对齐
  - stretch
     如果项目未设置高度或设为auto，将占满整个容器的高度

###### self

- ```
  slef
  ```

  跟css略有不同

  - 正整数
     `flex:<positive number>`等同于`flexGrow:<positive number>,flexShrink:1,flexBasis:0`
  - 0
     组件尺寸由width和height决定，此时不再具有弹性
  - -1
     组件尺寸一般还是由width和height决定。但是当空间不够时，组件尺寸会收缩到minWidth和minHeight所设定的值

