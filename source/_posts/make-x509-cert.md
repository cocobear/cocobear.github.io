title: 手动生成X509证书
tags:
  - x509
id: 737
categories:
  - 编程相关
date: 2009-10-19 11:24:04
---

X509证书的详细描述在[rfc2459](http://www.ietf.org/rfc/rfc2459.txt)中。

简单的来说X509证书是这样的：

     Certificate  ::=  SEQUENCE  {> 
             tbsCertificate       TBSCertificate,> 
             signatureAlgorithm   AlgorithmIdentifier,> 
             signatureValue       BIT STRING  }> 
     
        TBSCertificate  ::=  SEQUENCE  {> 
             version         [0]  EXPLICIT Version DEFAULT v1,> 
             serialNumber         CertificateSerialNumber,> 
             signature            AlgorithmIdentifier,> 
             issuer               Name,> 
             validity             Validity,> 
             subject              Name,> 
             subjectPublicKeyInfo SubjectPublicKeyInfo,> 
             issuerUniqueID  [1]  IMPLICIT UniqueIdentifier OPTIONAL,> 
                                  -- If present, version shall be v2 or v3> 
             subjectUniqueID [2]  IMPLICIT UniqueIdentifier OPTIONAL,> 
                                  -- If present, version shall be v2 or v3> 
             extensions      [3]  EXPLICIT Extensions OPTIONAL> 
                                  -- If present, version shall be v3> 
             }

X509由三部分组成，分别是TBSCertificate，AlgorithmIdentifier ，signatureValue 。TBSCertificate包含了证书的详细信息，如证书编号，颁发者，发行者，过期日期等;AlgorithmIdentifier 是指证书自身使用数字签名算法标识，TBSCertificate中也有一个AlgorithmIdentifier，这个是证书可以用作的数字签名算法标识;signatureValue是指使用AlgorithmIdentifier 所指定的算法对整个TBSCertificate签名得到的数字签名。

可以使用openssl命令行工具生成X509证书，不过需要用openssl先生成一对RSA密钥对，如果只有公钥需要生产证书，就需要自己通过编程调用openssl函数来生成证书了。openssl自带的例子中有一个生成证书的例子，不过也是先生成一对RSA密钥对。

通过对openssl源代码的分析，可以X509_set_pubkey函数只是用到了RSA结构中的公钥，所以我们可以通过自己创建一个openssl的RSA结构，只设置RSA密钥对中的公钥，来完成证书的生成。

[参考代码](http://code.google.com/p/cocobear/source/browse/trunk/openssl/mkcert.cpp)

该函数入口参数是一个证书的主题名，和128字节的公钥（1024位;RSA中的modulus INTEGER----n），返回一个证书ID，和DER编码的证书，默认使用的publicExponent INTEGER, -- e为65535。

参考：
[RFC2459](http://www.ietf.org/rfc/rfc2459.txt)
[PKCS#1](http://www.rsa.com/rsalabs/node.asp?id=2125)
[openssl](www.openssl.org/)
## Comments

**[欢欢](#7065 "2010-01-04 13:26:21"):** 搜些证书的东西，竟然链接到了你这里，呵呵~

**[Kermit Mei](#6630 "2009-10-23 18:05:59"):** 现在都不敢来DP这里了，每次进来，都是满头雾水……貌似每篇我都看不懂了……

**[草儿](#6618 "2009-10-19 17:06:10"):** 你到底在公司是做什么，怎么什么都研究

**[luguo](#6621 "2009-10-19 22:51:49"):** DP同学是名全面的选手。。。

**[Jerry](#8823 "2010-10-22 14:47:14"):** 有个供参考的资料，X.509标准简介:http://www.wosign.com/Basic/x509.htm

