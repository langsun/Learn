###一、Java设计模式七大原则
#####设计模式的目的
1. 代码重用性 (即:相同功能的代码,不用多次编写)



1. 单一职责原则
4. 里氏替换原则




###2.接口隔离原则(Interface Segregation Principle)

#####基本介绍
客户端不应该依赖它不需要的接口,即一个类对另一个类的依赖应该建立在最小的接口上

#####问题图
类A通过接口Interface1依赖类B,类C通过接口Interface1依赖类D,如果接口Interface1对于类A和类C
<img src = "https://github.com/langsun/Learn/blob/master/zimage/DesignPatterns/DesignPatterns01.jpg" width = "450" height = "350">
#####解决图
将接口 Interface1 拆分为独立的几个接口(这里我们拆分成 3 个接口),类 A 和类 C 分别与他们需要的接口建立 依赖关系。也就是采用接口隔离原则

<img src = "https://github.com/langsun/Learn/blob/master/zimage/DesignPatterns/DesignPatterns02.jpg" width = "500" height = "250">
###3.依赖倒转(倒置)原则
#####基本介绍
1. 高层模块不应该依赖低层模块,二者都应该依赖其抽象


3. setter 方式传递

#####注意事项和细节

1. 低层模块尽量都要有抽象类或接口,或者两者都有,程序稳定性更好.





#####注意事项


















 
