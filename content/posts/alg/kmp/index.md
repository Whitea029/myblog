---
title: "KMP算法-来自leetcode-28的思考" 
date: 2024-06-20T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["alg"] 
author: "Whitea"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "快来看看学了就会忘记的kmp吧" 
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/Whitea029/myblog/blob/main/content"
    Text: "Source Code" # edit text
    appendFilePath: true # to append file path to Edit link
---

本文的思考来自于`leetcode-28.找出字符串中第一个匹配项的下标`[leetcode链接](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/solutions/732236/shi-xian-strstr-by-leetcode-solution-ds6y/)

## 什么是KMP算法
> <font style="color:rgb(32, 33, 34);">在</font>[计算机科学](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)<font style="color:rgb(32, 33, 34);">中，</font>**<font style="color:rgb(32, 33, 34);">克努斯-莫里斯-普拉特</font>**[字符串查找算法](https://zh.wikipedia.org/wiki/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%9F%A5%E6%89%BE%E7%AE%97%E6%B3%95)<font style="color:rgb(32, 33, 34);">（英语：Knuth–Morris–Pratt algorithm，简称为</font>**<font style="color:rgb(32, 33, 34);">KMP算法</font>**<font style="color:rgb(32, 33, 34);">）可在一个</font>[字符串](https://zh.wikipedia.org/wiki/%E5%AD%97%E7%AC%A6%E4%B8%B2)<font style="color:rgb(0, 0, 0);background-color:rgb(248, 249, 250);">S</font><font style="color:rgb(32, 33, 34);">内查找一个词</font><font style="color:rgb(0, 0, 0);background-color:rgb(248, 249, 250);">W</font><font style="color:rgb(32, 33, 34);">的出现位置。一个词在不匹配时本身就包含足够的信息来确定下一个匹配可能的开始位置，此算法利用这一特性以避免重新检查先前配对的</font>[字符](https://zh.wikipedia.org/wiki/%E5%AD%97%E7%AC%A6)<font style="color:rgb(32, 33, 34);">。（引用维基百科）</font>
>

也就是说KMP算法是用来解决字符串匹配问题，即从一个文本串`text`中寻找一个子字符串（模式串）`pattern`，看这个字串是否在主串中出现过。<font style="color:rgb(76, 73, 72);">比如对于 </font>`text='abaacababcac'`<font style="color:rgb(76, 73, 72);"> 和</font> `pattern='ababc'`<font style="color:rgb(76, 73, 72);">，子串是包含在主串中的，同时它在主串中的索引是`5`

## 暴力算法
暴力解法即为两个`for`循环，将两个字符串挨个进行比较

第一个`for`循环对文本串`text`循环，表示开始**对比的起点**

第二个`for`循环对模板串循环，依次从文本串**对比的起点**挨个进行对比，不符合即跳出内层`for`循环，符合那就找到啦

不难看出，这样操作所导致的时间复杂度为`O(m * n)`，即两个字符串长度的乘积，其造成此时间复杂度的**原因**是每一次比对不符合时跳出循环都要重新挨个比对，浪费了大量的时间，而KMP算法正是为了解决此问题。以下为暴力算法的Java代码实现，后文不多赘述

```java
    public int strStr(String text, String pattern) {
            for (int i = 0; i + pattern.length() <= text.length(); i++) {
                    boolean flag = true;
                    for (int j = 0; j < pattern.length(); j++) {
                       if (text.charAt(i + j) != pattern.charAt(j)) {
                       flag = false;
                       break;
                   }
            }
            if (flag) {
                    return i;
                }
            }
            return -1;
    }
```

## KMP算法
在了解使用KMP算法解决字符串匹配问题之前应先了解相关的前置知识。

### 为什么使用KMP算法
KMP算法的聪明之处在于**当出现字符串不匹配问题时候，可以利用已经匹配成功的字符串所提供的一些有用信息将子串移动到某个位置，从该位置开始匹配**，此时的时间复杂度为`O(m + n)`，即两个字符串的长度的和。

实现KMP算法的重点是找出`**next(prefix)**`数组（前缀表），也是算法思想的难点，接下来介绍**到底何为前缀，后缀，何为前缀表**。

### 前缀 后缀
前缀：对于一个字符串而言，**从左到右**包括**首**字母，不包括**尾**字母的所有字串都称之为前缀。

后缀：对于一个字符串而言，**从左到右**不包括**首**字母，包括**尾**字母的所有子串都称之为后缀。

**重点：后缀也是从左向右看！！！这对之后理解算法流程有很大作用**

eg：模板串`aabaaf`中，前缀为`a`,`aa`,`aab`,`aaba`,`aabaa`后缀为`f`,`af`,`aaf`,`baaf`,`abaaf`

### 前缀表
前缀表就是一个数组，一般称之为`next`数组或者`prefix table`

前缀表的功能就是**用来回退的，它记录着当模板串和文本串不匹配的时候，应该回退到文本串的什么地方重新开始匹配**

那前缀表中记录的是什么呢？前缀表记录的是：**该字符串下标**`**i**`**处（包括**`**i**`**）之前的子串，最大相等前后缀的长度**

eg：模板串为`aabaaf`时，

`i = 0`时，子串为`a`，最大相等前后缀为`0`

`i = 1`时，子串为`aa`，最大相等前后缀为`1`

`i = 2`时，子串为`aab`，最大相等前后缀为`0`

`i = 3`时，子串为`aaba`，最大相等前后缀为`1`

`i = 4`时，子串为`aabaa`，最大相等前后缀为`2`

`i = 5`时，子串为`aabaaf`，最大相等前后缀为`0`

所以前缀表为`[0, 1, 0, 1, 2, 0]`

那为什么前缀表能让我们在遇到字符串不匹配的时候快速重新定位新的匹配起点呢？

### 算法流程
下面举例均使用 文本串`aabaabaaf` 模板串`aabaaf`作为样例

依然要采用嵌套循环的方式，**指针**`**i**`指向**文本串**，**指针**`**j**`指向**模板串**

![](images/kmp/001.png)

在第一次匹配的时候，当匹配到字母f的时候发现与文本串的b不匹配，此时就要利用前缀表进行**指针**`**j**`回溯，前缀表怎么看呢？

我们看`aabaaf`的前缀表`[0, 1, 0, 1, 2, 0]`中此时**指针j指向字母的前一位**的字符a对应的前缀表为`2`，那么此时回溯（移动`**j**`**指针**）到**模板串**下标为`2`的地方即字符`b`处重新开始匹配。

![](images/kmp/002.png)

为什么？？？

因为循环在走到f之前已经完全匹配上了前面的`aabaa`，我们又从前缀表中得知aabaa的最长相同前后缀为`aa`，长度为`2`

**这是一个非常好的提示，这告诉我们已经匹配的**`**aabaa**`**子串中，**`**aa**`**已经不用匹配了！**因为文本串中的aabaa的前缀`aa`和模板串中的`aabaa`的后缀`aa`相同！！！

此时将**指针i与指针j对齐**，继续匹配！

![](images/kmp/003.png)

接下来**重复上面的思想，直到完全匹配上。**

显然，KMP算法的实现相比于暴力求解，指针i并没有进行回溯（最多就是原地不动）这是KMP算法的优势。

### 计算前缀表
上面的实现是前缀表的一种实现，但是有一种更常用的前缀表实现方式，即将上面的前缀表整体**减一**，方便**条件的界定和指针j的回溯**

1. 初始化
2. 处理前后缀不相同的情况	
3. 处理前后缀相同的情况

统一减一

```java
public static void getNext(int[] next, String s) {
    
    // 初始化
    int j = -1;
    next[0] = j;
    
    // 对模板串进行for循环
    for (int i = 1; i < s.length(); i++) { // 注意i从1开始（因为0位置已经初始化不用比较）
        
        // 当不匹配的时候的处理，利用while循环
        while (j >= 0 && s.charAt(i) != s.charAt(j + 1)) { // 注意这里是j + 1(因为统一减一)
            j = next[j]; // 指针j回退
        }
        
        // 当匹配的时候的处理
        if (s.charAt(i) == s.charAt(j + 1)) {
            j++;
        }
        
        // 记录前缀表
        next[i] = j;
    }
}
```

统一不减一

```java
public void getNext(int[] next, String s) {

    // 初始化
    int j = 0;
    next[0] = 0;

    // 对模板串进行for循环
    for (int i = 1; i < s.length(); i++) { // 注意i从1开始（因为0位置已经初始化不用比较

        // 当不匹配的时候的处理，利用while循环
        while (j > 0 && s.charAt(i) != s.charAt(j)) { // j要保证大于0
            j = next[j - 1]; // 指针j回退
        }

        // 当匹配的时候的处理
        if (s.charAt(i) == s.charAt(j)) {
            j++;
        }

        // 记录前缀表
        next[i] = j;
    }
}
```

### KMP算法实现
统一减一

```java
public int strStr(String haystack, String needle) {
    if (needle.length() == 0) {
        return 0;
    }
    
    int[] next = new int[needle.length()];
    getNext(next, needle);

    int j = -1; // next数组里记录的起始位置为-1
    for (int i = 0; i < haystack.length(); i++) { // 注意i从0开始
        while (j >= 0 && haystack.charAt(i) != needle.charAt(j + 1)) { // 不匹配
            j = next[j]; // j寻找之前匹配的位置
        }
        if (haystack.charAt(i) == needle.charAt(j + 1)) { // 匹配，j和i同时向后移动
            j++; // i的增加在for循环里
        }
        if (j == needle.length() - 1) { // 文本串haystack里出现了模式串needle
            return i - needle.length() + 1;
        }
    }
    return -1;
}

public void getNext(int[] next, String s) {………}
```

统一不减一

```java
public int strStr(String haystack, String needle) {
    if (needle.length() == 0) {
        return 0;
    }

    int[] next = new int[needle.length()];
    getNext(next, needle);

    int j = 0;
    for (int i = 0; i < haystack.length(); i++) {
        while (j > 0 && haystack.charAt(i) != needle.charAt(j)) {
            j = next[j - 1];
        }
        if (haystack.charAt(i) == needle.charAt(j)) {
            j++;
        }
        if (j == needle.length()) {
            return (i - needle.length() + 1);
        }
    }
    return -1;
}

public void getNext(int[] next, String s) {………}
```

## 总结
以上就是KMP算法的核心原理及实现，当然KMP的实现方法不止上面一种，因此对应的next数组也有所不同，但是算法核心思想已经在上面简要阐述清楚。


