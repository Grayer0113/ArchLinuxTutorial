# 魔法学院

为了你的人身安全考虑，请不要在使用科学上网的同时使用 QQ、微信等专有软件。在移动设备上，这些软件有各种办法获取你所安装的应用列表，并通过各种方法查询你所进行的活动和发布的言论，并进行进一步的控制，这不是危言耸听，有无数的先例为证。

从理论上来说，PC 设备上也可以完成这样的操作。但幸运的是，PC 上你可以利用沙箱等软件对这些专有软件进行隔离运行，使其无法获取更多的个人信息。

为了可以正常查阅资料，访问程序源代码或者讨论程序问题，故此你需要配置科学上网。本章节内容简单介绍了如何在 Linux 下配置科学上网，之后的章节也建立在你已正确配置科学上网的前提下进行，否则后续一些软件你很可能无法正常安装使用。

## 准备节点

简单来说，节点应该是类似下面的神秘链接，不论你通过什么办法，如果你已拥有这样的链接或订阅，那么可直接阅读下一小节。

```bash
ss://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
vmess://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

如果你还没有这样的链接，那么你有两个选择，一个是自行部署服务器，另一个是在线购买相关订阅（俗称机场）。

如果你打算自行部署服务器，那么你需要拥有一个地理位置在中国大陆以外的服务器并进行节点搭建，这明显超出了本教程的范围，这里提供几篇 GFW Report 的文章作为参考。

- [如何部署一台抗封锁的 Shadowsocks-libev 服务器](https://gfw.report/blog/ss_tutorial/zh/)
- [防御 GFW 主动探测的实用指南](https://gfw.report/blog/ss_advise/zh/)

如果你打算购买机场订阅，我们推荐两个较为稳定机场，你可以自行选择。需要注意的是，机场在中国大陆属于非法产业，任何机场都有概率跑路，建议使用月付/季付等短期订阅，以免产生较大损失。**注意：我们不对推荐的机场提供任何担保，你可以自行寻找。**

- [老百姓机场](https://xn--mes53ddysu0o3gl.com/#/register?code=Q7v80I0Q)：该机场订阅非常便宜，且流量较多，适合作为备用和大型项目的上传下载
- [Yukihasu](https://azumase.ren/#/register?code=jdxcUI2r)：该机场相对稳定，据说不会轻易跑路，实测特殊期间也能正常使用

## 客户端推荐

我们推荐两款用于客户端的魔法上网软件，它们分别为 v2rayA^AUR^ 与 Nekoray^AUR^ 。v2ray 是老牌的客户端代理软件，几乎所有的机场都支持它；Nekoray 有着一些新的特性支持，同时完全兼容 v2ray，你可以自行选择使用。

### v2rayA

首先安装 v2ray 核心和分流代理支持：

**==警告：在执行安装命令前我们非常郑重的建议你更换软件源为非权威国家！==**

```bash
sudo pacman -S v2ray									# v2ray核心
sudo pacman -S v2ray-domain-list-community v2ray-geoip	# 分流代理支持
```

然后安装 v2rayA，它是 v2ray 的一个基于 Web 的 GUI 前端，同时启用相关服务：

```bash
paru -S v2raya											# 安装v2raya
sudo systemctl enable --now v2raya						# 启用服务
```

关于 v2rayA 的使用教程请阅读：[官方文档](https://v2raya.org/)。

如果你确实无法使用以上命令正常的安装 v2ray 相关服务，v2rayA 的官方文档提供了从 GitHub 手动安装的方式，你可以暂时使用，当你可以正常使用代理后再次按照以上命令安装即可。

### Nekoray

首先安装 Nekoray 程序本体：

```bash
paru -S nekoray											# 安装Nekoray
```

然后安装 sing-box 分流代理支持：

```bash
paru -S sing-geoip sing-geosite							# sing-box分流代理支持
paru -S sing-geoip-rule-set sing-geosite-rule-set		# 地区规则支持
```

安装完成后打开 Nekoray，第一次使用会提示你选择核心，我们使用 sing-box 核心。

更多关于 Nekoray 的使用教程请阅读：[官方文档](https://matsuridayo.github.io/n-configuration/)。
