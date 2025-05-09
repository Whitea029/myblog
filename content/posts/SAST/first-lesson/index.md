---
title: "SAST授课-啥是后端，如何学呢 🤔"
date: 2024-10-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["SAST"]
author: "Whitea"
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "2024 SAST.Web第一次授课"
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

## 碎碎念

首先欢迎大家成功通过笔试面试亦或者是免试进入到SASTWeb研发组，祝愿大家在这里能够学习到自己感兴趣的知识，结识更多志同道合的朋友。但是要记住，**加入SAST，意味着更大的舞台，更多的资源，但并不代表你很厉害。** 沉下心去学，学习的路真的还挺长的。😵

## 什么是后端

> 后端是指**一个软件系统的服务器端，也称作服务器端**。 它是指在一个软件系统中，负责处理数据存储、业务逻辑处理和与**前端**交互的一部分。 后端通常包括**数据库、服务器、应用程序和其他相关组件。**

比如：

- b站这个页面就是前端展示出来的，但是着其中的每一个词条，视频，数据来源都是来源于后端数据库，然后经过后端程序一些算法逻辑传递给前端进行展示。

![](images/first-lesson/001.png)

- GitHub（全球最大同性交友平台）的登录界面，这个界面就是前端展示的，而当我们输入Username和Password并点击Sign in之后，数据会传递给后端服务器处理，只有当后端服务器校验通过后，才会给前端返回正确的信息，前端才会跳转主页。

![](images/first-lesson/002.png)

## 后端开发用什么语言

[TIOBE Index - TIOBE](https://www.tiobe.com/tiobe-index/)

![](images/first-lesson/003.png)

在国内，Java仍是后端开发的主流语言，凭借其成熟的生态和广泛的社区支持，成为众多企业的首选。在招聘市场上，Java技能依然是后端开发岗位的主要要求。然而，随着云原生技术的发展，Go和Rust等新兴语言也逐渐崛起，特别是在字节跳动、腾讯等大厂，GoLang已经被广泛应用于后端开发。同时，Python、C#和Node.js等语言在特定场景下也发挥着重要作用，丰富了后端开发的语言选择。

[主流后端开发语言：JAVA、C、C++、GO、PYTHON对比](https://blog.csdn.net/wnm23/article/details/137127781)

[Top 7 Programming Languages for Backend Web Development - GeeksforGeeks](https://www.geeksforgeeks.org/top-7-programming-languages-for-backend-web-development/)

我们目前的授课计划前期依旧是以Java为主。

## 学习后端然后干嘛

这里有两种不同的情况：一种是你对后端开发感兴趣，想初步了解后端开发的实际工作内容；另一种是你计划在大学期间持续深入学习后端开发，最终达到企业招聘的标准，未来寻找后端开发相关的岗位。

> 强烈建议大家尽早规划好自己的大学四年和职业发展方向，无论是考研、保研、本科就业、研究生就业，还是考公、从事科研。目前大部分人最终都会走向就业市场，因此在大学四年内，专注于一个垂直领域并持续深入学习，会让你在校招中更具竞争力。（不要以为找工作还很遥远，现实是如果你仅仅按部就班跟随学校课程，毕业后找到理想工作的难度会非常大。很多人选择考研，其实只是为了暂时逃避就业的压力）

此外，建议大家了解一下当前后端开发工程师的行业薪资水平，以便更好地衡量自己的目标和努力方向。

[南京Java工资待遇_收入水平-BOSS直聘 (zhipin.com)](https://www.zhipin.com/salaryxc/c101190100_p100101.html)

![](images/first-lesson/004.png)

根据许多学长的经验，**对于前后端开发方向的同学来说**，考研的意义并不如就业那么明显（考研三年所积累的知识和三年实际工作经验相比，差距较大）。很多学长在大三时会主动寻找相关岗位的实习机会，这段实习经历往往会让他们在大四的秋招中对那些没有实习经历的同学形成降维打击，因为企业更倾向于招聘有实习经验的学生。

但无论你是打算简单了解后端开发的工作流程，还是计划深入学习并以此为职业发展方向，跟着我们的课程学习都是非常合适的选择！通过这些课程，你可以逐步积累专业知识，为未来的实习或就业打下坚实基础。

> 你可以不来上课，可以自学，但是你的进度不可以比我们的课程进度慢，否则你就慢了（

## 学习路线

这里粘贴一份网上的学习路线，还比较全，里面也给到了很多学习资料的推荐。

对于纯小白而言（比如我）刚开始是不适合看文档进行学习的，推荐大家跟着一门视频课程进行学习，但在后期要逐渐拓宽自己接受知识的途径，学会看文档高效学习。

[codefather/学习路线/Java学习路线(github.com)](https://github.com/liyupi/codefather/blob/main/%E5%AD%A6%E4%B9%A0%E8%B7%AF%E7%BA%BF/Java%E5%AD%A6%E4%B9%A0%E8%B7%AF%E7%BA%BF%20by%20%E7%A8%8B%E5%BA%8F%E5%91%98%E9%B1%BC%E7%9A%AE.md)

[The Roadmap to Becoming a Backend Java Developer | by Hussein Shamas | Medium](https://medium.com/@HusseinShamas/the-roadmap-to-becoming-a-backend-java-developer-d863b2c4e3b3)

[计算机教育中缺失的一课 · the missing semester of your cs education (missing-semester-cn.github.io)](https://missing-semester-cn.github.io/)

后端的学习在初期与前端有较大区别。前端学习一点知识，就可以做出一个小作品，看到即时的学习成果，写个小项目也相对容易。但后端并没有这种即时反馈的成就感，特别是在学习Spring Boot之前，可能无法独立完成项目。这就意味着，在前期较长一段时间里，你可能会感到迷茫，不确定自己在学什么，也不清楚学这些有什么用。然而，这个阶段正是需要坚持的关键！我们的授课计划已经充分考虑到了这一点。在本学期结束时，我们会帮助大家掌握开发一个小型后端CRUD项目的能力，逐步把理论与实践结合，让大家看到自己的学习成果，打破迷茫期，增强信心。

## 校内资源

- SAST的github里面有不少项目，当你具备了一定的能力，我们会带你参与其中的开发。[NJUPT SAST (github.com)](https://github.com/NJUPT-SAST)
- WoC（Winter of Code）寒假大家会有一次项目考核，通过他，基本上你就可以开始跟着我们做项目啦！（大家加油！每年的情况是4到5个人通过，大部分的人因为上半学期因为各种各样的原因没有持续学习而放弃）
- SoC（Summer of Code）暑假大家也会进行一次团队项目开发，这个项目就已经是科协的项目啦。（也可能是项目考核）
- 青柚工作室后端组，如果你学的不错，WoC之后，工作室会进行招新，通过面试的你可以加入到青柚工作室，接触到更多的更好的优质项目。[青柚工作室 - 用热爱创造无限可能 (njupt.edu.cn)](https://qingyou.njupt.edu.cn/)

## Java的发展历史

上个世纪90年代，消费类电子产品越来越受人追捧，为了提升消费类电子产品的智能化程度。Sun公司为了抢占市场先机，在1991年成立了一个称为Green的项目小组，专攻计算机在家电产品上的嵌入式应用。由于可以用的资源极其有限。很多成员发现C++太复杂以至很多开发者经常错误使用。而且C++缺少垃圾回收系统，还有可移植的安全性、分布程序设计、和多线程功能。为了解决这个麻烦的问题，他们决定开发出一种新的语言 **他就是java的前身：**

— — **Oak（橡树）**

对，java的前身就是Oak。之所以起Oak这个名字，是因为团队对这门语言的首先期望就是健壮，而当时团队的办公室外刚好有一颗橡树，高大，健壮。受此启发，Oak就被用作Java语言的第一个名字，寓意着这是一门**简单、健壮的语言。**

而后来因为版权的问题，Oak不得不改名，但关于改成什么，大家并没有灵感，**直到有一天团队的几个主要成员在一起喝咖啡，正品尝一种来自爪哇岛（Java）的咖啡**。此时，其中的一个工程师突然想到，他们之所以要创建一门新语言，在很大程度上，就是期望把程序员从工程的泥潭中拯救出来，从而让大家有闲暇的时间来品一杯美味的咖啡。于是**Oka就被改名为Java**，**寓意着程序员如果用Java开发，从此之后能过上从容、惬意的生活(不是)。**

![](images/first-lesson/005.png)

## Java主要特性

- **Java 语言是简单的：**

Java 语言的语法与 C 语言和 C++ 语言很接近，使得大多数程序员很容易学习和使用。另一方面，Java 丢弃了 C++ 中很少使用的、很难理解的、令人迷惑的那些特性，如操作符重载、多继承、自动的强制类型转换。特别地，Java 语言不使用指针，而是引用。并提供了自动分配和回收内存空间，使得程序员不必为内存管理而担忧。

- **Java 语言是面向对象的：**

Java 语言提供类、接口和继承等面向对象的特性，为了简单起见，只支持类之间的单继承，但支持接口之间的多继承，并支持类与接口之间的实现机制（关键字为 implements）。Java 语言全面支持动态绑定，而 C++语言只对虚函数使用动态绑定。总之，Java语言是一个**纯的面向对象程序设计语言**。

- **Java 是高性能的：**

与那些解释型的高级脚本语言相比，Java 的确是高性能的。事实上，Java 的运行速度随着 JIT(Just-In-Time）编译器技术的发展越来越接近于 C++。

- **Java具有完整并不断发展的生态**

Java发展时间长,受到全世界众多程序员的青睐,建立了完整的生态。你在编写Java程序时遇见的bug永远有人比你先遇见,所以你可以相关网站上寻找解答。Java还有强大的spring框架,在不久的学习后你们也会接触到

## 了解 JDK,SDK,Jar,JVM,JRE

**1. JDK (Java Development Kit)**

- **JDK** 是用于开发Java应用程序的完整软件开发工具包。它包括了编译器 (`javac`)、Java运行时环境 (JRE)、标准类库、调试器等开发工具。JDK 是编写、编译和运行Java程序的必备工具。
- **用途**：用于编写、编译和调试Java程序。

**2. SDK (Software Development Kit)**

- **SDK** 是一个通用术语，指的是用于特定平台或编程语言的开发工具集合。对于Java来说，JDK 就是 Java 的 SDK。除了 Java 以外，其他语言或平台（如 Android）也有自己的 SDK，它们提供了开发应用程序所需的库、工具和文档。
- **用途**：为特定平台提供开发工具。

**3. Jar (Java Archive)**

- **JAR** 文件是一种用于打包多个Java类文件及相关资源（如图片、配置文件等）的压缩文件格式。JAR 文件常用于分发Java应用程序或库。JAR文件还可以包含一个`MANIFEST.MF`文件，用于指定入口点的主类，使得JAR文件可以直接运行。
- **用途**：打包、分发和部署Java程序或库。

**4. JVM (Java Virtual Machine)**

- **JVM** 是 Java 虚拟机，它负责执行Java字节码，使Java程序能够在不同的平台上运行。JVM 屏蔽了底层操作系统的细节，提供了跨平台的能力，即"一次编写，处处运行"。JVM 负责内存管理、垃圾回收和线程管理等。
- **用途**：执行Java字节码，实现**跨平台运行**。

**5. JRE (Java Runtime Environment)**

- **JRE** 是 Java 运行时环境，包含 JVM 和 Java 的核心类库，但不包括开发工具（如编译器）。JRE 提供了运行Java应用程序所需的环境，但不能用于编写或编译Java程序。用户只需要安装JRE就能运行Java应用程序。
- **用途**：提供运行Java程序的环境。

## Hello SAST

现在我来编写第一个Java程序，以IDEA为例

![](images/first-lesson/006.png)

```java
    public class HelloWorld {
        public static void main(String[] args) {
            System.out.println("hello,world!");
           System.out.println("hello,SAST");
        }
    }
```

编写完以上代码后,按下IDEA快捷键`ctrl + shift + f10` 或者右键点击运行就能输出想要的结果啦！

### 上述代码的解释

- `public class HelloWorld{}`

这里的HelloWorld是类名，必须与文件名保持一致。

- `public static void main(String[] args) {}`

**main方法是一切程序的入口**，一个类只能有一个main方法，可通过键入`main`或`psvm`加`enter`快捷打出。`void`表示这个方法没有返回值。

## 作业

预习Java基础语法，数据类型，运算符，修饰符，注释，数组，流程控制

> 作为一名后端开发,有文章存在任何问题都可以联系我，当然也欢迎与我交流技术相关的问题，感谢你的阅读🤗