# 显卡驱动与硬件加速

因笔者目前只有 Intel + NVIDIA 显卡组合的电脑，故 AMD 相关教程暂缺，还请谅解，当然也欢迎你提供相关教程进行完善。

## Intel 核心显卡

Intel 核心显卡开箱即用，只需要安装如下驱动即可：

```bash
sudo pacman -S mesa vulkan-intel				# Intel显卡驱动，Vulkan支持
```

因为 Intel 显卡驱动开源，故 Linux 内核对其支持完善，你几乎不需要进行任何任何配置。

## NVIDIA 独立显卡

首先我们安装显卡驱动与相关软件：

```bash
sudo pacman -S nvidia-dkms nvidia-settings		# NVIDIA驱动，驱动设置界面
sudo pacman -S nvidia-prime opencl-nvidia		# 动态切换，OpenCL支持
```

我们需要设置内核级显示模式用以支持 Wayland 合成器的正常运行，同时为了解决独显直连时开机黑屏的问题，我们一同设置内核模块的加载：

编辑`/etc/kernel/cmdline`，添加如下内容：

```bash
nvidia_drm.modeset=1 nvidia_drm.fbdev=1
```

编辑`/etc/mkinitcpio.conf`，在`MODULES`中添加如下内容（i915是 Intel 核显所需，AMD 核显无需添加）：

**注意：该内容顺序不得改变，否则会引发未知问题！**

```bash
i915 nvidia nvidia_modeset nvidia_drm nvidia_uvm
```

我们还需要添加一个 pacman 钩子，防止你在更新 NVIDIA 驱动后忘记更新 initramfs：

```bash
sudo nano /etc/pacman.d/hooks/nvidia.hook		# 创建并编辑nvidia.hook
```

写入以下内容：

```bash
[Trigger]
Operation=Install
Operation=Upgrade
Operation=Remove
Type=Package
Target=nvidia-dkms
Target=linux-zen

[Action]
Description=Updating NVIDIA module in initcpio
Depends=mkinitcpio
When=PostTransaction
NeedsTargets
Exec=/usr/bin/sh -c 'while read -r trg; do case $trg in linux*) exit 0; esac; done; /usr/bin/mkinitcpio -P'
```

最后我们需要更新 initramfs 以使我们的修改生效：

```bash
sudo mkinitcpio -P								# 重建initramfs
```

## 硬件解码与 GPU 加速

