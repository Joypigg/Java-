# HashMap的遍历

## 基于迭代器Interator

Map  map=new HashMap();

```
//map函数声明
public class MapTest {
	@SuppressWarnings({ "rawtypes", "unchecked", "unused" })
	private static Map getMap() {
		Map map = new HashMap();
		map.put("50", 100);
		map.put("51", 100);
		map.put("52", 100);
		map.put("53", 100);
		map.put("54", 100);
		return map;
	}
}
```

##### 方法一：使用keySet

```
	@Test
	public void keySetTest() {
		Map map = getMap();
		System.out.println(map);//map
		Iterator iter = map.keySet().iterator();
		Object key = iter.next();//存在则只打印第一个key，不存在则报错
		System.out.println(key);//第一个key
		while (iter.hasNext()) {
			Object key1 = iter.next();
			Object value = map.get(key1);
			System.out.println(key1 + " " + value);
		}
	}
```

```
打印结果：
{50=100, 51=100, 52=100, 53=100, 54=100}
50
50 100
50 100
50 100
50 100
```

进行了两次遍历，一次是将keySet转为Iterator，一次是将map中取出key所对应的value

map.keySet()得到的是关于key的Set<> 集合

##### 方法二：使用entrySet

```
	//错误的遍历
	@Test
	public void entrySetTest() {
		Map map = getMap();
		Iterator iter = map.entrySet().iterator();
		System.out.println(iter.next());//第一个key-alue
		while (iter.hasNext()) {
			Object key = iter.next();
			Object value = map.get(key);
			System.out.println(key + " " + value);
		}
	}
```

```
打印结果：
50=100    //键值对的映射
51=100 null
52=100 null
53=100 null
54=100 null
```

entrySet得到的对象是key-value的映射，所以正确的key-value遍历应该是：

iter.next()强转成Map.Entry<>,然后getKey(),getValue()形式取值

```
@Test
	public void entrySetTest() {
		Map map = getMap();
		Iterator iter = map.entrySet().iterator();
		System.out.println(iter.next());//第一个
		System.out.println(iter.next());//第二个
		System.out.println(iter.next());//第三个
		while (iter.hasNext()) {
			Map.Entry<String,Integer> entry =(Entry<String, Integer>) iter.next();
			System.out.println(entry.getKey());//从第四个开始
			System.out.println(entry.getValue());
		}
	}
```

```
运行结果：
50=100
51=100
52=100
53
100
54
100
```

##### 方法三：基于keySet的for循环

```
	@Test
	public void keySetTest1() {
		Map map=getMap();
		for(Object obj:map.keySet()) {
			System.out.println(obj);//obj只是key
			System.out.println(map.get(obj));//通过key得到value
		}
	}
```

```
运行结果：
50
100
51
100
52
100
53
100
54
100
```

##### 方法三：基于entrySet的for循环

注：由于Map.Entry<> ,需指定Map数据为泛型

getMap方法返回数据类型中添加Map< Object,Object >

private static Map< Object,Objec t> getMap() {...}

```
	@Test
	public void entrySetTest1() {
		Map<Object,Object> map = getMap();
		for (Map.Entry obj : map.entrySet()) {
		System.out.println(obj.getKey());
		System.out.println(obj.getValue());
		}
	}
```

```
运行结果：
50
100
51
100
52
100
53
100
54
100
```

> 注：若只想遍历map里面的key或者value时：
>
> 1.只获取key：keySet
>
> 2.只获取value：map.values().iterator()

##### 拓展：

单Map中value不在单一属性、及出现list集合或者set集合时

对value的遍历时，则需添加一步对list/set集合遍历

example1：keySet

```
	@Test
	public void testValeList() {
		Map<String, int[]> map = new HashMap<>();
		map.put("1", new int[] { 4, 5 });
		map.put("2", new int[] { 1, 2, 3 });
		map.put("3", new int[] { 6, 7, 8, 9 });
		
		for (Object obj : map.keySet()) {
			 System.out.println(obj+" " +map.get(obj)); 
		}
	}
```

```
运行结果：
1 [I@137e0d2
2 [I@59eb14
3 [I@2e4553
```

example2：keySet

```
	@Test
	public void testValeList() {
		Map<String, int[]> map = new HashMap<>();
		map.put("1", new int[] { 4, 5 });
		map.put("2", new int[] { 1, 2, 3 });
		map.put("3", new int[] { 6, 7, 8, 9 });

		// keySet、并对value进行遍历
		for (Object obj : map.keySet()) {
			for (int i = 0; i < map.get(obj).length; i++) {
				 System.out.println(obj+" " +map.get(obj)[i]);
			}
		}
	}
```

```
运行结果：
1 4
1 5
2 1
2 2
2 3
3 6
3 7
3 8
3 9
```

example3：entrySet

```
		@Test
	public void testValeList() {
		Map<String, int[]> map = new HashMap<>();
		map.put("1", new int[] { 4, 5 });
		map.put("2", new int[] { 1, 2, 3 });
		map.put("3", new int[] { 6, 7, 8, 9 });
		
		for (Map.Entry<String, int[]> my : map.entrySet()) {
			System.out.println("entrySet:");
			// System.out.println(my);//键值映射
			int[] a = my.getValue();
			for (int i = 0; i < a.length; i++) {
				System.out.print(a[i] + " ");
			}
			System.out.println("\t");
		}
	}
```

```
运行结果：
entrySet:
4 5 	
entrySet:
1 2 3 	
entrySet:
6 7 8 9 
```

程序源代码：

```
package com.my.map;
import java.util.*;
import java.util.Map.Entry;
import org.junit.Test;

public class MapTest {
	@SuppressWarnings({ "rawtypes", "unchecked" })
	private static Map getMap() {
		Map map = new HashMap();
		map.put("50", 100);
		map.put("51", 100);
		map.put("52", 100);
		map.put("53", 100);
		map.put("54", 100);
		return map;
	}

	@Test
	public void keySetTest() {
		Map map = getMap();
		Iterator iter = map.keySet().iterator();
		Object key = iter.next();// 存在则只打印第一个，不存在则报错
		while (iter.hasNext()) {
			Object key1 = iter.next();
			Object value = map.get(key1);
			System.out.println(key1 + " " + value);
		}
	}

	@Test
	public void entrySetTest() {
		Map map = getMap();
		Iterator iter = map.entrySet().iterator();
		while (iter.hasNext()) {
			Map.Entry<String, Integer> entry = (Entry<String, Integer>) iter.next();
			System.out.println(entry.getKey());
			System.out.println(entry.getValue());
		}
	}

	@Test
	public void keySetTest1() {
		Map map = getMap();
		for (Object obj : map.keySet()) {
			System.out.println(obj);// obj只是key
			System.out.println(map.get(obj));// 通过key得到value
		}
	}
	
	@Test
	public void entrySetTest1() {
		Map<Object,Object> map = getMap();
		for (Map.Entry obj : map.entrySet()) {
		System.out.println(obj.getKey());
		System.out.println(obj.getValue());
		}
	}
}
```

