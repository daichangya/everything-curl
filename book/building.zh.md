
# 建筑安装

这个项目的源代码的编写方式允许它在几乎任何操作系统和平台上进行编译和构建,而限制和要求尽可能少.

如果您有一个32位(或更大)的CPU体系结构,如果您有一个符合C89的编译器,并且如果您有一个支持套接字API的POSI,那么您很可能为您的目标系统构建curl和libcurl.

对于最流行的平台,curl项目附带了已经完成并准备允许您自己轻松构建的构建系统.

还有一些友好的人和组织,他们把curl和libcurl的二进制包放在一起,并使它们可供下载.下面将讨论不同的选择.

## 最新版本?

看着cURL的网站[HTTPS://Curl HAXX.SE](https://curl.haxx.se)您可以从项目中获知最新的cURL和libcurl版本.这是你能得到的最新的源代码包.

当您为操作系统或所选发行版选择预构建和预打包的版本时,您可能并不总是能找到最新的版本,但是您可能必须满足于某个人为您的环境打包的最新版本,或者您需要自己从头构建它.总工程师.

CURL项目还提供了关于这个URL上更为机器可读的格式的最新版本的信息:`https://curl.haxx.se/info`.

## 走开!

当然,当从源代码构建时,也可以选择构建最新版本.[GIT仓库](https://github.com/curl/curl). 然而,它可能有点脆弱,可能需要稍微注意细节.
