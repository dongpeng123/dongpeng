# Java技术总结
  - 语法总结，
  - 面向对象总结，
  - 线程总结，
  - 输入输出流总结，
  - 集合总结，
  - 反射总结，
  - 算法总结
  
##  语法总结
  - break, 终止并跳出本次循环.
  - continue, 结束本次循环进行下次循环.
  
## 面向对象总结
  - 封装，继承，多态
    - 封装-------对一个事物封装起来,不让外界知道内部实现的细节，
    - 继承-------讲的类与类之间关系，一个类继承另一个类，在无序修改原有类的基础上，进行扩展，优势是 增强了代码的复用性，提高开发效率
    - 多态-------- 一个类中定义的属性和实现类，被其他类继承之后可以表现出，不同的行为和表达方式
## 线程总结
  - java中的线程有两种方式， 即继承Thread和实现 Runnable接口,由于java中是单继承 一个类继承Thread，将无法在继承其他类,顾java 为了避免这个局限性，提供了Runnable接口
## 5.1 集合总结
  - 集合分为单列集合（collection） ，和 双列集合（map）
    - 单列集合collection 有两个跟接口 list 和 set
      - list有两个实现类分别是： ArrayList 和 linkedList 特点是元素有序 元素可重复 
        - ArrayList内部封装的是数组，查询数据比较快
        - LinkedList 内部封装的是双向链表，即当前数据通过引用的方式同前一个对象和后一个对象关联起来，这样对增删改数据比较高效
      - set有两个实现类分别是： HashSet 和 TreeSet ，特点是元素无序，元素不可重复。
        - HashSet内部通过哈希值进行查找数据
        - TreeSet是二叉树的形式存数据
    - 双列集合map ，有两个实现类分别是 HashMap 和 TreeMap
      - HashMap呈现的是key 和 value 键值对映射的关系
## 其他知识点总结
  - final finaly finalize区别
    - final 是修饰符 类被final修饰，此类不能作为父类被继承。变量被final修饰不可更改
    - finaly 是异常捕获的关键字
    - finalize 是object 中的一个方法 该方法在垃圾回收器在清除垃圾前会被调用
  - String  StringBuffer StringBuilder区别
    - String为字符串常量，而StringBuilder和StringBuffer均为字符串变量，即String对象一旦创建之后该对象是不可更改的，但后两者的对象是变量，是可以更改的。以下面一段代码为例：
      ```java
      string  a = "abc"; 
      a = a + "de" //这个是创建了两次string对象
      
      string a = "abc" + "de" //这个是java默认为"abcde"
      ```
  - 如果一个StringBuffer对象在字符串缓冲区被多个线程使用时，StringBuffer中很多方法可以带有synchronized关键字，所以可以保证线程是安全的，但StringBuilder的方法则没有该关键字，所以不能保证线程安全，有可能会出现一些错误的操作。所以如果要进行的操作是多线程的，那么就要使用StringBuffer，但是在单线程的情况下，还是建议使用速度比较快的StringBuilder。
  - String：适用于少量的字符串操作的情况
  - StringBuffer：适用多线程下在字符缓冲区进行大量操作的情况
  - StringBuilder：适用于单线程下在字符缓冲区进行大量操作的情况
## 反射总结
    顾名思义就是以其之道还施彼身，java的思想，就是先有类在有对象，反射就是通过class拿到对象调用对应的方法，和属性，这就是反射的原理。

