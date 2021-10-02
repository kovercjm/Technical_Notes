# JAVA中的反射
## 获取反射中的Class对象
- Class.forName
  - 当你知道该类的全路径名时，你可以使用该方法获取 Class 类对象。
		Class clz = Class.forName("java.lang.String");
- .class
  - 只适合在编译前就知道操作的Class
		Class clz = String.class;
- 类对象的 getClass() 
		String str = new String("Hello");
		Class clz = str.getClass();

## 通过反射创建类对象
- Class 对象的 newInstance()	- 只能使用默认的无参数构造方法
		Class clz = Apple.class;
		Apple apple = (Apple)clz.newInstance();
- Constructor 对象的 newInstance() - 可以选择特定构造方法
		Class clz = Apple.class;
		Constructor constructor = clz.getConstructor(String.class, int.class);
		Apple apple = (Apple)constructor.newInstance("红富士", 15);

## 使用
	getFields()			- 获取公有属性
	getDeclaredFields() - 获取所有属性