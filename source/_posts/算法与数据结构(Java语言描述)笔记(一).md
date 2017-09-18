---
title: 算法与数据结构(Java语言描述)笔记(一)
date: 2017-09-18 19:21:24
tags:  [数据结构,算法,线性表,字符串]
categories: 算法与数据结构(Java语言描述)笔记
---
# <center>线性表(linear list)</center>

## 链表(linked list)
### 从表尾到表头逆向建立单链表(头插法)
```java
public LinkedList(){                    //此方法将一个字符串存储在单链表中
    char ch;            
    Lnode p;                            //待插入的节点
    h = new Lnode();                    //建立头结点
    h.next = null;                      //使头结点的指针域为空
    String str = scanner.nextLine();    //扫描输入
    int i = 0;
    while(i<str.length()){
        ch = str.charAt(i);             
        p = new Lnode();                //建立一个新节点p
        p.data = ch;                    //将ch赋给p的数据域
        p.next = h.next;                //令p指向h的下一个结点
        h.next = p;                     //令h指向p
        i++;
        
    }
}
```

### 从表头到表尾正向建立单链表(尾插法)
尾插法示意图

![](/images/weichafa.png)

```java
public LinkedList(){
    Lnode p,t;
    char ch;
    h = new Lnode();
    h.next = null;
    t = h;
    String str = scanner.nextLine();
    int i = 0;
    while(i<str.length()){
        ch = str.charAt(i);
        p = new Lnode();
        p.data = ch;
        p.next = null;
        t.next = p;
        t = p;
        i++;
    }
}
```

## 字符串(String)

### 求子串算法
```java

public static String SubStr(String sub,String s,int start,int len){
    if(sub.length() == 0){
        return null;
    }
    if((start >= 0 && start < s.length())&&(len >= 0$$len <= s.length() - start)){
        char[] temp = s.toCharArray();
        for(int i = start; i<start+len; i++){
            sub = sub + temp[i];
        }
    }
    return sub;
}
```
### 模式匹配

如果子串T在主串中存在，则返回存在的位置，如果不存在，则返回-1。
#### 简单模式匹配
 从主串的第pos位置字符开始和模式子串字符比较，如果相等，则继续逐个比较后续字符；否则从主串的下一个字符起再重新和模式子串的字符比较。直到找到匹配字符串或者是主串结尾。
 
 简单匹配示意图
 ![](images//bruteForceMatch.jpg)
```java
public static int bruteForceStringMatch(String source, String pattern)
    {
        int slen = source.length();
        int plen = pattern.length();
        char[] s = source.toCharArray();
        char[] p = pattern.toCharArray();
        int i = 0;
        int j = 0;
        
        if(slen < plen)
            return -1;                        //如果主串长度小于模式串，直接返回-1，匹配失败
        else
        {
            while(i < slen && j < plen)        
            {
                if(s[i] == p[j])            //如果i,j位置上的字符匹配成功就继续向后匹配
                {
                    ++i;
                    ++j;
                }
                else
                {
                    i = i - (j -1);            //i回溯到主串上一次开始匹配下一个位置的地方
                    j = 0;                    //j重置，模式串从开始再次进行匹配
                }
            }
            if(j == plen)                    //匹配成功
                return i - j;
            else
                return -1;                    //匹配失败
        }
    }               
}

```
#### KMP算法 <font color="red">(未掌握)</font>
改进了简单模式匹配中i回溯的操作，KMP算法中当一趟匹配过程中出现字符比较不等时，不直接回溯i，而是利用已经得到的“部分匹配”的结果将模式串向右移动（j-next[k]）的距离。

 KMP算法示意图
 ![](/images/KMPMatch.jpg)
```java
public static int kmpStringMatch(String source, String pattern)
    {
        int i = 0;
        int j = 0;
        char[] s = source.toCharArray();
        char[] p = pattern.toCharArray();
        int slen = s.length;
        int plen = p.length;
        int[] next = getNext(p);
        while(i < slen && j < plen)
        {
            if(j == -1 || s[i] == p[j])
            {
                ++i;
                ++j;
            }
            else
            {
                //如果j != -1且当前字符匹配失败，则令i不变，
                //j = next[j],即让pattern模式串右移j - next[j]个单位
                j = next[j];
            }
        }
        if(j == plen)
            return i - j;
        else
            return -1;
    }

```
<b>next函数值的实现</b>
```java
private static int[] getNext(char[] p)
    {
        /**
         * 已知next[j] = k, 利用递归的思想求出next[j+1]的值
         * 1.如果p[j] = p[k]，则next[j+1] = next[k] + 1;
         * 2.如果p[j] != p[k],则令k = next[k],如果此时p[j] == p[k],则next[j+1] = k+1
         * 如果不相等，则继续递归前缀索引，令k=next[k],继续判断，直至k=-1(即k=next[0])或者p[j]=p[k]为止
         */
        int plen = p.length;
        int[] next = new int[plen];
        int k = -1;
        int j = 0;
        next[0] = -1;                //这里采用-1做标识
        while(j < plen -1)
        {
            if(k == -1 || p[j] == p[k])
            {
                ++k;
                ++j;
                next[j] = k;
            }
            else
            {
                k = next[k];
            }
        }
        
        return next;
    }
```
#### 参考资料: [字符串的模式匹配](http://www.cnblogs.com/jiaohanhan/p/6654874.html),[字符串的模式匹配](http://www.cnblogs.com/lufangtao/p/3245647.html)

### 频率统计
统计每个英文字母在一个字符串中出现的次数
```java
public int[] Letter_Frequency(String text){
    String str1 = "abcdefghijklmnopqrstuvwxyz";
    String str2 = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    int[] text_Frequency = new int[26];
    int tint = 0;
    String c = "";
    for ( int i = 0; i < text.length; i++){
        c = text.substring(i,i+1);
        ting = str1.indexOf(c);
        if(tint == -1 ){
            tint = str2.indexOf(c);
        }
        if(tint >= 0 ){
            text_Frequency[ting]++;
        }
    }
    return text_Frequency;
}

```
