---
title: DNS
date: 2018-10-17
tags: protocol
---

## 1. 概述
DNS协议用于将便于记忆的域名（例如 example.com）转换为Internet通信地址（例如 192.168.0.1)。在HOSTS.txt的基础上，DNS在设计时使用了层次式的名称空间，域名使用'.'作为名称的边界分隔符（例如 baidu.com中含有'baidu','com'两级名称）。

## 2. ZONE
在DNS中，整个域名系统被划分为许多不同的zone。这些zone明确的划分DNS名称空间的管理区域。一个DNS zone是由特定组织或管理员管理的DNS名称空间的一部分。域名空间是一个分层树，DNS根域位于顶部。DNS zone可以从层次树中的域开始，也可以向下扩展到子域，以便一个实体可以管理多个子域。

zone的所有信息都存储在DNS zone文件中，它由Resource Record（RR）, 命令和注释构成。在Zone文件中，RR使用文本格式存储；而在DNS消息中，RR使用的则是二进制格式存储。

### 2.1 文本格式RR
文本格式RR的格式如下：
```TXT
OWNER-NAME  TTL  CLASS  TYPE  TYPE-SPECIFIC-DATA
```
其中各项的含义为：
- OWNER-NAME: 记录的拥有者，
- TTL: 缓存的有效时间（秒）
- CLASS: 协议族，同常为IN，即Internet 协议族
- TYPE: 资源记录类型，常见的有A（IPv4地址），MX（邮件服务器）等
- TYPE-SPECIFIC-DATA: 数据存储区域

### 2.2 Zone文件示例
Zone 文件由RR，命令和注释组成。Zone 文件的第一个RR必须是SOA记录,其中保存着当前Zone 文件的重要信息，如管理者的联系方式等。如下是一个Zone file示例。
```TXT
; zone file for example.com
$TTL 2d    ; 172800 secs default TTL for zone

@     IN    SOA     ns1.example.com. hostmaster.example.com. (
                      2003080800 ; se = serial number
                      12h        ; ref = refresh
                      15m        ; ret = update retry
                      3w         ; ex = expiry
                      3h         ; min = minimum
                      )
      IN    NS      ns1.example.com.
      IN    MX      10  mail.example.net.
joe   IN    A       192.168.254.3
www   IN    CNAME   joe 
```
该Zone file中， 以';'开始的行均为注释。以'$'开始的则是命令，如第二行的TTL命令，用于设置当前Zone file的默认缓存有效时间。紧接着便是一组RR。关于每条RR的具体含义， 可以参考RFC1035。

## 3. DNS 消息格式
DNS 消息格式如下。
```TXTGRAPH
    +---------------------+
    |        Header       |
    +---------------------+
    |       Question      | the question for the name server
    +---------------------+
    |        Answer       | RRs answering the question
    +---------------------+
    |      Authority      | RRs pointing toward an authority
    +---------------------+
    |      Additional     | RRs holding additional information
    +---------------------+
```
其中各项的含义为：
- HEADER： DNS头部信息
- Question： 查询数据存放区域，查询域名时，DNS客户端将待查询的域名放在这里。
- Answer: 响应数据存放区域，DNS服务器将结果放在这里返回给DNS客户端。
- Authority: 授权资源存放区域
- Additional： 附加数据存放区域

### 3.1 消息头部
其中头部信息的内容如下。
```TXTGRAPH
                                    1  1  1  1  1  1
      0  1  2  3  4  5  6  7  8  9  0  1  2  3  4  5
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                      ID                       |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |QR|   Opcode  |AA|TC|RD|RA|   Z    |   RCODE   |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                    QDCOUNT                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                    ANCOUNT                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                    NSCOUNT                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                    ARCOUNT                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
```
其中各项的含义为：
- ID: 用于匹配查询和响应。
- QR：0-查询，1-响应
- Opcode: 查询类型
  - QUERY 标准查询
  - IQUERY 递归查询 (弃用于RFC3425)
  - STATUS 服务状态查询
  - NOTIFY 数据库更新通知 (RFC1996)
  - UPDATE 动态数据库更新 (RFC2136) 
- AA: 已授权响应标记位， 0-从缓存中获取的查询结果，1-从权威fu服务器上获取到的查询结果
- TC: 截断标记位， UDP无法装载整个查询结果。
- RD: 期望递归查询标记位（查询中设置）
- RA: 支持递归查询标记位（响应中设置）
- Z: 保留
- RCODE: 返回码，用于扩展响应内容
- QDCOUNT: 查询数量
- ANCOUNT: 响应数量
- NSCOUNT: 域名服务器数量
- ARCOUNT: 附加记录数量

### 3.2 二进制格式RR
在DNS消息中，使用二进制格式的RR来返回查询结果，二进制格式RR的格式如下：
```TXT
                                    1  1  1  1  1  1
      0  1  2  3  4  5  6  7  8  9  0  1  2  3  4  5
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                                               |
    /                                               /
    /                      NAME                     /
    |                                               |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                      TYPE                     |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                     CLASS                     |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                      TTL                      |
    |                                               |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                   RDLENGTH                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--|
    /                     RDATA                     /
    /                                               /
```
其中各项的含义为：
- NAME： 记录的拥有者
- TYPE：资源记录类型，常见的有A（IPv4地址），MX（邮件服务器）等
- CLASS：协议族，同常为IN，即Internet 协议族
- TTL：缓存的有效时间（秒）
- RDLENGTH：RDATA的长度
- RDATA：用于资源描述

## 4. 域名格式
域名遵循如下的语法。一个域名有数个label构成，lable之间以'.'相连； label是以字母开始，字母或数字结束的，中间可包含字母，连字符，数字的字符串。label的长度不超过64字节，而且字母是不区分大小写的，即'A'和'a'是等价的。

```TXT
<domain> ::= <subdomain> | " "
<subdomain> ::= <label> | <subdomain> "." <label>
<label> ::= <letter> [ [ <ldh-str> ] <let-dig> ]
<ldh-str> ::= <let-dig-hyp> | <let-dig-hyp> <ldh-str>
<let-dig-hyp> ::= <let-dig> | "-"
<let-dig> ::= <letter> | <digit>
<letter> ::= any one of the 52 alphabetic characters A through Z in
upper case and a through z in lower case
<digit> ::= any one of the ten digits 0 through 9
```

## REFERENCE
[1] RFC1034, RFC1035

[2] [Let's hand write DNS messages](https://routley.io/tech/2017/12/28/hand-writing-dns-messages.html)

[3] [What is a DNS ZONE?](https://ns1.com/resources/dns-zones-explained)

[4] [DNS Resource Records (RRs)](http://www.zytrax.com/books/dns/ch8/)

[5] [Protocol and Format](http://www-inf.int-evry.fr/~hennequi/CoursDNS/NOTES-COURS_eng/msg.html)

[6] [DNS ZONE](https://www.cloudflare.com/learning/dns/glossary/dns-ZONE/)
