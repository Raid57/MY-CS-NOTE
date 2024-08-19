# JavaScript Api

# Json字符串解析

**1.path方法**

```JAVA
String apiBase = node.get("litellm_params").get("api_base") == null ? "" : node.get("litellm_params").get("api_base").asText();

/** -> 使用path方法替换上面 null 判度的逻辑
	-> 1. 如果path("api_base")返回是null，
			.asText()返回String默认值""空字符串
			.asInt()返回int默认值0
*/
String apiBase = node.get("litellm_params").path("api_base").asText();

```



# 函数式编程

## JAVA 常用函数式接口

Supplier供给型接口：`Supplier接口是不需要传入值，返回一个泛型类T`

Consumer消费者接口：`Consumer接口是传入一个泛型类T，无返回值`

Predicate断言型接口：`Predicate接口是传入一个泛型类T，返回要一个布尔类型的值`

Function函数接口：`Function接口是传入一个泛型类T，返回一个泛型类R`



## Stream流式 API

`Stream`提供的常用操作有：

**转换操作：**`map()`，`filter()`，`sorted()`，`distinct()`；

**合并操作：**`concat()`，`flatMap()`；

**并行处理：**`parallel()`；

**聚合操作：**`reduce()`，`collect()`，`count()`，`max()`，`min()`，`sum()`，`average()`；

**其他操作：**`allMatch()`, `anyMatch()`, `forEach()`。

**注意：**一个流只能被聚合消费一次

```java
IntStream intStream = personList.stream().mapToInt(p -> p.score);
int sum = intStream.sum();
// 错误用法：intStream已经被sum聚合消费了，不能再次使用
OptionalDouble average = intStream.average();
// 正确用法：新开一个流
//  OptionalDouble average = personList.stream().mapToInt(p -> p.score).average();
System.out.println("和：" + sum);
System.out.println("平均数：" + average.getAsDouble());
```



### 创建流

#### Stream.of()

```JAVA
```

#### 基于数组或Collection

#### 基于Supplier

```JAVA
```



### map

```JAVA
public static void main(String[] args) {
    List.of("  Apple ", " pear ", " ORANGE", " BaNaNa ")
            .stream()
            .map(String::trim) // 去空格
            .map(String::toLowerCase) // 变小写
            .forEach(System.out::println); // 打印
}
```

`map()`方法用于将一个`Stream`的每个元素映射成另一个元素并转换成一个新的`Stream`；

即，`map()`方法返回的也是一个Stream；



### filter

```JAVA
public static void main(String[] args) {
        IntStream.of(1, 2, 3, 4, 5, 6, 7, 8, 9)
                .filter(n -> n % 2 != 0) // 保留奇数的元素
                .forEach(System.out::println);
}
```

`filter()`，条件成立的元素被筛选留下；



### reduce

聚合操作



### 输出集合

#### List

```JAVA
public static void main(String[] args) {
    Stream<String> stream = Stream.of("Apple", "", null, "Pear", "  ", "Orange");
    List<String> list = stream.filter(s -> s != null && !s.isBlank()).collect(Collectors.toList());
    System.out.println(list);
}
```



#### 数组

```JAVA
List<String> list = List.of("Apple", "Banana", "Orange");
String[] array = list.stream().toArray(String[]::new);
```



#### Map

```JAVA
Stream<String> stream = Stream.of("APPL:Apple", "MSFT:Microsoft");
        Map<String, String> map = stream
                .collect(Collectors.toMap(
                        // 把元素s映射为key:
                        s -> s.substring(0, s.indexOf(':')),
                        // 把元素s映射为value:
                        s -> s.substring(s.indexOf(':') + 1)));
```



### 其他操作

#### 排序sort

```JAVA
// String元素排序
List<String> list = List.of("Orange", "apple", "Banana")
    .stream()
    .sorted()
    .collect(Collectors.toList());
System.out.println(list);

public class Main {
	public static void main(String[] args) {
		List<Person> personList = new ArrayList<>();
		personList.add(new Person("小白", 45));
		personList.add(new Person("小明", 88));
		personList.add(new Person("小黑", 62));
		personList.stream().sorted(new Comparator<Person>() {
			@Override
			public int compare(Person o1, Person o2) {
				return o1.score - o2.score;
			}
		}).forEach((p) ->{
			System.out.println(p.score);
		});
	}
}

// 类对象排序
class Person {
	String name;
	int score;
	Person(String name, int score) {
		this.name = name;
		this.score = score;
	}
}
```



#### 去重distinct

```JAVA
List.of("A", "B", "A", "C", "B", "D")
    .stream()
    .distinct()
    .collect(Collectors.toList());
```



#### 截取

截取操作常用于把一个无限的`Stream`转换成有限的`Stream`，

`skip()`用于跳过当前`Stream`的前N个元素，

`limit()`用于截取当前`Stream`最多前N个元素：

```java
List.of("A", "B", "C", "D", "E", "F")
    .stream()
    .skip(2) // 跳过A, B
    .limit(3) // 截取C, D, E
    .collect(Collectors.toList()); // [C, D, E]
```



#### 合并

```JAVA
Stream<String> s1 = List.of("A", "B", "C").stream();
Stream<String> s2 = List.of("D", "E").stream();
// 合并:
Stream<String> s = Stream.concat(s1, s2);
System.out.println(s.collect(Collectors.toList())); // [A, B, C, D, E]
```



#### flatMap

如果`Stream`的元素是集合：

```JAVA
Stream<Integer> i = s.flatMap(list -> list.stream());
```

因此，所谓`flatMap()`，是指把`Stream`的每个元素（这里是`List`）映射为`Stream`，然后合并成一个新的`Stream`：

<img src="D:\Users\yufeng.huang\AppData\Roaming\Typora\typora-user-images\image-20240806192154905.png" alt="image-20240806192154905" style="zoom:50%;" />



#### 并行

通常情况下，对`Stream`的元素进行处理是单线程的，即一个一个元素进行处理。但是很多时候，我们希望可以并行处理`Stream`的元素，因为在元素数量非常大的情况，并行处理可以大大加快处理速度。

把一个普通`Stream`转换为可以并行处理的`Stream`非常简单，只需要用`parallel()`进行转换：

```JAVA
Stream<String> s = ...
String[] result = s.parallel() // 变成一个可以并行处理的Stream
                   .sorted() // 可以进行并行排序
                   .toArray(String[]::new);
```

经过`parallel()`转换后的`Stream`只要可能，就会对后续操作进行并行处理。我们不需要编写任何多线程代码就可以享受到并行处理带来的执行效率的提升。



#### 其他聚合方法

除了`reduce()`和`collect()`外，`Stream`还有一些常用的聚合方法：

- `count()`：用于返回元素个数；
- `max(Comparator<? super T> cp)`：找出最大元素；
- `min(Comparator<? super T> cp)`：找出最小元素。

针对`IntStream`、`LongStream`和`DoubleStream`，还额外提供了以下聚合方法：

- `sum()`：对所有元素求和；
- `average()`：对所有元素求平均数。

还有一些方法，用来测试`Stream`的元素是否满足以下条件：

- `boolean allMatch(Predicate<? super T>)`：测试是否所有元素均满足测试条件；
- `boolean anyMatch(Predicate<? super T>)`：测试是否至少有一个元素满足测试条件。

最后一个常用的方法是`forEach()`，它可以循环处理`Stream`的每个元素，我们经常传入`System.out::println`来打印`Stream`的元素：
