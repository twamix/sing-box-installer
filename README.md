# sing-box-installer

修改自https://github.com/RayWangQvQ/sing-box-installer

基于`sing-box`和`docker`容器化搭建`vmess`、`naiveproxy`和`hysteria2`的保姆级教程。

<!-- more -->

<!-- TOC depthFrom:2 -->

- [1. 大概介绍下](#1-大概介绍下)
    - [1.1. 关于sing-box](#11-关于sing-box)
    - [1.2. 关于naiveproxy](#12-关于naiveproxy)
    - [1.3. 关于hysteria](#13-关于hysteria)
    - [1.4. 关于sing-box的配置](#14-关于sing-box的配置)
- [2. 部署服务端](#2-部署服务端)
    - [2.1. 思路](#21-思路)
    - [2.2. 一键脚本部署](#22-一键脚本部署)
    - [2.3. 手动部署](#23-手动部署)
- [3. 客户端](#3-客户端)
    - [3.1. 安卓-SagerNet](#31-安卓-sagernet)
        - [vmess](#vmess)
        - [3.1.1. hysteria](#311-hysteria)
        - [3.1.2. naive](#312-naive)
    - [3.2. IOS-小火箭](#32-ios-小火箭)
        - [vmess](#vmess-1)
        - [3.2.1. hysteria](#321-hysteria)
        - [3.2.2. naiveproxy](#322-naiveproxy)
    - [3.3. Win-Nekoray](#33-win-nekoray)
        - [vmess](#vmess-2)
        - [3.3.1. hysteria](#331-hysteria)
        - [3.3.2. naive](#332-naive)
    - [3.4. Win-V2RayN](#34-win-v2rayn)
        - [vmess](#vmess-3)
        - [3.4.1. hysteria](#341-hysteria)
        - [3.4.2. naive](#342-naive)
    - [3.5. Mac-Nekoray](#35-mac-nekoray)
- [4. FAQ](#4-faq)
    - [4.1. hy的端口hop](#41-hy的端口hop)

<!-- /TOC -->

省流，可以直接执行：

```
# get permission
sudo -i

# create a dir
mkdir -p ./sing-box && cd ./sing-box

# install
bash <(curl -sSL https://raw.githubusercontent.com/twamix/sing-box-installer/main/install.sh)
```

## 1. 大概介绍下

### 1.1. 关于sing-box

开源地址：[https://github.com/SagerNet/sing-box](https://github.com/SagerNet/sing-box)

`sing-box`是一个开源的**通用代理部署平台**，支持大部分协议，有了它，我可以使用同一套配置规则，部署多个不同的协议。

### 1.2. 关于naiveproxy

开源地址：
- 服务端：[https://github.com/klzgrad/forwardproxy](https://github.com/klzgrad/forwardproxy)
- 客户端：[https://github.com/klzgrad/naiveproxy](https://github.com/klzgrad/naiveproxy)

据说是当前最安全的协议**之一**

### 1.3. 关于hysteria

开源地址：[https://github.com/apernet/hysteria](https://github.com/apernet/hysteria)

`hysteria`的优势是快，基于`quic`协议，走udp，跟它名字一样（歇斯底里），并发去请求扔包，所以快。

已知问题是qos，来自服务商的限制，当请求流量过大时，会被限速、断流。以前看有图比是嫌清晰度不够，用了`hysteria`可能要反过来主动去自己调低清晰度了。

关于安全性，目前墙对UDP的管控技术还没有TCP那么成熟，所以相对来说算比较安全。

### 1.4. 关于sing-box的配置

文档：[https://sing-box.sagernet.org/zh/configuration/](https://sing-box.sagernet.org/zh/configuration/)

部署`sing-box`的关键，就是编写它的配置文件。

`sing-box`抽象出一套配置规则，有DNS，有路由（router），有入站（inbound）和出站（outbound）。

如果之前使用过`v2ray`，对这些概念很熟悉，那么你可以很轻松切换到`sing-box`；

当前sing-box的文档还处于待完善阶段（也可能是故意不想写的太详细），只有对各配置字段的解释，并不会告诉你它是什么以及为什么要这么配。

## 2. 部署服务端

### 2.1. 思路

我个人推荐使用`docker`容器化部署，容器化有很多好处，这里就不多说了。

下面会基于`sing-box`的官方`docker`镜像，使用`docker-compose`进行容器构建。

官方镜像地址：[https://github.com/orgs/SagerNet/packages?repo_name=sing-box](https://github.com/orgs/SagerNet/packages?repo_name=sing-box)

如果你的机器没有装过docker，请先安装docker，安装指令：

```
curl -sSL https://get.docker.com/ | sh
systemctl start docker
systemctl enable docker
```

然后基于docker容器run sing-box的官方镜像，我们只需要配置好配置文件config.json即可。

下面有两种模式：一键脚本部署和手动部署，任选其一即可。

### 2.2. 一键脚本部署

```
# get permission
sudo -i

# create a dir
mkdir -p ./sing-box && cd ./sing-box

# install
bash <(curl -sSL https://raw.githubusercontent.com/twamix/sing-box-installer/main/install.sh)
```
### 2.3. 手动部署

[教程](DIY.md)

## 3. 客户端

### 3.1. 安卓-SagerNet

#### vmess

略

#### 3.1.1. hysteria

![sing-box-client-sagernet-hysteria.jpg](https://996icu.free.hr/static/img/87514a662f253ae2a12d2e2b98441ae5.sing-box-client-sagernet-hysteria.jpg)

其中：

- `混淆密码`是配置中的`obfs`
- `认证荷载`是配置中的`auth_str`

#### 3.1.2. naive

![sing-box-client-sagernet-naive.jpg](https://996icu.free.hr/static/img/a65503bf1d71255a3ff3d0eebff9435f.sing-box-client-sagernet-naive.jpg)

其中，`密码`是配置中的`password`

### 3.2. IOS-小火箭

#### vmess

略

#### 3.2.1. hysteria

![shadowrocket-hy](doc/pic/shadowrocket-hy.png)

#### 3.2.2. naiveproxy

![shadowrocket-naive](doc/pic/shadowrocket-naive.png)

### 3.3. Win-Nekoray
[nekoray](https://github.com/MatsuriDayo/nekoray)

#### vmess

![nekoray-vmess](doc/pic/nekoray-vmess.png)

#### 3.3.1. hysteria
![nekoray-hy](doc/pic/nekoray-hysteria.png)

#### 3.3.2. naive

![nekeray-naive](doc/pic/nekoray-naive.png)

### 3.4. Win-V2RayN

如果你没有必要的理由，请优先选择使用Nekoray。

#### vmess

略

#### 3.4.1. hysteria


![hysteria-v2rayn-add.png](https://996icu.free.hr/static/img/ef39f4212a82a7c5dd5637be568fc63d.hysteria-v2rayn-add.png)

配置文件内容如下：

```
{
  "server": "sample.zai7lou.ml:10080",
  "server_name": "sample.zai7lou.ml",
  "obfs": "nicetofuckyou",
  "auth_str": "1234@qwer",
  "alpn": "h2",
  "up_mbps": 100,
  "down_mbps": 100,
  "socks5": {
    "listen": "127.0.0.1:10808"
  },
  "http": {
    "listen": "127.0.0.1:10809"
  }
}
```

#### 3.4.2. naive


![naive-v2rayn-add.png](https://996icu.free.hr/static/img/6f5d33a2e5a7129118687948251ac82e.naive-v2rayn-add.png)

配置文件内容如下：

```
{
  "listen": "socks://127.0.0.1:10808",
  "proxy": "https://Ray:1234@qwer@sample.zai7lou.ml:8090"
}

```

### 3.5. Mac-Nekoray

同上，跟windows下的Nekoray一样。

## 4. FAQ

### 4.1. hy的端口hop

sing-box暂不支持，目前只能多加几个hy节点，然后在客户端做负载均衡。
