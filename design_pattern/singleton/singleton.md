#### 单例模式
在Java中存在的陷阱
- 多个虚拟机
- 多个类加载器
- 错误的同步处理(懒汉式)
- 子类破坏了对象控制
	扩展性较好的单例模式实现方式的时候，就提到，由于类构造函数变得不再私有，就有可能失去对对象的控制。这种情况只能通过良好的文档来规范。
- 串行化（可序列化）
	因为一个串行化的对象在每次返串行化的时候，都会创建一个新的对象，而不仅仅是一个对原有对象的引用。为了防止这种情况，可以在单例类中加入readResolve 方法。

##### 饿汉式（静态常量）
```Java
public class Singleton{
	// final 防止反射修改
	private static final Singleton instance = new Singleton();
	private Singleton() {
	}
	public static Singleton getInstance() {
		return instance;
	}
}
```
优点：这种写法比较简单，就是在类装载的时候就完成实例化。避免了线程同步问题。

缺点：在类装载的时候就完成实例化，没有达到Lazy Loading的效果。如果从始至终从未使用过这个实例，则会造成内存的浪费。

##### 饿汉式（静态代码块）
```Java
public class Singleton{
	// final 防止反射修改
	private static final Singleton instance;
	static {
		instance = new Singleton();
	}
	private Singleton() {
	}
	public static Singleton getInstance() {
		return instance;
	}
}
```
这种方式和上面的方式其实类似，只不过将类实例化的过程放在了静态代码块中，也是在类装载的时候，就执行静态代码块中的代码，初始化类的实例。优缺点和上面是一样的。

###### 懒汉式（线程不安全）
```Java
public class Singleton{
	private static Singleton instance = null;
	private Singleton(){
	}
	public static Singleton getInstance() {
		if (instance == null) instance = new Singleton();
		return instance;
	}
}
```
多线程会产生多个实例对象

###### 懒汉式（线程安全）
```Java
public class Singleton{
	private static Singleton instance = null;
	private Singleton(){
	}
	public static synchronized Singleton getInstance() {
		if (instance == null) instance = new Singleton();
		return instance;
	}
}
```
避免多线程产生多个实例对象,但是效率低,每次都会加锁

###### 双重检查
```Java
public class Singleton {
	//没有volatile会导致双重检查时,因为指令重排,产生多个实例对象
	private static volatile Singleton instance;
	private Singleton() {}
	public static Singleton getInstance() {
		if (singleton == null) {
			synchronized (Singleton.class) {
				if (singleton == null) {
					singleton = new Singleton();
				}
			}
		}
	}
	
}
```

###### 静态内部类
```Java
public class Singleton {
	private Singleton() {}
	private static class SingletonInstance {
		private static final Singleton instance = new Singleton();
	}
	public static Singleton getInstance() {
		return SingletonInstance.instance;
	}
}
```

###### 枚举类
```Java
public enum Singleton {
	instance;
	public void whateverMethod() {
	}
}
```

