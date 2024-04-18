# 桌面环境与必要应用

本文会带你安装 Plasma (KDE) 桌面环境，并简单配置系统使其可供日常使用。

## 连接网络

对于使用有线上网的用户，我们需要启动 DHCP 服务：

```bash
systemctl start dhcpcd							# 启用DHCP服务
```

对于使用 Wi-Fi 上网的用户，我们还需要启动 iwd 服务，之后按照上一章节内容连接网络即可：

```bash
systemctl start iwd								# 启动iwd服务
```

验证网络连接：

```bash
ping -c 4 archlinux.org							# 验证网络连接状态
```

## 更新系统

在上一章节完成之后如果你放置了较长时间，那么建议你先更新一次系统再继续之后的安装：

```bash
pacman -Syyu									# 完整更新系统
```

## 添加普通用户

新安装的系统只有一个超级权限用户，即 root。使用 root 账户进行日常操作是非常不安全的，只有在进行系统维护时才进行使用。故我们创建一个普通用户，这里以 tutorial 举例：

```bash
useradd -m -G wheel -s /usr/bin/bash tutorial	# 创建tutorial用户
```

为 tutorial 用户设置密码：

```bash
passwd tutorial									# 设置tutorial用户密码
```

接下来我们为 tutorial 用户设置 sudo 权限，以便在日常使用中临时提权。

**==警告：该命令具有严重的安全隐患，日常严禁使用！！！==**

```bash
EDITOR=nano visudo /etc/sudoers.d/01_tutorial	# 设置tutorial用户权限
```

写入以下内容：

```bash
Defaults	pwfeedback							# 可选，设置密码输入回显
tutorial	ALL=(ALL:ALL)	ALL					# 设置合适的权限
```

部分教程会建议你修改`/etc/sudoers`文件，为`wheel`用户组设置权限，这有一定的安全隐患，我们不建议这么做，而是应该为每个用户单独设置权限。

## 设置 Swap 文件

你一定注意到我们之前设置了 swap 子卷，但并没有进行实际配置，在这一节我们进行配置。使用 swap 文件而不使用 swap 分区是因为 swap 文件更加灵活，且可以在需要时修改文件大小。

一般情况下我们建议将 swap 文件大小设置为略大于物理 RAM，这样可以支持系统休眠。

```bash
# 使用btrfs文档建议的方式创建swap文件，以16g大小举例
btrfs filesystem mkswapfile --size 16g --uuid clear /swap/swapfile
```

激活 swap 文件：

```bash
swapon /swap/swapfile							# 激活swap文件
```

编辑 fstab 文件，让系统在开机时自动加载 swap 文件。在`/etc/fstab`文件中追加以下内容：

```bash
/swap/swapfile	none	swap	defaults	0 0
```

## 安装网络管理软件

我们使用 NetworkManager 进行网络管理操作，使用如下命令安装并启用网络服务：

```bash
pacman -S networkmanager						# 安装NetworkManager软件
systemctl enable NetworkManager					# 启用服务
```

## 安装音频支持软件

Linux 下常用的音频支持程序有 Pulseaudio 和 Pipewire 两种，我们推荐使用 Pipewire，使用如下命令安装：

```bash
pacman -S pipewire pipewire-jack pipewire-pulse	# 安装音频支持软件
```

对于较新的电脑，我们还需要额外安装一个驱动：

```bash
pacman -S sof-firmware							# 安装音频硬件驱动
```

## 安装一些必要字体

```bash
# 谷歌Noto字体全家桶
pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
# 一个英文字体，一个编程用字体，一个终端用字体
pacman -S ttf-dejavu ttf-jetbrains-mono ttf-meslo-nerd
```

## 启用32位软件库

一些软件可能会需要32位支持库才能正常运行，为此我们启用32位支持库：

```bash
nano /etc/pacman.conf							# 编辑pacman配置文件
```

找到`[multilib]`一节，取消这两行的注释，保存文件并退出，然后刷新软件库：

```bash
pacman -Syy										# 刷新软件库
```

## 安装 Plasma 桌面环境

```bash
pacman -S plasma-meta							# 安装Plasma桌面环境
```

安装中会询问你一些问题，所有选项均保持默认即可。

然后我们安装一些必须软件，分别为：终端，文本编辑器，文件管理器及插件：

```bash
pacman -S konsole kate dolphin dolphin-plugins	# 一些必须软件
```

安装完成后我们需要启用显示管理服务，这样才能正确显示桌面：

```bash
systemctl enable sddm							# 启用SDDM显示管理服务
```

之后我们重启电脑，你就可以看到 SDDM 的登陆界面了，输入密码登陆到系统。

**从下一节开始，如无特殊说明，终端默认使用 nano 编辑器，GUI 环境默认使用 Kate 编辑器。**

## 设置系统为中文

打开系统设置软件（System Settings），找到 **Region & Language** 选项，修改其中的 **Language** 子菜单，添加“简体中文”，并将其移到顶端，然后**应用（Apply）**你的设置，重启电脑即可将系统设置为中文。

## 安装一些基础功能包

```bash
sudo pacman -S ntfs-3g dosfstools exfatprogs	# NTFS驱动，FAT32驱动，exFAT驱动
sudo pacman -S firefox-i18n-zh-cn				# FireFox（火狐）浏览器
sudo pacman -S ark								# 压缩文件管理器
sudo pacman -S p7zip unrar unarchiver lzop lrzip# 压缩文件管理器支持
sudo pacman -S packagekit packagekit-qt6		# 确保Discover（软件中心）可用
sudo pacman -S gwenview kamera					# 图片浏览器
sudo pacman -S kimageformats qt6-imageformats	# 图片格式支持
sudo pacman -S git git-lfs wget 				# Git和另一个终端下载程序
```

## 设置 DNS 服务器

通常来说，NetworkManager 可以自动处理 DNS 解析服务，但这存在一些问题。如果没有经过修改，那么你的 DNS 服务器将由 ISP（互联网服务提供商）提供，这存在安全风险，比如常见的 DNS 劫持、DNS 污染等等，ISP 也有可能应当局要求记录你的 IP 地址以及网络访问历史。

针对这些问题，我们建议的解决方案是使用可信的国际通用 DNS 服务器，比如 Google、CloudFlare 等等。但不幸的是，由于 GFW 的存在，你很可能无法使用这些服务器，我们只能退而求其次，使用中国大陆的相对安全的 DNS 服务器。在本文中我们暂时使用阿里云提供的 DNS 服务器，在下一章内容：魔法学院之后，你的 DNS 请求将全部经由代理服务器进行，这可以保证你的隐私安全。

下面针对阿里云 DNS 服务器进行 DNS 解析配置，同时启用 DOT，如果你可以正常访问 Google 等服务，那么请自行替换相关服务器地址。

温馨提示：在配置过程中可能会出现网络无法连接的提示，你可以安全的忽略它。

首先我们启用 Systemd 服务自带的 DNS 解析服务：

```bash
sudo systemctl enable --now systemd-resolved	# 启动systemd自带的DNS解析服务
```

配置 NetworkManager，使其使用 systemd 服务提供的 DNS 服务器，而不是自动处理：

```bash
sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf	# 配置服务
```

接下来我们设置 DNS 解析服务器，首先设置常规 DNS 服务器：

```bash
sudo mkdir -pv /etc/systemd/resolved.conf.d		#   创建配置存储文件夹
sudo nano /etc/systemd/resolved.conf.d/dns_servers.conf	# 配置DNS服务
```

写入以下内容：

```bash
[Resolve]
DNS=223.5.5.5 2400:3200::1 223.6.6.6 2400:3200:baba::1
Domains=~.
```

我们继续配置后备 DNS 服务器，这可以在首选 DNS 服务器解析失败时提供更多选择：

```bash
sudo nano /etc/systemd/resolved.conf.d/fallback_dns.conf	# 配置后备DNS服务
```

写入以下内容：

```bash
[Resolve]
FallbackDNS=127.0.0.1 ::1
```

最后我们配置 DOT（DNS Over TLS）服务，这可以加密传输 DNS 解析内容，保障你的隐私安全：

```bash
sudo nano /etc/systemd/resolved.conf.d/dns_over_tls.conf	# 配置DOT服务
```

写入以下内容：

```bash
[Resolve]
DNS=dns.alidns.com
DNSOverTLS=yes
```

我们还需要告知 NetworkManager 使用我们指定的 DNS 解析服务：

```bash
sudo nano /etc/NetworkManager/conf.d/dns.conf		# 配置NetworkManager
```

写入以下内容：

```bash
[main]
dns=systemd-resolved
[connection]
connection.mdns=2
```

至此，DNS 服务器配置就算完成了，重启你的电脑即可使用。

