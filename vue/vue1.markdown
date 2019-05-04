## vue 饿了么

mv*：

1. mvc
2. mvp
3. mvvm (vue，ng，react)

mvvm:

View  <--->  ViewModel   <---> Model

1. View:  视图   Dom（前端）
2. ViewModel:  连接视图和数据的中间件  （1）view和Model是不可以直接通讯的，所以通过viewmodel来通信  （2）viewmodel通常要实现一个observe观察者。当数据发生变化，viewmodel能够观察到这种数据的变化，然后通知到对应的视图做自动更新；当用户操作视图，viewmodel也能监听到视图的变化，然后通知数据做改动；即数据的双向绑定。
3. Model:  数据   javascript对象（前端）


