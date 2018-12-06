#### 工厂模式
工厂模式主要是为创建对象提供过渡接口，以便将创建对象的具体过程屏蔽隔离起来，
达到提高灵活性的目的。
工厂模式在《Java 与模式》中分为三类：

1）简单工厂模式（Simple Factory）

2）工厂方法模式（Factory Method）

3）抽象工厂模式（Abstract Factory）

这三种模式从上到下逐步抽象，并且更具一般性。

###### 简单工厂模式
通过工厂的判断,来调用接口的具体不同的实现类
```Java
//抽象产品角色
public interface Car{
    public void drive();
}
//具体产品
public class BCar implements Car{
    public void drive() {
        System.out.println("B Car");
    }
}

public class CCar implements Car{
    public void drive() {
        System.out.print("C Car");
    }
}

//简单工厂类
public class Driver{
	public static Car driverCar(String s) {
		switch (s) {
			case "B": return new BCar();
			case "C": return new CCar();
			default: return null;
		}
	}
}
```
###### 工厂方法模式
工厂方法模式去掉了简单工厂模式中工厂方法的静态属性，使得它可以被子类继承。
```Java
//抽象工厂角色
public interface Driver{
	public Car driverCar();
}
public class BDriver implements Driver{
	public Car driverCar(){
		return new BCar();
	}
}
public class CDriver implements Driver{
	public Car driverCar() {
		return new CCar();
	}
}
```
###### 抽象工厂模式
更加一般性的工厂方法模式
```Java

```
