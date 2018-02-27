# 浏览器内核和JavaScript单线程



###  Pages 3

- **Home**
- **Require.js使用**
- **浏览器内核和JavaScript单线程**

##### Clone this wiki locally

 Clone in Desktop

### JavaScript的单线程

之前一直听说过说JavaScript是单线程的，最近看到了require.js这样的基于AMD的异步模块加载框架，所以有点疑问，经过这两天有了一些了解。

#### JS的任务队列

------

首先JS确实是单线程语言，浏览器只分配给JS一个主线程，用来执行任务（函数）,JS有一个任务队列，存放需要执行的任务，JS主线程从里面按顺序拿 出任务来执行。

#### 浏览器内核

------

简单来说浏览器内核是通过取得页面内容、整理信息（应用CSS）、计算和组合最终输出可视化的图像结果，通常也被称为渲染引擎。从上面我们可以知道，Chrome浏览器为每个tab页面单独启用进程，因此每个tab网页都有由其独立的渲染引擎实例。

#### 浏览器是多线程的

------

那么既然JS是单线程的为啥还会有多个任务的情况产生呢？ 这是因为虽然JS是单线程的，但是浏览器不是单线程的在JS主线程执行的时候，浏览器还会有其他线程在执行，比如鼠标事件的监听线程，网络请求线程 这些线程会产生不同的任务，然后放到JS的任务队列中

#### 浏览器的线程

------

一个浏览器内核内，通常含有一下常驻线程

- GUI 渲染线程 :当页面需要重绘的时候回启动这个线程
- JavaScript引擎线程 :负责解析运行js代码
- 定时触发器线程 ：setTimeout等
- 事件触发线程 ：鼠标点击等
- 异步http请求线程：ajax

#### 线程之间的关系

------

首先一点就是JS线程和GUI渲染线程是冲突的，互斥的，在文档加载过程中遇到JS文件，或者JS代码，浏览器就会挂起页面渲染线程，加载JS文件，并解析JS代码，解析完成之后才可以恢复渲染。这个造成的问题就是，加载的时候白屏问题。

#### JS问什么要是单线程的

------

这是因为Javascript这门脚本语言诞生的使命所致：
JavaScript为处理页面中用户的交互，以及操作DOM树、CSS样式树来给用户呈现一份动态而丰富的交互体验和服务器逻辑的交互处理。如果JavaScript是多线程的方式来操作这些UI DOM，则可能出现UI操作的冲突； 如果Javascript是多线程的话，在多线程的交互下，处于UI中的DOM节点就可能成为一个临界资源，假设存在两个线程同时操作一个DOM，一个负责修改一个负责删除，那么这个时候就需要浏览器来裁决如何生效哪个线程的执行结果。当然我们可以通过锁来解决上面的问题。但为了避免因为引入了锁而带来更大的复杂性，Javascript在最初就选择了单线程执行。
