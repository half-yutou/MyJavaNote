# 面向对象（基础）

## 相关概念

**类**：自行创建的数据类型，包含**属性**和**方法**

**对象**：类中的一个**具体数据**

> 对比记忆：int 是开发者创建的数据类型，称为int类
>
> int 100 则代表100 是int类中的一个对象

**属性 == 成员变量 == filed**

```java
//声明类
class Cat {//类
    //属性
    String name;
    String color;
    int age;
    
    //类可以没有方法
}
//声明对象
Cat cat0 = new Cat();//cat0 即一个对象

//访问属性
int a = cat0.age; //用操作符 . 访问对象的属性
```



## 类和对象的内存分配机制

1. 栈：存放基本数据类型（局部变量）
2. 堆：存放对象（如Cat cat，数组等）
3. 方法区：常量池（存放常量，如字符串），类加载信息（属性信息 + 行为信息）

```java
Person p1 = new Person();//方法区先加载类信息（仅第一次）
						//堆 中分配空间，进行默认初始化
						//把该堆空间的地址赋给栈内的 p1 ,
p1.name = "xiaoming";
p1.age = 10;

Person p2 = p1;
/*此时p1和p2指向同一地址,即修改p2也会影响到p1*/
```



## 方法（成员方法）

**基本介绍** ：对象的行为用**方法**来执行（类似 C语言的函数）

```java
class Person {
    //类可以没有属性
    
    //方法（成员方法）来表示行为
    //类比C语言的函数
    //1.public表示方法公开
    //2.void：方法没有返回值
    //3.speak是方法名，()表示形参列表
    //4.{}方法体，调用方法时执行括号内代码
    public void speak() {
        System.out.println("i am john");
    }
}


//调用方法
Person p1 = new Person();
p1.speak();//用操作符 . 调用方法

```



**多个返回值** ：返回一个数组

```java
//通过返回一个数组实现多个返回值的效果
class Multi {
    public int[] get(int n, int m) {//注意此时方法的返回类型是 int[] int数组型
        int[] resArr = new int[2];
        
        resArr[0] = n + m;
        resArr[1] = n - m;
        
        return resArr;
    }
    
}
```

注：

1. 返回值可以是任意类型，但是**返回类型**和**return的值类型**必须**一致或兼容**
2. 方法不能嵌套定义（即方法内不能再定义方法）



## 方法调用机制

**单一方法的调用机制**

1. 程序执行方法时，立即开辟一个**独立的栈空间**；
2. 方法执行完毕 或 执行到return语句，就会返回；
3. 返回到调用方法的地方，该**独立方法栈**消失；
4. **主方法（main）**执行完毕后，**主方法栈**消失，程序也随之结束；



**类间方法调用机制**

1. 同一个类内的方法调用：直接调用

   ```java
   class A {
       public void SayHello {
           System.out.println("Hello");
       }
       
       public void SayHello_Hi {
           SayHello();				//此时直接调用了 SayHello 方法
           System.out.println("Hi");
       }
   }
   ```

2. 跨类的方法调用（A类调用B类内的方法）：通过对象名（B类的对象）调用

   ```java
   class B {
       public void SayOk {
           System.out.println("Ok");
       }
   }
   
   class A {
       public void SayHello_Ok {
           
           B b = new B();	//先创建 B的对象
           b.SayOk;		//再通过该对象调用B内的方法即可
           
           System.out.println("Hello");
       }
   }
   ```

3. 跨类调用还与**访问修饰符**有关，将在后续进阶内容讲解



## 方法的传参机制

**机制**

1. 基本数据类型 值传递
2. 引用数据类型 地址传递



**实例**

**克隆对象**：要求编写一个方法CopyPerson，可以复制一个Person对象并返回。注意新对象和原来的对象相互独立，只不过属性相同。

```java
class Person {
    String name;
    int age;
}

public Person CopyPerson(Person p) {
    
    Person pNew = new Person();
    /*不可直接 pNew = p,此举是将 p 的地址赋给 pNew,则两者仍是指向同一空间，并非独立*/
    pNew.name = p.name;
    pNew.age = p.age;
    
    return pNew;
}
```



## 方法重载

**概念介绍**：同一个类中，允许**多个同名方法**的存在，但要求==形参列表不一致==（个数，类型，顺序）

> 如：`System.out.println()` 可以输出不同的类型字符，实际上是多个同名的方法



**注意事项**

1. 方法名相同才能叫方法重载
2. 重载方法的返回值类型可以不同，但**不能以返回值不同**作为重载条件
3. **不能以参数名不同**作为重载条件



## 可变参数

**概念介绍**：Java中允许方法的形参个数是可变的，可以使得**多个功能相同但参数个数不同**的方法重写为					一个方法

**实例**

```java
public int sum(int... nums) { 
    /*
    int... 表示形参个数不确定，可以传入0-n个int类型的参数
    此时 nums 可以当作一个数组来操作，长度为传入参数的个数
    */
    int res = 0;
    for(int i = 0; i < nums.length; i++) {
        res += nums[i];
    }
    return res;
}
```



**注意事项**：

1. 可以传递一个数组给可变参数，因为可变参数实际上就是一个可变数组
2. 普通参数可以和可变参数一起放在形参列表，但是**可变参数必须放最后**
3. 一个方法只允许**有一个可变参数**

```java
public String ShowScore(String name, double... scores) {//可变参数仅一个且必须放最后
    double total = 0;
    for(int i = 0; i < scores.length; i++) {
        total += scores[i];
    }
    return name +"总分=" + total;
}
```



## 作用域

**基本概念**

1. 变量：Java中一般指**成员变量（属性）**和**局部变量**
2. 成员变量：即属性；局部变量：即在成员方法中定义的变量

**作用域分类**

1. 全局变量：即成员变量，作用域为**整个类**，且有默认值；
2. 局部变量：即方法内声明的变量，作用域为**方法内代码块**，**无默认值**，==故必须先赋值再使用==

```java
public cat {
    double weight;//全局变量，在整个 cat类 内都可以使用，且有默认值（此处为0.0)
    
    public void eat() {
        int times = 3;//局部变量，仅在 eat方法 内可以使用，且无默认值，必须先赋值再使用
    }
    
}
```



**注意事项**

1. 全局变量和局部变量可以重名，使用时遵循就近原则
2. 在同一个作用域中，两个变量不能重名
3. 全局变量还可以通过对象被其他类调用，而局部变量只能在方法内使用
4. 全局变量可以有修饰符，局部变量不可以有修饰符



## 构造器/构造方法

**概念介绍**

1. 构造器的本质是**类的一种特殊方法**，
2. 构造器可以完成对新对象的**初始化**(对象已经存在了)
3. 创建对象时，系统自动调用该类的构造器对对象完成初始化

**注意事项**

1. 构造器名必须和类名一致
2. 构造器没有返回值
3. 一个类可以有多个重载的构造器

```java
class Person {
    String name;
    int age;
    
    public Person(String pName, int pAge) {//构造器名与类名一致
        name = pName;
        age = pAge;
    }
    
    public Person(String pName) {//重载构造器
        name = pName;
    }
}
	

Person p1 = new Person("jack", 18);//调用构造器，完成对对象的初始化
Person p2 = new Person("John");
```



**补充说明**

1. 如果没有人为定义构造器，那么系统自动生成一个无参构造器

```java
class Dog {
    int age;
    
    /*系统自动生成如下默认无参构造器
    Dog() {
    
    }
    */
}

Dog dog = new Dog();//此处的括号实际上就是在调用该无参构造器
```



2. 一旦自己定义了构造器，便覆盖了系统默认的构造器，==就不能再使用无参构造器==！

   除非自行再定义一个无参构造器`public Dog(){}`



## this关键字

**概念介绍**

在方法内部，哪个对象调用了this，则该**this关键字**代表该对象，可用于：

1. 用于**区分**当前类的**属性和局部变量**
2. 调用本类的**方法**和**构造器**
3. 注意：若在本类中为找到对应属性或方法，则会向父类及更高类继续查找（详见super相关内容）

```java
/*1.用于区分同名属性和局部变量*/
class Dog {
    //属性
    String name;
    int age;
    //构造器
    Dog(String name, int age) {//形参列表直接使用属性的名称
        this.name = name;
        this.age = age;
        /*
        this.XXX 表示调用该类内对于的属性
        后面单独的 name 根据就近原则表示局部变量，age同理
        故1.用this调用了本类的属性；2区分了同名的属性和局部变量
        */ 
    }
}


/*2.用于调用本类方法*/
class Manner {
    
    public void f1() {
        System.out.println("f1");
    }
    
    public void f2() {
        System.out.println("f2");
        /*欲在f2()内调用f1(),两种方式*/
        
        /*第一种：类内直接调用*/
        f1();
        /*第二种：通过this调用*/
        this.f1();

        /*两种方式有所区别，具体在继承章节讲解*/
        
    }
}

/*3.用于调用构造器*/
/*此用法需注意以下细节
1.语法：this(参数列表)
2.用this调用构造器时，只能在构造器中使用(即只能在一个构造器内调用另一个构造器)
3.this语句必须放在构造器的第一条语句！！！
*/
class T {
    String name;
    int age;
    
    public T(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public T () {
        this("jack",100);//调用上述构造器，注意形参列表要一致
    }
    
    /*至于为什么只能在构造器中调用构造器，详见继承相关章节*/
    
}
```



# 面向对象（进阶）

## 包

**概念介绍** 

包可以用于存储不同的类

1. 实质上就是创建**不同的文件夹/目录**来储存**类文件**
2. 不同包内的类可以重名，二者互不影响



**命名**

命名规则：只能包含字母，数字，下划线，小圆点，且不能是数字开头，也不能是关键字或保留字

命名规范：com.公司名.项目名.业务模块名



**常用包**

1. java.lang.* //基本包，默认引入，不需要再次引入
2. java.util.*  //工具包，系统提供的工具包，如Scanner类	
3. java.net.* //网络包，网络开发相关
4. java.awt.* //java界面开发相关，GUI



**创建包和引用包**

```java
package com.example.mypackage//package 包名：将当前类打包
/*注意
1.一个类只能有一条package指令
2.该指令必须放在代码的第一句
*/
    
import java.util.Scanner; //只引用util包下的Scanner类
import java.util.*		  //引用util包下的所有类
/*更推荐使用第一种方式*/
```



## 访问修饰符

**概念介绍**

java提供四种**访问控制修饰符**，用于控制属性和方法的访问权限

1. public：公开，任何包、类都可访问
2. protected：受保护，对子类和同一个包的类公开
3. 无修饰符：默认，对同一个包的类公开
4. private：只有类本身可以访问，不对外公开



**注意事项**

1. 访问修饰符可以修饰类、方法、属性
2. 但是修饰类只可以用 **public**和**默认**



## ==OOP三核心==

### 封装

**概念介绍**

封装 即把抽象出的数据【属性】和对数据的操作【方法】封存在一起，数据被保护在内部，程序的其他部分只有通过被授权的操作【别的方法】，才能对数据进行操作



**封装的作用**

1. 隐藏实现细节：传参调用->得到结果（不关心具体实现）
2. 可以对数据进行验证，确保安全合理（在内部才可以改变数据，外部无法改变）



**实现步骤**

1. 属性私有化 private 【使得外界无法直接访问】
2. 提供一个公共的（public）set方法
3. 提供一个公共的（public）get方法

**案例**

要求编写一个程序逻辑，实现对人的名字，年龄，工资的管理

要求：名字在2-6个字符长度，年龄私有且范围在0-120之间，工资私有

```java
class Person {
    /*属性私有化*/
    private String name;
    private int age;
    private double salary;
 
    /*提供一个公开的setname方法，使得外界通过该方法修改name*/
    public void SetName(String name) {
        /*逻辑判断以确保数据正确*/
        if(name.length() >= 2 && name.length() <= 6){
            this.name = name;
        } else {
            System.out.println("请输入2-6个字符");
            this.name = "NULL";
        }
    }
    /*提供一个公开的getname方法，使得外界通过该方法得到name*/
    public String GetName() {
        return this.name;
    }
    
    
    /*提供一个公开的setname方法*/
    public void SetAge(int age) {
        if(age >= 0 && age <= 120){
            this.age = age; 
        } else {
            System.out.println("请输入0-120间的数字");
            this.age = 0;
        }
    }
    /*提供一个公开的getname方法*/
    public int GetAge() {
        return this.age;
    }
    
    
    /*salary的set和get同理，不再赘述*/
}
```



**封装与构造器**

如果使用构造器，即可绕过private的限制对数据进行不合理的赋值

解决方案是：**将set和get方法写入构造器**，而不是让构造器直接进行赋值

```java
public Person(String name, int age, double salary) {//这是一个构造器
    SetName(name);
    SetAge(age);
    SetSalary(salary);
}
```



### 继承

**概念介绍**

多个类有公共的属性或方法时，可以将公共属性和方法单独写成一个类，成为父类，然后其子类可以通过继承关键字**extends**继承父类的属性和方法，从而使得代码维护与拓展更加便利

子类可以再有子类

**实例**

定义**Student**父类，Pupil和Graduate子类，将公共信息写在父类中并继承到子类

```java
//父类
public class Student {
    public String name;
    public int age;
    private double score;

    public void setScore(double score) {
        this.score = score;
    }

    public double getScore() {
        return this.score;
    }
}


//Pupil子类
public class Pupil extends Student{//注意继承关键字：extends 父类名
    public void testing() {
        System.out.println("小学生" + name + "正在考小学数学");
    }
}


//Graduate子类
public class Graduate extends Student{//注意继承关键字：extends 父类名
    public void testing() {
        System.out.println("大学生" + name + "正在考大学数学");
    }
}

public class Extend_main {
    public static void main(String[] args) {

        Pupil pupil = new Pupil();
        Graduate graduate = new Graduate();

        pupil.name = "小明";
        pupil.age = 10;
        pupil.setScore(90.5);

        graduate.name = "大明";
        graduate.age = 20;
        graduate.setScore(80.5);

        pupil.testing();
        System.out.println(pupil.getScore());

        graduate.testing();
        System.out.println(graduate.getScore());
    }
}
```





**注意事项**

1. 父类的私有属性和私有方法不能在子类中直接访问，需要通过公共方法来访问

   > 类似于封装的作用

2. 子类创建时必须调用父类构造器，完成父类的初始化

3. 进行父类初始化时，默认总会去调用父类的无参构造器，且追溯到最高父类（Object）

4. **如果父类没有提供无参构造器，则必须在子类的构造器中用super关键字指定父类使用哪个构造器对父类完成初始化**

   > super语法后续讲解

5. 如果父类不能完成初始化，编译不会通过
6. java所有类都是Object的子类
7. java的继承机制是单继承，即一个子类只能有一个父类（当然，可以通过父类再继承父类来继承多个类）
8. 不要滥用继承，继承必须满足"is a"关系



**继承实质**

建立了子类与父类间的查找关系

即：

首先，子类与父类有同名属性或方法时不会冲突

其次，通过子类访问属性或方法时，遵循查找规则：

1. 首先看子类是否有该属性或方法
2. 若有，且**可以访问**，则返回子类信息，访问结束
3. 若没有，便查看父类有无此属性或方法
4. 递归访问，直到查找到对应属性或方法时返回

最后，注意有无属性和有无属性访问权限的区别，若存在该属性或方法但无返回权限，则不再向上查找并返回错误。





### 多态

多态，即同一个名字体现出多种状态

前提：两个类（对象）间存在继承关系



#### 方法的多态

重写和重载体现多态



#### 对象的多态（核心）

1. 一个对象的==编译类型==和==运行类型==可以不一致(披着羊皮的狼)
2. 编译类型在定义对象时就确定了，不能再该改变
3. 运行类型是可以变化的
4. 编译类型看定义时 = 的左边，运行类型看 = 的右边



**实例**

宠物喂养问题

```java
//Animal 类
public class Animal {
    public String name;

     public Animal(String name) {
         this.name = name;
     }
}

public class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }
}

public class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }
}

/************************************************************/

//Food 类
public class Food {
    public String name;

    public Food(String name) {
        this.name = name;
    }
}

public class Bone extends Food {
    public Bone(String name) {
        super(name);
    }
}

public class Fish extends Food{
    public Fish(String name) {
        super(name);
    }
}

/*******************************************/


public class master {
    public String name;

    public master(String name) {
        this.name = name;
    }
    /*
    1. animal 的编译类型是 Animal，可以指向Animal子类的对象
    2.food 的编译类型是Food，可以指向Food子类的对象
    */
    public void feed(Animal animal,Food food) {
        System.out.println(this.name +"喂" + food.name +"给" + animal.name);
        /*
        虽然在 Dog Cat Bone Fish类中没有 name属性，
        但是调用时会向上查找，看父类中有无name这个属性
        Animal，Food两个父类都有各自的name属性，
        故在子类中不写name属性也可以返回正确的名称
         */
    }
}

//主函数
public class main_function {
    public static void main(String[] args) {
        master jack = new master("jack");

        Dog dog = new Dog("大黄");
        Bone bone = new Bone("排骨");
        jack.feed(dog, bone);

        Cat cat = new Cat("小花");
        Fish fish = new Fish("鲤鱼");
        jack.feed(cat, fish);

    }
}

```



#### 向上转型

概念介绍

1. 本质：父类的引用指向了子类的对象
2. 语法：`父类类型 引用名 = new 子类类型（）`
3. 特点： 1.编译类型看左边，运行类型看右边

​				    2.可以调用父类中的所有成员（但要遵循访问权限） ，但是不能调用子类的特有成员

​					3.最终的运行效果看子类的具体实现（即仍然遵循查找规则，先看子类有无此方法，再向上查找）

```java
Animal animal = new Dog();//编译类型是 Animal ，运行类型是 Dog

animal = new Cat();//运行类型可以改变
```



#### 向下转型

概念介绍

1. 本质：将父类的引用强制转换为子类的引用
2. 语法：`子类类型 子类引用名 = （子类类型） 父类引用`
3. 特点：
   1. 使用新的子类引用便可以调用子类中的所有成员
   2. 只是强转了父类的引用，并不是强转了父类的对象
   3. 要求 父类引用指向的对象 必须是 目标类型的对象



```java
Animal animal = new Dog();//编译类型是 Animal ，运行类型是 Dog
/*此时是向上转型，animal引用 不能调用Dog类内的成员*/

Dog dog = (Dog) animal;//这样 Dog类型的引用dog 便会指向 Animal类型的引用animal 指向的对象
/*此时是向下转型，dog引用 便可以调用Dog类内的成员了*/


/*注意以下写法是错误的*/
/*
Cat cat = (Cat) animal;
animal 声明时是指向一个 Dog对象的，不能用一个 Cat类型的引用cat 来指向Dog类型的对象
*/

```

​	










## super关键字

**概念介绍**

1. super可以访问父类（或更高类）的属性，但是不能访问private属性

​	`super.属性名`

2. super可以访问父类（或更高类）的方法，但是不能访问private方法

​	`super.方法名（形参列表）`

3. super可以访问父类的构造器，但是注意：1.必须是子类构造器的第一句；2.只能有一句super指令

   ```java
   public B {//这是一个构造器
       /*默认存在这一句，表示调用父类的无参构造器*/
       //super();
       
       /*若手动添加super去访问父类构造器，则上述默认代码被删除*/
       super("jack");
   }
   ```
   
   

**便利与注意事项**

1. 使得父类由父类构造器初始化，子类由子类构造器初始化，分工明确

2. 当父类与子类中的成员有重名时，必须使用super访问父类中的成员

3. 如果没有重名，使用 super、this、直接访问 的效果是一样的（都遵循**查找逻辑**）

   > 但是，this与直接访问是从本类开始，一层层向上查找
   >
   > 然而，super是直接从父类开始，一层层向上查找





## instanceof关键字

**概念介绍**

1. 作用：使用instanceof关键字可以判断==对象的运行类型==是否为XX类型或XX的子类型
2. 语法：`bb instanceof AA` ，返回true或false





## 方法重写

**概念介绍**

若满足

1. 子类方法的==名称==、==形参列表==与父类方法完全一样
2. 子类方法的==返回类型==是父类方法返回类型的子类

则称为 **方法重写**



**注意事项**

1. 若子类方法名称和形参列表与父类方法一致，则返回类型必须是父类返回类型的子类
2. 方法重写时，子类方法不能缩小父类方法的访问权限
3. 属性没有重写一说，==属性的值看编译类型==



**重载和重写**

| 名称 | 发生范围 | 方法名   | 形参列表                     | 返回类型             | 修饰符                     |
| ---- | :------: | -------- | ---------------------------- | -------------------- | -------------------------- |
| 重载 |   本类   | 必须一样 | 个数类型或顺序至少有一个不同 | 无要求               | 无要求                     |
| 重写 |  父子类  | 必须一样 | 完全相同                     | 是父类返回类型的子类 | 子类不能缩小父类的访问范围 |



## ==动态绑定机制==

**概念介绍**

1. 调用对象方法的时候，该方法会和该对象的 ==内存地址/运行类型== 绑定
2. 调用对象属性的时候，没有动态绑定机制，哪里声明就在哪里使用



**实例**

```java
class A {
    public int i = 10;
    
    public int sum() {
        return getI() + 10;
    }
    
    public int getI() {
        return i;
    }
}


class B extends A {
    public int i = 20;
    
    public int getI() {
        return i + 10;
    }
}

//main中
A a = new B();
int test = a.sum;
/*test = 40*/
/*流程：
1.调用sum方法时，B类内没有，便向上寻找，在父类A中找到sum方法并执行
2.sum方法需执行getI方法，回到运行类型B类，找到了getI方法
3.执行运行类型B类内的getI方法，此时用到属性i，在B类调用方法则i选择B类的属性i
4.再继续完成sum方法
*/
```



## Object类

**常用方法**

1. equals

2. hashCode
3. toString

4. finalize



# 面向对象（高级）

## 类变量（静态变量）

**概念介绍**

作用：类变量即类对应的所有对象所共享的变量，任何一个对象去访问或修改时，都是对同一个变量进行操作

语法：`public static int count = 0` `修饰符 static 类型 变量名 = 初始值`

访问方式：`类名.类变量名`(推荐) 、`对象名.类变量名`(有时方便区分操作对象) -------遵守访问权限限制

起始时间：类变量是随着类加载而创建，所以即使没有对象实例也可以访问

​					随着类的消亡而消亡



## 类方法（静态方法）

**概念类似静态变量**

**适用环境**：

1. 如果我们希望不创建实例，也可以调用某方法（即当作工具来使用），这时使用静态方法非常合适

2. 往往将一些通用方法（如求和、遍历数组等）写成一个类（工具类）的静态方法

```java
class Stu {
    private String name;
    
    private static double fee = 0;
    
    public Stu(String name) {
        this.name = name;
    }
    
    public static void payFee(double fee) {
        Stu.fee += fee;
    }
    
    public static void showFee() {
        System.out.println("学费共计：" + Stu.fee);
    }
}

//主函数中
//创建两个学生对象，执行payFee函数

Stu jack = new Stu("jack");
Stu merry = new Stu("merry");
/*第一种方式：通过对象名调用类方法*/
jack.payFee(100);
merry.payFee(100);

/*第二种方式：通过类名调用类方法*/
Stu.shouFee();//输出300

```



**注意事项**

1. 静态方法中不允许使用和对象相关的关键字，如this和super
2. 静态方法只能访问静态变量和静态方法，普通方法既可以访问别的普通方法，也可以访问静态方法



## main方法

相关概念

```java
public class Hello{
    public static void main(String[] args) {
    	//遍历显示args 是如何传入的
       for(int i = 0; i < args.length; i++) {
           System.out println("第" + (i + 1) + "个参数 = " + args[i]);
       }
	}
}

//CMD区域
/*
javac Hello.java    	编译

java Hello				运行一
						结果一：无显示
						
Java Hello jack tom		运行二
jack tom				结果二：遍历运行时输入的字符串数组
*/
```

1. main()方法是由java虚拟机（JVM）调用的
2. 由于JVM和main()方法不在同一个类，故main()方法必须由public修饰
3. JVM执行main()方法时不需要创建对象，故main()方法是static方法
4. `String[] args` 在运行的主方法后面直接输入相关内容，则会被打包成字符串数组调用



## 代码块

1. 本质：相当于一种构造器的补充机制

2. 运用场景：多个构造器有重复的语句，则可以把重复的语句放入一个代码块

   ​					这样无论调用哪一个构造器，都会==首先==执行普通代码块内的语句

3. 语法：

``` java
[修饰符(选写)]{
    代码
};
/*
1.修饰符：static 和 无
2.有static修饰的称为静态代码块，无static修饰的成为普通代码块
3.大括号末的;可写可不写
*/
```

**注意事项**

1. 对于静态代码块

   1. 作用是对类进行初始化，==随着类的加载而被调用==

   2. 只会执行一次（一般情况下，类只会被加载一次)

   3. 类什么时候被加载？

      1. 创建对象实例时（new）
      2. 创建子类对象实例时，父类也会被加载（继承的本质：先加载父类，再加载子类）
      3. 使用类的静态成员时（静态属性、静态方法）

      

2. 对于普通代码块

   1. 对象被创建时被调用

   2. 使用类的静态成员时不会被调用（普通代码块跟类加载无关系）



3. 创建一个普通对象时，在一个类中调用顺序如下

   1. 首先 ==调用静态代码块==，==静态变量初始化==（二者同级，按定义顺序先后调用）
   2. 其次 ==调用普通代码块==，==普通变量初始化==（二者同级，按定义顺序先后调用）
   3. 最后 ==调用构造器==

   

4. 创建一个子类对象时（有继承关系），在一个类中的调用顺序如下[B extends A]
   1. 父类的==静态代码块==和==静态变量初始化==
   2. 子类的==静态代码块==和==静态变量初始化==
   3. 父类的==普通代码块==和==普通变量初始化==，构造器调用
   4. 子类的==普通代码块==和==普通变量初始化==，构造器调用

```java
   /*逻辑如下
   加载阶段
   先加载父类，此时调用父类的静态代码块，静态变量初始化
   再加载子类，此时调用子类的静态代码块，静态变量初始化
   
   
   因为子类构造器中先有super()，再有普通代码块，故
   
   
   构造阶段
   先构造父类，此时调用父类普通代码块，普通变量初始化，调用父类构造器
   再构造子类，此时调用子类普通代码块，普通变量初始化，调用子类构造器
   */
```



5. 静态代码块内只能调用静态成员，普通代码块可以调用任意成员





## 单例设计模式

**概念介绍**

所谓单例设计模式，就是采取一定的办法保证在整个软件系统中，对某一个类==只能存在一个对象实例==，并且该类==只提供一个取得其对象实例的方法==

单例模式有两种方式：1.饿汉式 2.懒汉式



### 饿汉式

**概念介绍**

在还未明确使用的情况下，随着类加载的进行该对象就已经被创建好了，故称为饿汉式



**步骤**

1. 将构造器私有化（外部不能再调用构造器）
2. 类的内部直接创建一个对象（为了能在静态方法中调用此对象，需要将其修饰为static）
3. 提供一个public static 方法，使该方法返回步骤2创建的对象

```java
class GirlFriend {
    private String name;
    /*构造器私有化，且在内部创建一个私有的对象*/
    private static GirlFriend gf = new GirlFriend("小红");
    private GirlFriend(String name) {
        this.name = name;
    }
    
    /*提供一个 public static 方法供外界调用，返回上述创建的对象*/
    public static GirlFriend getInstance() {
        return gf;
    }
}


//main函数中
/*外界调用Girlfriend类时，只能调用类内创建的那一个对象，不能再创建新的对象，从而实现单例设计*/
Girlfriend gf01 = Girlfriend.getInstance();
Girlfriend gf02 = Girlfriend.getInstance();
/*外界创建了两个索引，gf01和gf02，但实际上都指向了类内创建的那个对象*/
System.out.println(gf01 == gf02);//true
```



### 懒汉式

**概念介绍**

当需要使用到该对象时才创建



**步骤**

1. 将构造器私有化
2. 声明私有的静态变量，但不进行创建（new）
3. 提供一个 public static 方法，在需要时创建对象（new）或返回已经创建好的对象



```java
class Cat {
    private String name;
    private static Cat cat;//声明索引，但是不创建（new）
    private Cat(String name) {
        this.name = name;
    }
    
   	/*如果该对象未创建，则创建该对象再返回*/
    /*如果该对象已经创建，则直接返回*/
    public static Cat getInstance() {
        if(cat == null) {
            cat = new Cat("小猫");
        }
        return cat;
    }    
}
```



## final关键字

final 可以修饰 类、属性、方法、局部变量，使其变为常量，不可被继承、重写或修改



**注意事项**（类似于C语言const）

1. 一般用全大写字母和下划线命名 XX_XX_XX
2. 定义时必须赋初值，并且以后不能再更改，在如下任意一个位置赋初值即可
   1. 定义语句
   2. 构造器中
   3. 代码块中
3. 但如果是静态属性（类加载时就需要创建），则赋值只能在如下位置赋值
   1. 定义语句
   2. 静态代码块
4. 如果一个类不是final类，但是含有final方法，则这个类可以被继承且该方法可被子类调用，只不过不可重写该方法
5. final不能修饰构造器
6. final和static往往搭配使用，顺序可以呼唤，可以不经过类加载直接调用final成员，效率更高
7. 包装类（Integer,Double,Float,Boolean等)都是final类型，String也是final类型





**使用场景**

1. 当不希望类被继承时，可以用final修饰

```java
final class A {
    ~~~
}
class B extends A{}//报错，不可继承被final修饰的类
```

2. 当不希望父类的某个方法被子类重写时，可以用final修饰

```java
class C {
    public final void hi() {~~~}
}

class D extends C {
    @override
    public void hi() {~~~~~~} //报错，不能重写被final修饰的方法
}
```

3. 当不希望类的某个属性被修改，可以用final修饰

```java
class E {
    public final double TAXRATE = 0.08;
}

//主函数中
E e = new E();
e.TAXRATE = 0.09;//报错，不可更改final修饰的属性
```

4. 当不希望局部变量被修改时，可以用final修饰

```java
class F {
    public void cry() {
        final double NUM = 0.01;
        NUM = 0.09;//报错，不可更改final修饰的局部变量
        System.out.println("NUM=" + NUM);
    }
}
```



## 抽象类

**抽象方法**和**抽象类**

当父类需要声明一些方法但不具体确定如何实现时，可以将此方法声明为抽象方法，此时必须将抽象方法所在的类也声明为抽象类。抽象类会被继承，其子类会去实现抽象方法

**语法**

添加修饰符==abstract==即可

```java
abstract class Animal {//abstract修饰类，声明为抽象类
    private String name;
    int age;
    /*
    1. abstract修饰方法，声明为抽象方法
    2. 抽象方法即具体实现不知道的方法，故不需要有函数体
    3. 语句末尾需要分号
    */
    abstract public void cry();
}
```

**注意事项**

1. 抽象类不能被实例化，即不能new一个抽象对象
2. 抽象类不一定要包含抽象方法，但一旦声明抽象方法必须将对应的类声明为抽象类
3. 抽象类可以拥有任意成员【抽象类的本质还是类】
4. abstract只能修饰方法和类，不能修饰属性等
5. 如果一个类继承了抽象类，则它必须具体实现**所有**的抽象方法（被子类重写），除非它也声明为抽象类
6. 抽象方法不能被final，static，private修饰，因为这些关键字和重写相违背

**实例**

D:\Code_Java_Idea\OOP_STUDY\src\com\OOP\abstract_Study



## 接口

**概念介绍**

**接口**就是指给出一些没有实现的方法（抽象方法），封装在一起，当某个类需要使用到这个方法时，再根据具体情况把这些方法写出来。==在接口内写抽象方法可以省略abstract关键字==



**语法**

```java
interface 接口名 {
    //属性；
    //方法；  
 /* 现在接口内的方法可以有 静态方法、默认方法(加上default关键字)和抽象方法*/   
}

class 类名 implements 接口名 {
    //自己属性
    //自己方法
    //必须实现的 接口的抽象方法
}
```



**注意事项**

1. 接口不能被实例化
2. 接口中的属性都是 public static final
3. 接口中的方法都是public方法
4. 抽象类实现接口时可以不具体实现接口内的抽象方法
5. 一个类可以同时实现多个接口（此时所有接口内的具体方法都要实现）
6. 接口不能继承类，但是接口可以继承多个别的接口（extends而非implements）
7. 接口的修饰符只可以是 public 和 默认



## 内部类

**概念介绍**

一个类的内部又完整的嵌套了另一个类的结构，被嵌套的类称为**内部类**，嵌套的类称为**外部类**，和外部类平行的其他类称为**外部其他类**

==类的五大成员：属性、方法、构造器、代码块、内部类==

**基础语法**

```java
class Outer { //外部类
    class Inner {}//内部类
}

class Other {//外部其他类
    
}
```



**分类**

1. 定义在外部类的局部位置上（比如方法内）
   1. 局部内部类（有类名）
   2. 匿名内部类（无类名）

2. 定义在外部类的成员位置上
   1. 成员内部类（无static修饰）
   2. 静态内部类（有static修饰）



### 局部内部类

本质：局部变量 + 类

1. 局部内部类 是定义在外部类d局部位置，通常在方法或代码块内
2. 内部类可以直接访问外部类的所有成员，包括私有成员
3. 一个内部类可以继承平行位置的另一个内部类
4. 不能添加访问修饰符，但是可以用final修饰使得不能被继承
5. 外部其他类 无法访问局部内部类，因为局部内部类也是一个局部变量
6. 外部类和局部内部类重名时，遵循就近原则。如果想访问外部类的成员，可以使用`外部类名.this.成员`



### 匿名内部类

本质：类 + 无名字（系统取的，外部不显现）+ 一个对象

语法

```java
//在方法或代码块中
new 类或接口(参数列表) {
    类体
};
```

**实例**

1. 基于接口的匿名内部类

```java
/*
需求：某一个类如Tiger/Dog只使用一次，不希望将其单独写成一个类，此时可以用匿名内部类简化开发

*/

public class Anonymous {
    public static void main(String[] args) {//主函数
        Outer instance = new Outer();
        instance.method;//调用method，method调用Tiger.say(), 输出"匿名内部类"
    }
}


class Outer{
    public void method(){
        IA Tiger = new IA() {
           //本来接口是不能实例化的，但是此处意思并不是实例化一个接口
           //而是声明一个内部类，此内部类实现该接口，再实例化该内部类，最后返回地址给Tiger
          	//1.编译类型：IA
			//2.运行类型：匿名内部类【底层会分配一个类名(如Outer$1)给匿名内部类】
             @Override
             public void say() {
                System.out.println("匿名内部类");
            }
        };
        /*
        机制：在底层创建了一个类，并马上实例化，返回地址给Tiger
        底层代码：
        class Outer$1 implements IA {
            public void say() {
                System.out.println("匿名内部类");
            }
        };
        IA Tiger = new Outer$1();
        */   
        Tiger.say();//调用Tiger.say()
        }
    
}

interface IA {//接口
    public void say();
}

```



2. 基于类的匿名内部类

```java
public class Anonymous {
    public static void main(String[] args) {//主函数
        Outer instance = new Outer();
        instance.method;//调用method，method调用father.say(), 输出"匿名内部类重写了test方法"
    }
}


class Outer {
    public void method {
        Father father = new Father("Jack") {//注意此处的参数列表会传递给构造器
            @Override
            public void test() {
                System.out.println("匿名内部类重写了test方法");
            }
        };
        /*底层实质
        创建了一个匿名内部类，并马上实例化，返回地址给father
            class Outer$2 extends Father{
            @Override
            public void test() {
            System.out.println("匿名内部类重写了test方法");
            }
        }
        Father father = new Outer("Jack");
        */
        
        father.test();
    }
}

class Father {
    private String name;
    public Father(String name) {//构造器
        this.name = name;
    }
    public void test() {
        System.out.println("父类方法被调用");
    }
}
```



**注意事项**

1. 匿名内部类既是一个类，也可以看作是一个对象。故有两种调用方式

```java
class Stu{}
//1.第一种方式，返回地址再调用
Stu stu = new Stu() {
  public void say(){}  
};
stu.say();

//2.第二种方式，直接将其看作一个对象调用,不用再传地址给别的索引

new Stu() {
    public void say() {}
}.say;
```

2. 可以直接访问到外部类的所有成员，包括私有的
3. 不能添加访问修饰符，因为其实质是一个局部变量
4. 外部其他类是不能访问匿名内部类的
5. 外部类和匿名内部类重名时，遵循就近原则。如果想访问外部类的成员，可以使用`外部类名.this.成员`



### 成员内部类

**概念介绍**

成员内部类是定义在外部类的成员位置的内部类，并且没有static修饰

1. 可以添加任意访问修饰符，因为他的地位相当于一个成员
2. 可以直接访问外部类的所有成员，包括私有的
3. 外部类的其他成员可以调用成员内部类

```java
class Outer {
    private int num = 10;
    public String name = "张三";
    
    class Inner {//成员内部类
        public void say() {//内部类的一个方法
            /*内部类可以访问外部类的所有成员，包括私有的*/
            System.out.println("num = " + num + "name = " + name);
        }
    }
    
    public void useInner() {//成员内部类也可以被外部类的其他成员方法调用
        Inner inner = new Inner();
        inner.say;
    }
}
```



外部其他类如何实例化成员内部类？

```java
//1.通过外部类对象调用
Outer outer = new Outer();//先创建一个外部类对象
Outer.Inner inner = outer.new Inner();//通过外部类对象new Inner



//2.在外部类中编写一个方法，可以返回一个Inner类型的对象
public Inner getInner() {//外部类的方法
    return new Inner();
}

//外部其他类的操作
Outer outer = new Outer();
Outer.Inner inner = outer.getInner()   
```



### 静态内部类

**概念介绍**

静态内部类是定义在外部类的成员位置的内部类，并且有static修饰

1. 可以添加任意访问修饰符，因为其相当于一个静态成员
2. 只可以访问外部的所有静态成员，包括私有的



外部其他类如何调用静态内部类？

```java
//1.因为是静态成员，故可以直接用外部类名访问静态内部类

//2.在外部类编写一个方法，返回一个静态内部类的对象
```



# 枚举

**概念介绍**

枚举是一种特殊的类，里面只包含一组有限的特定对象（如四季，两性，12月，一周七天等）

## 自定义类实现枚举

```java
/*
1.将构造器私有化，防止外部new
2.去掉set相关方法，防止属性被修改
3.在类内部直接创建固定的对象
*/
class Season {
    private String name;
    private String desc;
    
    private Season(String name, String desc) {//构造器私有化
        this.name = name;
        this.desc = desc;
    }
    
    /*在类的内部直接创建固定的几个对象，可以加入final关键字进行优化*/
    public static final Season SPRING = new Season("春天"，"温暖");
    public static final Season SUMMER = new Season("夏天"，"炎热");
    public static final Season AUTUMN = new Season("秋天"，"凉爽");
    public static final Season WINTER = new Season("冬天"，"寒冷");
    
}
```



## enum关键字实现枚举

```java
/*
1.使用关键字 enum 替代 class
2.实例化时不需要再用new语句，直接使用  常量名(实参列表) 即可，多个常量之间用逗号分隔而不是分号
3.实例化的语句必须放在最前面
*/

enum Season {
    /*实例化语句必须放最前面*/
    SPRING("春天", "温暖"),
    //相当于：public static final Season SPRING = new Season("春天"，"温暖");
    SUMMER("夏天"，"炎热"),
    AUTUMN("秋天"，"凉爽"),//多个常量之间用逗号隔开
    WINTER("冬天"，"寒冷");//语句结束再使用分号
    
    /*属性和构造器紧随其后*/
    private String name;
    private String desc;    
    private Season(String name, String desc) {//构造器私有化
        this.name = name;
        this.desc = desc;
    }
    
}
```



**注意事项**

1. 使用enum关键字的类都会继承java.lang.Enum类，故不能再继承其他类（可以实现接口）

2. 简化的实例化语句`SPRING("春天", "温暖")`必须有与实参列表对应的构造器

3. 如果使用无参构造器创建枚举对象，那么小括号可以省略

   



## Enun常用方法

**以上述`enum Season`类为例子，在main函数中演示**

```java
public class EnumMethod {
    public static void main(String[] args) {
        Season spring = Season.SPRING;
      
        //1.name 直接返回枚举对象的名称(建议优先使用toString)
        System.out.println(spring.name());//SPRING
        
        //2.ordinal 返回枚举对象的编号（从0开始编号）
        System.out.println(spring.ordinal());//0(第一个)
        
        //3.values 返回一个枚举对象数组，包括所有的枚举对象
        Season[] value = Season.values();//注意是直接操作类名，返回一个数组
        for(Season season: value) {
            System.out.println(season);
            /*循环遍历所有枚举对象*/
        }
        
        //4.valueOf 将字符串转换为枚举对象，要求字符串必须为已有的常量名
        /*执行流程
        1. 根据你输入的字符串到枚举对象中去查找
        2.如果找到了就返回该对象，如果没找到就报错
        */
        Season autumn = Season.valueOf("AUTUMN");
        System.out.println(autumn);//{name = "秋天", desc = "凉爽"}
        
        //5.compareTo 比较两个枚举常量，比较的就是编号
        System.out.println(spring.compareTo(autumn));
        //return spring.ordinal() - autumn.ordinal()
    }
}
```



# 注解

**概念介绍**

1. ==注解(Annotation)==又被称为==元数据(Metadata)==,用于修饰解释 包，类，方法，属性，构造器，局部变量等数据信息
2. 和注释一样，注解不影响程序逻辑，但注解可以被编译和运行，相当于再代码中补充信息



## @Override

重写父类方法，该注解只能用于方法前

```java
class Father {
    public void fly() {
        sout("~~~");
    }
}

class Son extends Father {
    /*
    1.@Override注解放在fly()上，表示重写了父类的fly()方法
    2.如果没有写注解，依旧是重写了父类的fly()方法【注解不影响程序逻辑】
    3.如果写了注解，编辑器会先去检查该方法是否真的重写了父类方法
    若确实重写了，编译通过;否则，编译不通过【注解可以被编译和运行】
    */
    @Override
    public void fly() {
        sout("~~~~~~");
    }
}

/*
@Override的源码

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override{}

此处的 @interface 表示其是一个注解，而不是接口！
*/
```



## @Deprecated

```java
/*
1.@Deprecated 修饰某一个元素(包，类，方法，字段，属性)，表示该元素已经过时(过时不代表不能用)
2.不推荐使用，但是仍然可以使用
3.用于提示版本升级过渡使用
*/
```



## @SuppressWarnings

```java
/*
1.@Supppress 用于抑制编译器警告
2.可以在其后的({""})中输入想要抑制警告的代码范围
all 所有警告
unused 未使用警告
unchecked 为检查警告
rawtypes 泛型警告
......
3.作用范围和放置的位置相关，放在哪个方法前就管理哪个方法，甚至可以放到文件最前面管理所有方法
*/
```



## 元注解

**概念介绍**

修饰注解的注解，就称为元注解

**种类**

1. Retention 		指定注解的作用范围{SOURCE(源码)， CLASS(类)， RUNTIME(运行时)}
2. Target 		      指定注解能在哪些地方使用（包，类，方法，属性，字段等）
3. Documented   指定该注解是否会在javadoc体现
4. Inherited          子类会继承父类的注解





# 异常

## 异常体系图



## 常见运行异常

1. NullPointerException 空指针异常

2. ArithmeticException 数学运算异常
3. ArrayIndexOutOfBoundsException 数组越界异常
4. ClassCastException 类型转换异常
5. NumberFormatException 数字格式不正确异常



## 常见编译异常

后续学习到MySQL再补充





## 异常处理机制

一般地，==t-c-f机制==和==throws机制==二选一，

==运行异常==的默认机制为throws机制，但是==编译异常==必须手动处理异常



特殊地，最高级别的JVM异常处理机制为1.抛出异常信息2.终止程序运行



1. try-catch-finally

```java
try {
    //可能会抛出异常的代码
    //注意：如果异常发生了，则直接进入catch块，不会再执行异常语句后面的代码
}catch(Exception e) {
    //1.当异常发生时(无异常则不执行catch)，系统会将异常封装给到对象e，传递给catch
    //2.程序员可以选择在此处自行处理异常
}finally {//如果没有finally语法上也可以通过
    //1.无论try{}是否有异常发生，finally都会执行
    //2.故通常将释放资源的代码放到finally中
}


/*
1.如果try代码块可能有多个异常，可以用多个catch分别捕获不同的异常，进行处理
2.要求子类异常写在前面，父类异常写在后面(异常的父子类关系见异常体系图)
*/
```

2. throws

```java
//某个类的方法中
public void f1() throws FileNotFoundException {
    /*
    1.这里是一个FileNotFoundException,编译异常，其父类是Exception
    2.此处采用了throws机制抛出异常，让调用f1()方法的调用者(可能是另一个方法，如main方法)去处理
    3.throws后面的异常类型可以是对应的异常类型，也可以是他的父类
    4.throws后面的异常类型可以是一个 异常列表，即抛出多种异常
    */
    
    //创建了一个文件流对象，此处会出现文件异常
    FileInputStream fis = new FileInputStream("d://test.txt");    
}

/*注意事项
1.子类重写父类方法时，子类方法抛出的异常类型跟父类方法抛出的异常类型要么一致，要么也是其子类
2.在throws的过程中碰到了t-c-f机制，则相当于异常被处理，不必再向上抛出异常
```



## 自定义异常



# 常用类



## 包装类(Wrapper)

概念

针对八种基本数据类型相应的引用类型，称为包装类。有了类的特点，便可以调用类中的方法

| 基本数据类型 |   包装类    |
| :----------: | :---------: |
|   boolean    |   Boolean   |
|     char     |  Character  |
|     byte     |  ==Byte==   |
|    short     |  ==Short==  |
|     int      | ==Integer== |
|     long     |  ==Long==   |
|    float     |  ==Float==  |
|    double    | ==Double==  |

表中==高亮==的包装类的父类是Number类



### 包装类 <-> String类

以Integer和String转换为例

```java
//包装类->String类
Integer i = 10;

String Str1 = i + "";				//方式1
String Str2 = String.valueOf(i);	//方式2，使用valueOf方法
String Str3 = i.toString();			//方式3，使用toString方法


//String类->包装类
String Str = "1000";

Integer num1 = new Integer(Str);	//方式1，通过构造器创建
Integer num2 = Integer.valueOf(Str);//方式2，使用valueOf方法
```



### Character 常用方法

```java
Character.isDigit('a');
Character.isLetter('a');
Character.isUppercase('a');
Character.isLowercase('a');
Character.isWhitespace('a');

Character.toUppercase('a');
Character.toLowercase('a');
```











## String类

### 构造方式

``` java
//方式1 直接赋值
String Str1 = "Hello";

//方式2 调用构造器
String Str2 = new String("Hello");



//两种构造方式的区别是什么？
//方式1 栈中的索引直接指向常量池
//方式2 栈中的索引直接指向堆中的value数组，value数组指向常量池

String a = "Jack";
String b = new String("Jack");

//判断下列表达式的真假
//intern()方法返回常量池索引
a == b;			//F,a指向常量池中的"Jack"，b指向堆中的value数组
a.equals(b);	//T,equals()方法逐个比较字符串的各个字符
a == b.intern();//T,b.intern()也指向常量池中的"Jack"
b == b.intern();//F,一个指向value数组，一个指向常量池中的"Jack"
```



常用构造器

```java
String Str1 = new String();
String Str2 = new String(String original);
String Str3 = new String(char[] a);
String Str4 = new String(char[] a, int startIndex, int count);

String Str5 = new String(byte[] b);
```

注意事项

1. String类实现了==serializable接口==和==comparable接口==
2. String类是final类
3. String类有静态属性 private final char[] value, 用于存放字符串内容
4. 一定要注意：value是一个final类型，不可修改(==是地址不可修改，而不是里面的字符不可修改==)





### 常用方法

```java
/*第一组
1.equals(String str) 			区分大小写，判断字符串内容是否相同
2.equalsIgnoreCase(String str) 	不区分大小写，判断字符串内容是否相同
3.length() 						返回字符串长度
4.indexOf("a") 					获取字符或子串在字符串中第一次出现的索引，若无则返回-1
5.lastIndexOf("a") 				获取字符或子串在字符串中最后一次出现的索引，若无则返回-1
6.substring(int a, int n) 		从索引a开始截取至索引为(n-1)的字符，n可省略表示截取后续所有字符
*/




/*第二组*/
String test = "Hello";
/*
1.test.toUpperCase() 全部转为大写
2.test.toLowerCase()	全部转为小写

3.test.contact("Str1").contact("Str2") ...
拼接字符串：1.可连续拼接多个字符串；2.对原字符串无影响，该方法会返回一个新的字符串

4.test.replace("Str1","Str2")
替换子串：1.将test中的Str1全部替换为Str2；2.对原字符串无影响，该方法会返回一个新的字符串
*/

String pome = "锄禾日当午,汗滴禾下土,谁之盘中餐,粒粒皆辛苦";
/*
5.String[] SplitTest = pome.split(",");
分割字符串：1.将pome字符串以,为分割点分割为不同字串；2.对原字符串无影响，返回一个字符串数组
*/

String temp = "happy";
/*
6.char[] ChArray = temp.toCharArray();
转为字符数组：1.将temp字符串转换成一个字符数组；2.对原字符串无影响，返回一个字符数组
*/





/*第三组*/
String a = "Amiya";
String b = "Kerushi";

/*
1.a.compareTo(b)  返回 'A' - 'K'
比较字符串大小(ascll码大小),前者大返回正数，后者大返回负数，相等返回0
(1)在比较完较短字符串后能找出区别，则返回ascll码相减的结果
(2)若比较完较短字符串后找不出区别，则返回a.length() - b.length()
*/

/*
2.format()占位符格式化输出
*/

String name = "~~~";
int age = 18;
double score = 90.5;
char gender = "男";

String formatStr = "学生姓名%s 年龄%d 成绩%.2f 性别是%c";
String info = String.format(formatStr, name, age, score, gender);
sout(info);
```











## StringBuffer类

对String类的增强

**注意事项**

1. StringBuffer类 的直接父类是 AbstractStringBuilder
2. StringBuffer类 实现了 Serializable接口 
3. 在父类中有属性 char[] value 用于存放字符串内容
4. 与String类不同的是，该value数组并非final类型，故指向的value存放在堆中
5. StringBuffer类 本身是一个 final类



```java
String VS StringBuffer
    
1. String 保存的是字符串常量，每次修改String类时都会在常量池生成一个新的字符串常量，再让对象指向新的			常量，效率较低//private final char[] value   
2.StringBuffer 保存的是字符串变量，更新时直接在原字符串上进行修改即可//char[] value
```



### 构造方式

常用构造器

```java
StringBuffer sb1 = new StringBuffer();
//构造一个不带字符的字符串缓冲区，初始容量为16个字符

StringBuffer sb2 = new StringBuffer(int capacity);
//构造一个不带字符的字符串缓冲区，但是初始容量由capacity指定

StringBuffer sb3 = new StringBuffer(String str);
//构造一个字符串缓冲区，将其内容初始化为指定的字符串内容，此时sb3.value的总容量是 str.length + 16

StringBuffer sb4 = new StringBuffer(CharSequence seq);
//构造一个字符串缓冲区，包含与指定的seq相同的字符
```



### String <-> StringBuffer

```java
//String -> StringBuffer
String Str = "Hello";

//方式1 使用构造器
StringBuffer b1 = new StringBuffer(Str);

//方式2 用append()方法接收
StringBuffer b2 = new StringBuffer();//先构造一个空的sb类型
b2 = b2.append(Str);//使用append()方法接收




//StringBuffer -> String
StringBuffer sbChange = "edu";

//方式1 使用构造器
String Str1 = new String(sbChange);

//方式2 用toString()方法
String Str2 = sbChange.toString();
```



### 常用方法

```java
//append(String str) 增加
StringBuffer Str = "a";
Str.append("bcd").append("100").append(true);//abcd100true

//delete(int n, int m) 删除对应索引区间(左闭右开)
Str.delete(2,4);//ab100true [删除2和3号位元素]

//replace(int n, int m, String str) 把对应区间的元素替换成str
Str.replace(5, 9, false);//ab100false

//indexOf(String str) 查找子串第一次出现对应的索引，若无该子串则返回-1
int indexof = Str.indexOf("ab");//indexof == 0

//insert(int n, String str) 在索引为9的位置插入str,后续元素顺序后移
Str.insert(2, "kfc");//abkfc100false

//length() 获取长度
int length = str.length();
```















































































































































































































