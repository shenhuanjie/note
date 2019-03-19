这个是最常用的技术之一。程序员希望通过随机数的方式来处理众多的业务逻辑，测试过程中也希望通过随机数的方式生成包含大量数字的测试用例。

问题往往类似于：

* 如何随机生成 1~100 之间的随机数，取值包含边界值 1 和 100。 
* 如何随机生成随机的3位整数？

以 Java 语言为例，我们观察其 Random 对象的 nextInt(int) 方法，发现这个方法将生成 0 ~ 参数之间随机取值的整数。例如（假设先有 Random rand = new Random();，下同）：

``rand.nextInt(100);``

这行代码将生成范围 0~100 之间的随机数，有趣的是，取值可能为 0 ，但不可能为 100。我们用中学数学课学习的区间表示法，表示为：[0, 100)。

那么如果要获得区间 [1~100] 的随机数，该怎么办呢？稍微动动脑筋就可以想到：区间 [0, 100) 内的整数，实际上就是区间 [0, 99]。因为最大边界为100，可惜不能等于100，因此最大可能产生的“整数”就是99。

既然 rand.nextInt(100) 获得的值是区间 [0, 99]，那么在这个区间左右各加 1，就得到了区间 [1, 100]。因此，代码写成：

rand.nextInt(100) + 1;

即可。



运行下面的代码，将获得 [1, 100] 的 10 个取值。

```java
/**
 * 生成区间不重复随机数
 */
package com.shenhuanjie.utils;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class RandomNumUtil {
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis(); //开始测试时间
        List<Integer> lst = randomNum(1, 100);
        System.out.println("Array: " + lst.toString());
        System.out.println("Size: " + lst.size());
        long endTime = System.currentTimeMillis(); //获取结束时间
        System.out.println("Running Times： " + (endTime - startTime) + "ms");

    }

    /**
     * 首先生成一个不重复的数集（例如：0~9999），然后每次生成随机索引，
     * 并且与当前索引进行交换(swap)，最后返回目标List对象（也可用其他类型实现）;
     *
     * @param begin 区间开始
     * @param end   区间结束
     * @return 返回目标List
     */
    public static List<Integer> randomNum(int begin, int end) {
        Random rd = new Random();
        List<Integer> lst = new ArrayList<>();//存放有序数字集合
        for (int i = begin; i <= end; i++) {
            lst.add(i);
        }
        int count = 0;
        for (Integer element : lst) {
            int index = rd.nextInt(lst.size() - count) + count;//随机索引
            lst.set(count, lst.get(index));
            lst.set(index, element);
            count++;
        }
        return lst;
    }
}

```

[GitHub代码：https://github.com/shenhuanjie/java-random-num-util](https://github.com/shenhuanjie/java-random-num-util)

