# Windows 用户使用 PowerShell SSH 登录 VPS 指南

本指南将介绍如何使用 Windows PowerShell 通过 SSH 连接到您的 VPS 服务器。

## 准备工作

在开始之前，请确保您拥有以下信息：

*   VPS 的 IP 地址或域名
*   VPS 的 SSH 用户名
*   VPS 的 SSH 密码（或已配置的 SSH 密钥）

## 1. 检查 OpenSSH 客户端是否已安装

首先，我们需要检查您的 Windows 系统是否已安装 OpenSSH 客户端。在 PowerShell 中运行以下命令。**请注意，您需要以管理员身份运行 PowerShell 才能执行此命令。** 您可以右键单击 PowerShell 图标，然后选择“以管理员身份运行”。

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH.Client*'
```

这条命令会列出所有名称中包含 `OpenSSH.Client` 的 Windows 功能。如果输出中 `State` 显示为 `NotPresent`，则需要安装 OpenSSH 客户端。

### 1.1 安装 OpenSSH 客户端 （如果未安装）

如果 OpenSSH 客户端未安装，请使用以下命令进行安装。**请注意，您需要以管理员身份运行 PowerShell 才能执行此命令。**

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client*
```
这条命令会自动下载并安装 OpenSSH 客户端。安装完成后，建议重启 PowerShell 窗口以使更改生效。

## 2. 生成 SSH 密钥（可选，但强烈推荐）

使用 SSH 密钥进行身份验证比使用密码更安全。如果您尚未拥有 SSH 密钥，可以使用以下命令生成：

```powershell
ssh-keygen -t ed25519
```

这条命令会使用 ed25519 算法生成一对新的 SSH 密钥。这是目前推荐使用的加密算法。

执行此命令后，系统会提示您保存密钥的位置。默认情况下，密钥会保存在 `C:\Users\<用户名>\.ssh` 目录下。您可以直接按回车键接受默认位置。

然后，系统会提示您输入一个密码短语（passphrase）。这是一个额外的安全层，用于保护您的私钥。您可以选择设置或不设置密码短语。如果您设置了密码短语，每次使用私钥时都需要输入该短语。

生成密钥后，您会在 `.ssh` 目录下看到两个文件：

* `id_ed25519`：这是您的私钥，**请务必妥善保管，不要泄露给任何人**。
* `id_ed25519.pub`：这是您的公钥，您需要在创建 VPS 的时候上传或者将其复制到您的 VPS 上。

## 3. 使用 SSH 连接到 VPS

完成以上步骤后，您可以使用以下命令连接到您的 VPS：

```powershell
ssh user@192.168.1.1
```

**将 `user` 替换为您的 VPS 用户名，`192.168.1.1` 替换为您的 VPS IP 地址或域名。**

如果您使用了 SSH 密钥进行身份验证，并且没有设置密码短语，则可以直接连接到 VPS。如果设置了密码短语，则需要输入该短语。

如果您使用密码进行身份验证，系统会提示您输入密码。
