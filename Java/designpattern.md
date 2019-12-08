### 单例模式（创建型模式）
#### 饿汉式单例模式
```java
public class Singleton{
    //1.私有化构造器，使得外部不可调用
    private Singleton(){
    }
    //2.在类的内部创建类的实例
    private static Singleton instance = new Singleton();
    //3.私有化上面的对象
    //4.公共方法，只能通过类调用，设为static，所以实例也得为static
    public static Singleton getInstance(){
        return instance;
    }
}
```

#### 懒汉式单例模式（存在线程安全问题）
```java
public class Singleton{
    private Singleton(){
    }
    private static Singleton instance = null;
    public static Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 线程安全的饿汉式单例模式
##### 方式一：将getInstance变同步方法
```java
public class Singleton{
    private Singleton(){
    }
    private static Singleton instance = null;
    public synchronized static Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}
```
劣势：每次使用都得同步，效率较低

##### 方式二，同步代码块
```java
public class Singleton{
    private Singleton(){
    }
    private volatile static Singleton instance = null;
    public static Singleton getInstance(){
        if(instance == null){
            synchronized(Singleton.class){
                if(instance == null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
