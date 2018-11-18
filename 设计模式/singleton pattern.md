## 单例模式 --- singleton pattern
李建忠老师的  设计模式  中单例模式：

 

 这是一个类设计者考虑的设计模式：只允许存在一个实例对象

1. 先考虑  单线程下：

new方法应该为私有，不允许外界new对象

类中包含一个实例对象my_instance，通过get_instance 方法返回这个实例对象

即  if(my_instance == none)

my_instance = new ***;

return my_instance

2. 多线程下不安全    

所以加 lock

lock.acquire

if(my_instance == none)

my_instance = new ***;

lock. release

return my_instance

 

3. 2方法存在问题，如果my_instance != none, 这时候多个线程去get_instance 读 my_instance,  导致效率不高

 

4. 后加锁方式， 还是线程不安全的

 

if(my_instance == none)

lock.acquire

my_instance = new ***;

lock. release

return my_instance

 

5. 双检查方式

 

if(my_instance == none)

　　lock.acquire

　　　　if(my_instance == none)

　　　　　　my_instance = new ***;

　　lock. release

return my_instance

 

6. 但是my_instance = new ***;   这句话 正常编译器解析时     1. 分配内存  2 调用构造函数 3. 返回内存指针，赋值给my_instance

但是编译器可能会做 reorder操作进行优化，导致1,3 先进行  2后进行

假设线程1做完1,3后  阻塞了 ，这时候  线程2get_instance  得到的是没有经过构造函数的一段内存， 没有任何意义，甚至可能引发错误。

 

所以这是编译器的 锅

7. 解决这个问题     是C++11之后，引入  atomic模板解决的   。