# 👋 学习记录 👋

## 目录

[1.反射相关](#jump1)

[2.进程相关](#jump2)

[3.retrofit相关](#jump3)

***

<span id="jump1"></span>

## 1.反射 访问权限检查机制 setAccessible(true)
### 1.1 反射中访问权限检查机制原理

稍后补充
### 1.2 使用PropertyDescriptor get/set bean对象属性

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
        Class<?> clazz = obj.getClass();//获取对象的类型
        PropertyDescriptor pd = getPropertyDescriptor(clazz, propertyName);//获取 clazz 类型中的 propertyName 的属性描述器
        Method getMethod = pd.getReadMethod();//从属性描述器中获取 get 方法
        Object value = null;
        try {
            value = getMethod.invoke(obj, new Object[]{});//调用方法获取方法的返回值
        } catch (Exception e) {
            e.printStackTrace();
        }
        return value;
    }
}

```

***


## 2.进程相关

***

## 3.retrofit相关
