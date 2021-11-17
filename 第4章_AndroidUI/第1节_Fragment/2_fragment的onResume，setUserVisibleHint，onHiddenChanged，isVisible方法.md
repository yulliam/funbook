# fragment的setUserVisibleHint，onResume,onHiddenChanged三个方法的应用与之间的区别

要搞明白setUserVisibleHint 、onResume、onHiddenChanged之前，我们先回顾下fragment的其中两种加载方式：一，普通加载  二、懒加载，而懒加载的重要实现方式之一viewpager+fragment组合。

1、普通加载用的是hide()和show方法，因此与之关联的方法是onHiddenChanged()，而onUserVisibleHint()在此fragment中不会运行。

2、懒加载用于刷新数据的方法是setUserVisibleHint()，而onHiddenChanged()在此fragment中不会运行。

3、至于fragment中onResume()的用法。先看fragment的生命周期
![img](https://img-blog.csdnimg.cn/2019051622440877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpeHVlcWlhbmcwMDE=,size_16,color_FFFFFF,t_70)

fragment的onResume()方法跟activity的onResume()方法运行的节奏是一致的，activity是从不可见到可见，能实现人机交互，一定会运行onResume()方法。但是fragment由不可见到可见状态，能实现人机交互，但是却不一定会走onResume()方法，因此有时候fragment从不可见状态到可见状态时，想要数据刷新，只依赖onResume()是不可行的。于是有了onHiddenChanged()和setUserVisibleHint结合使用，才能达到我们想要刷新数据的目的。

总结，当fragment中的setUserVisibleHint()、onHiddenChanged()、onResume()其中一个方法“单纯”运行时，其他两种方法都不会再运行。