# 一、c#基本知识  

## 1.1 基本数据类型
  数据类型有以下几类
- 值类型
  - 结构体(int32,int64,double等)
  - 枚举  
- 引用类型
  - 类  
  - 接口
  - 委托

## 1.2 变量
变量表示存储位置，其用途也是存储数据  
广义变量的分类：  
- 静态变量
- 实例变量
- 数组元素
- 值参数
- 引用参数
- 输出形参
- 局部变量

## 1.3 修饰符  
- 访问修饰符：public、private、protected(只能在当前类及其子类中访问成员)、internal(只能在当前程序集(项目)中访问成员)等
- 继承修饰符：sealed(禁止重写)、abstract(必须被重写)、virtual(可写可不写)等
- 静态修饰符：static(静态，表示成员属于类而不是对象，例如MATH.PI)、const(定义常量，不能修改)、readonly(成员只能被构造函数或者定义的时候初始化)等
- 其他修饰符：unsafe(用于指针)、async、new(创建对象)等

## 1.4 命名空间  
命名空间限定你所编写程序的变量的作用域，引用命名空间时就可以使用命名空间中的类和方法，相当于python导入包。  


## 1.5 方法参数  
方法参数有以下几种：  
- 值参数，只传值，不传地址。
  - 传入值类型参数时，相当于创建了一个副本，外部变量的值不会被方法修改。  
  - 传入引用类型参数时，外部变量的值会被方法修改(副作用，要避免)。  
- 引用参数，传入的是地址。
  - 传入值类型参数时，外部变量的值会被方法修改。
  - 传入引用类型参数时，相对于c语言当中的指向指针的指针，指向一个地址，该地址指向该引用变量。  
  - 传入参数时，需要加ref，定义时也需要。
- 输出参数 (out)，目的是为了输出
  - 该参数传入的时候可以没有初始化，也就是没有赋值。

## 2 类
### 2.1 字段和属性  
字段和属性都属于类的成员变量，  
- 可以通过属性来访问和设置字段，相当于对类的字段进行保护，以避免一些异常值。
- 字段和属性都可以用修饰符来修饰，也即可以限定为只读。
  - **字段限定只读的方式：readonly关键字**
  - 属性限制只读的方式：
    - public get，private set，只能在类的内部通过方法来改变属性的值，外部不可以进行修改，但并不是完全只读。
    - public get，没有set，但是有类内的方法来更改属性对应的字段，也不是完全只读。
    - 只有get没有set，也没有方法来设置属性，那么就是完全的只读。
### 2.2 计算属性：
- **属性也可以通过动态计算得到**，例如知道一个人的年龄(属性)，想要知道一个人是否可以上班(bool 属性)，就可以通过年龄计算得到属性。
- 设置构造器，**直接在构造器的setter和getter中对属性进行计算**，
  - 优点：每次调用的时候就会计算该属性
  - 缺点：如果调用比较频繁就会浪费计算资源
- 设置构造器，**仅仅有getter，但通过方法对计算属性进行赋值(成为了只读属性)**
  - 优点：仅仅在初始化或者发生改变的时候就进行计算，相比于上面的方法节省了计算资源
  - 缺点：方法会占用内存，浪费内存资源
### 2.3 类的声明
- **类的声明默认为internal**，只能在该名称空间内访问。
- 类的修饰符：public、protected、internal、private、sealed、abstract、static

### 2.4类的继承与派生
重要概念：是一个  
#### 2.4.1 类的继承
- 子类一定是一个父类，而父类不一定是一个子类，就如骑车一定是一个交通工具，但是交通工具不一定是一个汽车。
  - 此外，父类的变量可以指向子类的实例对象，例如 Object obj = new Car();但是这个项目中比较少见。
  - 因为car一定是一个object，所以obj可以指向Car的实例。但是只能obj变量只能调用Object 的方法和属性，除非子类重写才可以调用被重写的属性和方法。
  - 实例化子类时，一定会先调用父类的构造器。如果父类的构造器含参数，那么可以在子类上使用base(args)让子类构造器给父类传参。
  - 但是父类的实例构造器不能够被子类所继承。
- 子类只能有一个一个基类，也就是只能有一个父类，但是可以有多个基接口。
- 子类的访问级别不能够超过父类的访问级别，例如父类是internal，子类就不能是public。
- 继承就是让一个类在基类的基础上，使类的成员进行横向和纵向的扩展。
- 限制类的成员访问级别的一些关键字,限制级别由低到高：
  - public：**项目文件都可以访问**
  - internal：**仅限于本项目文件中可以访问**，其他项目就不可以访问(引用了名称空间也不行)。
  - protected：**限制在继承链上，子类才能访问**。但是可以跨程序集，也就是说不是internal的。**更多应用在方法上**
  - private：**仅限于这个类中可以访问**，但是不能其他类中访问。
举一个例子说明：人在开车时肯定不能够直接接触到油箱，只能通过加速、减速、行驶、加油等操作改变油的量。字段设置为private，属性设置为protected，然后可以让子类(宝马A6)也继承父类(机动车)的方法，但是实例对象不能直接改变属性或者字段。
- 限制类的继承问级别：
  - sealed：隐藏的，不可以被继承

 

# 二、项目文件基本知识
- 一个解决方案可以由多个项目组成，每一个项目又可以被多个解决方案同时引用。
- 项目右键点击可以选择该项目生成库还是生成可执行文件，也可以配置路径，一般是项目名\bin\debug。
- 
## 1.1 快捷操作
注意有的时候快捷键会发生冲突，这时候就需要重新设置。  
- 代码重构：ctrl+r 然后ctrl+r(rename),+i(interface),+e(封装字段)
- 代码快捷生成：prop、propfull生成类属性，indexer生成索引器
- tab键可以快捷生成和补全代码
- 注释：ctrl+k 然后 ctrl+/
- 全局代码格式化：ctrl+k 然后 ctrl+d


