# JSPHorse

![](https://img.shields.io/badge/build-passing-brightgreen)
![](https://img.shields.io/badge/JavaParser-3.23.1-blue)
![](https://img.shields.io/badge/Java-8-red)

## ⚠️免责声明

本项目禁止进行未授权商业用途

本项目禁止二次开发后进行商业用途

本项目仅面向合法授权的企业安全建设行为，在使用本项目进行检测时，您应确保该行为符合当地的法律法规，并且已经取得了足够的授权

如您在使用本项目的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任

在使用本项目前，请您务必审慎阅读、充分理解各条款内容，限制、免责条款或者其他涉及您重大权益的条款可能会以加粗、加下划线等形式提示您重点注意。 除非您已充分阅读、完全理解并接受本协议所有条款，否则，请您不要使用本项目。您的使用行为或者您以其他任何明示或者默示方式表示接受本协议的，即视为您已阅读并同意本协议的约束

## 简介

一个JSP免杀Webshell生成器，主要支持普通回显Webshell，也实现了蚁剑的免杀

生成代码技术基于AST库`JavaParser`和字节码操作框架`ASM`

具体的原理参考先知社区文章：https://xz.aliyun.com/t/10507

```txt
　　 へ　　　　　／|
　　/＼7　　　 ∠＿/
　 /　│　　 ／　／
　│　Z ＿,＜　／　　 /`ヽ
　│　　　　　ヽ　　 /　　〉
　 Y　　　　　`　 /　　/
　?●　?　●　　??〈　　/
　()　 へ　　　　|　＼〈
　　>? ?_　 ィ　 │ ／／
　 / へ　　 /　?＜| ＼＼
　 ヽ_?　　(_／　 │／／
　　7　　　　　　　|／
　　＞―r￣￣~∠--|
```

主要的免杀技术：

- 基本的Java反射调用免杀
- BCEL格式字节码反射加载免杀
- ScriptEngine调用JS免杀
- Javac动态编译class免杀
- java.beans.Expression免杀
- 自定义ClassLoader加载字节码免杀
- native方法defineClass0加载字节码免杀
- ASM直接构造字节码(普通和BCEL)并加载执行免杀

代码生成方式：

- 双重随机异或运算加密数字常量
- 凯撒密码随机偏移并结合Base64双重加密字符串常量
- 使用控制流平坦化并随机生成分发器
- 所有标识符全部替换为随机字符串
- 支持全局Unicode编码
- 每次执行都会生成完全不同的马（结构相同内容不同）

简单测试了免杀效果：

| 名称 | 测试结果 |
| :----: | :----: |
| 百度WEBDIR+ | ![](https://img.shields.io/badge/pass-green) |
| 河马SHELLPUB | ![](https://img.shields.io/badge/pass-green) |
| Windows Defender | ![](https://img.shields.io/badge/pass-green) |

## Quick Start

![](https://github.com/EmYiQing/JSPHorse/blob/master/img/01.png)

在Github右侧Release页面下载

- 生成标准形式基础Webshell

`java -jar JSPHorse.jar -p your_password`

- 生成蚁剑的免杀Webshell

`java -jar JSPHorse.jar -p your_password --ant`

- 生成Javac动态编译class的Webshell

`java -jar JSPHorse.jar -p your_password --javac`

- 使用ScriptEngine调用JS免杀

`java -jar JSPHorse.jar -p your_password --js`

- 使用Expression免杀

`java -jar JSPHorse.jar -p your_password --expr`

- 使用自定义ClassLoader加载字节码

`java -jar JSPHorse.jar -p your_password --classloader`

- 用ASM动态生成字节码使用自定义ClassLoader加载

`java -jar JSPHorse.jar -p your_password --classloader-asm`

- 加载BCEL字节码免杀（动态构造字节码）

`java -jar JSPHorse.jar -p your_password --bcel`

- 用ASM动态生成BCEL字节码免杀

`java -jar JSPHorse.jar -p your_password --bcel-asm`

- 使用Proxy类的native方法`defineClass0`加载字节码

注意：原理是在JVM中注册类，不允许重复，所以这种马只能执行一次命令然后失效

但`JSPHorse`从字节码层面构造了不同的类，如果想要多次执行只要重复生成多个马即可

`java -jar JSPHorse.jar -p your_password --proxy`

- 使用`defineClass0`加载ASM直接构造的字节码

注意：原理同上，只能执行一次

但`JSPHorse`每次生成的类名不一致，可以重新生成来做多次执行

`java -jar JSPHorse.jar -p your_password --proxy-asm`

- 任何一种方式加入`-u`参数进行Unicode编码（有时候有奇效）

`java -jar JSPHorse.jar -p your_password --expr -u`

- 使用`-o`指定输出文件，默认输出到`result.jsp`

`java -jar JSPHorse.jar -p your_password -o 1.jsp`

- 如何使用

1. 普通情况：`1.jsp?pwd=your_password&cmd=ipconfig`

2. 蚁剑：正常使用

## 其他

### Star

![](https://starchart.cc/EmYiQing/JSPHorse.svg)

### 推荐

从混淆程度来看，最高的应该是以下几种，也许会有较高的免杀效果

1. ASM动态生成BCEL字节码并加载：`--bcel-asm -u`
2. 使用ScriptEngine加载JS：`--js -u`
3. 使用Javac动态编译：`--javac -u`

做了简单的原理和使用视频：[B站](https://space.bilibili.com/1106751850/channel/seriesdetail?sid=560683)

### 计划

后续计划：
1. 参考网上已有的免杀Webshell思路以及各种新姿势逐渐完善
2. 目前在从代码方面混淆，是否可以尝试从字节码层面混淆以实现免杀
3. 是否可以多种免杀手段随机组合，生成再进一步的免杀Webshell

## 感谢

参考三梦师傅的Webshell：https://github.com/threedr3am/JSP-Webshells

参考天下大木头师傅的Webshell：https://github.com/KpLi0rn/Shell

参考su18师傅的`defineClass0`方式：https://github.com/su18

## 免责申明

**未经授权许可使用`JSPHorse`攻击目标是非法的**

**本程序应仅用于授权的安全测试与研究目的**


