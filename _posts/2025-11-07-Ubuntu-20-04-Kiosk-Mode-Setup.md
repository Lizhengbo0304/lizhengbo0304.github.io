---
layout: post
title: "Ubuntu 20.04 Kiosk 模式完整配置方案"
date: 2025-11-07 18:00:00 +0800
categories: [Linux, Kiosk]
tags: [ubuntu, kiosk, openbox, lightdm]
---

# Ubuntu 20.04 Kiosk 模式完整配置方案

本文档提供了一个在 Ubuntu 20.04 上设置 Kiosk 模式的完整、详细的指南。该方案旨在创建一个双会话系统，允许在专用的 Kiosk 模式和标准桌面环境之间轻松切换，以方便管理和维护。

## 架构概览

- **主要会话**：一个轻量级的 Openbox 会话，自动启动一个全屏的单一应用程序（Kiosk 应用）。
- **备用会话**：一个完整的 GNOME 桌面环境，用于系统配置、维护和更新。

这种方法结合了 Kiosk 模式的安全性和稳定性，以及标准桌面的灵活性和易用性。

## 配置步骤

以下是详细的配置步骤。请按照顺序执行，以确保系统正确设置。

- [步骤 1：系统准备与更新](#步骤-1系统准备与更新)
- [步骤 2：安装 Openbox 和必要组件](#步骤-2安装-openbox-和必要组件)
- [步骤 3：创建 Kiosk 用户](#步骤-3创建-kiosk-用户)
- [步骤 4：配置 Openbox Kiosk 会话](#步骤-4配置-openbox-kiosk-会话)
- [步骤 5：创建 Openbox 会话入口](#步骤-5创建-openbox-会话入口)
- [步骤 6：配置 LightDM 自动登录](#步骤-6配置-lightdm-自动登录)
- [步骤 7：配置会话切换](#步骤-7配置会话切换)
- [步骤 8：最后的收尾工作](#步骤-8最后的收尾工作)
- [附录 A：通用故障排查指南](#附录-a通用故障排查指南)
- [附录 B：系统维护与安全](#附录-b系统维护与安全)

---

# 步骤 1：系统准备与更新

在开始配置 Kiosk 模式之前，请确保您的 Ubuntu 20.04 系统已完全更新，并安装了所有必要的工具。

## 1. 更新系统

首先，更新系统的软件包列表并升级已安装的软件包。

```bash
# 更新系统包列表
sudo apt update

# 升级已安装的包
sudo apt upgrade -y
```

## 2. 安装基础工具

安装一些在配置过程中可能会用到的基础工具。

```bash
# 安装必要的基础工具
sudo apt install -y vim git curl wget
```

## 可能遇到的问题与解决方案

### 问题 1.1：网络连接失败

**症状**：`apt update` 命令失败，并显示网络相关的错误。

**解决方案**：

1.  **检查网络状态**：

    ```bash
    ping -c 4 8.8.8.8
    ```

2.  **修复 DNS 问题**：如果 `ping` 失败，可能是 DNS 解析问题。您可以临时修改 DNS 设置：

    ```bash
    sudo nano /etc/resolv.conf
    ```

    在文件中添加以下行：

    ```
    nameserver 8.8.8.8
    ```

### 问题 1.2：软件源速度慢

**症状**：`apt update` 或 `apt upgrade` 过程非常缓慢。

**解决方案**：更换为国内的软件源镜像可以显著提高下载速度。以阿里云镜像为例：

1.  **备份原始源文件**：

    ```bash
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
    ```

2.  **编辑源文件**：

    ```bash
    sudo nano /etc/apt/sources.list
    ```

3.  **替换为阿里云镜像**：删除原有内容，并添加以下行（适用于 Ubuntu 20.04, `focal`）：

    ```
    deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
    ```

4.  **更新软件源**：

    ```bash
    sudo apt update
    ```

---

# 步骤 2：安装 Openbox 和必要组件

为了构建轻量级的 Kiosk 环境，我们需要安装 Openbox 窗口管理器、LightDM 显示管理器以及一些辅助工具。

## 1. 安装核心组件

```bash
# 安装 Openbox 窗口管理器
sudo apt install -y openbox obconf

# 安装 LightDM 显示管理器
sudo apt install -y lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings
```

## 2. 安装辅助工具

这些工具将帮助我们管理桌面环境、鼠标指针等。

```bash
sudo apt install -y \
    xserver-xorg \
    xinit \
    x11-xserver-utils \
    nitrogen \
    tint2 \
    unclutter \
    xdotool
```

| 工具 | 描述 |
|---|---|
| `xserver-xorg` | X Window 系统显示服务器 |
| `xinit` | 启动 X Window 系统的工具 |
| `x11-xserver-utils` | X server 的实用程序集 |
| `nitrogen` | 用于设置和恢复壁纸 |
| `tint2` | 一个轻量级的任务栏（可选） |
| `unclutter` | 在不活动时隐藏鼠标指针 |
| `xdotool` | 用于模拟键盘输入和鼠标活动的命令行工具 |

## 可能遇到的问题与解决方案

### 问题 2.1：LightDM 与 GDM3 冲突

**症状**：Ubuntu 20.04 默认使用 GDM3 作为显示管理器。在安装 LightDM 时，系统应该会提示您选择一个默认的显示管理器。

**解决方案**：

-   如果在安装过程中没有看到提示，可以手动运行以下命令来配置：

    ```bash
    sudo dpkg-reconfigure lightdm
    ```

-   在弹出的对话框中，使用方向键选择 `lightdm`，然后按回车键确认。

### 问题 2.2：图形界面无法启动

**症状**：重启后，系统无法进入图形登录界面。

**解决方案**：

1.  **检查 X server 状态**：

    ```bash
    systemctl status lightdm
    ```

2.  **查看 Xorg 日志**：检查是否有错误（以 `(EE)` 开头的行）：

    ```bash
    cat /var/log/Xorg.0.log | grep EE
    ```

3.  **重新安装显卡驱动**：如果日志表明存在显卡驱动问题，可以尝试自动重新安装驱动：

    ```bash
    sudo ubuntu-drivers autoinstall
    sudo reboot
    ```

---

# 步骤 3：创建 Kiosk 用户

为了安全起见，我们将创建一个专用的、低权限的 `kiosk` 用户来运行 Kiosk 会话。

## 1. 创建用户

```bash
# 创建 kiosk 用户
sudo adduser kiosk
```

在执行此命令时，系统会提示您为新用户设置密码。请设置一个强密码。对于其他信息（如全名、房间号等），您可以直接按回车键跳过。

## 2. 配置用户权限

为了限制 `kiosk` 用户的能力，我们需要进行一些权限调整。

```bash
# 将 kiosk 用户添加到必要的组，以确保其可以访问视频和音频设备
sudo usermod -aG video,audio kiosk

# 为安全起见，禁止 kiosk 用户使用 sudo
# 此命令会尝试从 sudo 组中移除 kiosk 用户，如果用户不在组中，则静默失败
sudo deluser kiosk sudo 2>/dev/null || true
```

## 可能遇到的问题与解决方案

### 问题 3.1：用户已存在

**症状**：在运行 `adduser kiosk` 时，系统提示用户已存在。

**解决方案**：

如果需要重新创建用户，可以先删除已存在的用户及其主目录：

```bash
# 删除已存在的用户和其主目录
sudo userdel -r kiosk

# 重新创建用户
sudo adduser kiosk
```

### 问题 3.2：Home 目录权限问题

**症状**：`kiosk` 用户无法登录，或登录后立即被踢出。

**解决方案**：

确保 `kiosk` 用户的主目录归其所有，并且具有正确的权限。

```bash
# 确保正确的所有权
sudo chown -R kiosk:kiosk /home/kiosk

# 设置正确的权限
sudo chmod 755 /home/kiosk
```

---

# 步骤 4：配置 Openbox Kiosk 会话

现在，我们将为 `kiosk` 用户配置 Openbox 环境，使其在登录时自动启动您的应用程序。

## 1. 创建 Openbox 配置目录

首先，切换到 `kiosk` 用户，并为其创建 Openbox 的配置目录。

```bash
# 切换到 kiosk 用户
sudo su - kiosk

# 创建配置目录
mkdir -p ~/.config/openbox
```

## 2. 创建 Openbox 自动启动脚本

`autostart` 脚本会在 Openbox 会话启动时自动执行。我们将在这里定义 Kiosk 环境的行为。

创建一个新文件 `~/.config/openbox/autostart` 并添加以下内容：

```bash
#!/bin/bash

# 禁用屏幕保护和电源管理
xset s off
xset -dpms
xset s noblank

# 在 3 秒无活动后隐藏鼠标指针
unclutter -idle 3 &

# 设置壁纸（可选）
# nitrogen --restore &

# 等待窗口管理器完全启动
sleep 2

# 启动您的 Kiosk 应用（此处以 Firefox 为例）
# 这是一个无限循环，如果应用崩溃，它会在 2 秒后自动重启
while true; do
    # 将下面的命令替换为您的实际应用
    firefox --kiosk https://example.com
    sleep 2
done
```

然后，赋予该脚本执行权限：

```bash
chmod +x ~/.config/openbox/autostart
```

## 3. 创建 Openbox 配置文件

`rc.xml` 文件用于配置 Openbox 的行为，例如快捷键和应用程序设置。

创建一个新文件 `~/.config/openbox/rc.xml` 并添加以下内容。此配置将：

-   定义一个快捷键 `Ctrl+Alt+Shift+G` 以切换回登录界面。
-   定义一个快捷键 `Ctrl+Alt+Shift+R` 以重启 Kiosk 应用。
-   禁用 `Alt+F4` 关闭窗口的功能。
-   强制所有应用程序全屏运行。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<openbox_config xmlns="http://openbox.org/3.4/rc">
  <keyboard>
    <!-- Ctrl+Alt+Shift+G: 切换到登录界面 -->
    <keybind key="C-A-S-g">
      <action name="Execute">
        <command>dm-tool switch-to-greeter</command>
      </action>
    </keybind>

    <!-- Ctrl+Alt+Shift+R: 重启 Kiosk 应用 -->
    <keybind key="C-A-S-r">
      <action name="Execute">
        <!-- 此处应与 autostart 脚本中的应用匹配 -->
        <command>killall firefox</command>
      </action>
    </keybind>

    <!-- 禁用 Alt+F4 关闭窗口 -->
    <keybind key="A-F4">
      <action name="None"/>
    </keybind>
  </keyboard>

  <applications>
    <!-- 强制所有应用全屏，无边框 -->
    <application name="*">
      <fullscreen>yes</fullscreen>
      <maximized>yes</maximized>
      <decor>no</decor>
    </application>
  </applications>
</openbox_config>
```

完成以上步骤后，退出 `kiosk` 用户，返回到您的管理员账户：

```bash
exit
```

## 可能遇到的问题与解决方案

### 问题 4.1：`autostart` 脚本不执行

**症状**：登录后，Kiosk 应用没有启动。

**解决方案**：

1.  **检查脚本语法**：

    ```bash
    bash -n /home/kiosk/.config/openbox/autostart
    ```

2.  **检查执行权限**：

    ```bash
    ls -l /home/kiosk/.config/openbox/autostart
    ```

3.  **查看 Openbox 日志**：

    ```bash
    tail -f /home/kiosk/.local/share/xsessions-errors
    ```

### 问题 4.2：应用无法启动

**症状**：`autostart` 脚本已执行，但应用未能成功启动。

**解决方案**：

在 `autostart` 脚本的开头添加日志记录，以捕获任何错误输出：

```bash
exec > /tmp/kiosk-startup.log 2>&1
set -x
```

然后，登录 Kiosk 会话，并检查日志文件：

```bash
cat /tmp/kiosk-startup.log
```

### 问题 4.3：快捷键不工作

**症状**：在 `rc.xml` 中定义的快捷键无效。

**解决方案**：

Openbox 的 XML 配置文件对格式要求非常严格。任何语法错误都可能导致整个文件加载失败。

-   **使用 `openbox` 命令验证和重新加载配置**：

    ```bash
    openbox --reconfigure
    ```

-   **使用 `obconf` 图形工具**：`obconf` 提供了一个图形界面来编辑 `rc.xml`，这可以帮助避免语法错误。

---

# 步骤 5：创建 Openbox 会话入口

为了让 LightDM 显示管理器识别我们的 Openbox Kiosk 会话，我们需要创建一个 `.desktop` 会话文件。

## 1. 创建会话文件

使用文本编辑器创建一个新文件 `/usr/share/xsessions/openbox-kiosk.desktop`：

```bash
sudo nano /usr/share/xsessions/openbox-kiosk.desktop
```

在该文件中添加以下内容：

```ini
[Desktop Entry]
Name=Openbox Kiosk
Comment=Lightweight kiosk session
Exec=openbox-session
Type=Application
DesktopNames=OPENBOX
```

这会告诉 LightDM，有一个名为 `Openbox Kiosk` 的会话可用，它通过执行 `openbox-session` 来启动。

## 2. 设置文件权限

确保该文件的权限设置正确，以便 LightDM 可以读取它。

```bash
sudo chmod 644 /usr/share/xsessions/openbox-kiosk.desktop
```

## 可能遇到的问题与解决方案

### 问题 5.1：会话不出现在登录界面

**症状**：在 LightDM 登录界面的会话选择器中，没有看到 `Openbox Kiosk` 选项。

**解决方案**：

1.  **验证 `.desktop` 文件格式**：

    ```bash
    desktop-file-validate /usr/share/xsessions/openbox-kiosk.desktop
    ```

2.  **检查 LightDM 是否扫描了会话**：

    ```bash
    ls -la /usr/share/xsessions/
    ```

3.  **重启 LightDM**：

    ```bash
    sudo systemctl restart lightdm
    ```

---

# 步骤 6：配置 LightDM 自动登录

现在，我们将配置 LightDM，使其在系统启动时自动登录到我们创建的 `kiosk` 用户和 `Openbox Kiosk` 会话。

## 1. 编辑 LightDM 配置

打开 LightDM 的主配置文件：

```bash
sudo nano /etc/lightdm/lightdm.conf
```

## 2. 添加自动登录配置

在该文件的 `[Seat:*]` 部分，添加或修改以下行：

```ini
[Seat:*]
# 启用自动登录，并指定用户
autologin-user=kiosk
autologin-user-timeout=0

# 设置默认会话为我们创建的 Openbox Kiosk 会话
autologin-session=openbox-kiosk

# 允许手动登录（这对于切换到管理员账户至关重要）
greeter-show-manual-login=true
greeter-hide-users=false
```

## 3. 将 Kiosk 用户添加到 `autologin` 组

为了使自动登录生效，`kiosk` 用户必须是 `autologin` 组的成员。

```bash
# 创建 autologin 组（如果它不存在）
sudo groupadd -r autologin

# 将 kiosk 用户添加到 autologin 组
sudo usermod -aG autologin kiosk
```

## 可能遇到的问题与解决方案

### 问题 6.1：自动登录不生效

**症状**：系统启动后，仍然停留在 LightDM 登录界面，没有自动登录。

**解决方案**：

1.  **检查 LightDM 配置语法**：

    ```bash
    sudo lightdm --test-mode --debug
    ```

2.  **查看 LightDM 日志**：

    ```bash
    sudo journalctl -u lightdm -n 50
    ```

3.  **验证用户是否在 `autologin` 组中**：

    ```bash
    groups kiosk
    ```

### 问题 6.2：登录循环

**症状**：系统尝试自动登录，但立即返回到登录界面。

**解决方案**：

这通常是由于会话启动失败导致的。最常见的原因是 `kiosk` 用户的 shell 脚本（如 `.profile` 或 `.bashrc`）或 Openbox 的 `autostart` 脚本中存在错误。

1.  **检查 `.xsession-errors`**：这是诊断会话启动问题的关键文件。

    ```bash
    cat /home/kiosk/.xsession-errors
    ```

2.  **临时禁用自动登录进行诊断**：

    -   编辑 `/etc/lightdm/lightdm.conf` 并注释掉 `autologin-user` 行。
    -   重启 LightDM (`sudo systemctl restart lightdm`)。
    -   在登录界面，手动选择 `kiosk` 用户和 `Openbox Kiosk` 会话进行登录。这通常会显示更直接的错误信息。

---

# 步骤 7：配置会话切换

一个关键的需求是能够从 Kiosk 模式轻松切换到管理会话（GNOME）。我们将通过一个快捷键来实现这一点。

## 方案：通过 `dm-tool` 切换到登录界面

`dm-tool` 是一个与显示管理器（如 LightDM）交互的命令行工具。我们可以使用它来切换回登录界面（greeter）。

在 [步骤 4：配置 Openbox Kiosk 会话](#步骤-4-配置-openbox-kiosk-会话) 中，我们已经在 `/home/kiosk/.config/openbox/rc.xml` 文件中配置了以下快捷键：

```xml
<!-- Ctrl+Alt+Shift+G: 切换到登录界面 -->
<keybind key="C-A-S-g">
  <action name="Execute">
    <command>dm-tool switch-to-greeter</command>
  </action>
</keybind>
```

### 使用方法

1.  在 Kiosk 模式下，按 `Ctrl+Alt+Shift+G`。
2.  系统将返回到 LightDM 登录界面。
3.  在登录界面，输入您的管理员用户名和密码。
4.  在会话选择器中，选择 `Ubuntu` 或 `GNOME`。
5.  您将正常进入 GNOME 桌面，可以进行系统管理和维护。

## 可能遇到的问题与解决方案

### 问题 7.1：`dm-tool` 命令无效

**症状**：按下快捷键后没有任何反应。

**解决方案**：

1.  **检查 `dm-tool` 是否存在**：

    ```bash
    which dm-tool
    ```

2.  **安装 LightDM 工具包**：如果 `dm-tool` 不存在，可能是因为没有安装 `lightdm` 包。

    ```bash
    sudo apt install -y lightdm
    ```

### 问题 7.2：快捷键冲突

**症状**：快捷键被 Kiosk 应用程序占用，无法触发 Openbox 的操作。

**解决方案**：

选择一个更独特、更不容易冲突的组合键。例如，在 `/home/kiosk/.config/openbox/rc.xml` 中，您可以将其更改为：

```xml
<!-- 使用 Ctrl+Alt+Shift+F12 -->
<keybind key="C-A-S-F12">
  <action name="Execute">
    <command>dm-tool switch-to-greeter</command>
  </action>
</keybind>
```

然后，使用 `openbox --reconfigure` 命令重新加载配置。

---

# 步骤 8：最后的收尾工作

在完成所有配置后，我们需要进行一些最后的清理和验证，以确保 Kiosk 系统稳定可靠。

## 1. 重启系统

这是验证所有配置是否正确的最终测试。重启后，系统应该会自动登录到 `kiosk` 用户，并全屏启动您指定的应用程序。

```bash
sudo reboot
```

## 2. 验证 Kiosk 模式

-   **自动登录**：系统是否自动登录到 `kiosk` 用户？
-   **应用程序启动**：Kiosk 应用程序（如 Firefox）是否自动全屏启动？
-   **禁用快捷键**：尝试使用 `Alt+F4`、`Alt+Tab` 等，确认它们已被禁用。
-   **会话切换**：按下 `Ctrl+Alt+Shift+G`，系统是否返回到登录界面？

## 3. 清理安装包缓存

为了释放磁盘空间，可以清理 `apt` 的缓存。

```bash
sudo apt clean
```

## 4. 备份关键配置文件

为了方便恢复或迁移，建议备份以下关键配置文件：

-   `/etc/lightdm/lightdm.conf`
-   `/usr/share/xsessions/openbox-kiosk.desktop`
-   `/home/kiosk/.config/openbox/rc.xml`
-   `/home/kiosk/.config/openbox/autostart`

您可以将它们复制到一个安全的位置，例如您的管理员主目录或外部存储设备。

```bash
mkdir ~/kiosk_backup
cp /etc/lightdm/lightdm.conf ~/kiosk_backup/
cp /usr/share/xsessions/openbox-kiosk.desktop ~/kiosk_backup/
cp /home/kiosk/.config/openbox/rc.xml ~/kiosk_backup/
cp /home/kiosk/.config/openbox/autostart ~/kiosk_backup/
```

## 恭喜！

您已成功配置了一个功能齐全、安全可靠的 Ubuntu Kiosk 系统！

---

# 附录 A：通用故障排查指南

本指南汇总了在配置和维护 Kiosk 系统时可能遇到的常见问题及其解决方案。

## 1. 图形界面问题

### 问题：黑屏或无法进入图形界面

**可能原因**：

-   显卡驱动问题。
-   LightDM 服务启动失败。
-   X Server 配置错误。

**排查步骤**：

1.  **切换到 TTY**：按 `Ctrl+Alt+F3` 进入命令行终端。
2.  **检查 LightDM 状态**：

    ```bash
    sudo systemctl status lightdm
    ```

3.  **查看 X Server 日志**：

    ```bash
    cat /var/log/Xorg.0.log | grep EE
    ```

4.  **重新安装显卡驱动**：

    ```bash
    sudo ubuntu-drivers autoinstall
    ```

## 2. 会话与登录问题

### 问题：登录循环或无法启动会话

**可能原因**：

-   `.xsession-errors` 中有错误。
-   `autostart` 脚本执行失败。
-   用户主目录权限不正确。

**排查步骤**：

1.  **检查 `.xsession-errors`**：这是诊断会话问题的首要文件。

    ```bash
    cat /home/kiosk/.xsession-errors
    ```

2.  **检查 `autostart` 脚本权限**：确保脚本有执行权限。

    ```bash
    ls -l /home/kiosk/.config/openbox/autostart
    ```

3.  **检查主目录所有权**：

    ```bash
    ls -ld /home/kiosk
    ```

    确保所有者是 `kiosk:kiosk`。

## 3. 应用程序问题

### 问题：Kiosk 应用程序无法启动或崩溃

**可能原因**：

-   应用程序缺少依赖。
-   应用程序配置错误。
-   DBus 会话问题。

**排查步骤**：

1.  **手动运行应用程序**：

    -   切换到 TTY，以 `kiosk` 用户身份登录。
    -   尝试手动启动应用程序，并观察终端输出的错误信息。

    ```bash
    sudo -u kiosk dbus-launch firefox --kiosk
    ```

2.  **检查 `journalctl`**：

    ```bash
    journalctl -f
    ```

    在另一个终端尝试启动应用程序，观察实时日志输出。

## 4. 网络问题

### 问题：Kiosk 模式下无法访问网络

**可能原因**：

-   `NetworkManager` 未运行。
-   网络配置问题（例如，静态 IP 配置错误）。

**排查步骤**：

1.  **检查 `NetworkManager` 状态**：

    ```bash
    sudo systemctl status NetworkManager
    ```

2.  **使用 `nmcli` 检查连接**：

    ```bash
    nmcli device status
    nmcli connection show
    ```

3.  **检查 DNS 配置**：

    ```bash
    cat /etc/resolv.conf
    ```

---

# 附录 B：系统维护与安全

## 1. 系统更新

定期更新系统是保持安全的关键。但是，在 Kiosk 环境中，自动更新可能会中断服务。建议采用手动、计划性的更新策略。

**更新流程**：

1.  切换到管理员会话。
2.  运行系统更新：

    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

3.  重启系统以应用内核等关键更新。

## 2. 禁用不必要的服务

为了减少攻击面和系统开销，可以禁用不需要的服务。

**示例：禁用蓝牙和打印服务**

```bash
sudo systemctl disable bluetooth.service
sudo systemctl disable cups.service
```

## 3. 防火墙配置 (UFW)

使用 `ufw` (Uncomplicated Firewall) 来限制网络访问。

**示例：只允许 HTTP/HTTPS 出站**

```bash
sudo ufw default deny incoming
sudo ufw default deny outgoing
sudo ufw allow out to any port 80 proto tcp
sudo ufw allow out to any port 443 proto tcp
sudo ufw enable
```

**注意**：在启用防火墙之前，请确保您了解 Kiosk 应用程序需要访问的所有端口。

## 4. 远程管理 (SSH)

为了方便远程维护，可以安装和配置 SSH 服务器。

1.  **安装 SSH 服务器**：

    ```bash
    sudo apt install -y openssh-server
    ```

2.  **安全配置**：编辑 `/etc/ssh/sshd_config`，进行安全加固。

    -   **禁用 root 登录**：`PermitRootLogin no`
    -   **只允许特定用户**：`AllowUsers your_admin_user`

3.  **重启 SSH 服务**：

    ```bash
    sudo systemctl restart sshd
    ```