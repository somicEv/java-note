# 类的继承

## 继承和多态
---
### 继承
**概念**：是对有着共同特性的多类事物，进行再抽象成一个类。这个类就是多类事物的父类。父类的意义在于抽取多类事物的共性。

**表示方式和特点**：  
*  在java中使用extends关键字来表示继承关系，并且java中不允许多继承，即**一个类只能继承一个类**；
*  子类不能访问父类的私有变量和方法；
*  子类继承了父类的所有的非私有变量和方法。 

**this和super:**
* 1、当出现同名的方法和变量时，可以用来区分属性和方法。this表示子类的方法，super表示父类的方法。

* 2、都可以用来调用类的构造方法，区别是super()调用的是父类的构造方法，this调用的是子类的构造方法，但是都要放在方法的第一行。
* 3、this和super都不能同时出现在同一个构造函数中。

* 4、this可以作为参数参与方法传递，当一个非静态方法没有任何参数时，程序会为他添加一个默认参数，这个默认参数就是this，代表调用当前方法的对象。

* 5、this表示的是一个对象的引用，而super只是java中的关键字。

### 多态
**概念**：允许不同类的对象对同一消息做出响应。方法的重载、类的覆盖正体现了多态。

**方法重载、重写**：
* 重载：
    1、发生在类内；
    2、是指两个具有相同名称但参数不同（类型、顺序、个数）的方法；

* 重写：
    1、发生在子类和父类之间；
    2、是指子类继承父类相同的方法（确保方法签名相同）后，方法的具体实现不同。重写要满足子类方法的返回值小于等于父类的方法；出现的异常要小于等于父类的方法；访问权限要大于等于父类；
    3、不能重写父类的私有方法。


## 类和对象的加载过程
---

基类代码：
```java
public class Base {
    public static int s;

    private int a;

    static {
        System.out.println("基类静态代码块 s:" + s);
        s = 1;
    }

    {
        System.out.println("基类实例代码块 a:" + a);
        a = 1;
    }

    public Base(){
        System.out.println("基类构造函数 a:" + a);
        a = 2;
    }

}
```
子类代码：
```java
public class Child extends Base{
    public static int s;

    private int a;

    static {
        System.out.println("子类静态代码块 s:" + s);
        s = 10;
    }

    {
        System.out.println("子类实例代码块 a:" + a);
        a = 10;
    }

    public Child(){
        System.out.println("子类构造函数 a:" + a);
        a = 20;
    }

}
```
测试代码：
```java
public static void main(String[] args){
    Child child = new Child();
}
```
### 类和对象的加载过程
1、首先，程序会将当前类的信息加载进内存。程序会在内存**方法区**中开辟一段空间存储类信息。

2、接着，程序会为类中的静态成员变量赋初始默认值。（数值型 -- 0 ，对象类型（自定义类型） -- null , char -- ‘\u0000’， 布尔类型 -- true）

3、然后会查找这个类是否有父类，如果有则将父类也加载进内存，在加载父类信息的同时，也会查找这个类是否存在父类，如果存在，则将父类加载进内存。

4、设置父子类关系。

5、执行类初始化代码。
>  - 定义静态变量时的初始化赋值语句。
>  - 先执行父类静态代码块，然后执行子类静态代码块。

6、类加载完成后，开始创建对象。   
 >*  分配内存：将引用对象存储在**栈**中，将对象实际引用的内容存放在**堆**中。
 >*  为所有实例变量赋初始默认值。
 >*  执行实例初始化代码：执行父类初始化代码块，执行父类构造函数；执行子类初始化代码，执行子类构造函数。

### 静态绑定与动态绑定
#### 1、静态绑定：在编译时就可以确定变量的类型。
>实例变量、private方法、静态变量和静态方法都是静态绑定的类型。

**实例变量**：实例变量一般是通过private修饰符修饰，所以只能在类内访问，访问的就是**当前类中的实例变量**；如果是public修饰的话，则访问的是**调用这个变量的对象**的实例变量。

**private、final方法**：私有方法在java中不予许继承的，所以只能在本类中访问，则访问的一定是本类中的private方法。final方法虽然允许继承，但**无法重写**所以访问的是父类的方法。

**静态变量**：静态变量也称作**类变量**，是只属于当前类的变量，一般是通过类名直接调用，调用的就是当前类中的静态变量，如果是通过该类的对象调用的话，调用的也是当前类的静态变量，因为静态变量在java中是该类的所有对象共同持有的。

**静态方法**：静态方法也称作**类方法**，是属于类的实例共有的方法。在类内调用的时候访问的是本类的方法，在类外调用的时候，访问的就是调用这个方法的对象的类的方法。

#### 2、动态绑定：指的是在运行时确定该调用什么方法。

**动态绑定的机制**：根据对象的实际类型查找要访问的方法，子类型中找不到要访问的方法，就去父类中查找。

**虚方法表**：虚方法表是在类加载时为每个类创建一张记录每个对象动态绑定方法及其地址的表，但一个方法只记录一次，如果子类重写了父类的方法，则记录子类的地址。

### 使用继承的优点和缺点
##### 优点： 
 * 可以让代码得到复用，并且便于管理。

##### 缺点： 
* 继承之后带来的多态性可能会导致父类方法失去原有的功能，例如：鸟类和企鹅，鸟类这个父类拥有fly()这个方法描述鸟类的飞行功能，而企鹅是不会飞的，但是企鹅这个子类重写了这个fly()方法，就导致用父类对象调用这个对象时，出现了不符合的现象。

* 子类在调用父类方法后，可能会出现因为父类方法修改而导致的错误；