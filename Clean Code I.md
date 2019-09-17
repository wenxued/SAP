### **Clean** Code I

#### name

- 类名、成员变量名词
- 方法动词
- 枚举形容词 -able

**避免噪音字**

反例：customer、customerData、customerObject、customerInfo

**一个概念一个词 pick one word per concept**

反例：customer.getAddress()、item.fetchEmail()

**名字应该被念出来 names should be pronounceable**

#### Comments

##### **bad Comments**

**non-local Information**

**Disinforamtion / out-of -data**

不应该靠注释增加对代码的理解

**Noise**

注释比代码还多

**Scary** **Noise**

每个方法不要写 javadoc

 如果想做一个API给别人用，可以写javadoc

**Position Markers**

#####  Good comments

Decoupling

**what makes writing tests hard**

比较远的

收费的

需要依赖的东西还没好

随着环境变化

随机性（测试不能依赖于随机值）

**设计模式-模板方法模式**

一个操作中算法的框架，将一些步骤延迟到子类中，使得子类可以不改变一个算法的结构即可可重定义该算法的某些特定步骤。

应用场景。。。

Mockito是mocking的框架，它让你用简洁的API做测试。https://blog.csdn.net/xiang__liu/article/details/81147933



**Different Types of test Doubles**

- Dummy
- stub
- spy
- mock
- fake

test double  https://www.cnblogs.com/hushaojun/p/4381493.html