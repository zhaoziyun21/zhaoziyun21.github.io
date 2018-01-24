---
title: redis采用序列化方案存对象
date: 2018-01-22 22:59:10
tags: [redis]
category: [缓存]
---
***首先来了解一下为什么要实现序列化***
        
    当一个类实现了Serializable接口(该接口仅为标 记接口,不包含任何方法定义),表示该类可以序列
    化.
    序列化的目的是将一个实现了Serializable接口的对象转换成一个字节序列,可以把该字节序列保存起来(例如:保存在一个文件里),以后可以随时将该字节序列恢复为原来的对象。
    甚至可以将该字节序列放到其他计算机上或者通过网络传输到其他计算机上恢复，只要该计 算机平台存在相应的类就可以正常恢复为原来的 对象。
    实现：要序列化一个对象，先要创建某些OutputStream对象，然后将其封装在一个ObjectOutputStream对象内，再调用writeObject()方法即可序列 化一个对象；反序列化也类似。
> 注意：使用对象流写入到文件是不仅要保证该对象是序列化的，而且该对象的成员对象也必须是序列化的

关于Serializable接口的类中的serialVersionUID:
关于Serializable接口的类中的serialVersionUID:
serialVersionUID是long类型的。在Eclipse中有两种生成方式：
默认的是1L：
private static final long serialVersionUID = 1L;
另外一个则是根据类名、接口名、成员方法以及属性等生成一个64位的哈希字段：
private static final long serialVersionUID = 3969438177161438988L;
serialVersionUID主要是为了解决对象反序列化的兼容性问题。
如果没有提供serialVersionUID，对象序列化后存到硬盘上之后，再增加或减少类的filed。这样，当反序列化时，就会出现Exception，造成不兼容问题。
但当serialVersionUID相同时，它就会将不一样的field以type的缺省值反序列化。这样就可以避开不兼容问题了。

以上方式只能恢复成Java对象，如果想要恢复成其他对象(如C++对象)，那就要将Java对象转换为XML格式，这样可以使其被各种平台和各种语言使用。可以使用随JDK一起发布的javax.xam.*类库，或者使用开源XOM类库(可以从www.xom.nu下载并获得文档)。

***接下来看看redis是怎么存对象的***

        package Object1;

        import java.io.ByteArrayInputStream;
        import java.io.ByteArrayOutputStream;
        import java.io.IOException;
        import java.io.ObjectInputStream;
        import java.io.ObjectOutputStream;
        
        import bean.Person;
        import redis.clients.jedis.Jedis;
        
        public class SerializeUtil {
            public static void main(String [] args){
                Jedis jedis = new Jedis("172.16.135.2");
                String keys = "name";
                // 删数据
                //jedis.del(keys);
                // 存数据
                jedis.set(keys, "zy");
                // 取数据
                String value = jedis.get(keys);
                System.out.println(value);
                
                //存对象
                Person p=new Person();  //peson类记得实现序列化接口 Serializable
                p.setAge(20);
                p.setName("姚波");
                p.setId(1);
                jedis.set("person".getBytes(), serialize(p));
                byte[] byt=jedis.get("person".getBytes());
                Object obj=unserizlize(byt);
                if(obj instanceof Person){
                    System.out.println(obj);
                }
            }
            
            //序列化 
            public static byte [] serialize(Object obj){
                ObjectOutputStream obi=null;
                ByteArrayOutputStream bai=null;
                try {
                    bai=new ByteArrayOutputStream();
                    obi=new ObjectOutputStream(bai);
                    obi.writeObject(obj);
                    byte[] byt=bai.toByteArray();
                    return byt;
                } catch (IOException e) {
                    e.printStackTrace();
                }
                return null;
            }
            
            //反序列化
            public static Object unserizlize(byte[] byt){
                ObjectInputStream oii=null;
                ByteArrayInputStream bis=null;
                bis=new ByteArrayInputStream(byt);
                try {
                    oii=new ObjectInputStream(bis);
                    Object obj=oii.readObject();
                    return obj;
                } catch (Exception e) {
                    
                    e.printStackTrace();
                }
            
                
                return null;
            }
        }