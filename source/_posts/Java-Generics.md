---
title: Java Generics
date: 2019-10-12 14:40:28
tags:
- java
---
# 泛型
解决代码泛化问题的一种手段（继承、接口等）。
泛型就是类型参数化。
设计上提供很多灵活性。
泛型、继承和接口，设计接口更加灵活。
类型安全，利用泛型可以在编译期间检查这类问题。避免用Object时，运行时才能发现类型问题。
使用时指定类型。

## 泛型解决什么问题？
泛型是解决代码泛化问题的一种手段。

代码复用，代码泛化是程序设计永恒的追求，希望同一份代码可以应用在多种情况，解决多种问题。

面相对象程序设计中，利用多态来解决代码复用问题，这里的多态是指通过继承、抽象类、接口等技术来实现代码复用。我们只需要针对父类、抽象类或者接口写代码，就可以应用于他们的子类或者实现类中。

多态可以解决很多问题，但是其中不可避免的问题是，在代码中，具体的类型信息丢失了，因为实际的类型在代码中向上转型为父类、抽象类或者接口了。所以当我们从这些代码中取回对象时，就需要进行强制类型转换。

强制类型转化会出现运行时问题，出错会因为严重的运行时异常，这在代码编译时无法检查。例如在java中，实现通用的集合，集合内存储的是Object，而当取回集合内的内容是，一定会强制类型转换到需要的类型，这部分在编译时是无法检查的。

如果可以把类型参数化，在使用集合的使用确定类型，这样，编译器可以进行类型检查，可以解决类型安全问题。这就是泛型技术的直接原因。


```java
//基于多态
public class A {
    private Object b;
    public void setB(Object b) {
        this.b = b;
    }
    public Object getB() {
        return b;
    }
}
--------------------------------------  
A a=new A();
a.setB(1);
int b=(int)a.getB();//需要做类型强转
String c=(String)a.getB();//运行时，ClassCastException

//基于泛型
public class A<T> {
    private T b;
    public void setB(T b) {
        this.b = b;
    }
    public T getB() {
        return b;
    }
}

A<Integer> a=new A<Integer>();
a.setB(1);
int b=a.getB();//不需要做类型强转，自动完成
String c=(String)a.getB();//编译期报错,直接编译不通过

```

# 语法
类型参数为非基础类型。基础类型可以使用包装类。

## 泛型类
```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
public class Generic<T>{ 
    //key这个成员变量的类型为T,T的类型由外部指定  
    private T key;

    public Generic(T key) { //泛型构造方法形参key的类型也为T，T的类型由外部指定
        this.key = key;
    }

    public T getKey(){ //泛型方法getKey的返回值类型为T，T的类型由外部指定
        return key;
    }
}
```
使用时，也可以不传参数类型。

## 泛型接口

```java
//定义一个泛型接口
public interface Generator<T> {
    public T next();
}
```
当实现泛型接口的类，未传入泛型实参时
```java
/**
 * 未传入泛型实参时，与泛型类的定义相同，在声明类的时候，需将泛型的声明也一起加到类中
 * 即：class FruitGenerator<T> implements Generator<T>{
 * 如果不声明泛型，如：class FruitGenerator implements Generator<T>，编译器会报错："Unknown class"
 */
class FruitGenerator<T> implements Generator<T>{
    @Override
    public T next() {
        return null;
    }
}
```
当实现泛型接口的类，传入泛型实参时
```java
/**
 * 传入泛型实参时：
 * 定义一个生产器实现这个接口,虽然我们只创建了一个泛型接口Generator<T>
 * 但是我们可以为T传入无数个实参，形成无数种类型的Generator接口。
 * 在实现类实现泛型接口时，如已将泛型类型传入实参类型，则所有使用泛型的地方都要替换成传入的实参类型
 * 即：Generator<T>，public T next();中的的T都要替换成传入的String类型。
 */
public class FruitGenerator implements Generator<String> {

    private String[] fruits = new String[]{"Apple", "Banana", "Pear"};

    @Override
    public String next() {
        Random rand = new Random();
        return fruits[rand.nextInt(3)];
    }
}

```

## 泛型方法
使用时不需要是声明类型参数，编译器可以通过函数签名推断。

相较于泛型类，可以优先使用泛型方法。

```java
/**
 * 泛型方法的基本介绍
 * @param tClass 传入的泛型实参
 * @return T 返回值为T类型
 * 说明：
 *     1）public 与 返回值中间<T>非常重要，可以理解为声明此方法为泛型方法。
 *     2）只有声明了<T>的方法才是泛型方法，泛型类中的使用了泛型的成员方法并不是泛型方法。
 *     3）<T>表明该方法将使用泛型类型T，此时才可以在方法中使用泛型类型T。
 *     4）与泛型类的定义一样，此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型。
 */
public <T> T genericMethod(Class<T> tClass)throws InstantiationException ,
  IllegalAccessException{
        T instance = tClass.newInstance();
        return instance;
}

```


# 核心问题

## 擦除
在泛型代码内部，无法获得任何有关泛型参数类型的信息。

无法通过反射取得类型信息。

可以不使用类型参数。

导致运行时instanceof，new无法使用。

不能创建泛型数组。

## Class

Class<T>类型标记。

```java
//返回类型参数占位符
Class.getTypeParameters()
//List<String>与List<Integer>是同一种类型。
```
## 类型限定
限定泛型类型，相当于与传统多态相结合，方便调用限定类型的方法。这里只能是extends，不能用super。

```java
<T extends <class> & <class>>
```

## 通配符

### 限定通配符
泛型类型必须用限定内的类型来进行初始化，否则会导致编译错误。
主要是解决由于类型擦除导致泛型类没法调用实际泛型类所拥有的方法。

```Java
//保证泛型类型必须是 T 的子类来设定泛型类型的上边界
<? extends T>
//来保证泛型类型必须是 T 的父类来设定类型的下边界
<? super T>

```
以上为赋值限制，下面是读写操作限制。
#### extends上界能读不能写

> List<? extends E>表示该list集合中存放的都是E的子类型（包括E自身），由于E的子类型可能有很多，但是我们存放元素时实际上只能存放其中的一种子类型（这是为了泛型安全，因为其会在编译期间生成桥接方法<Bridge Methods>该方法中会出现强制转换，若出现多种子类型，则会强制转换失败）为了安全，Java只能将其设计成不能添加元素。
虽然List<? extends E>不能添加元素，但是由于其中的元素都有一个共性--有共同的父类，因此我们在获取元素时可以将他们统一强制转换为E类型，我们称之为get原则。

#### super下届能写不能读

> 对于List<? super E>其list中存放的都是E的父类型元素（包括E），我们在向其添加元素时，只能向其添加E的子类型元素（包括E类型），这样在编译期间将其强制转换为E类型时是类型安全的,但是，由于该集合中的元素都是E的父类型（包括E），其中的元素类型众多，在获取元素时我们无法判断是哪一种类型，故设计成不能获取元素(获取类型只能是Object)，我们称之为put原则。

#### PECS
producer-extends,consumer-super。

所有的comparable和comparator都是consumer。

### 非限定通配符

```java
//用任意泛型类型来代替，因为泛型是不支持继承关系的，所以<?>很大程度上弥补了这一不足。
Generic<T>逻辑上可以理解为Generic<Integer>、Generic<String>等的父类。（实际上没有继承关系）
```
example：
```java
public class Test1 {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<Integer>();
        list.add(12);
        handle(list);
        List<Float> list1 = new ArrayList<Float>();
        list1.add(123.0f);
        handle(list1);
    }
    private static void handle(List<?> list) {
        System.out.println(list.get(0));
    }
}
```
但是这里可以不写泛型。

可以将List<?>当做List<XXX>的“父类”，虽然说泛型没有继承关系。
