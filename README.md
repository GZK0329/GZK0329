# 👋 学习记录 👋

## 目录

[1.List接口相关](#jump1)

[2.Set接口相关](#jump2)

[3.反射相关](#jump3)

[4.进程相关](#jump4)

[5.retrofit相关](#jump5)

***
<span id="jump1"></span>
## 1.List接口

### 1.1 ArrayList接口
ArrayList的继承实现关系如下图所示:
![](https://static-pic-github-personal.oss-cn-shanghai.aliyuncs.com/github-personal-learning/List/ArrayList-1.png)
ArrayList接口实现了Serializable、Cloneable、RandomAccess、List接口，继承了AbstractList抽象类。

| 接口   |    功能  | 
| :----  | :---- | 
| Cloneable  | 重写clone()方法，即ArrayList支持深拷贝
| RandomAccess  | 随机读取，ArrayList支持随机读取列表中元素 |
| AbstractList  | 继承抽象类AbstractList中的公共方法，重写了crud方法| 

#### 1.1.1扩容机制？


#### 1.1.2是不是线程安全？

#### 1.1.3 拷贝机制？

#### 1.1.4 序列化？

#### 1.1.5 一些实用的API

subList()

spliterator()

retainAll()

### 1.2 LinkedList接口


***

<span id="jump2"></span>

***

<span id="jump3"></span>

## 3.反射 访问权限检查机制 setAccessible(true)
### 3.1 反射中访问权限检查机制原理

稍后补充
### 3.2 使用PropertyDescriptor get/set bean对象属性

```java
public class PropertyUtil {

    /**
     * 构造PropertyDescriptor
     * @param clazz 所在类
     * @param propertyName 属性名
     * @return
     */
    public static PropertyDescriptor getPropertyDescriptor(Class clazz, String propertyName) {
        //构建一个可变字符串用来构建方法名称
        StringBuffer sb = new StringBuffer();
        Method setMethod = null;
        Method getMethod = null;
        PropertyDescriptor pd = null;
        try {
            //根据字段名来获取字段
            Field f = clazz.getDeclaredField(propertyName);
            if (f != null) {
                //构建方法的后缀
                String methodEnd = propertyName.substring(0, 1).toUpperCase() + propertyName.substring(1);
                sb.append("set" + methodEnd);
                //构建set方法
                setMethod = clazz.getDeclaredMethod(sb.toString(), new Class[]{f.getType()});

                sb.delete(0, sb.length());

                sb.append("get" + methodEnd);
                //构建get 方法
                getMethod = clazz.getDeclaredMethod(sb.toString(), new Class[]{});

                //构建一个属性描述器 把对应属性 propertyName 的 get 和 set 方法保存到属性描述器中
                pd = new PropertyDescriptor(propertyName, getMethod, setMethod);
            }
        } catch (Exception ex) {
            ex.printStackTrace();
        }

        return pd;
    }

    /**
     *
     * @param obj  类对象
     * @param propertyName 属性名
     * @param value 需写入的属性值
     */
    public static void setProperty(Object obj, String propertyName, Object value) {
        //获取对象的类型
        Class clazz = obj.getClass();
        //获取 clazz 类型中的 propertyName 的属性描述器
        PropertyDescriptor pd = getPropertyDescriptor(clazz, propertyName);
        //从属性描述器中获取 set 方法
        Method setMethod = pd.getWriteMethod();
        try {
            //调用 set 方法将传入的value值保存属性中去
            setMethod.invoke(obj, new Object[]{value});
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     *
     * @param obj 类对象
     * @param propertyName 属性名
     * @return
     */
    public static Object getProperty(Object obj, String propertyName) {
        //获取对象的类型
        Class<?> clazz = obj.getClass();
        //获取 clazz 类型中的 propertyName 的属性描述器
        PropertyDescriptor pd = getPropertyDescriptor(clazz, propertyName);
        //从属性描述器中获取 get 方法
        Method getMethod = pd.getReadMethod();
        Object value = null;
        try {
            //调用方法获取方法的返回值
            value = getMethod.invoke(obj, new Object[]{});
        } catch (Exception e) {
            e.printStackTrace();
        }
        return value;
    }
}

```

***

<span id="jump4"></span>

## 4.进程相关

***

<span id="jump5"></span>

## 5.retrofit相关
