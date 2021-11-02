---
title: 关于Java的一些困惑
layout: post
typora-root-url: ../
---

# 关于Java的一些困惑

## 重载对具有继承关系的参数会调用哪一个

会根据传入的参数的声明类而不是多态式调用实际实现类

```java
class Father{}
class Child extends Father{}
class OverloadClass{
    public void test(Father father) {
        System.out.println("in father");
    }

    public void test(Child child) {
        System.out.println("in child");
    }
}
public class OverloadTest {
    public static void main(String[] args) {
        OverloadClass overloadClass = new OverloadClass();
        Father father = new Father();
        Child child = new Child();
        Father fOfChild = new Child();
        overloadClass.test(father);
        overloadClass.test(child);
        overloadClass.test(fOfChild);
    }
}

/*  --->
in father
in child
in father
*/
```



