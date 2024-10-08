## 前言

在github上配置ssh key很容易，网上一大堆教程，但基本没有详细解释其原理的，为什么要配？每使用一台主机都要配？配了为啥就不用密码了？下面将简单通俗地解释一下。

我们在往github上push项目的时候，如果走https的方式，每次都需要输入账号密码，非常麻烦。而采用ssh的方式，就不再需要输入，只需要在github自己账号下配置一个ssh key即可。


## 配置SSH

git使用SSH配置， 初始需要以下三个步骤

1. 使用秘钥生成工具生成rsa秘钥和公钥
2. 将rsa公钥添加到代码托管平台
3. 将rsa秘钥添加到ssh-agent中，为ssh client指定使用的秘钥文件

具体操作如下：

第一步：检查本地主机是否已经存在ssh key

```bash
cd ~/.ssh
ls //看是否存在 id_rsa 和 id_rsa.pub文件，如果存在，说明已经有SSH Key
```

如下图所示，则表明已经存在

![1728041323507(1)](https://github.com/user-attachments/assets/264239ce-5c29-473d-a24b-23e90722361f)

如果存在，直接跳到第三步

第二步：生成ssh key

如果不存在ssh key，使用如下命令生成

```bash
ssh-keygen -t rsa -C "xxx@xxx.com"
//执行后一直回车即可
```

生成完以后再用第二步命令，查看ssh key

第三步：获取ssh key公钥内容（id_rsa.pub）

```bash
cd ~/.ssh
cat id_rsa.pub
```

如下图所示，复制该内容

![image](https://github.com/user-attachments/assets/0a388afb-21cb-4105-9ca8-cc565fb631f5)


第四步：Github账号上添加公钥


进入Settings设置

![image](https://github.com/user-attachments/assets/ab4f6f16-798f-4a6b-9c22-48e0ad21ce77)

添加ssh key，把刚才复制的内容粘贴上去保存即可

![image](https://github.com/user-attachments/assets/6e177142-f043-4dcb-90a9-c66c3c61cbf8)

第五步：验证是否设置成功

```bash
ssh -T git@github.com
```

显示如下信息表明设置成功

![image](https://github.com/user-attachments/assets/2e2534f5-1593-4566-b5ce-af026ca9c8c7)

设置成功后，即可不需要账号密码clone和push代码

注意之后在clone仓库的时候要使用ssh的url，而不是https！

## 验证原理

SSH登录安全性由非对称加密保证，产生密钥时，一次产生两个密钥，一个公钥，一个私钥，在git中一般命名为id_rsa.pub, id_rsa。

那么如何使用生成的一个私钥一个公钥进行验证呢？

本地生成一个密钥对，其中公钥放到远程主机，私钥保存在本地
当本地主机需要登录远程主机时，本地主机向远程主机发送一个登录请求，远程收到消息后，随机生成一个字符串并用公钥加密，发回给本地。本地拿到该字符串，用存放在本地的私钥进行解密，再次发送到远程，远程比对该解密后的字符串与源字符串是否等同，如果等同则认证成功。
通俗解释！！
重点来了：一定要知道ssh key的配置是针对每台主机的！，比如我在某台主机上操作git和我的远程仓库，想要push时不输入账号密码，走ssh协议，就需要配置ssh key，放上去的key是当前主机的ssh公钥。那么如果我换了一台其他主机，想要实现无密登录，也就需要重新配置。

下面解释开头提出的问题：

（1）为什么要配？

配了才能实现push代码的时候不需要反复输入自己的github账号密码，更方便

（2）每使用一台主机都要配？

是的，每使用一台新主机进行git远程操作，想要实现无密，都需要配置。并不是说每个账号配一次就够了，而是每一台主机都需要配。

（3）配了为啥就不用密码了？

因为配置的时候是把当前主机的公钥放到了你的github账号下，相当于当前主机和你的账号做了一个关联，你在这台主机上已经登录了你的账号，此时此刻github认为是该账号主人在操作这台主机，在配置ssh后就信任该主机了。所以下次在使用git的时候即使没有登录github，也能直接从本地push代码到远程了。当然这里不要混淆了，你不能随意push你的代码到任何仓库，你只能push到你自己的仓库或者其他你有权限的仓库！
