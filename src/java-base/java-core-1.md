# java 基础问题

## 类的继承和实现
    一.抽象类特点:
    1.抽象类和方法都用abstact关键字修饰
    2.抽象类不能创建实例(及new() 对象)
    3.只有覆盖了抽象类所有的抽象方法后,其子类才可以实例化,否则该子类还是一个抽象类.
   
    二.抽象具体问题:
    1.抽象类有构造函数
    2.抽象类是否可以不定义抽象方法?答是可以的,意义,仅仅是不让该类创建对象
    3.抽象关键字不可以和哪些关键字共存:1.final 2.private 3.static
    
 ## final 修饰类,方法,变量
   1.final 修饰类:不能实例化(创建对象)
   2.final修饰方法:修饰的方法不被其子类重写.但是可以重载,还可以被继承
   3.final修饰变量:
   * final修饰变量:变量变为常量,不在改变(初始化后就不会被改变)
   * final修饰引用变量:始终指向一个对象的地址