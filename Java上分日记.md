# Java上分日记



## 1. 一切从泪(类)开始

​		某一天，菜鸡对菜狗说，我们学Java吧！菜狗笑了笑，看着自己电脑上朴实无华的Visual Studio 2017，菜鸡也笑了笑，默默地下载了Eclipse。在经过一个学期过后，终于，菜鸡和菜狗一起买上了去往非洲的船票，听说那里信息不发达，一个Hello Wrold 也可以变成大佬。

---



前言：所有的知识都是在用到的时候，或者项目代码的时候再重点讲解和分析！



### 1.1 Java垫脚石



==两个基础命令==：

>javac  Hello.java        //编译代码
>
>java   Hello                //Java虚拟机运行代码



==添加注释==：

```java
public class HelloWorld {
   /* 多行注释的示例
    * _              _
    *| | _______   _(_)_ __
    *| |/ / _ \ \ / / | '_ \
    *|   <  __/\ V /| | | | |
    *|_|\_\___| \_/ |_|_| |_|
    */
    public static void main(String []args){
       // 这是单行注释的示例
       /* 这个也是单行注释的示例 */
       System.out.println("Hello Kevin，Hello Virtual Studio 2017"); 
    }
}
```



==Java 源程序与编译型运行区别==：

![java1.png](http://yanxuan.nosdn.127.net/9bcda8febded28aba9e4c8b75e743c70.png)

---



### 1.2 Java的家庭成员



Java语言提供了八种基本类型.六种数字类型（四个整数型，两个浮点型），一种字符类型，还有一种布尔型.

|  ==类型==  |     byte     |    short     |          int           |      long      |
| :--------: | :----------: | :----------: | :--------------------: | :------------: |
|  ==大小==  | 1字节,有符号 | 2字节,有符号 |     4字节，有符号      | 8字节，有符号  |
| ==默认值== |      0       |      0       |           0            |       0L       |
|  ==范围==  |   -128~127   | -32768~32767 | -2147483648~2147483647 | -2^63 ~ 2^63-1 |


|  ==类型==  |          float           |          double          |    boolean     |     char      |
| :--------: | :----------------------: | :----------------------: | :------------: | :-----------: |
|  ==大小==  |      单精度，4字节       |      双精度，8字节       |   1 **比特**   |     2字节     |
| ==默认值== |           0.0F           |           0.0d           |     false      |    \u0000     |
|  ==范围==  | 不能表示精确的值，如货币 | 不能表示精确的值，如货币 | 只有真假两个值 | \u0000~\uffff |



==附加：==

Java 5.0引入了枚举，枚举限制变量只能是预先设定好的值。使用枚举可以减少代码中的 bug。

例如，我们为果汁店设计程序，它将限制果汁为小杯、中杯、大杯。这就意味着它不允许顾客点除了这三种尺寸外的果汁。

```java
class FreshJuice {
   enum FreshJuiceSize{ Small, Medium, Big}
   FreshJuiceSize size;
}
 
public class FreshJuiceTest {
   public static void main(String []args){
      FreshJuice juice = new FreshJuice();
      juice.size = FreshJuice.FreshJuiceSize.Medium;
   }
}
```

---



### 1.3 Java常量

常量在程序运行时是不能被修改的，在 Java 中使用 final 关键字来修饰常量。

```java
final double PI = 3.1415927;
```

---



### 1.4 自动类型转换

转换从低级到高级：


> byte,short,char—> int —> long—> float —> double 

* 不能对boolean类型进行类型转换。
* 不能把对象类型转换成不相关类的对象。
*  在把容量大的类型转换为容量小的类型时必须使用强制类型转换。
* 转换过程中可能导致溢出或损失精度。
* 浮点数到整数的转换是通过舍弃小数得到。

---



### 1.5 隐含强制类型转换



1. 整数的默认类型是int。
2. 浮点型不存在这种情况，因为在定义 float 类型时必须在数字后面跟上 F 或者 f，浮点数默认是double类型。

---



### 1.6 变量类型



```java
public class Test{
    static int allClicks=0;    // 1.类变量  独立于方法之外的变量，用 static 修饰
    String str="hello world";  // 2.实例变量  独立于方法之外的变量，没有 static 修饰
    
    public void method(){
        //访问修饰符不能用于局部变量
        int i = 0;  // 3.局部变量  没有默认值 必须初始化
        //局部变量是在栈上分配的(栈帧知识)
    }
}
```



#### 1.6.1 局部变量

```java
public class Test{
    public void method(){
        int num = 0;
    }
}
```

```c
#include<stdio.h>
void func();
int main(){
    func();
    return 0;
}

void func()
{
    int num;
    printf("%d\n", num);
}
```



​		局部变量声明在方法、构造方法或者语句块中，Java的method函数里面的i相当于C语言里面func函数里面的num，只不过Java里面method函数是定义在Test类里面的，所以==必须==给一个初始值。

​		调用Test类里面的method方法，就是先创建一个Test类 `Test test = new Test()`，然后再调用 `test.method();`

其余的就和C语言调用函数一样了，调用函数的时候在栈上创建栈帧，创建局部变量，然后函数结束后，就销毁栈帧，销毁局部变量。 [具体参考C语言栈帧](https://blog.csdn.net/qq51931373/article/details/45372541)

---



#### 1.6.2 实例变量

```java
public class Variable{
    String str="hello world";  // 实例变量
 
    public void method(){
    }
}
```

* 实例变量声明在一个类中，但在方法、构造方法和语句块之外，跟着类走，类创建它就创建，类销毁它就销毁。
* 访问修饰符可以修饰实例变量。
* 实例变量对于类中的方法、构造方法或者语句块是可见的。一般情况下应该把实例变量设为私有。通过使用访问修饰符可以使实例变量对子类可见。
* 实例变量具有默认值。
* 实例变量可以直接通过变量名访问。但在静态方法以及其他类中，就应该使用完全限定名：ObejectReference.VariableName。

---



#### 1.6.3 类变量（静态变量）

```java
 public class Employee {
    //salary是静态的私有变量
    private static double salary;
    // DEPARTMENT是一个常量
    public static final String DEPARTMENT = "开发人员";
    public static void main(String[] args){
    salary = 10000;
        System.out.println(DEPARTMENT+"平均工资:"+salary);
    }
}
```

* 在类中以 static 关键字声明，但必须在方法之外。
* 静态变量除了被声明为常量外很少使用。
* 实例变量具有默认值。
* 静态变量可以通过：*ClassName.VariableName*的方式访问。(java文件名 + 变量的名字)
* 静态变量在第一次被访问时创建，在程序结束时销毁。

---



### 1.7 ==修饰符==



#### 1.7.1 访问修饰符

| 修饰符      | 当前类 | 同一包内 | 子孙类(同一包) | 子孙类(不同包)                                               | 其他包 |
| :----- | :----- | :------- | :------- | :------ | :----- |
|`public`   | Y      | Y      | Y      | Y     | Y      |
|`protected`| Y      | Y      | Y      | Y/N   | N      |
|`default`  | Y      | Y      | Y      | N     | N      |
|`private`  | Y      | N      | N      | N     | N      |

1. `default`使用默认访问修饰符声明的变量和方法，对同一个包内的类是可见的。

   接口里的变量都隐式声明为 **public static final**,而接口里的方法默认情况下访问权限为 **public**。

2. `private`被声明为 **private** 的方法、变量和构造方法只能被所属类访问，并且类和接口不能声明为 **private**。

   声明为私有访问类型的变量只能通过类中**公共的getter **方法被外部类访问。

   ```java
   public class Test {      //标准案例
      private String Age;
      public String getAge() {    //访问Age
         return this.format;
      }
      public void setAge(String Age) {   //设置Age
         this.Age = Age;
      }
   }
   ```

   

3. `public`被声明为 public 的类、方法、构造方法和接口能够被任何其他类访问。如果几个相互访问的 public 类分布在不同的包中，则需要**导入相应 public 类所在的包**。由于类的继承性，类所有的公有方法和变量都能被其子类继承。

   >  Java 程序的 main() 方法必须设置成公有的，否则，Java 解释器将不能运行该类。



4. `protected`