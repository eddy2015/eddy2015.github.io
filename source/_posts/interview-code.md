---
title: Android 面试：编程算法题
date: 2016-10-26 12:07:03
categories:
- Android 面试
tags:
- Android 面试
---

本文收集整理了 Android 面试中会遇到的编程算法题。<!--more-->

## 有一个容器类 ArrayList，保存整数类型的元素，现在要求编写一个帮助类，类内提供一个帮助函数，帮助函数的功能是删除 容器中<10的元素。



## LeetCode上股票利益最大化问题 



## 剑指offer上第一次只出现一次的字符



## 反转字符串



## 字符串中出现最多的字符。



##  实现stack 的pop和push接口 要求：

- 1.用基本的数组实现
- 2.考虑范型
- 3.考虑下同步问题
- 4.考虑扩容问题

## 快速排序




## 冒泡排序
## 求素数
## 单例模式——写一个Singleton出来

参考：http://yuweiguocn.github.io/java-instance/

## 七种方式实现Singleton模式

```java
public class Test {

    /**
     * 单例模式，懒汉式，线程安全
     */
    public static class Singleton {
        private final static Singleton INSTANCE = new Singleton();

        private Singleton() {

        }

        public static Singleton getInstance() {
            return INSTANCE;
        }
    }

    /**
     * 单例模式，懒汉式，线程不安全
     */
    public static class Singleton2 {
        private static Singleton2 instance = null;

        private Singleton2() {

        }

        public static Singleton2 getInstance() {
            if (instance == null) {
                instance = new Singleton2();
            }
            return instance;
        }
    }

    /**
     * 单例模式，饿汉式，线程安全，多线程环境下效率不高
     */
    public static class Singleton3 {
        private static Singleton3 instance = null;

        private Singleton3() {

        }

        public static synchronized Singleton3 getInstance() {
            if (instance == null) {
                instance = new Singleton3();
            }
            return instance;
        }
    }

    /**
     * 单例模式，懒汉式，变种，线程安全
     */
    public static class Singleton4 {
        private static Singleton4 instance = null;

        static {
            instance = new Singleton4();
        }

        private Singleton4() {

        }

        public static Singleton4 getInstance() {
            return instance;
        }
    }

    /**
     * 单例模式，使用静态内部类，线程安全（推荐）
     */
    public static class Singleton5 {
        private final static class SingletonHolder {
            private static final Singleton5 INSTANCE = new Singleton5();
        }

        private static Singleton5 getInstance() {
            return SingletonHolder.INSTANCE;
        }
    }

    /**
     * 静态内部类，使用枚举方式，线程安全（推荐）
     */
    public enum Singleton6 {
        INSTANCE;
        public void whateverMethod() {

        }
    }

    /**
     * 静态内部类，使用双重校验锁，线程安全（推荐）
     */
    public static class Singleton7 {
        private volatile static Singleton7 instance = null;

        private Singleton7() {

        }

        public static Singleton7 getInstance() {
            if (instance == null) {
                synchronized (Singleton7.class) {
                    if (instance == null) {
                        instance = new Singleton7();
                    }
                }
            }
            return instance;
        }
    }

}
```

## 二叉树遍历

# 重建二叉树

------

题目：

```
输入二叉树的前序遍历和中序遍历的结果,重建出该二叉树。假设前序遍历和中序遍历结果中都不包含重复的数字,例如输入的前序遍历序列 {1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}重建出如图所示的二叉 树。

```

解题思路：

```
前序遍历第一个结点是父结点，中序遍历如果遍历到父结点，那么父结点前面的结点是左子树的结点，后边的结点的右子树的结点，这样我们可以找到左、右子树的前序遍历和中序遍历，我们可以用同样的方法去构建左右子树，可以用递归完成。

```

代码：

```java
public class BinaryTreeNode {

    public static int value;
    public BinaryTreeNode leftNode;
    public BinaryTreeNode rightNode;
}


public class Solution {

    public static BinaryTreeNode constructCore(int[] preorder, int[] inorder) throws Exception {
        if (preorder == null || inorder == null) {
            return null;
        }
        if (preorder.length != inorder.length) {
            throw new Exception("长度不一样，非法的输入");
        }
        BinaryTreeNode root = new BinaryTreeNode();
        for (int i = 0; i < inorder.length; i++) {
            if (inorder[i] == preorder[0]) {
                root.value = inorder[i];
                System.out.println(root.value);
                root.leftNode = constructCore(Arrays.copyOfRange(preorder, 1, i + 1),
                        Arrays.copyOfRange(inorder, 0, i));
                root.rightNode = constructCore(Arrays.copyOfRange(preorder, i + 1, preorder.length),
                        Arrays.copyOfRange(inorder, i + 1, inorder.length));
            }
        }
        return root;

    }
}
```

# 数值的整数次方

------

题目：

> 实现函数double Power(double base,int exponent)，求base的exponent次方，不得使用库函数，同时不需要考虑大数问题。

看到了很多人会这样写：

```
public static double powerWithExponent(double base,int exponent){
        double result = 1.0;
        for(int i = 1; i <= exponent; i++){
            result = result * base;
        }
        return result;
    }

```

输入的指数(exponent)小于1即是零和负数时怎么办？

当指数为负数的时候，可以先对指数求绝对值，然后算出次方的结果之后再取倒数，当底数(base)是零且指数是负数的时候，如果不做特殊处理，就会出现对0求倒数从而导致程序运行出错。最后，由于0的0次方在数学上是没有意义的，因此无论是输出0还是1都是可以接受的。

```
public double power(double base, int exponent) throws Exception {
        double result = 0.0;
        if (equal(base, 0.0) && exponent < 0) {
            throw new Exception("0的负数次幂无意义");
        }
        if (equal(exponent, 0)) {
            return 1.0;
        }
        if (exponent < 0) {
            result = powerWithExponent(1.0 / base, -exponent);
        } else {
            result = powerWithExponent(base, exponent);
        }
        return result;
    }

    private double powerWithExponent(double base, int exponent) {
        double result = 1.0;
        for (int i = 1; i <= exponent; i++) {
            result = result * base;
        }
        return result;
    }

    // 判断两个double型数据，计算机有误差
    private boolean equal(double num1, double num2) {
        if ((num1 - num2 > -0.0000001) && (num1 - num2 < 0.0000001)) {
            return true;
        } else {
            return false;
        }
    }

```

一个细节，再判断底数base是不是等于0时，不能直接写base==0，这是因为在计算机内表示小数时(包括float和double型小数)都有误差。判断两个数是否相等，只能判断 它们之间的绝对值是不是在一个很小的范围内。如果两个数相差很小，就可以认为它们相等。

还有更快的方法。

如果我们的目标是求出一个数字的32次方，如果我们已经知道了它的16次方，那么只要在16次方的基础上再平方一次就好了，依此类推，我们求32次方只需要做5次乘法。

我们可以利用如下公式：

[![img](https://camo.githubusercontent.com/ec88bcb539f7040696223964d7d3fe5e86984ed2/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313530373331303834303339363533)](https://camo.githubusercontent.com/ec88bcb539f7040696223964d7d3fe5e86984ed2/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313530373331303834303339363533)

```
private double powerWithExponent2(double base,int exponent){
        if(exponent == 0){
            return 1;
        }
        if(exponent == 1){
            return base;
        }
        double result = powerWithExponent2(base, exponent >> 1);
        result *= result;
        if((exponent&0x1) == 1){
            result *= base;
        }
        return result;
    }

```

我们用右移运算代替除2，用位与运算符代替了求余运算符（%)来判断一个数是奇数还是偶数。位运算的效率比乘除法及求余运算的效率要高很多。

# 扑克牌的顺子

------

题目:

```
从扑克牌中随机抽 5 张牌,判断是不是顺子,即这 5 张牌是不是连续的。 2-10 为数字本身,A 为 1,J 为 11,Q 为 12,K 为 13,而大小王可以看成任意的 数字。

```

> 解题思路：我们可以把5张牌看成是由5个数字组成的俄数组。大小王是特殊的数字，我们可以把它们都定义为0，这样就可以和其他的牌区分开来。
>
> 首先把数组排序，再统计数组中0的个数，最后统计排序之后的数组中相邻数字之间的空缺总数。如果空缺的总数小于或者等于0的个数，那么这个数组就是连续的，反之则不连续。如果数组中的非0数字重复出现，则该数组是不连续的。换成扑克牌的描述方式就是如果一幅牌里含有对子，则不可能是顺子。

详细代码：

```java
import java.util.Arrays;

public class Solution {

    public boolean isContinuous(int[] number){
        if(number == null){
            return false;
        }
        Arrays.sort(number);
        int numberZero = 0;
        int numberGap = 0;
        //计算数组中0的个数
        for(int i = 0;i < number.length&&number[i] == 0; i++){
            numberZero++;
        }
        //统计数组中的间隔数目
        int small = numberZero;
        int big = small + 1;
        while(big<number.length){
            //两个数相等，有对子，不可能是顺子
            if(number[small] == number[big]){
                return false;
            }
            numberGap+= number[big] - number[small] - 1;
            small = big;
            big++;
        }
        return (numberGap>numberZero)?false:true;

    }
}

```

# 圆圈中最后剩下的数字

------

题目

> 0,1,...,n-1这n个数字排成一个圆圈，从数字0开始每次从这个圆圈里删除第m个数字。求这个圆圈里剩下的最后一个数字。

解法:

> 可以创建一个总共有n个结点的环形链表，然后每次在这个链表中删除第m个结点。我们发现使用环形链表里重复遍历很多遍。重复遍历当然对时间效率有负面的影响。这种方法每删除一个数字需要m步运算，总共有n个数字，因此总的时间复杂度为O(mn)。同时这种思路还需要一个辅助的链表来模拟圆圈，其空间复杂度O(n）。接下来我们试着找到每次被删除的数字有哪些规律，希望能够找到更加高效的算法。
>
> 首先我们定义一个关于n和m的方程f(n,m)，表示每次在n个数字0，1，。。。n-1中每次删除第m个数字最后剩下的数字。
>
> 在这n个数字中，第一个被删除的数字是（m-1)%n.为了简单起见，我们把（m-1)%n记为k，那么删除k之后剩下的n-1个数字为0，1，。。。。k-1，k+1,.....n-1。并且下一次删除从数字k+1,......n-1,0,1,....k-1。该序列最后剩下的数字也应该是关于n和m的函数。由于这个序列的规律和前面最初的序列不一样（最初的序列是从0开始的连续序列），因此该函数不同于前面的函数，即为f'(n-1,m)。最初序列最后剩下的数字f(n,m)一定是删除一个数字之后的序列最后剩下的数字，即f(n,m)=f'(n-1,m).
>
> 接下来我么把剩下的这n-1个数字的序列k+1,....n-1，0，1，,,,,,,k-1做一个映射，映射的结果是形成一个从0到n-2的序列

```
k+1        ------>     0
k+2      --------->    1
。。。。
n-1      -----    > n-k-2
0         ------->    n-k-1
1       --------->    n-k
.....
k-1   --------->    n-k

```

> 我们把映射定义为p，则p(x) = (x-k-1)%n。它表示如果映射前的数字是x,那么映射后的数字是（x-k-1)%n.该映射的逆映射是p-1(x)= (x+k+1)%n.
>
> 由于映射之后的序列和最初的序列具有同样的形式，即都是从0开始的连续序列，因此仍然可以用函数f来表示，记为f(n-1,m).根据我们的映射规则，映射之前的序列中最后剩下的数字f'(n-1,m) = p-1[(n-1,m)] = [f(n-1,m)+k+1]%n ,把k= (m-1)%n代入f(n,m) = f'(n-1,m) =[f(n-1,m)+m]%n.
>
> 经过上面的复杂的分析，我们终于找到一个递归的公示。要得到n个数字的序列中最后剩下的数字，只需要得到n-1个数字的序列和最后剩下的数字，并以此类推。当n-1时，也就是序列中开始只有一个数字0，那么很显然最后剩下的数字就是0.我们把这种关系表示为：

[![这里写图片描述](https://camo.githubusercontent.com/9b2add44731441635cad2549412c97899ad90743/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313630373035313731383233323739)](https://camo.githubusercontent.com/9b2add44731441635cad2549412c97899ad90743/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313630373035313731383233323739)

代码如下：

```
public static int lastRemaining(int n, int m){
        if(n < 1 || m < 1){
            return -1;
        }
        int last = 0;
        for(int i = 2; i <= n; i++){
            last = (last + m) % i;
        }
        return last;
    }
```

# two-sum

------

Question

```
Given an array of integers, find two numbers such that they add up to a specific target number.
The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution.
Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

```

题目大意：

```
给定一个整数数组，找到2个数字，这样他们就可以添加到一个特定的目标号。功能twosum应该返回两个数字，他们总计达目标数，其中index1必须小于index2。请注意，你的答案返回（包括指数和指数）不为零的基础。你可以假设每个输入都有一个解决方案。
输入数字numbers= { 2，7，11，15 }，目标= 9输出：index1 = 1，index2= 2

```

解题思路：

```
可以申请额外空间来存储目标数减去从头遍历的数，记为key，如果hashMap中存在该key，就可以返回两个索引了。

```

代码；

```
import java.util.HashMap;

public class Solution {

    public int[] twoSum(int[] numbers, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < numbers.length; i++){
            if(map.get(numbers[i]) != null){
                int[] result = {map.get(numbers[i]) + 1, i+1};
                return result;
            }else {
                map.put(target - numbers[i], i);
            }
        }
        int[] result = {};
        return result;
    }
}
```

# 设计一个有getMin功能的栈

------

实现一个特殊的栈，在实现栈的基本功能的基础上，在实现返回栈中最小元素的操作。

要求：

1. pop、push、getMin操作的时间复杂度都是O(1)
2. 设计的栈类型可以使用现成的栈结构

解题：

```
package chapter01_stackandqueue;

import java.util.Stack;

/**
 * 
 * 实现一个特殊的栈，在实现栈的基本功能的基础上，在实现返回栈中最小元素的操作。 要求： 1. pop、push、getMin操作的时间复杂度都是O(1)
 * 2. 设计的栈类型可以使用现成的栈结构
 * 
 * @author dream
 *
 */
public class Problem01_GetMinStack {

    public static class MyStack1 {

        /**
         * 两个栈，其中stacMin负责将最小值放在栈顶，stackData通过获取stackMin的peek()函数来获取到栈中的最小值
         */
        private Stack<Integer> stackData;
        private Stack<Integer> stackMin;

        /**
         * 在构造函数里面初始化两个栈
         */
        public MyStack1() {
            stackData = new Stack<Integer>();
            stackMin = new Stack<Integer>();
        }

        /**
         * 该函数是stackData弹出栈顶数据，如果弹出的数据恰好等于stackMin的数据，那么stackMin也弹出
         * @return
         */
        public Integer pop() {
            Integer num = (Integer) stackData.pop();
            if (num == getmin()) {
                return (Integer) stackMin.pop();
            }
            return null;
        }

        /**
         * 该函数是先判断stackMin是否为空，如果为空，就push新的数据，如果这个数小于stackMin中的栈顶元素，那么stackMin需要push新的数，不管怎么样
         * stackData都需要push新的数据
         * @param value
         */
        public void push(Integer value) {
            if (stackMin.isEmpty()) {
                stackMin.push(value);
            }

            else if (value < getmin()) {
                stackMin.push(value);
            }
            stackData.push(value);
        }

        /**
         * 该函数是当stackMin为空的话第一次也得push到stackMin的栈中，返回stackMin的栈顶元素
         * @return
         */
        public Integer getmin() {
            if (stackMin == null) {
                throw new RuntimeException("stackMin is empty");
            }
            return (Integer) stackMin.peek();

        }
    }

    public static void main(String[] args) throws Exception {
        /**
         * 要注意要将MyStack1声明成静态的，静态内部类不持有外部类的引用
         */
        MyStack1 stack1 = new MyStack1();
        stack1.push(3);
        System.out.println(stack1.getmin());
        stack1.push(4);
        System.out.println(stack1.getmin());
        stack1.push(1);
        System.out.println(stack1.getmin());
        System.out.println(stack1.pop());
        System.out.println(stack1.getmin());

        System.out.println("=============");
    }

}

```

# 由两个栈组成的队列

------

题目：

```
编写一个类，用两个栈实现队列，支持队列的基本操作(add、poll、peek)。

```

解题：

```
/**
 * 
 * 编写一个类，用两个栈实现队列，支持队列的基本操作(add、poll、peek)。
 * 
 * @author dream
 *
 */
public class Problem02_TwoStacksImplementQueue {

    public static class myQueue{

        Stack<Integer> stack1;
        Stack<Integer> stack2;

        public myQueue() {
            stack1 = new Stack<Integer>();
            stack2 = new Stack<Integer>();
        }
        /**
         * add只负责往stack1里面添加数据
         * @param newNum
         */
        public void add(Integer newNum){
            stack1.push(newNum);
        }

        /**
         * 这里要注意两点：
         * 1.stack1要一次性压入stack2
         * 2.stack2不为空，stack1绝不能向stack2压入数据
         * @return
         */
        public Integer poll(){
            if(stack1.isEmpty() && stack2.isEmpty()){
                throw new RuntimeException("Queue is Empty");
            }else if(stack2.isEmpty()){
                while (!stack1.isEmpty()) {
                    stack2.push(stack1.pop());
                }
            }
            return stack2.pop();
        }

        public Integer peek(){
            if(stack1.isEmpty() && stack2.isEmpty()){
                throw new RuntimeException("Queue is Empty");
            }else if(stack2.isEmpty()){
                while (!stack1.isEmpty()) {
                    stack2.push(stack1.pop());
                }
            }
            return stack2.peek();
        }
    }

    public static void main(String[] args) {
        myQueue mQueue = new myQueue();
        mQueue.add(1);
        mQueue.add(2);
        mQueue.add(3);
        System.out.println(mQueue.peek());
        System.out.println(mQueue.poll());
        System.out.println(mQueue.peek());
        System.out.println(mQueue.poll());
        System.out.println(mQueue.peek());
        System.out.println(mQueue.poll());

    }
}

```

# 如何仅用递归函数和栈操作逆序一个栈

------

题目：

```
一个栈一次压入了1、2、3、4、5，那么从栈顶到栈底分别为5、4、3、2、1.将这个栈转置后，从栈顶到栈底为1、2、3、4、5，也就是实现栈中元素的逆序，但是只能用递归函数来实现，不能用其他数据结构。

```

解题：

```
/**
 * 一个栈一次压入了1、2、3、4、5，那么从栈顶到栈底分别为5、4、3、2、1.将这个栈转置后，
 * 从栈顶到栈底为1、2、3、4、5，
 * 也就是实现栈中元素的逆序，但是只能用递归函数来实现，不能用其他数据结构。
 * @author dream
 *
 */
public class Problem03_ReverseStackUsingRecursive {

    public static void reverse(Stack<Integer> stack) {
        if (stack.isEmpty()) {
            return;
        }
        int i = getAndRemoveLastElement(stack);
        reverse(stack);     
        stack.push(i);
    }

    /**
     * 这个函数就是删除栈底元素并返回这个元素
     * @param stack
     * @return
     */
    public static int getAndRemoveLastElement(Stack<Integer> stack) {
        int result = stack.pop();
        if (stack.isEmpty()) {
            return result;
        } else {
            int last = getAndRemoveLastElement(stack);
            stack.push(result);
            return last;
        }
    }

    public static void main(String[] args) {
        Stack<Integer> test = new Stack<Integer>();
        test.push(1);
        test.push(2);
        test.push(3);
        test.push(4);
        test.push(5);
        reverse(test);
        while (!test.isEmpty()) {
            System.out.println(test.pop());
        }

    }

}

```

## 最长不重复子串（最长重复子串）

## 有一个一维整型数组int[]data保存的是一张宽为width，高为height的图片像素值信息。请写一个算法，将该图片所有的白色不透明(xffffffff)像素点的透明度调整为5%。
## 写一个求递归程序 求54321
## 请使用java或者C++实现反转单链表
## 生产者、消费者
## 死锁（同步嵌套同步且锁不同）
## 写一个多线程实例代码；
## 写一个方法，交换两个变量的值？
## 给最外层的rootview，把这个根视图下的全部button背景设置成红色，手写代码，不许用递归
## 给一串字符串比如abbbcccd，输出a1b3c3d1，手写代码（注意有个别字符可能会出现十次以上的情况）
## 一个序列，它的形式是12349678，9是最高峰，经历了一个上升又下降的过程，找出里面的最大值的位置，要求效率尽可能高
## 二叉查找树的删除操作，手写代码
## 二分查找，手写代码
## 有海量条 url，其中不重复的有300万条，现在希望挑选出重复出现次数最高的 url，要求效率尽可能的高
## 一篇英语文章，去掉字符只留下k个，如何去掉才能使这k个字符字典序最小
## 弗洛伊德算法和 Dijkstra算法的区别？复杂度是多少？讲讲 Dijkstra算法的具体过程
## 反转字符串，要求手写代码，优化速度、优化空间
## 给出两个无向图，找出这2个无向图中相同的环路。手写代码


## 通过反射获取字段的值和方法名

```java
import java.lang.reflect.Field;

public class ReflectClass {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		Person p = new Person(1, "ctl", true, 'c', 2.0f, 2.0, 1L, (short) 1, (byte) 1);

		p.setId(0);
		p.setName("张三");
		p.setIsMen(true);
		p.setCh('c');
		p.setFloat_(2.0f);
		p.setDouble_(3.0);
		p.setLong_(2l);
		p.setShort_((short) 1);
		p.setByte_((byte) 2);
		reflect(p);
	}

	public static void reflect(Object obj) {
		if (obj == null)
			return;
		Field[] fields = obj.getClass().getDeclaredFields();
		String[] types1 = { "int", "java.lang.String", "boolean", "char", "float", "double", "long", "short", "byte" };
		String[] types2 = { "Integer", "java.lang.String", "java.lang.Boolean", "java.lang.Character",
				"java.lang.Float", "java.lang.Double", "java.lang.Long", "java.lang.Short", "java.lang.Byte" };
		for (int j = 0; j < fields.length; j++) {
			fields[j].setAccessible(true);
			// 字段名
			System.out.print(fields[j].getName() + ":");
			// 字段值
			for (int i = 0; i < types1.length; i++) {
				if (fields[j].getType().getName().equalsIgnoreCase(types1[i])
						|| fields[j].getType().getName().equalsIgnoreCase(types2[i])) {
					try {
						System.out.print(fields[j].get(obj) + "     ");
					} catch (Exception e) {
						e.printStackTrace();
					}
				}
			}
		}
	}
}

class Person {
	public int id;
	public String name;
	public boolean isMen;
	public Character ch;
	public Float float_;
	public Double double_;
	public Long long_;
	public Short short_;
	public Byte byte_;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public boolean getIsMen() {
		return isMen;
	}

	public void setIsMen(boolean isMen) {
		this.isMen = isMen;
	}

	public Character getCh() {
		return ch;
	}

	public void setCh(Character ch) {
		this.ch = ch;
	}

	public Float getFloat_() {
		return float_;
	}

	public void setFloat_(Float float_) {
		this.float_ = float_;
	}

	public Double getDouble_() {
		return double_;
	}

	public void setDouble_(Double double_) {
		this.double_ = double_;
	}

	public Long getLong_() {
		return long_;
	}

	public void setLong_(Long long_) {
		this.long_ = long_;
	}

	public Short getShort_() {
		return short_;
	}

	public void setShort_(Short short_) {
		this.short_ = short_;
	}

	public Byte getByte_() {
		return byte_;
	}

	public void setByte_(Byte byte_) {
		this.byte_ = byte_;
	}

	public Person(int id, String name, Boolean isMen, Character ch, Float float_, Double double_, Long long_,
			Short short_, Byte byte_) {
		super();
		this.id = id;
		this.name = name;
		this.isMen = isMen;
		this.ch = ch;
		this.float_ = float_;
		this.double_ = double_;
		this.long_ = long_;
		this.short_ = short_;
		this.byte_ = byte_;
	}

	public Person() {
		super();
	}
}
```

## 用 Arraylist,用数组 写一个队列


## 有一个数组最多存储6个数，如果有普通用户的话，存储四个 vip的客户，另外两个是普通用户（留出一定的空间给普通用户），让考虑全面点（一般都是结合实际场景，让你写出一个算法，要具备的能力就是抽象，处理问题的思路与细节，还有最基本的编码功底）