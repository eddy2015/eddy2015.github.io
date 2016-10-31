---
title: Android 面试：编程算法题
date: 2016-10-26 12:07:03
categories:
- Android 面试
tags:
- Android 面试
---

本文收集整理了 Android 面试中会遇到的编程算法题。<!-- more -->

## 参考

[Mr-YangCheng/ForAndroidInterview](https://github.com/Mr-YangCheng/ForAndroidInterview/tree/master/algorithm/swordForOffer) — 该处题目好像也是来源于剑指 Offer，不过每题都有详细思路和解法，值得一看。

[牛客网-剑指 Offer](http://www.nowcoder.com/ta/coding-interviews?page=1)

## 在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。 

根据题意画了个简单的图，首先我们要确定查找开始的位置，因为它是二维的数组，它的下标变化是两个方向的，根据四个边界点来分析。

　　a b

​	c d

> Ａ：向下　增　，向右　增
>
> Ｂ：向左　减　，向下　增
>
> Ｃ：向上　减　，向右　增
>
> Ｄ：向左　减　，向上　减

　　可以看出从B 或Ｃ点开始查找刚好可以构建一个二叉查找树。

> 　　二叉查找树（Binary Search Tree），（又：二叉搜索树，二叉排序树）它或者是一棵空树，或者是具有下列性质的二叉树： 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值； 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值； 它的左、右子树也分别为二叉排序树。

　　先确定二维数组的行数和列数，把 查找值 与 二叉查找树的根节点（Ｂ或者　Ｃ）开始比较，如果相等返回true，小于查找左子树，大于就查找右子树。如果遍历超过数组边界，就返回 false。

> 以C为根节点查找

```java
public class Solution {
    public boolean Find(int [][] array,int target) {

        int i = array.length -1;
        int m = array[0].length -1;
        int j = 0;

        while(i>=0 && j<=m){
            if(target == array[i][j]){
                return true;
            }else if(target >array[i][j]){
                j++;
            }else{
                i--;
            }
        }

        return false;
    }
}
```

> 以B为根节点查找

```java
public class Solution {
    public boolean Find(int [][] array,int target) {

        int i = array.length -1;
        int j = array[0].length -1;
        int n = 0;

        while(j>=0 && n<=i){
            if(target == array[n][j]){
                return true;
            }else if(target >array[n][j]){
                n++;
            }else{
                j--;
            }
        }

        return false;
    }
}
```

## 输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

题目分析

> 解法一　　运行时间：29m 　占用内存：629k

```java
public int  NumberOf1(int n) {
        String s=Integer.toBinaryString(n);
        char[] c=s.toCharArray();
        int j=0;
        for(int i=0;i<c.length;i++){

                if(c[i]=='1'){
                    j++;
                }

        }
        return j;
     }
}
```

解析：

> public static String toBinaryString(int i)
>
> 以二进制（基数 2）无符号整数形式返回一个整数参数的字符串表示形式。  

①先把整数转换成二进制字符串 

②把字符串转换成字符数组 

③遍历该数组，判断每位是否为1，为1 计数加1。 

④遍历完成返回1的个数

> 解法二   运行时间：30ms    占用内存：629k

```java
public class Solution {
public int  NumberOf1(int n) {
      int count =0;
      while(n!=0){
          count++;
          n = n&(n-1);
      }
     return count; 
   }
}
```

解析：

　　

> 如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。

举个例子：

①二进制数　　1100 

② 减１后，得　1011 

③1100&1011=1000

对比①和③你会发现，**把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0.那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。**

## 定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。

```java
import java.util.Stack;

public class Solution {

    Stack<Integer> stack = new Stack<Integer>();
    Stack<Integer> min = new Stack<Integer>();
    public void push(int node) {
        stack.push(node);
        if(min.isEmpty()){
            min.push(node);
        }else{
          if (node <= min.peek()) {
            min.push(node);
          }
        }
    }

    public void pop() {
      	if (stack.peek() == min.peek()) {
          min.pop();
        }
        stack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int min() {
        return min.peek();
    }
}
```

思路：实现两个栈，stack用来存储数据，min用来保存最小值；为了使min栈顶元素一直是当前stack栈中的最小值，要在stack入栈时进行判断，如果入栈值比当前min栈顶值更小，则把该值入为min新的栈顶，否则再**重复入一次min栈顶元素**（为了stack和min同时出栈，虽然出栈值可能不一样，但并不影响min栈顶是当前stack中的最小元素）。
　　在min函数中min.peek()就行了，返回栈顶但不出栈。

## 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法

题目分析

- 设n阶的跳数为f(n) 
- 当n=1时，f(1) = 1
- 当n=2时，分为最后一步 跳2阶和跳1阶 两种情况，有f(2)=f(0)+f(1)=1+1=2  
- 当n=3时，分为最后一步 跳3阶、跳2阶和跳1阶 三种情况，有f(3)=f(0)+f(1)+f(2)=1+1+2=4  
- 有 　　  f(n) = f(n-1)+f(n-2)+...+f(1) + f(0)成立同时有　f(n-1)＝f(n-2)+...+f(1) + f(0)　成立，可得出f(n)=2f(n-1)  (n>=2)

![](https://camo.githubusercontent.com/f840353bd8a32a5a02166410d818243d02c182fe/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313630343237313435363036363037)

很明显可以得出递推公式：

　　　　  | 1       　　　　(n=0 ) f(n) =   　  | 1     　　　　  (n=1 )　　　　 | 2*f(n-1)　　(n>=2)

> 解法一 　运行时间：35ms  　 占用内存：654k

```java
public class Solution {
    public int JumpFloorII(int target) {
        if(target<=1)  return 1;
        return 2*JumpFloorII(target-1);
    }
}
```

> 解法二   　运行时间：34ms　  占用内存：654k

```java
public class Solution {
    public int JumpFloorII(int target) {
   　　 if(target<=1)  return 1;
        return 1<<(target-1);
    }
}
```

　　换一种思路想一下：一共有ｎ个台阶，最后一个台阶是一定要跳上去的，其他的　ｎ－１个可跳可不跳，一共有多少总情况？

　　

> ２（ｎ－１）

这里用移位实现乘法，时间上要快一些！

## 请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

输入描述

> str 是StringBuffer的对象

输出描述

> 返回替换后的字符串的String对象

题目分析

　　**首先要对输入参数进行判空和越界判断**（任何算法题都要注意）。

> 解法一 （运行时间：36ms    占用内存：688k）

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        if(str==null){
            return null;
        }
        StringBuffer sb = new StringBuffer();
        char [] strChar = str.toString().toCharArray();
        for(int i=0;i<strChar.length;i++){
            if(strChar[i]==' '){
                sb.append("%20");
            }else{
                sb.append(strChar[i]);
            }
        }
        return sb.toString();
    }
}
```

　　把传入的StringBuffer 对象 str 转换成字符串 再转换成**字符数组**,申请一个新的StringBuffer 对象 sb来存最后的结果，遍历整个字符数组，如果当前字符为空格则把　＂％２０＂加入到sb中，否则直接追加当前字符，最后把sb转换成字符串返回。

> 解法二 （运行时间：34ms  占用内存：503k）

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        if(str == null){
            return null;
        }
        for(int i =0;i<str.length();i++){
            char c = str.charAt(i);
            if(c==' '){
                str.replace(i,i+1,"%20");
            }
        }
        return str.toString();
    }
}
```

> public StringBuffer replace(int start, int end, String str)
>
> 　　使用给定 String 中的字符替换此序列的子字符串中的字符。该子字符串从指定的 start 处开始，一直到索引 end - 1 处的字符，如果不存在这种字符，则一直到序列尾部。**先将子字符串中的字符移除，然后将指定的 String 插入 start。**（如果需要，序列将延长以适应指定的字符串。）参数：start - 起始索引（包含）。end - 结束索引（**不包含**）。str - 将替换原有内容的字符串。返回：此对象。

源码（可以看出它调用的是父类的方法）

```java
  public synchronized StringBuffer More ...replace(int start, int end, String str) {
453        toStringCache = null;
454        super.replace(start, end, str);
455        return this;
456    }
```

父类AbstractStringBuilder

```java
822     public AbstractStringBuilder More ...replace(int start, int end, String str) {
823         if (start < 0)
824             throw new StringIndexOutOfBoundsException(start);
825         if (start > count)
826             throw new StringIndexOutOfBoundsException("start > length()");
827         if (start > end)
828             throw new StringIndexOutOfBoundsException("start > end");
829 
830         if (end > count)
831             end = count;
832         int len = str.length();
833         int newCount = count + len - (end - start);
834         ensureCapacityInternal(newCount);
835 
836         System.arraycopy(value, end, value, start + len, count - end);
837         str.getChars(value, start);
838         count = newCount;
839         return this;
840     }
```

　　可以看出StringBuffer类 提供了 插入的函数，值得注意的是 StringBuilder 也是继承于AbstractStringBuilder这个抽象类的。所以它也有这个方法，但是String并没有。　　

> 解法三（不推荐，运行时间：38ms  占用内存：654k）

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        return str.toString().replaceAll("\\s", "%20");
    }
}
```

## 输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。 结果请按字母顺序输出。

输入描述

> 输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

输出描述

> 顺序输出字符串的所有排列

题目分析

　　这是一个字符串全排列的问题，把全部序列存在TreeSet中默认可得到字典顺序。

> TreeSet  　　　　基于TreeMap实现的SortedSet，可以对Set集合中的元素进行排序，排序后按升序排列元素（缺省是按照自然排序），非线程安全。

思路：

　　固定一个字符串之后，之后再将问题变小，只需求出后面子串的排列个数就可以得出结果，然后依次将后面的字符串与前面的交换，再递归子串的排列结果，最后当所有字符都固定结束递归。

下面这张图很清楚的给出了递归的过程：

[![这里写图片描述](https://camo.githubusercontent.com/5ef7cb004bc0072b24ef062b3085f872974cf463/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313630353233313135383231333836)](https://camo.githubusercontent.com/5ef7cb004bc0072b24ef062b3085f872974cf463/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313630353233313135383231333836)　

> 解法　运行时间：131ms　占用内存：1477k　

```java
import java.util.*;
public class Solution {
    //用于最后返回结果
    ArrayList<String> list  = new ArrayList<>();
    //遍历的时候来存储序列实现排序
    TreeSet<String> set = new TreeSet<>();

    public ArrayList<String> Permutation(String str) {      
       if(str==null || str.length()==0) return list;

       Permutation(str.toCharArray(),0);
       //容器转换
       list.addAll(set);
       return list;
    }
    public void Permutation(char[] s,int index){
        if(s==null ||s.length==0 || index<0 || index>s.length-1)  return ;
        if(index==s.length-1){//递归固定到最后一个位置，把该串加入集合
            set.add(new String(s));
        }else{//固定前index+1个字符，递归后面所有可能的子串
           for(int i = index;i<s.length;i++){
               swap(s,index,i);//交换一次形成一个子串
               Permutation(s,index+1);
               swap(s,i,index);//复原使下次循环产生下一个子串
           }
        }
    }
    public void swap(char[] s,int i,int j){
        char temp = s[i];
        s[i] = s[j];
        s[j] = temp;
    }
}
```

## 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

  思路一：**数组排序后，如果符合条件的数存在，则一定是数组中间那个数**。（比如：1，2，2，2，3；或2，2，2，3，4；或2，3，4，4，4等等） 

  这种方法虽然容易理解，但由于涉及到快排sort，其时间复杂度为**O(NlogN)**并非最优； 

  参考代码如下：

```java
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers)
    {
        // 因为用到了sort，时间复杂度O(NlogN)，并非最优
        if(numbers.empty()) return 0;
         
        sort(numbers.begin(),numbers.end()); // 排序，取数组中间那个数
        int middle = numbers[numbers.size()/2];
         
        int count=0; // 出现次数
        for(int i=0;i<numbers.size();++i)
        {
            if(numbers[i]==middle) ++count;
        }
         
        return (count>numbers.size()/2) ? middle :  0;
    }
};
 
```

  思路二：如果有符合条件的数字，则它出现的次数比其他所有数字出现的次数和还要多。 

  在遍历数组时保存两个值：一是数组中一个数字，一是次数。遍历下一个数字时，若它与之前保存的数字相同，则次数加1，否则次数减1；若次数为0，则保存下一个数字，并将次数置为1。遍历结束后，所保存的数字即为所求。然后再判断它是否符合条件即可。 

  参考代码如下：

```java
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers)
    {
        if(numbers.empty()) return 0;
         
        // 遍历每个元素，并记录次数；若与前一个元素相同，则次数加1，否则次数减1
        int result = numbers[0];
        int times = 1; // 次数
         
        for(int i=1;i<numbers.size();++i)
        {
            if(times == 0)
            {
                // 更新result的值为当前元素，并置次数为1
                result = numbers[i];
                times = 1;
            }
            else if(numbers[i] == result)
            {
                ++times; // 相同则加1
            }
            else
            {
                --times; // 不同则减1               
            }
        }
         
        // 判断result是否符合条件，即出现次数大于数组长度的一半
        times = 0;
        for(int i=0;i<numbers.size();++i)
        {
            if(numbers[i] == result) ++times;
        }
         
        return (times > numbers.size()/2) ? result : 0;
    }
};
```



## 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。

输入描述

> 一个整数n

输出描述

> 斐波那契数列的第n项。

题目分析

什么是斐波那契数列？

　　

> 斐波那契数列（Fibonacci sequence），又称黄金分割数列
>
> 在数学上，斐波纳契数列以如下被以递归的方法定义：**F（0）=0，F（1）=1**，F（n）=F(n-1)+F(n-2)（n≥2，n∈N*）　　
>
> 指的是这样一个数列：0、1、1、2、3、5、8、13、21、34、……　

递归？

　　看到这个的第一想法就是用递归，当ｎ＜２时返回ｎ，ｎ＞＝２时返回ｆ(n-1)+f(n-2), 于是就来试一下...　　

```java
public class Solution {
    public int Fibonacci(int n) {
        if(n<2){
            return n;
        }

        return Fibonacci(n-1)+Fibonacci(n-2);
    }
}
```

> 运行超时:您的程序未能在规定时间内运行结束，请检查是否循环有错或算法复杂度过大。

为什么呢？

　　因为重复计算，比如：

```
ｆ(4) = ｆ(3) + ｆ(2);
    　 = ｆ(2) + ｆ(1) + ｆ(1) + ｆ(0);
    　 = ｆ(1) + ｆ(0) + ｆ(1) + ｆ(1) + ｆ(0);
```

求ｆ(4)就要计算三次ｆ(1)和两次ｆ(0)，显然这是不行的。

> 解法 (动态规划)
>
> 运行时间：27ms     占用内存：629k

```java
public class Solution {
    public int Fibonacci(int n) {
         int i = 0, j = 1;
         for(;n>0;n--){
            j += i;
            i = j-i;
        }
        return i;
    }
}
```

```java
public class Solution {
    public int Fibonacci(int n) {
        int a=1,b=1,c=0;
        if(n<0){
            return 0;
        }else if(n==1||n==2){
            return 1;
        }else{
            for (int i=3;i<=n;i++){
                c=a+b;
                b=a;
                a=c;
            }
            return c;
        }
    }
}
```



根据n的大小，从f(0)=i 和 f(1)=j 从头开始遍历整个序列

> 有f(n)=f(n-1)+f(n-2)  （**n≥2**，n∈N*）
>
> j+=i, 使j成为新的f(n-1)i = j-i ，使i成为f(n-2)
>
> 完成后，返回 f(n-2)

注意：java中

> while(n--)

会报编译错误：

> required: booleanfound: int

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
