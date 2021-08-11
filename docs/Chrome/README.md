<!--
 * @Author: tangdaoyong
 * @Date: 2021-04-14 14:54:15
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-08-11 10:22:44
 * @Description: Chrome
-->
# Chrome

[Chrome 多进程](https://blog.csdn.net/weixin_42071117/article/details/104595132)

![Chrome 多进程](../Chrome多进程.png)

最新的Chrome浏览器包含了：`1个浏览器（Browser）主进程、1个GPU进程、1个网络（NetWork）进程、多个渲染进程和多个插件进程。`
我们逐个分析这几个进程的功能

* 浏览器进程：主要负责界面显示、用户交互、子进程管理，同时提供存储功能。
* 渲染进程：核心任务是将HTML、CSS和JavaScript转换为用户可以与之交互的网页，排版引擎Blink和JavaScript引擎V8都运行在该进程中，默认情况下，Chrome会为每个Tab标签创建一个渲染进程（浏览上下文组和同一站点对此有影响，后续会介绍）。出于安全考虑，渲染进程都是运行在安全沙箱模式下。
* GPU进程：GPU的使用初始是为了实现3D CSS的效果。随后网页、Chrome 的UI界面都选择采用GPU来绘制，这使得GPU成为浏览器普遍的需求。最后Chrome在其多进程架构上也引入了GPU进程。
* 网络进程：主要负责页面的网络资源加载，之前是作为一个模块运行在浏览器进程里面的，后来成为一个单独的进程。
* 插件进程：主要负责插件的运行，因插件容易崩溃，所以需要通过进程来隔离，以保证插件进程崩溃不会对浏览器和页面造成影响。