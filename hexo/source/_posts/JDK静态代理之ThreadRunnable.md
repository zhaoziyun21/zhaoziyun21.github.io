---
title: JDK静态代理之ThreadRunnable
date: 2017-01-22 22:58:44
tags: [设计模式,线程]
category: [设计模式]
---
## ***静态代理4要素***
- ***目标角色***
- ***代理角色*** 
- ***目标角色和代理角色实现同一接口。***
- ***代理角色持有目标角色的引用。***

## **简单的静态代理模式示例** 

***统一接口：***
```
package com.xs.pattern.staticproxy;  
  
/** 
 * 统一接口 
 *  
 * @author zhaoziyun 
 *  
 */  
public interface Callable {  
    void call();  
}  
```
***目标角色：***
```
package com.xs.pattern.staticproxy;  
  
/** 
 * 目标角色 
 *  
 * @author zhaoziyun 
 *  
 */  
public class Target implements Callable {  
  
    public void call() {  
        System.out.println("call...");  
    }  
  
}  
```

***代理角色***
```
package com.xs.pattern.staticproxy;  
  
/** 
 * 代理角色 
 *  
 * @author zhaoziyun 
 *  
 */  
public class Proxy implements Callable {  
  
    Callable callable;  
  
    public Proxy(Callable callable) {  
        this.callable = callable;  
    }  
  
    public void call() {  
        System.out.println("before...");  
        callable.call();  
        System.out.println("after...");  
    }  
  
}  
```

***测试***
```
package com.xs.pattern.staticproxy;  
  
public class StaticProxyApp {  
    public static void main(String[] args) {  
        Callable target = new Target();  
        Callable proxy = new Proxy(target);  
        proxy.call();  
    }  
}  
```
***输出***
```
before...  
call...  
after...  
```

***JDK中最典型的静态代理就是线程的使用。***
```
package com.xs.pattern.staticproxy;  
  
public class ThreadStaticProxy {  
    public static void main(String[] args) {  
        Runnable target = new MyTarget();// 目标角色  
        Thread proxy = new Thread(target);// 代理角色  
        proxy.start();  
    }  
}  
  
class MyTarget implements Runnable {  
  
    public void run() {  
        System.out.println("run...");  
    }  
}  
```
**线程体（也就是我们要执行的具体任务）实现了Runnable接口和run方法。同时Thread类也实现了Runnable接口。此时，线程体就相当于目标角色，Thread就相当于代理角色。当程序调用了Thread的start()方法后，Thread的run()方法会在某个特定的时候被调用。thread.run()方法：**
```
public void run() {  
(target != null) {  
 target.run();  
  
}  
```
***实际上是执行了线程体的代码。***