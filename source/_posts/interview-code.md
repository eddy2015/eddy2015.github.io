---
title: interview-code
date: 2016-10-26 11:07:03
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

## 二叉树遍历
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