### Java 抽象类

#### 定义抽象类

定义抽象类的时候需要用到关键字  `abstract`，放在  `class`  关键字前，就像下面这样。

```java
abstract class AbstractPlayer {
}
```

#### 抽象类的特征

抽象类是不能实例化的，尝试通过  `new`  关键字实例化的话，编译器会报错，提示“类是抽象的，不能实例化”。
虽然抽象类不能实例化，但可以有子类。子类通过  `extends`  关键字来继承抽象类。就像下面这样。

```java
public class BasketballPlayer extends AbstractPlayer {
}
```

如果一个类定义了一个或多个抽象方法，那么这个类必须是抽象类。
抽象类中既可以定义抽象方法，也可以定义普通方法，就像下面这样：

```java
public abstract class AbstractPlayer {
    abstract void play();

    public void sleep() {
        System.out.println("运动员也要休息而不是挑战极限");
    }
}
```

抽象类派生的子类必须实现父类中定义的抽象方法。比如说，抽象类 AbstractPlayer 中定义了  `play()`  方法，子类 BasketballPlayer 中就必须实现。

```java
public class BasketballPlayer extends AbstractPlayer {
    @Override
    void play() {
        System.out.println("我是张伯伦，篮球场上得过 100 分");
    }
}
```

#### 抽象类的应用场景

当我们希望一些通用的功能被多个子类复用的时候，就可以使用抽象类。
当我们需要在抽象类中定义好 API，然后在子类中扩展实现的时候就可以使用抽象类。

- 1、抽象类不能被实例化。
- 2、抽象类应该至少有一个抽象方法，否则它没有任何意义。
- 3、抽象类中的抽象方法没有方法体。
- 4、抽象类的子类必须给出父类中的抽象方法的具体实现，除非该子类也是抽象类。
