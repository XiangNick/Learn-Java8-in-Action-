
# 2.11. First attempt：filtering green apple
在《Java8 in Action》的2.11节，提到了一个为农场主挑选苹果的案例，以引出Lambda表达式

* 普通的写法可能为：

```java
public static List<Apple> filterGreenApples(List<Apple> apples) {
     List<Apple> list = new ArrayList<Apple>();
     for (Apple apple : apples) {
         if ("green".equals(apple.getColor())) {
         list.add(apple);
         }
     }
     return list;
 }
```
通过foreach遍历出适合的apple加入返回的list中

随后，如果农场主改变了需求，他需要一个红色的苹果，一个很天真的做法就是再写一个filterRedApples的方法来满足需求。如果农场主又改变了需求，需要黄色、亮绿色的苹果，我们是不是还得在写新的方法？再写了很多相似的代码后，我们会将原来的方法改进，在其中添加一个颜色的参数以实现复用。
```java
public static List<Apple> filterApplesByColor(List<Apple> apples,String color) {
    List<Apple> list = new ArrayList<Apple>();
        for (Apple apple : apples) {
            if (color.equals(apple.getColor())) {
            list.add(apple);
            }
        }
        return list;
}

```


随后农场主又会提出新的需求，他要红色的并且比150g重的苹果，上面的方法便无法满足了。我们会写一个过滤苹果的方法和接口来满足需求。

**过滤方法**
```java
public static List<Apple> findApple(List<Apple> apples,AppleFilter appleFilter) {
    List<Apple> list = new ArrayList<Apple>();
        for (Apple apple : apples) {
            if (appleFilter.filter(apple)) {
            list.add(apple);
            }
        }
        return list;
};
```
**接口**

```java
public interface AppleFilter {
    boolean filter(Apple apple);
};
```


随后我们再实现它
```java
public static class redGreater150WeightFilter implements AppleFilter {
    @Override
    public boolean filter(Apple apple) {
        return ("red".equals(apple.getColor()) && apple.getWeight() >= 150);
    }
}
```
但是这样如果农场主再改变需求的话，我们得不停得实现接口，冗余太多，解决方法之一可以是在过滤的方法直接使用匿名内部类。
```java
List<Apple> result = findApple(totalApple, new AppleFilter() {
    @Override
    public boolean filter(Apple apple) {
        return (apple.getColor().equals("green") && apple.getWeight() >= 170);
    }
});
```
好处是不用不断地实现新的AppleFilter，需要哪个就直接写匿名内部类，但是复用性差，并且阅读性低。
由此，可以引出Java8的Lambda表达式。
