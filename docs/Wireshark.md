# Wireshark







## Wireshark捕获本机 HTTPS请求（TLS解密）
从 Wireshark 3.0 开始，TLS 剖析器已从 SSL 重命名为 TLS。使用 ssl 显示过滤器将发出警告，应使用tls。

Wireshark本身针对TLS协议的解密提供了一下三种主要方式：
![](./images/Wireshark/image.png)
1. 推荐方式，读取 TLS 密钥日志文件以进行解密的路径。
2. 使用 RSA 私有密钥进行解密。
3. 使用预共享密钥 （PSK） 解密。

密钥日志文件是一种通用机制，即使正在使用 Diffie-Hellman （DH） 密钥交换，它也始终启用解密。RSA 私钥仅在有限数量的情况下有效。

密钥日志文件是在设置 SSLKEYLOGFILE 环境变量时由 Firefox、Chrome 和 curl 等应用程序生成的文本文件。准确地说，它们的底层库（NSS、OpenSSL 或 boringssl）将所需的每个会话密钥写入文件。此文件随后可以在 Wireshark 中配置（#使用 （Pre）-Master Secret）。

RSA 私钥文件只能在以下情况下使用：
- 服务器选择的密码套件未使用 （EC）DHE。
- 协议版本为 SSLv3，（D）TLS 1.0-1.2。它不适用于 TLS 1.3。
- 私钥与服务器证书匹配。它不适用于客户端证书，也不适用于证书颁发机构 （CA） 证书。
- 会话尚未中断。握手必须包括 ClientKeyExchange 握手消息。

通常建议使用密钥日志文件，因为它在所有情况下都有效，但需要能够持续从客户端或服务器应用程序导出密钥。RSA 私钥的唯一优点是，它只需要在 Wireshark 中配置一次即可启用解密，但受上述限制的约束。

### 相关设置项
转到 Edit -> Preferences。打开 Protocols （协议） 树并选择 TLS。或者，在数据包列表中选择一个 TLS 数据包，右键单击数据包详细信息视图中的 TLS 层，然后打开 Protocol preferences 菜单。

使用TLS协议解密需要注意以下设置项：
- (Pre)-Master-Secret log filename （tls.keylog_file）：设置为 TLS 密钥日志文件的路径以进行解密。
针对需要提供给Wireshark的TLS 密钥日志文件的生成步骤，下面将会以Firefox为示例进行TLS 密钥日志文件的生成：
    - 完全关闭浏览器（检查您的任务管理器以确保）。
    - 将环境变量 SSLKEYLOGFILE 设置为可写文件的绝对路径。
    - 启动浏览器。
    ```shell
    export SSLKEYLOGFILE=$HOME/Desktop/keylogfile.txt
    open -a firefox
    ```
    - 验证是否已创建步骤 2 中的位置。
    - 在 Wireshark 中，转到 Edit -> Preferences -> Protocols -> TLS，然后将 （Pre）-Master-Secret 日志文件名首选项更改为步骤 2 中的路径。
    - 启动 Wireshark 捕获。
    - 打开一个网站，例如 https://www.wireshark.org/。
    - 检查解密后的数据是否可见。例如，使用 tls and (http or http2) 过滤器。
- RSA 密钥列表：配置用于解密的 RSA 私钥。已弃用，取而代之的是 Preferences -> RSA Keys 对话框。

- Pre-Shared-Key：用于配置 PSK 密码套件的解密密钥。被一些 IoT 嵌入式设备使用，尤其是 MQTT。
- TLS debug file （tls.debug_logfile）：设置后将写入有关解密过程的内部详细信息。将包含解密结果和此过程中使用的密钥。这可用于诊断解密失败的原因。

启用 TLS 解密还需要以下 TCP 协议首选项：
- 允许 subdissector 重新组合 TCP 流。默认启用。
- 重新组合乱序段（自 Wireshark 3.0 起，默认禁用）。

### 显示过滤器

在显示过滤器中仅显示TLS时应使用tls。
TLS 显示过滤器字段的[完整列表](https://www.wireshark.org/docs/dfref/t/tls.html)可在显示过滤器参考中找到。

### 捕获过滤器

捕获时，您无法直接过滤 TLS 协议。但是，如果您知道使用的 [TCP](https://wiki.wireshark.org/Transmission_Control_Protocol) 端口，则可以过滤该端口，例如使用` tcp port 443`。



[Wireshark 官方使用说明](https://www.wireshark.org/docs/wsug_html_chunked/)

[Wireshark TLS部分使用说明](https://wiki.wireshark.org/TLS)