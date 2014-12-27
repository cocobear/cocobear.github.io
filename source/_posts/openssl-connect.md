title: Openssl建立SSL连接分析
tags:
  - https
  - openssl
id: 659
categories:
  - Life
date: 2009-05-22 10:29:15
---

SSL_library_init();
SSL的初始化过程，只是添加一些算法，使用EVP_add_cipher函数。

ssl_method = SSLv3_client_method();
设置使用的SSL版本，SSLv3_client_method 函数的定义是通过IMPLEMENT_ssl3_meth_func(ssl/s3_clnt.c)这个宏来实现，而这个宏在ssl/ssl_locl.h 文件中实现，使用了粘贴宏的技巧来根据传入不同的函数名生成不同的函数定义。
SSLv3_client_method 定义了一组关于SSL V3的操作函数：
	static SSL_METHOD func_name##_data= { 
		SSL3_VERSION, 
		ssl3_new, 
		ssl3_clear, 
		ssl3_free, 
		s_accept, 
		s_connect, 
		ssl3_read, 
		ssl3_peek, 
		ssl3_write, 
		ssl3_shutdown, 
		ssl3_renegotiate, 
		ssl3_renegotiate_check, 
		ssl3_get_message, 
		ssl3_read_bytes, 
		ssl3_write_bytes, 
		ssl3_dispatch_alert, 
		ssl3_ctrl, 
		ssl3_ctx_ctrl, 
		ssl3_get_cipher_by_char, 
		ssl3_put_cipher_by_char, 
		ssl3_pending, 
		ssl3_num_ciphers, 
		ssl3_get_cipher, 
		s_get_meth, 
		ssl3_default_timeout, 
		&SSLv3_enc_data, 
		ssl_undefined_void_function, 
		ssl3_callback_ctrl, 
		ssl3_ctx_callback_ctrl, 
	}; 

SSL_CTX = SSL_CTX_new(ssl_method);
创建SSL_CTX这个结构体，进行初始化赋值操作，设置一些参数，使用默认的engine。openssl的engine是对基础的加密算法进行了封装，这样可以方便的对底层的加密算法进行替换，例如你可能使用硬件加密来替换掉openssl的软实现加密算法，而你所做的只是按照engine的接口实现相关的算法，然后设置engine的名字。例如U-KEY的RSA签名必须在硬件中完成，这时你就可以通过engine调用CSP来实现签名算法。

SSL_CTX_use_certificate_file(SSL_CTX,"client.cer",SSL_FILETYPE_PEM)
设置客户端使用的证书，从文件中读取BASE64(SSL_FILETYPE_PEM这个参数指定)编码的证书，然后转换为openssl 的X509结构，提取公钥，设置SSL_CTX相关的结构。

SSL_CTX_use_PrivateKey_file(SSL_CTX, "client.key", SSL_FILETYPE_PEM)
设置客户端使用的私钥，这种方式也是直接从文件中读取，更安全的方式是使用SSL_CTX_use_RSAPrivateKey(SSL_CTX, RSA)，RSA是一个包含了RSA算法相关数据的结构体，这里定义了RSA算法相关的操作，在这里可以把私钥相关的算法操作使用自己的CSP来完成，而不是直接使用私钥完成。

SSL = SSL_new(SSL_CTX);
创建SSL 这个复杂的结构体。太多了，就不细说了。

SSL_set_fd(SSL, sock_fd);
设置连接使用的socket的fd，我们要做的socket操作只是得到这个fd，read,write操作将由openssl来完成。

SSL_connect(SSL);
这个函数将根据前面设置的SSL版本来进行相应的连接操作。

server_cert = SSL_get_peer_certificate(SSL);
得到服务器端的证书，下来可能要做一些服务器证书的验证，这里就省略了。

X509_free(server_cert);
SSL_write();
SSL_read();
接下来的操作就很好理解了.

[http://code.google.com/p/cocobear/source/browse/trunk/openssl/openssl_test.cpp](http://code.google.com/p/cocobear/source/browse/trunk/openssl/openssl_test.cpp)

上面写了一代简单的openssl进行HTTPS通信的代码，在Win和Linux下都可以运行，只是在socket操作有一点点的差异。
## Comments

**[crazyfranc](#5947 "2009-05-23 02:54:43"):** GG啊,看你对ssl和u—key这么有研究，你可以来应聘sinfor的移动应用部门了。那边老大和我是哥们儿~

**[Kermit](#5943 "2009-05-22 19:50:46"):** 你要专攻安全领域了？

**[可可熊](#5945 "2009-05-22 21:09:13"):** 工作需要嘛。

**[可可熊](#5948 "2009-05-26 15:18:48"):** 你们做这一块吗？

