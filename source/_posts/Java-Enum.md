---
title: Java Enum
date: 2019-10-10 18:18:19
tags: 
- java
---
# enum

> 关键字enum可以将一组具名的值的有限集合创建为一种新的类型，而这些具名的值可以作为常规的程序组件使用。

# enum的基本特性
创建enum时，编译器会生成一个相关的类，这个类继承自java.lang.Enum。

## 常用方法
### values()
返回enum实例的数组，用于遍历enum实例。这个方法不存在于java.lang.Enum。是编译器添加的一个静态方法。功能上与Class中getEnumConstants()方法相同。
### == 和equals()
相等比较
### compareTo()
实现Comparable接口
### name()和toString()
返回enum实例值的字符串表示
### valueOf()
根据名字返回enum实例

## java.lang.Enum
所有的枚举类型都会继承Enum,结构如下

{% asset_img Enum.png This is an example image %}


- name为枚举类型声明时的名字
- ordinal为枚举实例声明的次序，从0开始

## 自定义方法
enum可以看做是一种类。enum类型不能继承，因为枚举都继承自java.lang.Enum，也不能被继承，因为会被编译成final类。可以向enum类型中添加方法。

```java
public enum OzWitch {
    WEST("WEST description"),
    NORTH("NORTH description");

    private String description;

    OzWitch(String description) {
        this.description = description;
    }
    public String getDescription(){
        return description;
    }
    @Override
    public String toString(){
        return name().toLowerCase();
    }

    public static void main(String[] args){
        for (OzWitch ozWitch : OzWitch.values()){
            System.out.println(ozWitch);
            System.out.println(ozWitch.getDescription());
        }
    }
}
```
上面的例子为枚举类型修改了构造方法，同时添加了两个方法。其中构造方法不能是public。当然也可以覆盖枚举类型的方法。

# 实现接口
enum实例不能继承，但是可以实现接口。
# switch语句中使用enum
switch中只能使用整数值，而枚举实例本身就具有整数值的次序。

# 常量方法
实现常量方法，需要为enum定义abstract方法，然后为每个实例实现该抽象方法。

```java
public enum Shrubbery {
    GROUND {
        String getInfo() {
            return name().toLowerCase();
        }
    },
    CRAWLING {
        String getInfo() {
            return name().toLowerCase();
        }
    },
    HANGING {
        String getInfo() {
            return name().toLowerCase();
        }
    };
    abstract String getInfo();
    
    public static void main(String[] args){
        for (Shrubbery shrubbery : Shrubbery.values()){
            System.out.println(shrubbery.getInfo());
        }
    }
}
```
# enum类型编译
下面为enum Shrubbery编译之后的代码。编译之后为final类。继承自java.lang.Enum。Shrubbery中的值均为Shrubbery类型的static final 实例。编译器添加了两个方法，分别是values()和valueOf()

```java
public final class Shrubbery extends java.lang.Enum<Shrubbery> {
  public static final Shrubbery GROUND;
  public static final Shrubbery CRAWLING;
  public static final Shrubbery HANGING;
  public static Shrubbery[] values();
  public static Shrubbery valueOf(java.lang.String);
  static {};
}

```




