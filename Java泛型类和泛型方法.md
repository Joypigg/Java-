# Java泛型类和泛型方法

### 泛型：

解决杂乱使用Object变量，然后使用强制类型转换的问题

编写的代码可以被不同类型的对象所重用

例如，我们不希望聚集String和File而编写不同的类，而ArrayList便可以采用泛型聚集任何类型的对象

### 泛型类：

普通类的工厂

一个泛型类就是具有一个或多个类型变量的类。

定义简单的泛型类：

```
public class Pair<T> {                //泛型类
	private T first;                 //T类型的变量、局部变量的范围
	private T second;                

	public Pair() {
	}

	public Pair(T first, T second) {
		this.first = first;
		this.second = second;
	}

	public T getFirst() {           //指定方法的返回类型
		return first;
	}

	public void setFirst(T first) {
		this.first = first;
	}

	public T getSecond() {
		return second;
	}

	public void setSecond(T second) {
		this.second = second;
	}
}
```

实例化：

> 用具体的类型替换类型变量就可以实例化泛型类型
>
> 如：Pair< String >

```
public class MyTest {
	public static void main(String[] args) {
		String[] words= {"Mb","Mary","jian","had"};
		Pair<String> mm=ArrayAlg.minmax(words);   //得到泛型类
		System.out.println("min="+mm.getFirst());
		System.out.println("max="+mm.getSecond());
//		System.out.println(words[1].compareTo(words[2]));
	}
}
class ArrayAlg {                     //compareTo得到min和max
	public static Pair<String> minmax(String[] a) {
		if (a == null || a.length == 0) {
			return null;
		}
		String min = a[0];
		String max = a[0];
		for (int i = 1; i < a.length; i++) {
			if (min.compareTo(a[i]) > 0) {
				min = a[i];
			}
			if (max.compareTo(a[i]) < 0) {
				max = a[i];
			}
		}
		return new Pair<>(min,max);     //Pair<>(T first,T second)
	}
}
```

优点：今后开发中、比较只需要替换String[]类型和Pair< T  >中泛型参数即可、简化开发、提高开发效率

### 泛型方法：

泛型方法可以放在普通类中、也可以放在泛型类中

类型变量位置：修饰符public static 后面，返回类型 T 前面

```
public class ArrayAlg {

	public static <T> T getMiddle(T... a) {
		return a[a.length / 2];
	}
}
```

调用泛型方法时：

```
ArrayAlg .<String>getMiddle("s","b","e");
```

```
类型变量的限定：
class ArrayAlg {
//限定了方法返回类型是实现Comparable接口的类
	public static <T extends Comparable<T>> T min(T[] a) {
		if (a == null || a.length == 0) {
			return null;
		}
		T min = a[0];
		for (int i = 1; i < a.length; i++) {
			if (min.compareTo(a[i]) > 0) {
				min = a[i];
			}
		}
		return min;
	}
}
当进行方法调用时：
//Integer、String...
public static void main(String[] args) {
		Integer[] obj = new Integer[] { 20, 21 };
		System.out.println(ArrayAlg.min(obj));
	}
```

> 限定类型用&分割，而类型变量则用,分割（类定义后面< T,V>）
>
> 如：public static < T extends Comparable< T>&Serializable >   T   method( ){
>
> ​		return null;
>
> }
>
> 注：在java继承中，可以根据需要拥有多个接口超类型，但限定中至多只有一个类。并且用一个类进行限定，它必须在限定列表的第一个
>
> 如：
>
> public static < T extends JavaBean< T>&Comparable< T>&Serializable>  T  method(){
>
> ​                  return null;
>
> }

### 翻译泛型表达式： 

当调用泛型方法时，擦除返回数据类型：

```
ArrayAlg .<String>getMiddle("s","b","e");
ArrayAlg .getMiddle("s","b","e");
```

①.对原始方法的调用；

②.将返回的Object类型强转为等号左边类型

### 翻译泛型方法：

```
public static < T extends Comparable< T>>   T   method(T[]  a){
		return null;
}
擦除后为：类型参数T已经被擦除，只留下限定类型
public static Comparable main(Comparable[] a){
    return null;
}
```

### Tips：

```
1.虚拟机中没有泛型，只有普通的类和方法
2.所有的类型参数都用他们的限定类型替换
3.桥方法被用来合成保持多态
4.为保持类型安全性，必要时插入类型转化
```



