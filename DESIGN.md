# NameManager

## 需求

在NDN网络中，需要一种类似于DHCP一般的使设备正常连入网络中的操作。在我的调研中，我认为这是一种类似于Bootstrapping的操作。然而，目前的Bootstrapping主要完成的是NDN设备的接入，并不负责具体的应用的操作。

那么先明确一些概念：

Bootstrapping
Trust Schema
Certification
Signature

明确一些关系：

```yaml
Entity: Producer, Admin, Name, Data, NameZone(Zone), NameSpace(Space)
Direct Relations:
Producer 1-n Name
NameZone 1-n Name
Name 1-1 Data
Admin 1-1 Zone
NameSpace 1-n Zone

Enhanced Entity: Public Key(PRI), Private Key(PUB).
Not only data can be given a name, but also entities.
To validate the integrity of the "name", we need KEY.

Scope: Zone
Zone: public key("name cert", "used by name"), private key("admin"), cert (self-certificated)
Name: public key("verify data", "used by consumer"), private key("producer"), cert("Zone")

So, Assume one "Zone" has one admin who manage the relationship between its "name" and "name public Key".
The "Zone" itself has its "zone public key", it can be self-certificated, or authorized by higher-level infrastructure, or ...
The "Zone" s "admin" also has its "admin public key", or it can be "zone private key", with this, "admin" can manage the "name" and "name public key" under the "zone".

Scope: Role
Admin: Holds "Zone" "private key".
Producer: Holds "Name" "private key".
Consumer: Use "Name" "public key" to verify "signature", user "Name" "Cert" to verify "public key"

Scope: special data
public key
username
```

ICN needs to guarantee (i) exclusive ownership of names by publishers, 出版商对名称的独家所有权
(ii) the arbitration of rightful holders of named resources, and 对命名资源的合法持有人的仲裁
(iii) the verification of the content 消费者对内容源的认证

翻译：(i) producer 拥有这个 name的所有权。(ii) 这个 name究竟由哪位producer持有 (ii) consumer对data的完整性验证。

## 用户角色

#### Admin

与名称空间绑定。负责为特定名称空间下的数据(名字)指派证书。管理用户-公钥、用户-数据、数据-公钥联系。

#### Producer

可以生产数据。

#### Consumer

#### Guest

## 功能

#### 接收消息

使用gondp包

#### 对data签名

producer使用自己的私钥对data包进行签名。

#### 生成自己的公私钥

username, user.pri.key, user.pub.key

#### 验证数据完整性

1、使用生产者的公钥对data包的数字签名进行认证。
2、根据data包中的keylocator字段获取该数据包的证书。
3、直接从CA处获取证书。

