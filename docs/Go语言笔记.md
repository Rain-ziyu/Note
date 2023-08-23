
# Golang引入

【1】简介： 

​    Go（又称 Golang）是 Google 的 Robert Griesemer，Rob Pike 及 Ken Thompson 开发的一种计算机编程语言语言。
【2】设计初衷： 

​    Go语言是谷歌推出的一种的编程语言，可以在不损失应用程序性能的情况下降低代码的复杂性。谷歌首席软件工程师罗布派克(Rob
Pike)说：我们之所以开发Go，是因为过去10多年间软件开发的难度令人沮丧。派克表示，和今天的C++或C一样，Go是一种系统语言。他解释道，"使用它可以进行快
开发，同时它还是一个真正的编译语言，我们之所以现在将其开源，原因是我们认为它已经非常有用和强大。" 


1)  计算机硬件技术更新频繁，性能提高很快。目前主流的编程语言发展明显落后于硬件，不能合理利用多核多CPU的优势提升软件系统性能。 

2)  软件系统复杂度越来越高，维护成本越来越高，目前缺乏一个足够简洁高效的编程语言。 

3)  企业运行维护很多c/c++的项目，c/c++程序运行速度虽然很快，但是编译速度确很慢，同时还存在内存泄漏的一系列的困扰需要解决。 


【3】应用领域: 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809151701.png)




【4】用go语言的公司： 

1、Google 

这个不用多做介绍，作为开发Go语言的公司，当仁不让。Google基于Go有很多优秀的项目，比如：https://github.com/kubernetes/kubernetes ，大家也可以在Github上 https://github.com/google/ 查看更多Google的Go开源项目。 

2、Facebook 

Facebook也在用，为此他们还专门在Github上建立了一个开源组织facebookgo，大家可以通过https://github.com/facebookgo访问查看facebook开源的项目，比如著名的是平滑升级的grace。 

3、腾讯 

腾讯作为国内的大公司，还是敢于尝试的，尤其是Docker容器化这一块，他们在15年已经做了docker万台规模的实践，具体可以参考http://www.infoq.com/cn/articles/tencent-millions-scale-docker-application-practice 。 

主要职责是： 

负责腾讯游戏蓝鲸平台后台开发工作 

负责容器相关的开发工作 

和蓝鲸平台，容器开发有关。腾讯作为主要使用C/C++的公司，使用Go会方便很多，也有很多优势，不过日积月累的C/C++代码很难改造，也不敢动，所以新业务会在Go方面尝试。 

4、百度 

目前所知的百度的使用是在运维这边，是百度运维的一个BFE项目，负责前端流量的接入。他们的负责人在2016年有分享，大家可以看下这个
http://www.infoq.com/cn/presentations/application-of-golang-in-baidu-frontend . 

其次就是百度的消息系统，从其最近的Golang招聘介绍就可以看出来. 

负责公司手百消息通讯系统服务器端开发及维护 

5、京东 

京东云消息推送系统、云存储，以及京东商城等都有使用Go做开发。 

6、小米 

小米对Golang的支持，莫过于运维监控系统的开源，也就是 http://open-falcon.com/ 。 

此外，小米互娱、小米商城、小米视频、小米生态链等团队都在使用Golang。 

7、360 

360对Golang的使用也不少，一个是开源的日志搜索系统Poseidon，托管在Github上，https://github.com/Qihoo360/poseidon. 

还有360的推送团队也在使用，他们还写了篇博文在Golang的官方博客上 https://blog.golang.org/qihoo。 

360直播在招聘Golang开发工程师。 

美团、滴滴、新浪、阿里、京东以及七牛等。一般的选择，都是选择用于自己公司合适的产品系统来做，比如消息推送的、监控的、容器的等，Golang特别适合做网络并发的服务，这是他的强项，所以也是被优先用于这些项目。 




【5】前景：   

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809152134.png)




----------------------------------------

# Golang简史

【1】开发团队： 

罗伯特·格瑞史莫（Robert Griesemer），罗勃·派克（Rob Pike）及肯·汤普逊（Ken
Thompson）于2007年9月开始设计Go，稍后Ian Lance Taylor、Russ Cox加入项目。 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809152158.png)




【2】Go语言发展简史 

2007年，谷歌工程师Rob Pike, Ken Thompson和Robert Grisemer开始设计一门全新的语言，这是Go语言的最初原型。 

2009年11月，Google将Go语言以开放源代码的方式向全球发布。  

2015年8月，Go1.5版发布，本次更新中移除了"最后残余的c代码"    

2017年2月,Go语言Go 1.8版发布。 

2017年8月，Go语言Go 1.9版发布。 

2018年2月，Go语言Go1.10版发布。 

2018年8月，Go语言Go1.11版发布。 

2019年2月，Go语言Go1.12版发布。 

2019年9月，Go语言Go1.13版发布。 

2020年2月，Go语言Go1.14版发布。 

2020年8月，Go语言Go1.15版发布。 

....一直迭代 

【2】Go语言的吉祥物 - 金花鼠Gordon。 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809152217.png)



----------------------------------------

# 开发工具介绍

【1】工具介绍: 

1) visual studio code， Microsoft产品(简称VSCode):一个运行于Mac Os、Windows和Linux
之上的，默认提供Go语言的语法高亮，安装Go语言插件，还可以支持智能提示，编译运行等功能。 

2) Sublime
Text,可以免费使用，默认也支持Go代码语法高亮,只是保存次数达到一定数量之后就会提示是否购买，点击取消继续用，和正式注册版本没有任何区别 

3) Vim: Vim是从vi发展出来的一个文本编辑器,代码补全、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用 

4) Emacs : Emacs传说中的神器，她不仅仅是一个编辑器，因为功能强大，可称它为集成开发环境 

5) Eclipse IDE工具，开源免费，并提供GoEclipse插件 

6) LitelDE，LitelDE是一款专门为Go语言开发的跨平台轻量级集成开发环境（IDE），是中国人开发的。 

7) JetBrains公司的产品:PhpStrom、WebStrom和PyCharm等IDE工具，都需要安装Go插件。 




【2】VSCode的安装: 

下载vscode安装软件 

https://code.visualstudio.com/download 


【3】安装略


【4】使用VSCode： 

（1）双击打开 

（2）在盘符建立一个文件夹：gocode 

（3）在VSCode中打开文件夹： 

（4）创建go文件： 

（5）开始编写代码： 

```go
package main  //声明文件所在的包，每个go文件必须有一个所在的包
import "fmt"  //引入程序需要的包，为了使用包下的函数
func main(){ //程序主函数
	fmt.Println("helloworld")
}
```


（6）注意保存代码 


----------------------------------------

# 开发环境搭建

【1】搭建Go开发环境 - 安装和配置SDK 

基本介绍: 

1) SDK的全称(Software Development Kit 软件开发工具包) 

2) SDK是提供给开发人员使用的，其中包含了对应开发语言的工具包。 


【2】SDK下载 

1) Go语言的官网为: golang.org ,无法访问，需要翻墙。 

2) SDK下载地址 : Golang中文社区：[https://studygolang.com/dl](https://studygolang.com/dl) 

【3】安装SDK： 

请注意：安装路径不要有中文或者特殊符号如空格等 

SDK安装目录建议:一般我安装在d:/golang_sdk安装时 , 基本上是傻瓜式安装，解压就可以使用 


（1）解压zip： 

go整个目录就是sdk 


（2）go目录下： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809152800.png)


----------------------------------------

# DOS命令讲解

【1】DOS操作系统 

--Microsoft公司推出的操作系统。（在windows之前的操作系统） 

--DOS是英文"Disk Operating System"的缩写,其中文含意是"磁盘操作系统". 

--DOS是单用户、单任务的操作系统.（只能执行一个任务） 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809153055.png)

【2】DOS命令 

--在windows中，我们通过鼠标菜单等来操作系统，而在dos操作系统中，要通过dos命令来操作系统。 

--是DOS操作系统的命令，是一种面向磁盘的操作命令， 

--不区分大小写。 

【3】命令学习： 

windows给我们保留了类似dos系统的操作界面，可以直接操作磁盘！ 

dos 也是一种
[操作系统](https://www.baidu.com/s?wd=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)
，是在windows出现以前用的，后来windows出来后基本没人用了，但是当windows崩溃的时候，还是要的dos方式解决，它是一种纯命令方式，cmd其实就是在windows状态下进入dos方式。

控制命令台：`win+r--->cmd` 


【4】具体dos命令： 

（1）切换盘符： c:  d:  e:   大小写没有区分 

（2）显示详细信息：dir 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809154840.png)




（3）改变当前目录：cd 


（4） 

. 当前目录 

..  代表上一层目录 


（5）清屏：cls 

（6）切换历史命令：上下箭头  

（7）补全命令： tab按键 

（8）创建目录：md 

删除目录：rd 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809154937.png)

（9）复制文件命令：copy: 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809154954.png)

（10）删除文件：del 

del后面如果接的是文件夹/目录：那么删除的就是这个文件夹下的文件，而不是文件夹 




![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155006.png)

----------------------------------------

# 测试SDK环境搭建成功

【1】进入控制命令台： win+R -->cmd 

【2】证明SDK环境成功： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155134.png)




【3】如果我想要在任意的路径下执行某个命令，需要将这个命令所在的目录配置到环境变量path中去 

   将命令“注册”到当前的计算机中： 

   解决如下错误： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155157.png)

出错原因：就是没有配置环境变量，然后你想在任意的路径下执行go，不行 




【4】解决办法：配置path环境变量： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155252.png)




【5】再次验证path是否好用：（注意：控制命令台需要重启）


----------------------------------------

# 第一段程序：HelloWorld快速入门

【1】go基本目录结构： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155450.png)

【2】在VSCode下写代码：在VSCode中打开上面的基本目录： 

【3】创建go源文件： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155534.png)

【4】开始写代码： 第一个HelloWorld : 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155610.png)

【5】对源文件test.go进行编译：go build 


![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155738.png)

【6】执行操作： 

```shell
.\test.exe
```

【7】通过go run直接可以帮我们编译 执行 源文件： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155911.png)


----------------------------------------

# Golang执行流程

【1】执行流程分析： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809155938.png)




【2】上述两种执行流程的方式区别 

1)在编译时，编译器会将程序运行依赖的库文件包含在可执行文件中，所以，可执行文件 变大了很多。 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809160055.png)

2)如果我们先编译生成了可执行女件，那么我们可以将该可执行文件拷贝到没有go开发环 境的机器上，仍然可以运行 

3)如果我们是直接go run go源代码，那么如果要在另外一个机器上这么运行，也需要go 开发环境，否则无法执行。 

4)go run运行时间明显要比第一种方式  长一点点 

【3】编译注意事项： 

编译后的文件可以另外指定名字： 

![](https://prod.huayu.asia:9000/picgo/2023/08/09/20230809160203.png)


----------------------------------------

# 语法注意事项

（1）源文件以"go"为扩展名。 

（2）程序的执行入口是main()函数。 

（3）严格区分大小写。 

（4）方法由一条条语句构成，每个语句后不需要分号(Go语言会在每行后自动加分号)，这也体现出Golang的简洁性。 

（5）Go编译器是一行行进行编译的，因此我们一行就写一条语句，不能把多条语句写在同一个，否则报错 

（6）定义的变量或者import的包如果没有使用到，代码不能编译通过。 

（7）大括号都是成对出现的，缺一不可 

----------------------------------------

# 注释

【1】注释的作用： 

用于注解说明解释程序的文字就是注释，注释提高了代码的阅读性; 

注释是一个程序员必须要具有的良好编程习惯。 

将自己的思想通过注释先整理出来，再用代码去体现。 

【2】Golang中注释类型： 

Go支持c语言风格的/**/块注释，也支持c++风格的//行注释。行注释更通用，块注释主要用于针对包的详细说明或者屏蔽大块的代码 

（1）行注释 //     VSCode快捷键：ctrl+/  再按一次取消注释 

（2）块注释（多行注释） /**/        VSCode快捷键：shift+alt+a 再按一次取消注释 

         注意：块注释中不可以嵌套块注释 

提示：官方推荐使用行注释 // 


----------------------------------------

# 代码风格

【1】注意缩进 

向后缩进：tab 

向前取消缩进：shift+tab 

通过命令完成格式化操作： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810095946.png)

【2】成对编程 {} （） “”  ‘’  

【3】运算符两边加空白 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100101.png)

【4】注释：官方推荐行注释// 

【5】以下代码是错误的： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100034.png)

原因：go的设计者想要开发者有统一的代码风格，一个问题尽量只有一个解决方案是最好的 

【6】行长约定： 

一行最长不超过80个字符，超过的请使用换行展示，尽量保持格式优雅 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100132.png)

----------------------------------------

# API

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100201.png) 

Go语言提供了大量的标准库，因此 google
公司也为这些标准库提供了相应的API文档，用于告诉开发者如何使用这些标准库，以及标准库包含的方法。官方位置：https://golang.org 


Golang中文网在线标准库文档: https://studygolang.com/pkgdoc 


![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100221.png)




函数对应的源码查看： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100238.png)

也可以使用离线API： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100306.png)

----------------------------------------

# 变量

【1】变量的引入： 

一个程序就是一个世界 

不论是使用哪种高级程序语言编写程序,变量都是其程序的基本组成单位， 


【2】变量的介绍： 

变量相当于内存中一个数据存储空间的表示 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100442.png)

【3】变量的使用步骤： 

1.声明 

2.赋值 

3.使用   

PS：看到VSCode的目录结构： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810100458.png)



【4】代码练习： 


1. ```go
     package main
     import "fmt"
     func main(){
     //1.变量的声明
     var age int
     //2.变量的赋值
     age = 18
     //3.变量的使用
     fmt.Println("age = ",age);
     //声明和赋值可以合成一句：
     var age2 int = 19
     fmt.Println("age2 = ",age2);
     // var age int = 20;
     // fmt.Println("age = ",age);
       /*变量的重复定义会报错：
      
       # command-line-arguments
      
       .\main.go:16:6: age redeclared in this block
      
       previous declaration at .\main.go:6:6
      
       */
      
   //不可以在赋值的时候给与不匹配的类型
   
    var num int = 12.56
   
    fmt.Println("num = ",num);
   
    }
   ```


【5】变量的4种使用方式： 

【6】支持一次性声明多个变量（多变量声明） 

1. ```go
     package main
     import "fmt"
     //全局变量：定义在函数外的变量
     var n7 = 100
     var n8 = 9.7
     //设计者认为上面的全局变量的写法太麻烦了，可以一次性声明：
     var (
     n9 = 500
     n10 = "netty"
     )
     func main(){
     //定义在{}中的变量叫：局部变量
     //第一种：变量的使用方式：指定变量的类型，并且赋值，
     var num int = 18
     fmt.Println(num)
     //第二种：指定变量的类型，但是不赋值，使用默认值 
     var num2 int
     fmt.Println(num2)
     //第三种：如果没有写变量的类型，那么根据=后面的值进行判定变量的类型 （自动类型推断）
     var num3 = "tom"
     fmt.Println(num3)
     //第四种：省略var，注意 := 不能写为 =   
     sex := "男"
     fmt.Println(sex)
     fmt.Println("---------------------------------------------------------------
        \--")
     //声明多个变量：
     var n1,n2,n3 int
     fmt.Println(n1)
     fmt.Println(n2)
     fmt.Println(n3)
     var n4,name,n5 = 10,"jack",7.8
     fmt.Println(n4)
     fmt.Println(name)
     fmt.Println(n5)
     n6,height := 6.9,100.6
     fmt.Println(n6)
     fmt.Println(height)
     fmt.Println(n7)
     fmt.Println(n8)
     fmt.Println(n9)
     fmt.Println(n10)
     } 
   ```


----------------------------------------

# 数据类型

变量的数据类型： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810104410.png)


----------------------------------------

## 基本数据类型


----------------------------------------

### 扩展：进制和进制转换

【1】进制的介绍： 

十进制整数，如：99, -500, 0 

八进制整数，要求以 0 开头，如：015 

十六进制数，要求 0x 或 0X 开头，如：0x15 

二进制：要求0b或者0B开头，如：0b11 

几进制：就是逢几进1的问题： 


平时实际生活中用的最多的是：十进制 

计算机用二进制最多  


![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810104523.png)

【2】二进制转换为十进制： 

二进制： 1101 




    1*2^3  +   1*2^2   +  0*2^1  +     1*2^0 

=    8         +      4       +     0       +      1 

=  13  







【3】十进制转换为二进制： 

十进制  13    

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810105845.png)




【4】八进制转换十进制： 

八进制： 16 


1*8^1 +   6*8^0 

=   8     +  6 

=14 



【5】十进制转换为八进制： 

十进制14： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810105913.png)




【6】八进制转换为十六进制： 




把十进制当做一个中转站： 




八进制---》十进制---》十六进制 







实际上根本不用自己转换这么麻烦：我们可以直接用系统中提供给我们的计算器： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810105929.png)



----------------------------------------

### 整数类型

【1】整数类型介绍： 

简单的说，就是用于存放整数值的，比如10,-45,6712等等。 


【2】有符号整数类型： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810110013.png)

PS:127怎么算出来的？ 

01111111 -->二进制 ---》转为十进制： 

   1*2^6   +   1*2^5  +  1*2^4  +   1*2^3  +   1*2^2  +   1*2^1  +    1*2^0 

 = 64      +   32         +    16          +   8        +      4       +    2  
  \+   1 

= 127 

PS：-128怎么算出来的？ 

10000000 --->二进制 --->一看就是个负数  


        10000000 --》负数的二进制 

减1：01111111 

取反：10000000     ---》得到一个正数    2^7 = 128 

加负号：-128 

代码测试超出范围： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810110041.png)

【3】无符号整数类型： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810110105.png)

表数范围的边界计算： 

11111111= 2^7+127 = 128 + 127 = 255 

00000000 = 0  

超出边界报错： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810110125.png)

【4】其他整数类型： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810110152.png)

PS：Golang的整数类型，默认声明为int类型 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810110223.png)







PS :变量占用的字节数： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810110509.png)


【5】这么多整数类型，使用的时候该如何选择呢？ 

Golang程序中整型变量在使用时,遵守保小不保大的原则, 

即:在保证程序正确运行下,尽量使用占用空间小的数据类型 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810110652.png)


----------------------------------------

### 浮点类型

【1】浮点类型介绍： 

简单的说，就是用于存放小数值的，比如3.14、0.28、-7.19等等。   

【2】浮点类型种类： 

![image-20230810110729144](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230810110729144.png)

PS:底层存储空间和操作系统无关 

PS：浮点类型底层存储：符号位+指数位+尾数位，所以尾数位只是存了 一个大概，很可能会出现精度的损失。 


【3】代码： 

1. ```go
     package main
     import "fmt"
     func main(){
     //定义浮点类型的数据：
     var num1 float32 = 3.14
     fmt.Println(num1)
     //可以表示正浮点数，也可以表示负的浮点数
     var num2 float32 = -3.14
     fmt.Println(num2)
     //浮点数可以用十进制表示形式，也可以用科学计数法表示形式  E 大写小写都可以的  
     var num3 float32 = 314E-2 
     fmt.Println(num3)
     var num4 float32 = 314E+2
     fmt.Println(num4)
     var num5 float32 = 314e+2
     fmt.Println(num5)
     var num6 float64 = 314e+2
     fmt.Println(num6)
     //浮点数可能会有精度的损失，所以通常情况下，建议你使用：float64 
     var num7 float32 = 256.000000916
     fmt.Println(num7)
     var num8 float64 = 256.000000916
     fmt.Println(num8)
     //golang中默认的浮点类型为：float64 
     var num9 = 3.17
     fmt.Printf("num9对应的默认的类型为：%T",num9)
     } 
   ```


----------------------------------------

### 字符类型

【1】Golang中没有专门的字符类型，如果要存储单个字符(字母)，一般使用byte来保存。 

【2】Golang中字符使用UTF-8编码 

【3】ASCII码表： 

左面是不可见字符 右面是可见字符 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810111507.png)




【4】查看UTF-8编码表： 

[http://www.mytju.com/classcode/tools/encode_utf8.asp](http://www.mytju.com/classcode/tools/encode_utf8.asp)
![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810111543.png)


【5】代码：  


1. ```GO
   package main
   
   import "fmt"
   
   func main() {
   	//定义字符类型的数据：
   	var c1 byte = 'a'
   	fmt.Println(c1) //97
   	var c2 byte = '6'
   	fmt.Println(c2) //54
   	var c3 byte = '('
   	fmt.Println(c3 + 20) //40
   	//字符类型，本质上就是一个整数，也可以直接参与运算，输出字符的时候，会将对应的码值做一个输出
   	//字母，数字，标点等字符，底层是按照ASCII进行存储。
   	var c4 int = '中'
   	fmt.Println(c4)
   	//汉字字符，底层对应的是Unicode码值
   	//对应的码值为20013，byte类型溢出，能存储的范围：可以用int
   	//总结：Golang的字符对应的使用的是UTF-8编码（Unicode是对应的字符集，UTF-8是Unicode的其中的一种编码方案）
   	var c5 byte = 'A'
   	//想显示对应的字符，必须采用格式化输出
   	fmt.Printf("c5对应的具体的字符为：%c", c5)
   }
   ```

【6】转义字符： 

\转义字符：将后面的字母表示为特殊含义 


![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810114735.png)

1. ```go
   package main
   import "fmt"
   func main(){
           //练习转义字符：
           //\n  换行
           fmt.Println("aaa\nbbb")
           //\b 退格
           fmt.Println("aaa\bbbb")
           //\r 光标回到本行的开头，后续输入就会替换原有的字符
           fmt.Println("aaaaa\rbbb")
           //\t 制表符
           fmt.Println("aaaaaaaaaaaaa")
           fmt.Println("aaaaa\tbbbbb")
           fmt.Println("aaaaaaaa\tbbbbb")
           //\"
           fmt.Println("\"Golang\"")
   }
   ```


----------------------------------------

### 布尔类型

【1】布尔类型也叫bool类型，bool类型数据只允许取值true和false 

【2】布尔类型占1个字节。 

【3】布尔类型适于逻辑运算，一般用于程序流程控制 

【4】代码： 

```go
package main
import "fmt"
func main(){
        //测试布尔类型的数值：
        var flag01 bool = true
        fmt.Println(flag01)
        var flag02 bool = false
        fmt.Println(flag02)
        var flag03 bool = 5 < 9
        fmt.Println(flag03)
}
```


----------------------------------------

### 字符串类型

【1】介绍： 

字符串就是一串固定长度的字符连接起来的字符序列。 

【2】字符串的使用：   

1. ```go
   package main
   import "fmt"
   func main(){
           //1.定义一个字符串：
           var s1 string = "你好全面拥抱Golang"
           fmt.Println(s1)
           //2.字符串是不可变的：指的是字符串一旦定义好，其中的字符的值不能改变
           var s2 string = "abc"
           //s2 = "def"
           //s2[0] = 't'
           fmt.Println(s2)
           //3.字符串的表示形式：
           //（1）如果字符串中没有特殊字符，字符串的表示形式用双引号
           //var s3 string = "asdfasdfasdf"
           //（2）如果字符串中有特殊字符，字符串的表示形式用反引号 ``
           var s4 string = `
           package main
           import "fmt"
           
           func main(){
                   //测试布尔类型的数值：
                   var flag01 bool = true
                   fmt.Println(flag01)
           
                   var flag02 bool = false
                   fmt.Println(flag02)
           
                   var flag03 bool = 5 < 9
                   fmt.Println(flag03)
           }
           `
           fmt.Println(s4)
           //4.字符串的拼接效果：
           var s5 string = "abc" + "def"
           s5 += "hijk"
           fmt.Println(s5)
           //当一个字符串过长的时候：注意：+保留在上一行的最后
           var s6 string = "abc" + "def" + "abc" + "def" + "abc" + "def" + "abc" +
            "def"+ "abc" + "def" + "abc" + "def"+ "abc" + "def" + "abc" + "def"+
             "abc" + "def" + "abc" + "def"+ "abc" + "def" + "abc" + "def"+ "abc" +
              "def" + "abc" + "def"+ "abc" + "def" + "abc" + "def"+ "abc" + "def" + 
              "abc" + "def"+ "abc" + "def"
           fmt.Println(s6)
   }
   ```

----------------------------------------

### 基本数据类型的默认值

【1】在Golang中数据类型都有一个默认值，当程序员没有赋值时，就会保留默认值(默认值又叫零值)。 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810115447.png)


【2】代码验证: 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810115512.png)




----------------------------------------

### 基本数据类型之间的转换

【1】Go在不同类型的变量之间赋值时需要显式转换，并且只有显式转换(强制转换)。 

【2】语法： 

表达式T(v)将值v转换为类型T 

T : 就是数据类型 

v : 就是需要转换的变量 


【3】案例展示：  

1. ```go
   package main
   import "fmt"
   func main(){
           //进行类型转换：
           var n1 int = 100
           //var n2 float32 = n1  在这里自动转换不好使，比如显式转换
           fmt.Println(n1)
           //fmt.Println(n2)
           var n2 float32 = float32(n1)
           fmt.Println(n2)
           //注意：n1的类型其实还是int类型，只是将n1的值100转为了float32而已，n1还是int的类型
           fmt.Printf("%T",n1)  //int
           fmt.Println()
           //将int64转为int8的时候，编译不会出错的，但是会数据的溢出
           var n3 int64 = 888888
           var n4 int8 = int8(n3)
           fmt.Println(n4)//56
           var n5 int32 = 12
           var n6 int64 = int64(n5) + 30  //一定要匹配=左右的数据类型
           fmt.Println(n5)
           fmt.Println(n6)
           var n7 int64 = 12
           var n8 int8 = int8(n7) + 127  //编译通过，但是结果可能会溢出
           //var n9 int8 = int8(n7) + 128 //编译不会通过
           fmt.Println(n8)
           //fmt.Println(n9)
   }
   ```


----------------------------------------

### 基本数据类型转为string

【1】基本数据类型和string的转换介绍 

在程序开发中，我们经常需要将基本数据类型转成string类型。或者将string类型转成基本数据类型。 

【2】基本类型转string类型 

方式1:fmt.Sprintf("%参数",表达式)    ---》 重点练习这个，推荐方式 

方式2:使用strconv包的函数    


【3】代码测试： 

方式1： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810115732.png)

1. ```go
   package main
   import "fmt"
   func main(){
           var n1 int = 19
           var n2 float32 = 4.78
           var n3 bool = false
           var n4 byte = 'a'
           var s1 string = fmt.Sprintf("%d",n1)
           fmt.Printf("s1对应的类型是：%T ，s1 = %q \n",s1, s1)
           var s2 string = fmt.Sprintf("%f",n2)
           fmt.Printf("s2对应的类型是：%T ，s2 = %q \n",s2, s2)
           var s3 string = fmt.Sprintf("%t",n3)
           fmt.Printf("s3对应的类型是：%T ，s3 = %q \n",s3, s3)
           var s4 string = fmt.Sprintf("%c",n4)
           fmt.Printf("s4对应的类型是：%T ，s4 = %q \n",s4, s4)
   }
   ```

方式2： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810115928.png)

1. ```go
   package main
   import(
           "fmt"
           "strconv"
   )
   func main(){
           var n1 int = 18
           var s1 string = strconv.FormatInt(int64(n1),10)  //参数：第一个参数必须转为int64类型 ，第二个参数指定字面值的进制形式为十进制
           fmt.Printf("s1对应的类型是：%T ，s1 = %q \n",s1, s1)
           var n2 float64 = 4.29
           var s2 string = strconv.FormatFloat(n2,'f',9,64)
           //第二个参数：'f'（-ddd.dddd）  第三个参数：9 保留小数点后面9位  第四个参数：表示这个小数是float64类型
           fmt.Printf("s2对应的类型是：%T ，s2 = %q \n",s2, s2)
           var n3 bool = true
           var s3 string = strconv.FormatBool(n3)
           fmt.Printf("s3对应的类型是：%T ，s3 = %q \n",s3, s3)
   }
   ```


----------------------------------------

### string转为基本数据类型

【1】string类型转基本类型 

方式:使用strconv包的函数    

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810155637.png)

【2】测试： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810155710.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810155730.png)

1. ```go
   package main
   import(
           "fmt"
           "strconv"
   )
   func main(){
           //string-->bool
           var s1 string = "true"
           var b bool
           //ParseBool这个函数的返回值有两个：(value bool, err error)
           //value就是我们得到的布尔类型的数据，err出现的错误
           //我们只关注得到的布尔类型的数据，err可以用_直接忽略
           b , _ = strconv.ParseBool(s1)
           fmt.Printf("b的类型是：%T,b=%v \n",b,b)
           //string---》int64
           var s2 string = "19"
           var num1 int64
           num1,_ = strconv.ParseInt(s2,10,64)
           fmt.Printf("num1的类型是：%T,num1=%v \n",num1,num1)
           //string-->float32/float64
           var s3 string = "3.14"
           var f1 float64
           f1,_ = strconv.ParseFloat(s3,64)
           fmt.Printf("f1的类型是：%T,f1=%v \n",f1,f1)
           //注意：string向基本数据类型转换的时候，一定要确保string类型能够转成有效的数据类型，否则最后得到的结果就是按照对应类型的默认值输出
           var s4 string = "golang"
           var b1 bool
           b1 , _ = strconv.ParseBool(s4)
           fmt.Printf("b1的类型是：%T,b1=%v \n",b1,b1)
           var s5 string = "golang"
           var num2 int64
           num2,_ = strconv.ParseInt(s5,10,64)
           fmt.Printf("num2的类型是：%T,num2=%v \n",num2,num2)
   }
   ```


----------------------------------------

## 指针

【1】基本数据类型和内存： 




1. ```go
   package main
   import(
           "fmt"
   )
   func main(){
           var age int = 18
           //&符号+变量 就可以获取这个变量内存的地址
           fmt.Println(&age) //0xc0000a2058
   }
   ```

   

内存： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810160250.png)


【2】指针变量： 


1. ```go
   package main
   import(
           "fmt"
   )
   func main(){
           var age int = 18
           //&符号+变量 就可以获取这个变量内存的地址
           fmt.Println(&age) //0xc0000a2058
           //定义一个指针变量：
           //var代表要声明一个变量
           //ptr 指针变量的名字
           //ptr对应的类型是：*int 是一个指针类型 （可以理解为 指向int类型的指针）
           //&age就是一个地址，是ptr变量的具体的值
           var ptr *int = &age
           fmt.Println(ptr)
           fmt.Println("ptr本身这个存储空间的地址为：",&ptr)
           //想获取ptr这个指针或者这个地址指向的那个数据：
           fmt.Printf("ptr指向的数值为：%v",*ptr) //ptr指向的数值为：18
   }
   ```

内存： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810160423.png)


总结：最重要的就是两个符号： 

1.& 取内存地址 

2.* 根据地址取值 

----------------------------------------

### 指针细节

【1】可以通过指针改变指向值 


1. ```go
   func main(){
   	var num int = 10
   	fmt.Println(num)
   	var ptr *int = &num
   	*ptr = 20
   	fmt.Println(num)
   }
   ```


【2】指针变量接收的一定是地址值 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810160652.png)




【3】指针变量的地址不可以不匹配 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810160715.png)

PS:*float32意味着这个指针指向的是float32类型的数据，但是&num对应的是int类型的不可以。 


【4】基本数据类型（又叫值类型），都有对应的指针类型,形式为*数据类型， 

比如int的对应的指针就是*int, float32对应的指针类型就是*float32。依次类推。 

----------------------------------------

# 标识符的使用

【1】标识符：读音   biao zhi fu 

【2】什么是标识符？  

         变量，方法等,只要是起名字的地方,那个**名字**就是标识符  var age int = 19   var price float64 =9.8 

【3】标识符定义规则： 

1.三个可以（组成部分）：数字，字母，下划线_    


PS:字母含义比较宽泛，使用汉字也是可以的： 

![image-20230810161234705](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230810161234705.png)

不建议使用，建议用字母：26字母 

2.四个注意：不可以以数字开头，严格区分大小写，不能包含空格，不可以使用Go中的保留关键字  

3.见名知意：增加可读性 


4.下划线"_"本身在Go中是一个特殊的标识符，称为空标识符。可以代表任何其它的标识符，但是它对应的值会被忽略(比如:忽略某个返回值)。所以仅能被作为占位符使用，不能单独作为标识符使用。 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810161252.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810161403.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810161432.png)




5.可以用如下形式，但是不建议： var int int = 10  (int,float32,float64等不算是保留关键字，但是也尽量不要使用) 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810161920.png)

6.长度无限制，但是不建议太长  asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasfd 




7.起名规则： 

（1）包名:尽量保持package的名字和目录保持一致，尽量采取有意义的包名，简短，有意义，和标准库不要冲突 

- 1.为什么之前在定义源文件的时候，一般我们都用package main 包 ？ 

main包是一个程序的入口包，所以你main函数它所在的包建议定义为main包，如果不定义为main包，那么就不能得到可执行文件。 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810161939.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810161957.png)

- 2.尽量保持package的名字和目录保持一致 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810162225.png) 

- 和标准库不要冲突 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810162241.png)




（2）变量名、函数名、常量名 : 采用驼峰法。 

就是单词按照大写区分开  

var stuNameDetail string = 'lili' 

**（3）如果变量名、函数名、常量名首字母大写，则可以被其他的包访问;** 

         **如果首字母小写，则只能在本包中使用  （利用首字母大写小写完成权限控制）** 

注意： 

import导入语句通常放在文件开头包声明语句的下面。 

导入的包名需要使用双引号包裹起来。 包名是从$GOPATH/src/后开始计算的，使用/进行路径分隔。 




需要配置一个环境变量：GOPATH 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810162333.png)

代码展示： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810162353.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810162509.png)


![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810162521.png)




----------------------------------------

## 关键字和预定义标识符

【1】关键字就是程序发明者规定的有特殊含义的单词，又叫保留字。  

go语言中一共有25个关键字。 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810162553.png)




【2】预定义标识符：一共36个预定标识符，包含基础数据类型和系统内嵌函数  

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810162604.png)




----------------------------------------

# 第3章：运算符

运算符是—种特殊的符号，用以表示数据的运算、赋值和比较等 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810111731.png)




----------------------------------------

## 算术运算符

【1】算术运算符：+ ，-，*，/，%，++，-- 

【2】介绍：算术运算符是对数值类型的变量进行运算的，比如，加减乘除。 

【3】代码展示： 

```go
package main
import "fmt"
func main(){
	// 加号 
	// 1.正数 2.相加操作 3.字符串拼接
	var b1 int = +10
	fmt.Println(b1)
	var b2 int = 4+7
	fmt.Println(b2)
	var s1 string = "abc"+"def"
	fmt.Println(s1)
	//  /除号
	fmt.Println(10/3) //两个int类型进行数据运算 结果一定为整数类型
	fmt.Println(10.0/3) //浮点类型参与运算，结果为浮点类型
	// 求余 取模 等价公式： a%b=a-a/b*b
	fmt.Println(10%3)
	fmt.Println(-10%3)
	fmt.Println(10%-3)
	fmt.Println(-10%-3)
	// 自增操作
	var a int = 10
	a++
	fmt.Println(a)
	a--
	fmt.Println(a)
	    //++ 自增 加1操作，--自减，减1操作
        //go语言里，++，--操作非常简单，只能单独使用，不能参与到运算中去
        //go语言里，++，--只能在变量的后面，不能写在变量的前面 --a  ++a  错误写法
}
```

----------------------------------------

## 赋值运算符

【1】赋值运算符:=,+=，-=，*=，/=,%= 

【2】赋值运算符就是将某个运算后的值，赋给指定的变量。 

【3】代码展示： 


1. ```go
   package main
   import "fmt"
   func main(){
           var num1 int = 10
           fmt.Println(num1)
           var num2 int = (10 + 20) % 3 + 3 - 7   //=右侧的值运算清楚后，再赋值给=的左侧
           fmt.Println(num2)
           var num3 int = 10
           num3 += 20 //等价num3 = num3 + 20;
           fmt.Println(num3)
   }
   ```

【4】练习： 


1. ```go
   		//练习：交换两个数的值并输出结果：
           var a int = 8
           var b int = 4
           fmt.Printf("a = %v,b = %v",a,b)
           //交换：
           //引入一个中间变量：
           var t int
           t = a
           a = b
           b = t
           fmt.Printf("a = %v,b = %v",a,b)
   ```

内存： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810113041.png)

----------------------------------------

## 关系运算符

【1】关系运算符:==,!=,>,<,> =，<= 

        关系运算符的结果都是bool型，也就是要么是true，要么是false 

【2】关系表达式经常用在流程控制中 

【3】代码展示： 

```go
package main
import "fmt"
func main(){
        fmt.Println(5==9)//判断左右两侧的值是否相等，相等返回true，不相等返回的是false， ==不是=
        fmt.Println(5!=9)//判断不等于
        fmt.Println(5>9)
        fmt.Println(5<9)
        fmt.Println(5>=9)
        fmt.Println(5<=9)
}
```


----------------------------------------

## 逻辑运算符

【1】逻辑运算符:&&(逻辑与/短路与)，||（逻辑或/短路或），!（逻辑非） 

【2】用来进行逻辑运算的 

【3】代码展示 


1. ```go
   package main
   import "fmt"
   func main(){
           //与逻辑：&& :两个数值/表达式只要有一侧是false，结果一定为false
           //也叫短路与：只要第一个数值/表达式的结果是false，那么后面的表达式等就不用运算了，直接结果就是false  -->提高运算效率
           fmt.Println(true&&true)
           fmt.Println(true&&false)
           fmt.Println(false&&true)
           fmt.Println(false&&false)
           //或逻辑：||:两个数值/表达式只要有一侧是true，结果一定为true
           //也叫短路或：只要第一个数值/表达式的结果是true，那么后面的表达式等就不用运算了，直接结果就是true -->提高运算效率
           fmt.Println(true||true)
           fmt.Println(true||false)
           fmt.Println(false||true)
           fmt.Println(false||false)
           //非逻辑：取相反的结果：
           fmt.Println(!true)
           fmt.Println(!false)
   }
   ```

----------------------------------------

## 位运算符




----------------------------------------

## 其他运算符

【1】其它运算符： 

& ：返回变量的存储地址  

*：取指针变量对应的数值 

【2】代码练习： 

```go
package main
import "fmt"
func main(){
        //定义一个变量：
        var age int = 18
        fmt.Println("age对应的存储空间的地址为：",&age)//age对应的存储空间的地址为： 0xc0000100b0
        var ptr *int = &age
        fmt.Println(ptr)
        fmt.Println("ptr这个指针指向的具体数值为：",*ptr)
}
```

----------------------------------------

## 运算符的优先级别

Go语言有几十种运算符，被分成十几个级别，有的运算符优先级不同，有的运算符优先级相同，请看下表。 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810163514.png)

一句话：为了提高优先级，可以加（） 


----------------------------------------

## 获取用户终端输入

【1】介绍：在编程中，需要接收用户输入的数据，就可以使用键盘输入语句来获取。 

【2】API： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810163543.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810163555.png)




【3】代码练习： 


1. ```go
   package main
   import "fmt"
   func main(){
           //实现功能：键盘录入学生的年龄，姓名，成绩，是否是VIP
           //方式1：Scanln
           var age int
           // fmt.Println("请录入学生的年龄：")
           //传入age的地址的目的：在Scanln函数中，对地址中的值进行改变的时候，实际外面的age被影响了
           //fmt.Scanln(&age)//录入数据的时候，类型一定要匹配，因为底层会自动判定类型的
           var name string
           // fmt.Println("请录入学生的姓名：")
           // fmt.Scanln(&name)
           var score float32
           // fmt.Println("请录入学生的成绩：")
           // fmt.Scanln(&score)
           var isVIP bool
           // fmt.Println("请录入学生是否为VIP：")
           // fmt.Scanln(&isVIP)
           //将上述数据在控制台打印输出：
           //fmt.Printf("学生的年龄为：%v,姓名为：%v,成绩为：%v,是否为VIP:%v",age,name,score,isVIP)
           //方式2：Scanf
           fmt.Println("请录入学生的年龄，姓名，成绩，是否是VIP，使用空格进行分隔")
           fmt.Scanf("%d %s %f %t",&age,&name,&score,&isVIP)
           //将上述数据在控制台打印输出：
           fmt.Printf("学生的年龄为：%v,姓名为：%v,成绩为：%v,是否为VIP:%v",age,name,score,isVIP)
   }
   ```


----------------------------------------

# 流程控制引入

【1】流程控制的作用： 

流程控制语句是用来控制程序中各语句执行顺序的语句，可以把语句组合成能完成一定功能的小逻辑模块。 

【2】控制语句的分类： 

控制语句分为三类：顺序、选择和循环。 

“顺序结构”代表“先执行a，再执行b”的逻辑。 

“条件判断结构”代表“如果…，则…”的逻辑。 

“循环结构”代表“如果…，则再继续…”的逻辑。 


三种流程控制语句就能表示所有的事情！不信，你可以试试拆分你遇到的各种事情。这三种基本逻辑结构是相互支撑的，它们共同构成了算法的基本结构，无论怎样复杂的逻辑结构，
可以通过它们来表达。所以任何一种高级语言都具备上述两种结构。 

本章是大家真正进入编程界的“门票”。   

【3】流程控制的流程： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810163730.png)




----------------------------------------

## 单分支

【1】基本语法 

if 条件表达式 { 

  逻辑代码 

} 

当条件表达式为ture时，就会执行得的代码。 

PS：条件表达式左右的()可以不写，也建议不写  

PS：if和表达式中间，一定要有空格 

PS：在Golang中，{}是必须有的,就算你只写一行代码。 


【2】代码练习： 

1. ```go
   package main
   import "fmt"
   func main(){
   	var count int = 100
   	if count > 30 {
   		fmt.Println("你好，库存数量充足")
   	}
   	        //实现功能：如果口罩的库存小于30个，提示：库存不足：
           //var count int = 100
           //单分支：
           // if count < 30 {
           // 	fmt.Println("对不起，口罩存量不足")
           // }
           //if后面表达式，返回结果一定是true或者false，
           //如果返回结果为true的话，那么{}中的代码就会执行
           //如果返回结果为false的话，那么{}中的代码就不会执行
           //if后面一定要有空格，和条件表达式分隔开来
           //{}一定不能省略
           //条件表达式左右的()是建议省略的
           //在golang里，if后面可以并列的加入变量的定义：
       if count := 20;count < 30 {
   		fmt.Println("对不起，口罩存量不足")
   	}
   }
   ```

----------------------------------------

## 双分支

【1】基本语法 

if 条件表达式 { 

   逻辑代码1 

} else { 

   逻辑代码2 

} 

当条件表达式成立，即执行逻辑代码1，否则执行逻辑代码2。{}也是必须有的。 




PS：下面的格式是错误的： 

if 条件表达式 { 

   逻辑代码1 

} 

else { 

   逻辑代码2 

}  




PS：空格加上，美观规范 


【2】代码练习： 

```go
package main

import "fmt"

func main() {
	var count int = 100
	if count > 30 {
		fmt.Println("你好，库存数量充足")
	} else {
		fmt.Println("库存不足")
	}
}
```

----------------------------------------

## 多分支

【1】基本语法 

if 条件表达式1 { 

    逻辑代码1 

} else if 条件表达式2 { 

    逻辑代码2 

} 

....... 

else { 

逻辑代码n 

} 


【2】代码练习： 


1. ```go
   package main
   import "fmt"
   func main(){
           //实现功能：根据给出的学生分数，判断学生的等级：
           // >=90  -----A
           // >=80  -----B
           // >=70  -----C
           // >=60  -----D
           // <60   -----E
           //方式1：利用if单分支实现：
           //定义一个学生的成绩：
           var score int = 18
           //对学生的成绩进行判定：
           // if score >= 90 {
           // 	fmt.Println("您的成绩为A级别")
           // }
           // if score >= 80 && score < 90 {
           // 	fmt.Println("您的成绩为B级别")
           // }
           // if score >= 70 && score < 80 {
           // 	fmt.Println("您的成绩为C级别")
           // }
           // if score >= 60 && score < 70 {
           // 	fmt.Println("您的成绩为D级别")
           // }
           // if score < 60 {
           // 	fmt.Println("您的成绩为E级别")
           // }
           //上面方式1利用多个单分支拼凑出多个选择，多个选择是并列的，依次从上而下顺序执行，即使走了第一个分支，那么其它分支也是需要判断
           
           //方式2：多分支：优点：如果已经走了一个分支了，那么下面的分支就不会再去判断执行了
           // if score >= 90 {
           // 	fmt.Println("您的成绩为A级别")
           // } else if score >= 80 {//else隐藏：score < 90
           // 	fmt.Println("您的成绩为B级别")
           // } else if score >= 70 {//score < 80
           // 	fmt.Println("您的成绩为C级别")
           // } else if score >= 60 {//score < 70
           // 	fmt.Println("您的成绩为D级别")
           // } else {//score < 60
           // 	fmt.Println("您的成绩为E级别")
           // } //建议你保证else的存在，只有有了else才会真正 起到多选一 的效果
           if score > 10 {
                   fmt.Println("aaa")
           } else if score > 6{
                   fmt.Println("bbb")
           }
   }
   ```


----------------------------------------

## switch分支

【1】基本语法： 

switch 表达式 { 

case 值1,值2,.….: 

语句块1 

case 值3,值4,...: 

语句块2 

.... 

default: 

        语句块 

} 


【2】代码练习： 


1. ```go
   package main
   
   import "fmt"
   
   func main() {
   	var score int = 870
   	//根据分数判断等级：
   	//switch后面是一个表达式，这个表达式的结果依次跟case进行比较，满足结果的话就执行冒号后面的代码。
   	//default是用来“兜底”的一个分支，其它case分支都不走的情况下就会走default分支
   	//default分支可以放在任意位置上，不一定非要放在最后。
   	// 与其它语言不同 不需要加break来结束
   	switch score / 10 {
   	case 10:
   		fmt.Println("你是满分")
   	case 9:
   		fmt.Println("你是A级")
   	case 8:
   		fmt.Println("你是B级")
   	case 7:
   		fmt.Println("你是C级")
   	case 6:
   		fmt.Println("你是D级")
   	case 5:
   		fmt.Println("你是E级")
   	default:
   		fmt.Println("你的成绩有误")
   	}
   }
   ```

【3】注意事项： 

（1）switch后是一个表达式(即:常量值、变量、一个有返回值的函数等都可以) 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810165745.png)

（2）case后面的值如果是常量值(字面量)，则要求不能重复 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810165828.png)

（3）case后的各个值的数据类型，必须和 switch 的表达式数据类型一致 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810170013.png)

（4）case后面可以带多个值，使用逗号间隔。比如 case 值1,值2... 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810170042.png)

（5）case后面不需要带break  

（6）default语句不是必须的，位置也是随意的。 

（7）switch后也可以不带表达式，当做if分支来使用 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810170126.png)

（8）switch后也可以直接声明/定义一个变量，**必须分号结束，并且是:=形式声明**，不推荐 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810170358.png)

（9）switch穿透，利用fallthrough关键字，如果在case语句块后增加fallthrough
,则会继续执行下一个case,也叫switch穿透。 


![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810170438.png)

----------------------------------------

## for循环

【1】语法结构： 

for 初始表达式; 布尔表达式; 迭代因子 {                  

          循环体;                            

}

for循环语句是支持迭代的一种通用结构，是最有效、最灵活的循环结构。for循环在第一次反复之前要进行初始化，即执行初始表达式；随后，对布尔表达式进行判定，若判定结果为true，则执行循环体，否则，终止循环；最后在每一次反复的时候，进行某种形式的“步进”，即执行迭代因子。 

1.  初始化部分设置循环变量的初值 
2.  条件判断部分为任意布尔表达式 
3.  迭代因子控制循环变量的增减 

for循环在执行条件判定后，先执行的循环体部分，再执行步进。 

for循环结构的流程图如图所示： 

![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810171247.png)

【2】代码展示： 


步骤1： 

1. ```go
   package main
   import "fmt"
   func main(){
           //实现一个功能：求和： 1+2+3+4+5：
           //定义变量：
           var i1 int = 1
           var i2 int = 2
           var i3 int = 3
           var i4 int = 4
           var i5 int = 5
           //求和：
           //定义一个变量，用来接收这个和
           var sum int = 0
           sum += i1
           sum += i2
           sum += i3
           sum += i4
           sum += i5
           //输出结果：
           fmt.Println(sum)
   }
   ```

   

缺点：变量的定义太多了 

解决：利用for循环

**注意for循环中初始化表达式只能使用:=形式**

**注意：for循环实际就是让程序员写代码的效率高了，但是底层该怎么执行还是怎么执行的，底层效率没有提高，只是程序员写代码简洁了而已**

1. ```go
   package main
   import "fmt"
   func main(){
   
   
           //求和：
   
           var sum int = 0
           for i := 0; i < 5; i++ {
   			sum +=i;
   		}
           //输出结果：
           fmt.Println(sum)
   }
   ```


for循环原理： 


![](https://prod.huayu.asia:9000/picgo/2023/08/10/20230810171547.png)










----------------------------------------

### 细节

【1】格式灵活： 


1. ```go
   package main
   import "fmt"
   func main(){
   
   
           //求和：
   
           var sum int = 0
           for i := 0; i < 5; i++ {
   			sum +=i;
   		}
           //输出结果：
           fmt.Println(sum)
   		sum = 0
   		i := 0;
   		for  i < 5  {
   			sum +=i;
   			i++
   		}
           //输出结果：
           fmt.Println(sum)
   }
   ```

   

【2】死循环： 


1. ```go
   package main
   import "fmt"
   func main(){
           //死循环：
           // for {
           // 	fmt.Println("你好 Golang")
           // }
           for ;; {
                   fmt.Println("你好 Golang")
           }
   }
   ```

在控制台中结束死循环：ctrl+c 


----------------------------------------

## for range

**（键值循环） for range结构是Go语言特有的一种的迭代结构，在许多情况下都非常有用，for range 可以遍历数组、切片、字符串、map**

及通道，for range 语法上类似于其它语言中的 foreach 语句，一般形式为： 

for key, val := range coll {     ... } 


代码展示： 




1. ```go
   	package main
   		import "fmt"
   		func main(){
   				//定义一个字符串：
   				var str string = "hello golang你好"
   				//方式1：普通for循环：按照字节进行遍历输出的 （暂时先不使用中文）
   				// for i := 0;i < len(str);i++ {//i:理解为字符串的下标
   				// 	fmt.Printf("%c \n",str[i])
   				// }
   				//方式2：for range
   				for i , value := range str {
   						fmt.Printf("索引为：%d,具体的值为：%c \n",i,value)
   				}
   				//对str进行遍历，遍历的每个结果的索引值被i接收，每个结果的具体数值被value接收
   				//遍历对字符进行遍历的
   		}
   ```

for-range的结果： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811084424.png)




----------------------------------------

## break

【1】感受break在循环中的作用： 

1. ```go
   package main
   import "fmt"
   func main(){
           //功能：求1-100的和，当和第一次超过300的时候，停止程序
           var sum int = 0
           for i := 1 ; i <= 100 ; i++ {
                   sum += i
                   fmt.Println(sum)
                   if sum >= 300 {
                           //停止正在执行的这个循环：
                           break 
                   }
           }
           fmt.Println("-----ok")
   }
   ```

   

总结： 

1.switch分支中，每个case分支后都用break结束当前分支，但是在go语言中break可以省略不写。 

2.break可以结束正在执行的循环 

【2】深入理解： 

1. ```go
   package main
   import "fmt"
   func main(){
           //双重循环：
           for i := 1; i <= 5; i++ {
                   for j := 2; j <= 4; j++ {
                           fmt.Printf("i: %v, j: %v \n",i,j)
                           if i == 2 && j == 2 {
                                   break
                           }
                   }
           }
   }
   ```

总结：break的作用结束离它最近的循环 

结果： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811084818.png)


【3】标签的使用展示： 


1. ```GO
   package main
   import "fmt"
   func main(){
           //双重循环：
           label2:
           for i := 1; i <= 5; i++ {
                   for j := 2; j <= 4; j++ {
                           fmt.Printf("i: %v, j: %v \n",i,j)
                           if i == 2 && j == 2 {
                                   break label2   //结束指定标签对应的循环
                           }
                   }
           }
           fmt.Println("-----ok")
   }
   ```

   

注意：如果那个标签没有使用到 的话，那么标签不用加，否则报错：定义未使用 

结果： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811084942.png)


----------------------------------------

## continue

【1】continue的作用： 


1. ```GO
   package main
   import "fmt"
   func main(){
           //功能：输出1-100中被6整除的数：
           //方式1：
           // for i := 1; i <= 100; i++ {
           // 	if i % 6 == 0 {
           // 		fmt.Println(i)
           // 	}
           // }
           //方式2：
           for i := 1; i <= 100; i++ {
                   if i % 6 != 0 {
                           continue //结束本次循环，继续下一次循环
                   }
                   fmt.Println(i)
           }
   }
   ```

   

【2】深入理解： 


1. ```GO
   package main
   import "fmt"
   func main(){
           //双重循环：
           for i := 1; i <= 5; i++ {
                   for j := 2; j <= 4; j++ {			
                           if i == 2 && j == 2 {
                                   continue
                           }
                           fmt.Printf("i: %v, j: %v \n",i,j)
                   }
           }
           fmt.Println("-----ok")
   }
   ```

   

结果： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811085134.png)


结论：continue的作用是结束离它近的那个循环，继续离它近的那个循环 


【3】标签的作用： 


1. ```go
   package main
   import "fmt"
   func main(){
           //双重循环：
           label:
           for i := 1; i <= 5; i++ {
                   for j := 2; j <= 4; j++ {			
                           if i == 2 && j == 2 {
                                   continue label
                           }
                           fmt.Printf("i: %v, j: %v \n",i,j)
                   }
           }
           fmt.Println("-----ok")
   }
   ```

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811085259.png)




----------------------------------------

## goto

【1】Golang的 goto 语句可以无条件地转移到程序中指定的行。 

【2】goto语句通常与条件语句配合使用。可用来实现条件转移. 

【3】在Go程序设计中一般不建议使用goto语句，以免造成程序流程的混乱。 

【4】代码展示： 


1. ```go
   package main
   import "fmt"
   func main(){
           fmt.Println("hello golang1")
           fmt.Println("hello golang2")
           if 1 == 1 {
                   goto label1 //goto一般配合条件结构一起使用
           }
           fmt.Println("hello golang3")
           fmt.Println("hello golang4")
           fmt.Println("hello golang5")
           fmt.Println("hello golang6")
           label1:
           fmt.Println("hello golang7")
           fmt.Println("hello golang8")
           fmt.Println("hello golang9")
   }
   ```

----------------------------------------

## return




1. ```go
   package main
   import "fmt"
   func main(){
           for i := 1; i <= 100; i++ {
                   fmt.Println(i)
                   if i == 14 {
                           return //结束当前的函数
                   }
           }
           fmt.Println("hello golang")
   }
   ```

----------------------------------------

# 函数的引入

【1】为什么要使用函数： 

提高代码的复用型，减少代码的冗余，代码的维护性也提高了 


【2】函数的定义： 

为完成某一功能的程序指令(语句)的集合,称为函数。 


【3】基本语法 

func   函数名（形参列表)（返回值类型列表）{ 

执行语句.. 

return + 返回值列表 

} 


【4】函数的定义和函数的调用案例： 

1. ```go
   package main
   import "fmt"
   // func   函数名（形参列表)（返回值类型列表）{
   // 	执行语句..
   // 	return + 返回值列表
   // }
   //自定义函数：功能：两个数相加：
   func cal (num1 int,num2 int) (int) { //如果返回值类型就一个的话，那么()是可以省略不写的
           var sum int = 0
           sum += num1
           sum += num2
           return sum
   }
   func main(){
           //功能：10 + 20
           //调用函数：
           sum := cal(10,20)
           fmt.Println(sum)
           // var num1 int = 10
           // var num2 int = 20
           //求和：
           // var sum int = 0
           // sum += num1
           // sum += num2
           // fmt.Println(sum)
           //功能：30 + 50
           var num3 int = 30
           var num4 int = 50
           //调用函数：
           sum1 := cal(num3,num4)
           fmt.Println(sum1)
           //求和：
           // var sum1 int = 0
           // sum1 += num3
           // sum1 += num4
           // fmt.Println(sum1)
   }
   ```

----------------------------------------

## 函数细节详讲01

【1】函数： 

对特定的功能进行提取，形成一个代码片段，这个代码片段就是我们所说的函数 

【2】函数的作用：提高代码的复用性 

【3】函数和函数是并列的关系，所以我们定义的函数不能写到main函数中 

【4】基本语法 

func   函数名（形参列表)（返回值类型列表）{ 

执行语句.. 

return + 返回值列表 

}  

（1）函数名： 

遵循标识符命名规范:见名知意 addNum,驼峰命名addNum   

首字母不能是数字 

**首字母大写该函数可以被本包文件和其它包文件使用(类似public)** 

**首学母小写只能被本包文件使用，其它包文件不能使用(类似private)** 


（2）形参列表： 

形参列表：个数：可以是一个参数，可以是n个参数，可以是0个参数 

形式参数列表：作用：接收外来的数据 

实际参数：实际传入的数据 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811090244.png)

（3）返回值类型列表：函数的返回值对应的类型应该写在这个列表中 

返回0个： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811091056.png)

返回1个： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811091121.png)

返回多个： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811091141.png)




![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811091205.png)







----------------------------------------

## 函数细节详讲02(值传递)

【5】通过例题感受内存分析： 




1. ```go
   package main
   import "fmt"
   //自定义函数：功能：交换两个数
   func exchangeNum (num1 int,num2 int){ 
           var t int
           t = num1
           num1 = num2
           num2 = t
   }
   func main(){	
           //调用函数：交换10和20
           var num1 int = 10
           var num2 int = 20
           fmt.Printf("交换前的两个数： num1 = %v,num2 = %v \n",num1,num2)
           exchangeNum(num1,num2)
           fmt.Printf("交换后的两个数： num1 = %v,num2 = %v \n",num1,num2)
   }
   ```

   

内存分析： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811092313.png)




----------------------------------------

## 函数细节详讲03

【6】**Golang中函数不支持重载** 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811092438.png)

【7】Golang中支持可变参数 (如果你希望函数带有可变数量的参数) 


1. ```go
   package main
   import "fmt"
   //定义一个函数，函数的参数为：可变参数 ...  参数的数量可变
   //args...int 可以传入任意多个数量的int类型的数据  传入0个，1个，，，，n个
   func test (args...int){
           //函数内部处理可变参数的时候，将可变参数当做切片来处理
           //遍历可变参数：
           for i := 0; i < len(args); i++ {
                   fmt.Println(args[i])
           }
   }
   func main(){	
           test()
           fmt.Println("--------------------")
           test(3)
           fmt.Println("--------------------")
           test(37,58,39,59,47)
   }
   ```


【8】基本数据类型和数组默认都是值传递的，即进行值拷贝。在函数内修改，不会影响到原来的值。 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811093128.png)

【9】以值传递方式的数据类型，如果希望在函数内的变量能修改函数外的变量，可以传入变量的地址&，函数内以指针的方式操作变量。从效果来看类似引用传递。 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811093414.png)







----------------------------------------

## 函数细节详讲04

【10】在Go中，函数也是一种数据类型，可以赋值给一个变量，则该变量就是一个函数类型的变量了。通过该变量可以对函数调用。 


1. ```go
   package main
   import "fmt"
   //定义一个函数：
   func test(num int){
           fmt.Println(num)
   }
   func main(){
           //函数也是一种数据类型，可以赋值给一个变量	
           a := test//变量就是一个函数类型的变量
           fmt.Printf("a的类型是：%T,test函数的类型是：%T \n",a,test)//a的类型是：func(int),test函数的类型是：func(int)
           //通过该变量可以对函数调用
           a(10) //等价于  test(10)
   }
   ```

   




【11】函数既然是一种数据类型，因此在Go中，函数可以作为形参，并且调用 

（把函数本身当做一种数据类型） 


1. ```go
   package main
   import "fmt"
   //定义一个函数：
   func test(num int){
           fmt.Println(num)
   }
   //定义一个函数，把另一个函数作为形参：
   func test02 (num1 int ,num2 float32, testFunc func(int)){
           fmt.Println("-----test02")
   }
   func main(){
           //函数也是一种数据类型，可以赋值给一个变量	
           a := test//变量就是一个函数类型的变量
           fmt.Printf("a的类型是：%T,test函数的类型是：%T \n",a,test)//a的类型是：func(int),test函数的类型是：func(int)
           //通过该变量可以对函数调用
           a(10) //等价于  test(10)
           //调用test02函数：
           test02(10,3.19,test)
           test02(10,3.19,a)
   }
   ```


【12】为了简化数据类型定义,Go支持自定义数据类型 

基本语法: type 自定义数据类型名  数据类型        

可以理解为 : 相当于起了一个别名 

例如:type mylnt int -----》这时mylnt就等价int来使用了. 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811094645.png)

例如:type mySum func (int , int) int -----》这时mySum就等价一个函数类型func(int, int) int 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811094858.png)

【13】支持对函数返回值命名 

传统写法要求：返回值和返回值的类型对应，顺序不能差 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811095105.png)




升级写法：对函数返回值命名，里面顺序就无所谓了，顺序不用对应 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811095138.png)


----------------------------------------

# 包的引入

【1】使用包的原因： 

（1）我们不可能把所有的函数放在同一个源文件中，可以分门别类的把函数放在不同的原文件中  

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811095641.png)

（2）解决同名问题：两个人都想定义一个同名的函数，在同一个文件中是不可以定义相同名字的函数的。此时可以用包来区分  


【2】案例展示包： 

项目的结构： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811101343.png)




代码展示： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811101413.png)




![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811101429.png)


----------------------------------------

## 包的细节详讲01

1.package进行包的声明，建议：包的声明这个包和所在的文件夹同名 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811101533.png)

2.main包是程序的入口包，一般main函数会放在这个包下 

  main函数一定要放在main包下，否则不能编译执行 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811101553.png)

3.打包语法： 

package 包名 

4.引入包的语法：import "包的路径" 

包名是从$GOPATH/src/后开始计算的，使用/进行路径分隔。 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102204.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102242.png)

5.如果有多个包，建议一次性导入,格式如下： 

import( 

        "fmt" 

        "gocode/testproject01/unit5/demo09/crm/dbutils" 

) 

6.在函数调用的时候前面要定位到所在的包 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102329.png)

7.函数名，变量名首字母大写，函数，变量可以被其它包访问 

![image-20230811102350781](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230811102350781.png)

8.一个目录下不能有重复的函数 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102423.png)

9.包名和文件夹的名字，可以不一样 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102515.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102527.png)

10.一个目录下的同级文件归属一个包 

同级别的源文件的包的声明必须一致 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102609.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102654.png)

11.包到底是什么： 

（1）在程序层面，所有使用相同  package 包名  的源文件组成的代码模块 

（2）在源文件层面就是一个文件夹 


----------------------------------------

## 包的细节详讲02

12.可以给包取别名，取别名后，原来的包名就不能使用了 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811102743.png)


----------------------------------------

## init函数

【1】init函数：初始化函数，可以用来进行一些初始化的操作 

每一个源文件都可以包含一个init函数，该函数会在main函数执行前，被Go运行框架调用。 

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817175817.png)

【2】全局变量定义，init函数，main函数的执行流程？ 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821162159.png)

【3】多个源文件都有init函数的时候，如何执行： 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821162534.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821162554.png)







执行结果： 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821163119.png)


执行过程： 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821163143.png)

----------------------------------------

## 匿名函数

【1】Go支持匿名函数，如果我们某个函数只是希望使用一次，可以考虑使用匿名函数 

【2】匿名函数使用方式： 

（1）在定义匿名函数时就直接调用，这种方式匿名函数只能调用一次（用的多） 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821163455.png)

（2）将匿名函数赋给一个变量(该变量就是函数变量了)，再通过该变量来调用匿名函数（用的少） 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821163535.png)

【3】如何让一个匿名函数，可以在整个程序中有效呢?将匿名函数给一个全局变量就可以了 




1. ```go
   package main
   
   import (
   	"fmt"
   )
   var Func01 = func (num1 int,num2 int) int {
   	return num1+num2
   }
   func main() {
   
   	fmt.Println(Func01(10,20))
   	fmt.Println(Func01(20,20))
   }
   ```

----------------------------------------

## 闭包

【1】什么是闭包： 

闭包就是一个函数和与其相关的引用环境组合的一个整体 


【2】案例展示： 


1. ```go
   package main
   import "fmt"
   //函数功能：求和
   //函数的名字：getSum 参数为空
   //getSum函数返回值为一个函数，这个函数的参数是一个int类型的参数，返回值也是int类型
   func getSum() func (int) int {
           var sum int = 0
           return func (num int) int{
                   sum = sum + num 
                   return sum
           }
   }
   //闭包：返回的匿名函数+匿名函数以外的变量num
   func main(){
           f := getSum()
           fmt.Println(f(1))//1 
           fmt.Println(f(2))//3
           fmt.Println(f(3))//6
           fmt.Println(f(4))//10
   }
   ```

感受：匿名函数中引用的那个变量会一直保存在内存中，可以一直使用 


【3】闭包的本质： 

闭包本质依旧是一个匿名函数，只是这个函数引入外界的变量/参数 

**匿名函数+引用的变量/参数 = 闭包** 


【4】特点： 

（1）返回的是一个匿名函数，但是这个匿名函数引用到函数外的变量/参数 ,因此这个匿名函数就和变量/参数形成一个整体，构成闭包。 

（2）闭包中使用的变量/参数会一直保存在内存中，所以会一直使用---》意味着闭包不可滥用（对内存消耗大） 


【5】不使用闭包可以吗？ (可以借助全局变量)

1. ```go
   package main
   import "fmt"
   //函数功能：求和
   //函数的名字：getSum 参数为空
   //getSum函数返回值为一个函数，这个函数的参数是一个int类型的参数，返回值也是int类型
   func getSum() func (int) int {
           var sum int = 0
           return func (num int) int{
                   sum = sum + num 
                   return sum
           }
   }
   //闭包：返回的匿名函数+匿名函数以外的变量num
   func main(){
           f := getSum()
           fmt.Println(f(1))//1 
           fmt.Println(f(2))//3
           fmt.Println(f(3))//6
           fmt.Println(f(4))//10
           fmt.Println("----------------------")
           fmt.Println(getSum01(0,1))//1
           fmt.Println(getSum01(1,2))//3
           fmt.Println(getSum01(3,3))//6
           fmt.Println(getSum01(6,4))//10
   }
   func getSum01(sum int,num int) int{
           sum = sum + num
           return sum
   }
   //不使用闭包的时候：我想保留的值，不可以反复使用
   //闭包应用场景：闭包可以保留上次引用的某个值，我们传入一次就可以反复使用了
   
   ```

----------------------------------------

# defer关键字

【1】defer关键字的作用： 

在函数中，程序员经常需要创建资源，为了在函数执行完毕后，及时的释放资源，Go的设计者提供defer关键字 


【2】案例展示： 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821164436.png)




【3】代码变动一下，再次看结果： 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821164842.png)

发现：遇到defer关键字，会将后面的代码语句压入栈中，也会将相关的值同时拷贝入栈中，不会随着函数后面的变化而变化。 


【4】defer应用场景： 

比如你想关闭某个使用的资源，在使用的时候直接随手defer，因为defer有延迟执行机制（函数执行完毕再执行defer压入栈的语句）， 

所以你用完随手写了关闭，比较省心，省事 


----------------------------------------

# 字符串方法

## 详讲01

【1】统计字符串的长度,按字节进行统计： 

     len(str) 

         使用内置函数也不用导包的，直接用就行 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821164926.png)

【2】字符串遍历： 

(1)利用方式1：for-range键值循环： 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165050.png)

（2）r:=[]rune(str) 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165132.png)

【3】字符串转整数： 

n, err := strconv.Atoi("66")  


【4】整数转字符串： 

str = strconv.Itoa(6887) 

【5】查找子串是否在指定的字符串中:  

strings.Contains("javaandgolang", "go") 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165218.png)

【6】统计一个字符串有几个指定的子串: 

strings.Count("javaandgolang","a")  

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165234.png)

【7】不区分大小写的字符串比较: 

  strings.EqualFold("go" , "Go") 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165250.png)

【8】返回子串在字符串第一次出现的索引值，如果没有返回-1 : 

  strings.lndex("javaandgolang" , "a")  

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165317.png)




----------------------------------------

## 详讲02

【9】字符串的替换： 

strings.Replace("goandjavagogo", "go", "golang", n)  

n可以指定你希望替换几个,如果n=-1表示全部替换，替换两个n就是2 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165703.png)

【10】按照指定的某个字符，为分割标识，将一个学符串拆分成字符串数组: 

strings.Split("go-python-java", "-") 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165723.png)

【11】将字符串的字母进行大小写的转换: 

strings.ToLower("Go")// go  

strings.ToUpper"go")//Go 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165747.png)

【12】将字符串左右两边的空格去掉: 

strings.TrimSpace("     go and java    ") 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165803.png)

【13】将字符串左右两边指定的字符去掉: 

strings.Trim("~golang~ ", " ~")   

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165903.png)

【14】将字符串左边指定的字符去掉: 

strings.TrimLeft("~golang~", "~") 


【15】将字符串右边指定的字符去掉: 

strings.TrimRight("~golang~", "~") 


【16】判断字符串是否以指定的字符串开头:  

strings.HasPrefix("http://java.sun.com/jsp/jstl/fmt", "http") 


【17】判断字符串是否以指定的字符串结束:  

strings.HasSuffix("demo.png", ".png") 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821165934.png)

----------------------------------------

# 日期函数详讲

【1】时间和日期的函数，需要到入time包，所以你获取当前时间，就要调用函数Now函数： 


1. ```go
   package main
   import (
           "fmt"
           "time"
   )
   func main(){
           //时间和日期的函数，需要到入time包，所以你获取当前时间，就要调用函数Now函数：
           now := time.Now()
           //Now()返回值是一个结构体，类型是：time.Time
           fmt.Printf("%v ~~~ 对应的类型为：%T\n",now,now)
           //2021-02-08 17:47:21.7600788 +0800 CST m=+0.005983901 ~~~ 对应的类型为：time.Time
           //调用结构体中的方法：
           fmt.Printf("年：%v \n",now.Year())
           fmt.Printf("月：%v \n",now.Month())//月：February
           fmt.Printf("月：%v \n",int(now.Month()))//月：2
           fmt.Printf("日：%v \n",now.Day())
           fmt.Printf("时：%v \n",now.Hour())
           fmt.Printf("分：%v \n",now.Minute())
           fmt.Printf("秒：%v \n",now.Second())
   }
   ```

【2】日期的格式化： 

（1）将日期以年月日时分秒按照格式输出为字符串： 

1. ```go
   	//Printf将字符串直接输出：
   	fmt.Printf("当前年月日： %d-%d-%d 时分秒：%d:%d:%d  \n", now.Year(), now.Month(),
   		now.Day(), now.Hour(), now.Minute(), now.Second())
   	//Sprintf可以得到这个字符串，以便后续使用：
   	datestr := fmt.Sprintf("当前年月日： %d-%d-%d 时分秒：%d:%d:%d  \n", now.Year(), now.Month(),
   		now.Day(), now.Hour(), now.Minute(), now.Second())
   	fmt.Println(datestr)
   ```

（2）按照指定格式： 


1. ```go
   	//这个参数字符串的各个数字必须是固定的，必须这样写
   	datestr2 := now.Format("2006/01/02 15/04/05")
   	fmt.Println(datestr2)
   	//选择任意的组合都是可以的，根据需求自己选择就可以（自己任意组合）。
   	datestr3 := now.Format("2006 15:04")
   	fmt.Println(datestr3)
   ```


----------------------------------------

# 内置函数

【1】什么是内置函数/内建函数： 

Golang设计者为了编程方便，提供了一些函数，这些函数不用导包可以直接使用，我们称为Go的内置函数/内建函数。 


【2】内置函数存放位置： 

在builtin包下，使用内置函数也的，直接用就行 


【3】常用函数： 

（1）len函数： 

统计字符串的长度,按字节进行统计 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821170714.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821170732.png)

（2）new函数： 

分配内存，主要用来分配值类型（int系列, float系列, bool, string、数组和结构体struct） 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821170804.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821170814.png)




内存分析： 

![](https://prod.huayu.asia:9000/picgo/2023/08/21/20230821171032.png)

（3）make函数： 

分配内存，主要用来分配引用类型（指针、slice切片、map、管道chan、interface 等） 





# 错误处理

----------------------------------------

## defer+recover机制处理错误

【1】**展示错误：** 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811103556.png)

发现：程序中出现错误/恐慌以后，程序被中断，无法继续执行。 


【2】**错误处理/捕获机制：** 

go中追求代码优雅，引入机制：defer+recover机制处理错误 


内置函数recover: 


![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811105246.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811105302.png)


优点：提高程序健壮性 

----------------------------------------

## 自定义错误

自定义错误：需要调用errors包下的New函数：函数返回error类型 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811105330.png)




代码： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811105350.png)

有一种情况：程序出现错误以后，后续代码就没有必要执行，想让程序中断，退出程序： 

借助：builtin包下内置函数：panic  主动抛出异常

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811105422.png)

代码： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811105434.png)


----------------------------------------

# 数组的引入

【1】练习引入： 


1. ```go
   package main
   import "fmt"
   func main(){
           //实现的功能：给出五个学生的成绩，求出成绩的总和，平均数：
           //给出五个学生的成绩：
           score1 := 95
           score2 := 91
           score3 := 39
           score4 := 60
           score5 := 21
           //求和：
           sum := score1 + score2 + score3 + score4 + score5 
           //平均数：
           avg := sum / 5
           //输出
           fmt.Printf("成绩的总和为：%v,成绩的平均数为：%v",sum,avg)
   }
   ```

缺点： 

成绩变量定义个数太多，成绩管理费劲，维护费劲  ---》 将成绩找个地方存起来 ---》 数组 

---》存储相同类型的数据 


【2】数组解决练习： 


1. ```go
   package main
   import "fmt"
   func main(){
           //实现的功能：给出五个学生的成绩，求出成绩的总和，平均数：
           //给出五个学生的成绩：--->数组存储：
           //定义一个数组：
           var scores [5]int
           //将成绩存入数组：
           scores[0] = 95
           scores[1] = 91
           scores[2] = 39
           scores[3] = 60
           scores[4] = 21
           //求和：
           //定义一个变量专门接收成绩的和：
           sum := 0
           for i := 0;i < len(scores);i++ {//i: 0,1,2,3,4 
                   sum += scores[i]
           }
           //平均数：
           avg := sum / len(scores)
           //输出
           fmt.Printf("成绩的总和为：%v,成绩的平均数为：%v",sum,avg)
   }
   ```


总结： 

数组定义格式： 

1.  var 数组名 [数组大小]数据类型 

例如： 

var scores [5]int 

**赋值：** 

**scores[0] = 95** 

**scores[1] = 91** 

**scores[2] = 39**

**scores[3] = 60**

**scores[4] = 21**

----------------------------------------

## 内存分析

【1】代码： 


1. ```go
   package main
   import "fmt"
   func main(){
           //声明数组：
           var arr [3]int16
           //获取数组的长度：
           fmt.Println(len(arr))
           //打印数组：
           fmt.Println(arr)//[0 0 0]
           //证明arr中存储的是地址值：
           fmt.Printf("arr的地址为：%p",&arr)
           //第一个空间的地址：
           fmt.Printf("arr的地址为：%p",&arr[0])
           //第二个空间的地址：
           fmt.Printf("arr的地址为：%p",&arr[1])
           //第三个空间的地址：
           fmt.Printf("arr的地址为：%p",&arr[2])
   }
   ```

运行结果： 


![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811110553.png)


【2】内存分析： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811110618.png)

PS : 数组每个空间占用的字节数取决于数组类型 


【3】赋值内存：（数组是值类型，在栈中开辟内存） 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811110703.png)


数组优点：访问/查询/读取 速度快 


----------------------------------------

## 数组的遍历

【1】普通for循环 

【2】键值循环 

**（键值循环） for range结构是Go语言特有的一种的迭代结构，在许多情况下都非常有用，for range 可以遍历数组、切片、字符串、map**

及通道，for range 语法上类似于其它语言中的 foreach 语句，一般形式为： 

**for** key, val := **range** coll {     ... } 


注意： 

（1）coll就是你要的数组 

（2）每次遍历得到的索引用key接收，每次遍历得到的索引位置上的值用val 

（3）key、value的名字随便起名  k、v   key、value   

（4）key、value属于在这个循环中的局部变量 

（5）你想忽略某个值：用_就可以了： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811111023.png)


代码： 


1. ```go
   package main
   import "fmt"
   func main(){
           //实现的功能：给出五个学生的成绩，求出成绩的总和，平均数：
           //给出五个学生的成绩：--->数组存储：
           //定义一个数组：
           var scores [5]int
           //将成绩存入数组：(循环 + 终端输入)
           for i := 0; i < len(scores);i++ {//i：数组的下标
                   fmt.Printf("请录入第个%d学生的成绩",i + 1)
                   fmt.Scanln(&scores[i])
           }
           //展示一下班级的每个学生的成绩：（数组进行遍历）
           //方式1：普通for循环：
           for i := 0; i < len(scores);i++ {
                   fmt.Printf("第%d个学生的成绩为：%d\n",i+1,scores[i])
           }
           fmt.Println("-------------------------------")
           //方式2：for-range循环
           for key,value := range scores {
                   fmt.Printf("第%d个学生的成绩为：%d\n",key + 1,value)
           }
   }
   ```

运行结果： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811111324.png)




----------------------------------------

## 数组的初始化方式


1. ```go
   package main
   import "fmt"
   func main(){
           //第一种：
           var arr1 [3]int = [3]int{3,6,9}
           fmt.Println(arr1)
           //第二种：
           var arr2 = [3]int{1,4,7}
           fmt.Println(arr2)
           //第三种：
           var arr3 = [...]int{4,5,6,7}
           fmt.Println(arr3)
           //第四种：
           var arr4 = [...]int{2:66,0:33,1:99,3:88}
           fmt.Println(arr4)
   }
   ```

----------------------------------------

## 注意事项

【1】长度属于类型的一部分 ： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811111745.png)




【2】Go中数组属值类型，在默认情况下是值传递，因此会进行值拷贝。 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811111848.png)

【3】如想在其它函数中，去修改原来的数组，可以使用引用传递(指针方式)。 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811111904.png)




----------------------------------------

## 二维数组

【1】二维数组的定义，并且有默认初始值： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811112810.png)


【2】二维数组内存： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811112850.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811112955.png)




【3】赋值操作： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811113119.png)

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811113140.png) 

【4】二维数组的初始化： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811113235.png)




----------------------------------------

## 二维数组的遍历

【1】普通for循环 

【2】键值循环（for range） 

代码： 

```go
package main
import "fmt"
func main(){
        //定义二维数组：
        var arr [3][3]int = [3][3]int{{1,4,7},{2,5,8},{3,6,9}}
        fmt.Println(arr)
        fmt.Println("------------------------")
        //方式1：普通for循环：
        for i := 0;i < len(arr);i++{
                for j := 0;j < len(arr[i]);j++ {
                        fmt.Print(arr[i][j],"\t")
                }
                fmt.Println()
        }
        fmt.Println("------------------------")
        //方式2：for range循环：
        for key,value := range arr {
                for k,v := range value {
                        fmt.Printf("arr[%v][%v]=%v\t",key,k,v)
                }
                fmt.Println()
        }
}
```

----------------------------------------

# 切片的引入

【1】切片(slice)是golang中一种特有的数据类型 

【2】数组有特定的用处，但是却有一些呆板(数组长度固定不可变)，所以在 Go
语言的代码里并不是特别常见。相对的切片却是随处可见的，切片是一种建立在数组类型之上的抽象，它构建在数组之上并且提供更强大的能力和便捷。 

【3】切片(slice)是对数组一个连续片段的引用，所以切片是一个引用类型。这个片段可以是整个数组，或者是由起始和终止索引标识的一些项的子集。需要注意的是，终止索引标识的项不包括在切片内。切片提供了一个相关数组的动态窗口。
【4】代码： 

切片的语法： 

var 切片名 []类型 = 数组的一个片段引用 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811141437.png)

----------------------------------------

## 内存分析

切片有3个字段的数据结构：一个是指向底层数组的指针，一个是切片的长度(实际通过切片可以访问的元素个数)，一个是切片的容量（即从被切的首个元素到原数组中最后一个元素的个数）。 


代码： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811141701.png)


内存： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811141722.png)




----------------------------------------

## 切片的定义

【1】方式1：定义一个切片，然后让切片去引用一个已经创建好的数组。 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811141745.png)

【2】方式2：通过make内置函数来创建切片。基本语法: var切片名[type = make([], len,[cap]) 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811141757.png)

PS : make底层创建一个数组，对外不可见，所以不可以直接操作这个数组，要通过slice去间接的访问各个元素，不可以直接对数组进行维护/操作 

【3】方式3：定一个切片，直接就指定具体数组，使用原理类似make的方式。 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811141817.png)


----------------------------------------

## 切片的遍历

【1】方式1：for循环常规方式遍历 

【2】方式2：for-range 结构遍历切片 

1. ```go
   package main
   import "fmt"
   func main (){
   	slice := make([]int,4,20)
   	slice[0] = 66
   	slice[1] = 88
   	slice[2] = 99
   	slice[3] = 100
   	for i := 0; i < len(slice); i++ {
   		fmt.Println("当前值为：",slice[i])
   	}
   	for i,value := range slice {
   		fmt.Println("当前索引值为：",i,"当前值为：",value)
   	}
   }
   ```


----------------------------------------

## 切片注意事项

【1】切片定义后不可以直接使用，需要让其引用到一个数组，或者make一个空间供切片来使用 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811160839.png)

【2】切片使用不能越界。 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811161127.png)

【3】简写方式： 

1) var slice = arr[0:end]  ----》 var slice = arr[:end] 

2) var slice = arr[start:len(arr)]  ----》  var slice = arr[start:] 

3) var slice = arr[0:len(arr)]   ----》 var slice = arr[:] 


【4】切片可以继续切片   

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811161158.png)


【5】切片可以动态增长 


1. ```go
   package main
   import "fmt"
   func main (){
   	var intarr [6]int = [6]int{1,4,7,3,6,9}
   	var slice []int = intarr[1:4]
   	fmt.Println(len(slice))
   	// 这里有问题 如果append的元素超过了原本的slice的容量 那么创建出的slice2的修改并不会影响源数据，但是如果不超过容量，那么使用的仍然是原数组地址，并影像数据
   	        //底层原理：
           //1.底层追加元素的时候对数组进行扩容，老数组扩容为新数组：
           //2.创建一个新数组，将老数组中的4,7,3复制到新数组中，在新数组中追加88,50
           //3.slice2 底层数组的指向 指向的是新数组 
           //4.往往我们在使用追加的时候其实想要做的效果给slice追加：
   	slice2 := append(slice,88,50,765)
   	fmt.Println(slice2)
   	slice2[0] = 999
   	fmt.Println(intarr)
   	fmt.Println(slice)
   }
   ```

   

可以通过append函数将切片追加给切片： 

![](https://prod.huayu.asia:9000/picgo/2023/08/11/20230811162444.png)

【6】切片的拷贝： 

```go
package main
import "fmt"
func main(){
        //定义切片：
        var a []int = []int{1,4,7,3,6,9}
        //再定义一个切片：
        var b []int = make([]int,10)
        //拷贝：
        copy(b,a) //将a中对应数组中元素内容复制到b中对应的数组中
		b[0] = 10
        fmt.Println(b)
		fmt.Println(a)
}
```


----------------------------------------

# map的引入

【1】映射（map）, Go语言中内置的一种类型，它将键值对相关联，我们可以通过键 key来获取对应的值 value。 类似其它语言的集合 

【2】基本语法 

var map变量名 map[keytype]valuetype 


PS：key、value的类型：bool、数字、string、指针、channel 、还可以是只包含前面几个类型的接口、结构体、数组 

PS：key通常为int 、string类型，value通常为数字（整数、浮点数）、string、map、结构体 

PS：key：slice、map、function不可以 


【3】代码：    

map的特点： 

**（1）map集合在使用前一定要make** 

**（2）map的key-value是无序的** 

**（3）key是不可以重复的，如果遇到重复，后一个value会替换前一个value** 

**（4）value可以重复的** 

​	**(5) make函数的第二个参数size可以省略，默认就分配一个内存(预分配一个，之后会进行扩容）**


1. ```go
   package main
   import "fmt"
   func main(){
           //定义map变量：
           var a map[int]string
           //只声明map内存是没有分配空间
           //必须通过make函数进行初始化，才会分配空间：
           a = make(map[int]string,10) //map可以存放10个键值对
           //将键值对存入map中：
           a[20095452] = "张三"
           a[20095387] = "李四"
           a[20097291] = "王五"
           a[20095387] = "朱六"
           a[20096699] = "张三"
           //输出集合
           fmt.Println(a)
   }
   ```

----------------------------------------

## map3种创建方式


1. ```go
   package main
   import "fmt"
   func main(){
           //方式1：
           //定义map变量：
           var a map[int]string
           //只声明map内存是没有分配空间
           //必须通过make函数进行初始化，才会分配空间：
           a = make(map[int]string,10) //map可以存放10个键值对
           //将键值对存入map中：
           a[20095452] = "张三"
           a[20095387] = "李四"
           //输出集合
           fmt.Println(a)
           //方式2：
           b := make(map[int]string)
           b[20095452] = "张三"
           b[20095387] = "李四"
           fmt.Println(b)
           //方式3：
           c := map[int]string{
                   20095452 : "张三",
                   20098765 : "李四",
           }
           c[20095387] = "王五"
           fmt.Println(c)
   }
   ```

----------------------------------------

## map的操作

【1】增加和更新操作: 

map["key"]= value  ——》 如果key还没有，就是增加，如果key存在就是修改。 

【2】删除操作： 

delete(map，"key") , delete是一个内置函数，如果key存在，就删除该key-value，如果k的y不存在，不操作，但是也不会报错 

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817141325.png)

【3】清空操作： 

（1）如果我们要删除map的所有key ,没有一个专门的方法一次删除，可以遍历一下key,逐个删除 

（2）或者map = make(...)，make一个新的，让原来的成为垃圾，被gc回收 

【4】查找操作： 

value ,bool = map[key] 

value为返回的value，bool为是否返回 ，要么true 要么false  

1. ```go
   package main
   import "fmt"
   func main(){
           //定义map
           b := make(map[int]string)
           //增加：
           b[20095452] = "张三"
           b[20095387] = "李四"
           //修改：
           b[20095452] = "王五"
           //删除：
           delete(b,20095387)
           delete(b,20089546)
           fmt.Println(b)
           //查找：
           value,flag := b[200]
           fmt.Println(value)
           fmt.Println(flag)
   }
   ```


【5】获取长度：len函数 


【6】遍历：for-range 


1. ```go
   package main
   import "fmt"
   func main(){
           //定义map
           b := make(map[int]string)
           //增加：
           b[20095452] = "张三"
           b[20095387] = "李四"
           b[20098833] = "王五"
           //获取长度：
           fmt.Println(len(b))
           //遍历：
           for k,v := range b {
                   fmt.Printf("key为：%v value为%v \t",k,v)
           }
           fmt.Println("---------------------------")
           //加深难度：
           a := make(map[string]map[int]string)
           //赋值：
           a["班级1"] = make(map[int]string,3)
           a["班级1"][20096677] = "露露"
           a["班级1"][20098833] = "丽丽"
           a["班级1"][20097722] = "菲菲"
           a["班级2"] = make(map[int]string,3)
           a["班级2"][20089911] = "小明"
           a["班级2"][20085533] = "小龙"
           a["班级2"][20087244] = "小飞"
           for k1,v1:= range a {
                   fmt.Println(k1)
                   for k2,v2:= range v1{
                           fmt.Printf("学生学号为：%v 学生姓名为%v \t",k2,v2)
                   }
                   fmt.Println()
           }
   }
   ```


----------------------------------------

# 面向对象的引入

【1】Golang语言面向对象编程说明： 

（1）Golang也支持面向对象编程(OOP)，但是和传统的面向对象编程有区别，并不是纯粹的面向对象语言。所以我们说Golang支持面向对象编程特性是比较准确的。
（2）Golang没有类(class)，Go语言的结构体(struct)和其它编程语言的类(class)有同等的地位，你可以理解Gelang是基于struct来实现OOP特性的。
（3）Golang面向对象编程非常简洁，去掉了传统OOP语言的方法重载、构造函数和析构函数、隐藏的this指针等等 

（4）Golang仍然有面向对象编程的继承，封装和多态的特性，只是实现的方式和其它OOP语言不一样，比如继承:Golang没有extends
关键字，继承是通过匿名字段来实现。  


【2】结构体的引入： 


具体的对象： 

一位老师：珊珊老师： 姓名：赵珊珊   年龄：31岁   性别 ：女 ......  


可以使用变量来处理： 


1. ```go
   package main
   import "fmt"
   func main(){
           //珊珊老师： 姓名：赵珊珊   年龄：31岁   性别 ：女
           var name string = "赵珊珊"
           var age int = 31
           var sex string = "女"
           //马士兵老师：
           var name2 string = "马士兵"
           var age2 int = 45
           var sex2 string = "男"
          
   }
   ```

缺点： 

（1）不利于数据的管理、维护 

（2）老师的很多属性属于一个对象，用变量管理太分散了 


----------------------------------------

## 结构体

【1】结构体、结构体对象的引入： 

案例：老师结构体 

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817173058.png)

后续实践中按照自己的需求定义结构体即可 


【2】代码： 




1. ```go
   package main
   import "fmt"
   //定义老师结构体，将老师中的各个属性  统一放入结构体中管理：
   type Teacher struct{
           //变量名字大写外界可以访问这个属性
           Name string
           Age int
           School string
   }
   func main(){
           //创建老师结构体的实例、对象、变量：
           var t1 Teacher // var a int
           fmt.Println(t1) //在未赋值时默认值：{ 0 }
           t1.Name = "马士兵"
           t1.Age = 45
           t1.School = "清华大学"
           fmt.Println(t1)
           fmt.Println(t1.Age + 10)
   }
   ```

----------------------------------------

## 内存分析

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817174422.png)




----------------------------------------

## 结构体实例创建方式

【1】方式1：直接创建： 

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817174552.png)

【2】方式2： 

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817175020.png)

【3】方式3：返回的是结构体指针： 

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817175052.png)


【4】方式4：返回的是结构体指针： 

![](https://prod.huayu.asia:9000/picgo/2023/08/17/20230817175201.png)

----------------------------------------

## 结构体之间的转换

【1】结构体是用户单独定义的类型，和其它类型进行转换时需要有完全相同的字段(名字、个数和类型) 


1. ```go
   package main
   import "fmt"
   type Student struct {
           Age int
   }
   type Person struct {
           Age int
   }
   func main(){
           var s Student = Student{10}
           var p Person = Person{10}
           s = Student(p)
           fmt.Println(s)
           fmt.Println(p)
   }
   ```


【2】结构体进行type重新定义(相当于取别名)，Golang认为是新的数据类型，但是相互间可以强转 


1. ```go
   package main
   import "fmt"
   type Student struct {
           Age int
   }
   type Stu Student
   func main(){
           var s1 Student = Student{19}
           var s2 Stu = Stu{19}
           s1 = Student(s2)
           fmt.Println(s1)
           fmt.Println(s2)
   }
   ```

## 方法引入

在GO中，给一个结构体绑定一个方法，用于为该结构体提供行为能力。

与函数的声明方式不同

```go
func (teacher Teacher) test (sum int) int{
	fmt.Println(sum+teacher.Age)
	return 10 
}
```

但是在函数声明时，即没有绑定调用类型，其他类型调用该方法会报错

```
func test (sum int) int{
	fmt.Println(sum+teacher.Age)
	return 10 
}
```

完整代码：

```go
package main

import "fmt"

//定义老师结构体，将老师中的各个属性  统一放入结构体中管理：
type Teacher struct {
	//变量名字大写外界可以访问这个属性
	Name   string
	Age    int
	School string
}

func (teacher Teacher) test(sum int) int {
	teacher.Age = sum + teacher.Age
	fmt.Println(teacher.Age)
	return 10
}
func main() {
	//创建老师结构体的实例、对象、变量：
	var t1 Teacher  // var a int
	fmt.Println(t1) //在未赋值时默认值：{ 0 }
	t1.Name = "马士兵"
	t1.Age = 45
	t1.School = "清华大学"
	fmt.Println(t1)
	fmt.Println(t1.Age + 10)
	t1.test(200)
	fmt.Println(t1.Age)
}

```

并且以上实例可以看出，go语言是完全的值传递，即使是对象，如果在方法内部被修改了值，其外部也无法被改变

![](https://prod.huayu.asia:9000/picgo/2023/08/23/20230823173259.png)

如果想要在方法内修改对象值，需要传递其对象的指针

```go
package main

import "fmt"

//定义老师结构体，将老师中的各个属性  统一放入结构体中管理：
type Teacher struct {
	//变量名字大写外界可以访问这个属性
	Name   string
	Age    int
	School string
}

func (teacher *Teacher) test(sum int) int {
	(*teacher).Age = sum + teacher.Age
	fmt.Println(teacher.Age)
	return 10
}
func main() {
	//创建老师结构体的实例、对象、变量：
	var t1 Teacher  // var a int
	fmt.Println(t1) //在未赋值时默认值：{ 0 }
	t1.Name = "马士兵"
	t1.Age = 45
	t1.School = "清华大学"
	fmt.Println(t1)
	fmt.Println(t1.Age + 10)
	(&t1).test(200)
	fmt.Println(t1.Age)
}

```

## 方法的注意事项：

**（1）结构体类型是值类型，在方法调用中，遵守值类型的传递机制，即传递的都是值的拷贝**

**（2）如果程序员希望在方法中，改变结构体变量的值，可以通过结构体指针的方式来处理**

![](https://prod.huayu.asia:9000/picgo/2023/08/23/20230823174211.png)

**以及go语言编辑器在底层帮我们做了优化，在我们传递指针的时候不需要我们主动使用*teacher进行调用，编译器会自动帮我们加上`*`.**

**(3)golang中的方法作用在指定的数据类型上，与对应的数据类型绑定，因此自定义类型都可以有方法，而不仅仅是struct，比如int,float32等都可以有方法，通过自定义别名并绑定**

```go
package main

import "fmt"
type integer int


func (teacher *integer) test1(sum int) int {
	*teacher = 100
	return 10
}
func main() {
	var tes integer = 0
	(&tes).test1(10)
	fmt.Println(tes)
}
```

