# tcpdump抓包

tcpdump抓包工具从筛选条件来分类，主要有三大类。

## 一是针对关键字

警如主机名 (HOST)、网段(NET)、端口(PORT)

## 二是针对包的方向

警如源地址(src) 、目的地址(dst)，且可以支持逻辑运算符号 (src and dst、src or dst) 

## 三是针对协议进行抓包

警如抓取tcp/udp/imcp等协议的数据包

1.截取端口eth0的所有关于主机192.168.137.21的数据包

```shell
tcpdump -i eth0 host 192.168.137.21
```

2.截取所有端门的关于192.169.137.0网段的数据包

```shell
tcpdump net 192.168.137.0/24
```

3、截取端口eth0的关于192.169.137.0网段且端口为23的数据包

```shell
tcpdump -i eth0 net 192.168.137.0/24 and port 23
```

4、截取主机名为hostname的主机发送的通信数据包

```shell
tcpdump src host hostname
```

5、在端口etho上截取主机名为hostname的主机接收的的通信数据包

```shell
tcpdump -i eth0 dst host hostname
```

6.用tcpdum命令可以抓指定IP的包，具体命令为:

```shell
tcpdump tcp -i eth1 -t -s 0 - 100 and dst port 22 and src net 192.168.1.1 -w ./target.cap
```

## 搭建tls1.2，tls1.3.通过tcpdump分析，分析tls1.2，tls1.3的区别。cipher在里面的作用

要搭建TLS 1.2和TLS 1.3协议，首先需要确保所使用的服务器和客户端都支持相应的协议版本。然后，可以按照以下步骤进行搭建：

1. 生成服务器证书和私钥：使用工具如OpenSSL生成服务器证书和私钥文件。

2. 配置服务器：将生成的证书和私钥配置到服务器上，并配置服务器以支持TLS 1.2和TLS 1.3协议。

3. 配置客户端：将生成的证书配置到客户端上，并配置客户端以支持TLS 1.2和TLS 1.3协议。

4. 进行握手：启动服务器和客户端，并进行TLS握手过程。

在进行TLS 1.2和TLS 1.3的分析之前，你需要安装并使用tcpdump工具来捕获网络流量。可以使用以下命令来捕获TLS流量：

```shell
sudo tcpdump -i <interface> -w tls_traffic.pcap port <port>
```

其中，`<interface>`是你要捕获流量的网络接口，`<port>`是你要捕获流量的端口号。将流量保存到`tls_traffic.pcap`文件中。

然后，可以使用Wireshark等工具来打开`tls_traffic.pcap`文件，并分析TLS 1.2和TLS 1.3的区别。以下是一些可能的区别：

1. 协议版本：在TLS握手过程中，可以从ClientHello和ServerHello消息中识别所使用的`TLS协议版本`。

2. 密钥交换算法：TLS 1.2和TLS 1.3支持不同的`密钥交换算法`。可以从握手消息中查看所使用的算法。

3. 加密套件：TLS 1.2和TLS 1.3支持不同的`加密套件`。可以从握手消息中查看所使用的加密套件。

4. 握手过程：TLS 1.3引入了`0-RTT和1-RTT`握手模式，与TLS 1.2的握手过程有所不同。可以从握手消息中观察到这些差异。

至于cipher在TLS中的作用，cipher是加密套件中的一部分，用于`加密和解密`通信数据。它由`加密算法`、`密钥交换算法`和`消息认证码算法`组成。在TLS握手过程中，客户端和服务器会协商选择一个适合双方的cipher。所选择的cipher将用于加密和解密TLS通信中的数据。不同的cipher具有不同的安全性和性能特性，因此在选择cipher时需要综合考虑安全性和性能需求。