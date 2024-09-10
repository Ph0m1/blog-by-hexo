---
title: 关于plasma 6 更新后出现的问题
date: 2024-03-07
updated: 2024-03-07
categories: linux
tags: linux
---

# 无法使用archinstall安装KDE桌面系统

   随着 plasma 6 的推送，原先的 plasma-wayland-session 包被替换为 plasma-workspace， 但是 archinstall 中有关KDE桌面环境的安装依然选用的是 旧的包，导致安装 archlinux 时异常中断。

   ## 解决办法
      
      取消使用 archinstall 中的桌面环境部分，其他的照常选择，待安装完毕后，手动安装 sddm、sddm-kcm、xorg 等包
      ```sh
      pacman -S sddm sddm-kcm xorg plasma-meta plasma-workspace kde-applications-meta 

      pacman -S <显卡驱动>

      systemctl enable sddm

      reboot
      ```
