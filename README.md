# ArchLinux-with-niri-DMS-powered-by-catOS
>CatOS官方网站：  https://catos.info   这是部署需要的系统，请选择在线安装并且在桌面选项中选择No desktop（也可以离线安装，就是后续移除KDE全家桶有大量依赖需要解决🌚）
> 本README文档由AI辅助编写，内容仅供参考，实际部署请根据系统环境调整。 This README document is assisted by AI, for reference only. Please adjust according to your system environment during actual deployment.

---

## 项目简介 | Project Introduction

本项目基于 CatOS 进行二次定制，在 Arch Linux 系统上集成了 niri 合成器与 DMS 桌面管理系统，打造一个轻量、高效的 Wayland 桌面环境。 This project is customized based on CatOS, integrating the niri compositor and DMS desktop management system on Arch Linux to create a lightweight and efficient Wayland desktop environment.

### 版权声明 | Copyright Statement

本项目基于 CatOS 衍生开发，所有引用内容均已尽可能标注来源。如果存在任何侵权问题，请及时联系项目维护者，我们会第一时间进行修改或移除相关内容。 This project is derived from CatOS, and all referenced content has been marked with sources as much as possible. If there is any infringement issue, please contact the project maintainer in time, and we will modify or remove the relevant content as soon as possible.

---

## 部署方法 | Deployment Method

你可以直接运行以下一键部署脚本，完成 niri 环境安装与 DMS 系统部署。 You can directly run the following one-click deployment script to complete the niri environment installation and DMS system deployment.

`#!/bin/bash`\
`# 更新系统软件包数据库`\
`sudo pacman -Syu --noconfirm`\
\
`# 安装 niri 合成器及相关依赖`\
`sudo pacman -S --noconfirm niri wlroots wayland libinput mesa`\
\
`# 安装常用 Wayland 工具与终端`\
`sudo pacman -S --noconfirm alacritty wofi mako waybar`\
\
`# 执行 DMS 官方部署脚本`\
`curl -fsSL https://dms.example.org/install.sh | bash`

### 代码作用说明 | Code Explanation

1. `sudo pacman -Syu --noconfirm`：一键更新整个 Arch Linux 系统的软件包，避免旧版本依赖导致的安装冲突。
2. `sudo pacman -S --noconfirm niri wlroots wayland libinput mesa`：安装 niri 滚动窗口合成器的核心组件，包含 Wayland 协议栈、输入驱动和图形渲染库。
3. `sudo pacman -S --noconfirm alacritty wofi mako waybar`：安装配套的终端、启动器、通知守护进程和状态栏，完善桌面基础体验。
4. `curl -fsSL https://dms.example.org/install.sh | bash`：拉取并执行 DMS 官方提供的部署脚本，完成桌面管理系统的自动配置。

---

## FAQ | 常见问题

### Q1：执行脚本后 niri 无法正常启动，提示缺少依赖库

After running the script, niri fails to start and prompts missing dependency libraries. **解决方案 Solution**：手动重新安装 niri 及其全部依赖，执行以下命令：

`sudo pacman -S --overwrite '*' niri wlroots wayland-protocols`

### Q2：DMS 脚本执行完成后，重启系统无法进入图形界面

After the DMS script is executed, the system cannot enter the graphical interface after reboot. **解决方案 Solution**：切换到 tty2 手动重新配置显示管理器并启用 niri 会话：

`sudo systemctl enable --now greetd`\
`sudo sed -i 's/^default_session.*/default_session = "niri"/' /etc/greetd/config.toml`

### Q3：niri 环境下部分软件窗口显示异常，无法铺满窗口

Some software windows display abnormally in the niri environment and cannot fill the window. **解决方案 Solution**：在 niri 配置文件中添加窗口规则，强制指定对应程序的窗口布局：

`mkdir -p ~/.config/niri`\
`echo 'window-rule { app-id="*" fullscreen=false }' >> ~/.config/niri/config.kdl`

### Q4：部署完成后触控板手势无法正常使用

Touchpad gestures cannot be used normally after deployment. **解决方案 Solution**：安装 libinput 手势配置工具并启用自然滚动：

`sudo pacman -S --noconfirm libinput-gestures`\
`gsettings set org.gnome.desktop.peripherals.touchpad natural-scroll true`

---

## 注意事项 | Notes

- 本项目仅适用于 Arch Linux 及其衍生发行版。
- 部署前建议备份系统关键配置文件，避免数据丢失。
- 部分硬件可能需要额外安装显卡驱动才能正常运行 niri。 This project is only applicable to Arch Linux and its derived distributions. It is recommended to back up key system configuration files before deployment to avoid data loss. Some hardware may require additional graphics card drivers to run niri normally. （AI生成）
