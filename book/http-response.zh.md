
### HTTP响应

当HTTP客户端，与服务器进行HTTP对话时,服务器*将*使用HTTP响应消息，或curl认为响应是一个错误,并返回带有错误消息"来自服务器的空回复"的52错误代码.

#### HTTP响应的大小

HTTP响应具有一定的大小,cURL需要计算出来.有几种不同的方法来指示HTTP响应的结束,但最基本的方法是使用响应中的`Content-Length:`标头,指定响应体中的字节数.

一些早期的HTTP服务器实现存在文件大小，大于2GB的问题,并且错误地发送了Content-Length: 具有负值的大小或者只是纯错误的数据. cURL可以用`--ignore-content-length`完全忽略Content-Length:标题. 这样做可能有一些负面的副作用,但至少应该让你得到数据.

### HTTP响应码

HTTP传输在响应第一行中获得3位响应代码.响应代码是服务器向客户端提供，关于请求是如何处理的提示方式.

重要的是要注意,即使响应代码提示，无法交付(或类似)所请求的文档, curl也不认为这是一个错误.curl认为HTTP的成功发送和接收是很好的.

HTTP响应代码的第一个数字是一种"错误类":

-   1xx:瞬态响应,更多即将来临
-   2XX:成功
-   3xx:重定向
-   4XX:客户要求服务器无法提供的东西.
-   5XX:服务器有问题

记住你可以使用cURL的`--write-out`选项以提取响应代码.见[--write-out](usingcurl-verbose.zh.md#--writeout)部分.

### 连接响应代码

由于在同一cURL传输中，可以存在HTTP请求和单独的CONNECT请求,因此我们经常将CONNECT响应(来自代理)，与远程服务器的HTTP响应分离.

Connect也是HTTP请求,因此它在相同的数值范围内得到响应代码,并且您也可以使用`--write-out`提取那个代码.

### 分块传输编码

HTTP 1.1服务器可以决定用"块"编码响应,这是HTTP 1中不存在的特征.

当发送块响应时,没有`Content-Length:`响应指示其大小.相反,有一个`Transfer-Encoding: chunked`标头, 告诉curl有块数据到来,然后在响应体中,数据出现在一系列"块"中. 在开始，每一个块体带有特定的块大小(十六进制), 然后换行然后块的内容.重复这一过程直到响应结束,会用一个零大小的块发出信号. 这种信号是让客户端能够找出已经结束的时机,即使服务器在开始之前并不知道它的全尺寸.通常情况下,当响应是动态的，信号在请求到来的时候生成.

当然,像curl这样的客户端将解码块,而不将块大小显示给用户.

### 压缩传输

可以通过压缩格式发送HTTP上的响应.这通常是服务器在包含一个`Content-Encoding: gzip`服务器时完成的. 作为在响应中对客户端的提示. 压缩是一种提升,当静态资源发送(在被压缩以前的一个时候)，或甚至在运行的时候，CPU功率老比带宽更多更有用. 发送一个更小的数据量通常是首选.

您可以请求curl同时请求压缩内容.*将*自动和转换地解压缩gzip压缩数据时,用`--compressed`接收内容gzip编码(或是,curl的了解任何其他压缩算法):

```
curl --compressed http://example.com/
```

### 传递编码

与传输编码一起使用的一个不太常见的特征是压缩.

压缩本身是常见的.随着时间的推移,如上文所述`Content-Encoding`用于HTTP压缩和Web兼容方式将成为主导. 但是HTTP最初是传输编码允许转换压缩的,curl支持这个特性.

然后,客户端简单地要求服务器进行压缩传输编码,如果可以接受的话,它将用一个标头来响应,标头指示它将和curl将在到达时，转换地解压缩该数据.用户可以`--tr-encoding`请求压缩传输编码.:

```
curl --tr-encoding http://example.com/
```

应该注意的是,野生的HTTP服务器，并没有支持这一点.

### 传递编码传递

在某些情况下,您可能希望使用cURL作为某种代理或其他软件之间的关系.在那些情况下,那么curl处理传输编码报头，并随后转换地解码实际数据的方式可能不理想.如果终端接收机*也*要做同样的事情.

解决是,可以请求curl传递原生数据,而不进行解码. 这意味着压缩传输编码,以分块编码格式或压缩格式等都不用了.

```
curl --raw http://example.com/
```
