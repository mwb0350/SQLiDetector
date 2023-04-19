# [SQLiDetector：一款功能强大的SQL注入漏洞检测工具](https://mp.weixin.qq.com/s/uMnpi_f-5MfxNwj7uIZZng)

本文共 1857 字阅读完需 8 分钟

> 该工具支持BurpBouty配置文件。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR3ibQXnnWM9sE3TCmAH9tTFrrFBCgb19I8rggpQia1BiaMSmKRkSiar8eaLR1icb17ITMlaFGarCAsFHoqQ/640?wx_fmt=jpeg) 



## **关于SQLiDetector**

SQLiDetector是一款功能强大的SQL注入漏洞检测工具，该工具支持BurpBouty配置文件，可以帮助广大研究人员通过发送多个请求（包含14种Payload）并检查不同数据库的152个正则表达式模式来检测基于错误的SQL注入漏洞。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR3ibQXnnWM9sE3TCmAH9tTFrrFtsq88VXjMHRic7HvmGv1Jfvz9GX7nHKoYrnszUaBR44ITOXrXcFtLA/640?wx_fmt=jpeg) 



## **功能介绍**

该工具的主要目标是帮助研究人员通过使用不同的Payload来扫描基于错误的SQL注入漏洞，例如：

```scilab
'123
''123
`123
")123
"))123
`)123
`))123
'))123
')123"123
[]123
""123
'"123
"'123
\123
```

并且支持针对不同数据库的152中错误正则表达式模式。



## **工具运行流程**

1、运行子域名搜索工具；

2、将所有收集到的子域名传递给httpx或httprobe来收集活动子域；

3、使用你的链接和URL工具获取所有的waybackurl，如waybackurl、gau、gauplus等；

4、使用URO工具对其进行过滤并降低噪声；

5、获取仅包含参数的所有链接，可以使用grep或gf工具；

6、将最终的URL结果文件传递给SQLiDetector并进行测试；

最终的URL结果文件内容类似如下：

```awk
https://aykalam.com?x=test&y=fortest
http://test.com?parameter=ayhaga

```



## **工具运行机制**

该工具与其他类似SQL注入检测工具的区别在于，如果我们拿到了一个类似下列形式的链接：

```awk
https://example.com?file=aykalam&username=eslam3kl

```

即拥有了两个参数，那么该工具将会创建两个可能存在漏洞的URL地址。

1、下列形式的地址适用于每一个Payload：

```dts
https://example.com?file=123'&username=eslam3kl


https://example.com?file=aykalam&username=123'
```

2、工具将会对每一个URL链接发送一个请求，并使用正则表达式检测是否匹配其中某个模式；

3、针对任何包含漏洞的链接地址，工具将会在单独的文件中进行过程存储；



## **工具安装和使用**

广大研究人员可以使用下列命令将该项目源码克隆至本地：

```awk
git clone https://github.com/eslam3kl/SQLiDetector.git

```

然后运行下列命令安装工具所需的依赖组件：

```awk
~/eslam3kl/SQLiDetector# pip3 install -r requirements.txt

```

接下来就可以运行该工具了：

```awk
# cat urls.txt
http://testphp.vulnweb.com/artists.php?artist=1
# python3 sqlidetector.py -h
usage: sqlidetector.py [-h] -f FILE [-w WORKERS] [-p PROXY] [-t TIMEOUT] [-o OUTPUT]
A simple tool to detect SQL errors
optional arguments:
  -h, --help            show this help message and exit]
  -f FILE, --file FILE  [File of the urls]
  -w WORKERS, --workers [WORKERS Number of threads]
  -p PROXY, --proxy [PROXY Proxy host]
  -t TIMEOUT, --timeout [TIMEOUT Connection timeout]
  -o OUTPUT, --output [OUTPUT [Output file]
# python3 sqlidetector.py -f urls.txt -w 50 -o output.txt -t 10
```



## **BurpBounty模块**

我们还创建了一个BurpBounty配置文件，它会使用相同Payload并在不同的位置注入，例如：参数名、参数值、Header和路径。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR3ibQXnnWM9sE3TCmAH9tTFrrANqc9VTEtQibl219Iy0MamtkuVM59CCwOPX32CxlWDCJthKeDcicrSpw/640?wx_fmt=jpeg) 

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR3ibQXnnWM9sE3TCmAH9tTFrrr3X1YU9SoMZZibqWiajD42WgX0XH6DhiayzPzIkxLRhgoEibqo4jsibO8wQ/640?wx_fmt=jpeg) 



## **工具运行结果**

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR3ibQXnnWM9sE3TCmAH9tTFrrKGClrXp2Z0gqKg2CyX6VMfay8YbdgpLHYYWb9cHHyBFZw03ZH0NAnw/640?wx_fmt=jpeg) 



## **项目地址**

**SQLiDetector**：

https://github.com/eslam3kl/SQLiDetector



## **参考资料**

> https://github.com/sqlmapproject/sqlmap/blob/master/data/xml/errors.xml