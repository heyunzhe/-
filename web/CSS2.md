## CSS盒子模型（Box Model）
* 概念：所有HTML都可以看作盒子，在CSS中,"box model"这一术语是用来设计和布局使用
* CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：外边距（margin）、边框（border）、内边距（padding）和实际内容（content）
1. Margin（外边距）：清除边框外的区域，外边距是透明的(两个值：第一个上下，第二个左右)
2. Border（边框）：围绕在内边距的内容外的边框
3. Padding（内边距）：清除内容周围的区域，内边距是透明的(两个值：第一个上下，第二个左右)
4. Content（内容）：盒子的内容，显示文本和图像
--------------
## 弹性盒子模型（flex box）
* 弹性盒子由弹性容器（Flex container）和弹性子元素（Flex item）组成
* **弹性容器通过设置display属性的值为flex将其定义为弹性容器**
* 弹性容器内包含了一个或多个弹性子元素
* PS：弹性容器外及弹性子元素内是正常渲染的，弹性盒子之定义了弹性子元素如何在弹性容器内布局，默认弹性盒里内容横向拜访
### 父元素上的属性
#### display弹性盒
* display:flex：开启弹性盒，开启后子元素默认水平排列
--------
#### flex-direction排列
* 指定了弹性子元素在父容器的位置
1. row：横向从左到右排列（左对齐），默认排列方式
2. column：纵向排列
--------
#### justify-content对齐方式
* 内容对齐属性应用在弹性容器上，把弹性项沿着弹性容器的主轴线（main axis）对齐
1. flex-start：弹性项目向行头紧挨着填充，默认值
2. flex-end：弹性项目向行尾紧挨着填充
3. center：弹性项目居中挨着填充
4. space-evenly：均匀排放每个元素，每个元素之间的间隔相等
5. space-around：均匀排放每个元素，每个元素周围分配相同的空间
6. spcae-between：均匀排放每个元素，首个元素放置于起点，末尾元素放置于终点
--------------
#### align-items对齐方式
* 设置或检索弹性盒子元素在侧轴（纵轴）方向上的对齐方式
1. flex-start：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界
2. flex-end：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界
3. center：弹性盒子元素在该行的侧轴（纵轴）上居中放置（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）
-----------------
### 子元素上的属性
#### flex权重
* 根据弹性盒子元素所设置的扩展因子作为比率（权重）来分配空间
* 默认为0，即如果存在剩余空间，也不会放大
* 如果只有一个子元素设置，那么按扩展因子转化的百分比对其分配剩余空间，0.1即10%，1即100%，超出按100%
----------
## 文档流
* 文档流是文档中可显示对象在排列时所占用的位置/空间
* 块元素自上而下摆放，内联元素，从左到右摆放
* 标准流里的限制非常多，导致很多页面效果无法实现
-----------
### 文档流产生的问题
1. 高矮不齐，底边对齐
2. 空白折叠现象
3. 无论多少个空格、换行、tab都会折叠为一个空格
4. 如果我们想让img标签之间没有空隙，必须紧密连接
--------
#### 脱离文档流
* 解决文档流所产生的问题
* 三种方式：
1. 浮动
2. 绝对定位
3. 固定定位
##### 浮动
* float属性定义元素在那个方向浮动，任何元素都可以浮动，left向左浮动，right向右
* 浮动以后使元素脱离的文档流
* 浮动只有左右浮动，没有上下浮动
------
###### 清除浮动
* 浮动副作用：浮动元素会造成父级元素的高度塌陷，后续元素会受到影响
* 解决方法
1. 父元素设置高度
2. 受影响的元素增加clear属性：clear: both;
3. overflow清除浮动：在父元素添加overflow: hidden;和clear: both;
4. 伪对象方式
-----------
##### 定位
* position属性指定了元素的定位类型
* 参数含义：
1. relative：相对定位
2. absolute：绝对定位
3. fixed：固定定位
* 其中绝对定位和固定定位会脱离文档流，设置定位后可以通过四个方向值：left、right、top、botto进行位置调整   
* PS：设置定位之后，相对定位和绝对定位是相对于具有定位的父级元素进行调整，如果父级元素不存在定位，则继续向上逐级寻找，直到顶层文档
* z-index属性设置元素的堆叠顺序，z-index的值的越大，顺序越靠前显示
------------
## CSS新特征
### 圆角
* border-radius
1. 赋1个值：四个圆角相同
2. 赋2个值：第一个值为左上角和右下角，第二个值为右上角与左下角
3. 赋3个值：第一个值为左上角，第二个值为右上角和左下角，第三个值为右下角
4. 赋4个值：第一个值为左上角，第二个为右上角，第三个为右下角，第四个为左下角
-------
### 阴影
* box-shadow：向框添加一个或多个阴影
* 格式：box-shadow: h-shadow v-shadow blur color;
1. h-shadow：必选，水平阴影位置
2. v-shadow：必选，垂直阴影位置
3. blur：可选，模糊距离
4. color：可选，阴影的颜色
----------------
## 动画
* 动画是使元素从一种样式逐渐变化为另一种样式的效果
* 您可以改变任意多的样式任意多的次数
* 请用百分比来规定变化发生的时间，或用关键词“from”和“to”，等同于0%和100%
* 0%是动画的开始，100%是动画的完成
### 使用@keyframes创建动画
* 格式
```html
@keyframes name {
    0%{
        css样式
    }
    50%{
        css样式
    }
    100%{
        css样式
    }
}
```
* 执行动画格式：animation: name duration timing-function delay iteration-count direction fill-mode;
1. name：设置动画名称
2. duration：设置动画持续时间
3. timing-function：设置动画效果的速率
* ease：逐渐变慢（默认）
* linear：匀速
* ease-in：加速
* ease-out：减速
* ease-in-out：先加速后减速
4. delay：设置动画的开始时间
5. iteration-count：设置动画循环的次数，infinite为无限次数循环
6. direction：设置动画播放的方向
* normal：默认值，表示向前播放
* alternate：动画播放在第偶数次向前播放，第奇数次向反方向播放
7. fill-mode：控制动画的播放状态，running代表播放，paused代表停止播放
-----------
## 媒体查询
* 媒体查询能使页面在不同的终端设备下达到不同的效果
* 媒体查询会根据设备的大小自动识别加载不同的样式
* 代码展示：
```html
    <style>
        .box{
            width: 300px;
            height: 300px;
        }
        @media screen and (max-width: 768px){
            .box{
                background-color: blue;
            }
            .p1{
                display: none;
            }
            .p2{
                display: none;
            }
        }
        @media screen and (min-width: 768px) and (max-width: 996px){
            .box{
                background-color: green;
            }
            .p1{
                display: block;
            }
            .p2{
                display: none;
            }
        }
        @media screen and (min-width: 996px) {
            .box{
                background-color: black;
            } 
        }
    </style>
<body>
    <div class="box"></div>
    <p class="p1">lalala</p>
    <p class="p2">hahaha</p>
</body>
```
* 在不同大小的屏幕上显示不同的效果
----------------
### 使用设备的宽度作为视图宽度并禁止初始的缩放
* 在`<head>`标签中添加 `<meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1,user-scalable=no">`
1. width = device-width：宽度等于当前设备宽度
2. initial-scale：初始的缩放比例（默认值为1.0）
3. maximum-scale：允许用户缩放到最大的比例（默认为1.0）
4. user-scalable：用户是否可以手动缩放（默认设置为no）
-----------
## 雪碧图
* CSS Sprite也叫CSS精灵图、CSS雪碧图，是一种网页图片应用处理方式，它允许你将一个页面涉及到的所有零星图片都包含到一张大图中去
* 优点：
1. 减少图片的字节
2. 减少网页HTTP请求，从而大大的提高页面的性能
* 原理：
1. 通过background-image引入背景图片
2. 通过background-position把背景图片移动到自己需要的位置  
* 代码展示
```html
    <style>
        .icon{
            display: block;
            width: 45px;
            height: 45px;
            background-image: url(image-15.png);
            border: 1px solid red;
            background-position: 0px 0px;
        }
        .icon{
            display: block;
            width: 45px;
            height: 45px;
            background-image: url(image-15.png);
            border: 1px solid red;
            background-position: -168px -220px; //移动位置定位图片
        }
    </style>
</head>
<body>
    <span class="icon"></span>    
    <span class="icon1"></span>
</body>
```
原图：![image-15](C:/Users/HYun/Desktop/文件/xxbj/image/image-15.png)
---------------
## 字体图标
### 优点：
1. 轻量性：加载速度快，减少http请求
2. 灵活性：可以利用CSS设置大小颜色等
3. 兼容性：网页字体支持所有现代浏览器，包括IE低版本
----------------
### 使用方法
1. 注册账号并登录
2. 选取图标或搜索图标
3. 添加到购物车
4. 下载代码
5. 选择font-class使用
----------------------
## 扩展
* 加滚动条overflow: scroll;
* calc()：各种单位换算，例如：height: calc(100vh - 100px);
* margin: 0 auto:左右平均分配，用于容器居中
* 透明度：opacity
* 单选按钮默认选中：checked
### 相对长度单位
* em 和 rem 分别相对于父元素和根元素的字体大小。
* vh 和 vw 分别相对于视口的高度和宽度。
-------------
