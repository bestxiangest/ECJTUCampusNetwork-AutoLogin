# 华东交通大学校园网自动登录脚本(ECJTU Campus Network Auto Login Script)

## 简介

本项目包含一个 Python 脚本，用于自动登录华东交通大学（ECJTU）的校园网 Eportal 认证系统。当你连接到校园网（有线或 Wi-Fi）但尚未认证时，运行此脚本可以自动完成登录过程，省去手动打开浏览器输入账号密码的步骤。

脚本基于对特定登录过程的网络日志分析编写，模拟了浏览器登录时的 POST 请求。

**注意:** 此脚本目前仅自动检测 IP 地址，MAC 地址使用固定的 `00-00-00-00-00-00`。如果校园网系统要求或校验真实的 MAC 地址，此脚本可能需要修改。但实测本校的MAC校验较弱，目前可以使用。

## 功能特性

* 自动检测当前是否需要登录校园网。
* 自动获取当前的本地 IP 地址用于登录请求。
* 使用用户配置的账号、密码和运营商信息发送登录 POST 请求。
* 验证登录是否成功。

## 环境要求

* 操作系统：Windows 11 (理论上也兼容 Windows 10)
* Python：需要安装 Python 3.x 环境。
    * 下载地址: [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/)
    * **重要**: 安装时请务必勾选 "Add Python to PATH" 选项。
* Python 库：需要安装 `requests` 库。

## 安装与配置

1.  **下载脚本**:
    * 通过 Git 克隆本仓库：
        ```bash
        git clone https://github.com/bestxiangest/ECJTU-Campus-Network-Auto-Login-Script
        cd ECJTU-Campus-Network-Auto-Login-Script
        ```
    * 或者直接下载仓库中的 `campus_login.py` 文件。

2.  **安装依赖库**:
    * 打开命令提示符 (CMD) 或 PowerShell。
    * 运行以下命令安装 `requests` 库：
        ```bash
        pip install requests
        ```

3.  **配置用户信息**:
    * 使用文本编辑器（如记事本、VS Code、Notepad++ 等）打开 `campus_login.py` 文件。
    * 找到以下几行代码：
        ```python
        # --- 用户凭据 ---
        USERNAME = "2023061004000xxx"
        PASSWORD = "xxxxx"
        OPERATOR_SUFFIX = "@unicom" # 中国联通对应的后缀
        ```
    * **将 `USERNAME` 的值修改为你自己的学号或账号。**
    * **将 `PASSWORD` 的值修改为你自己的密码。**
    * **确认 `OPERATOR_SUFFIX`**:
        * 如果你是中国联通用户，保持 `@unicom` 不变。
        * 如果你是中国电信用户，修改为 `@telecom`。
        * 如果你是中国移动用户，修改为 `@cmcc`。
        * 如果你的账号登录时不需要选择运营商或没有后缀，将其设置为空字符串 `""`。
    * **保存文件。**

## 使用方法 (手动运行)

1.  确保你已连接到华东交通大学的校园网（有线或无线）。
2.  打开命令提示符 (CMD) 或 PowerShell。
3.  使用 `cd` 命令切换到 `campus_login.py` 文件所在的目录。例如，如果你把它保存在桌面上：
    ```bash
    cd %USERPROFILE%\Desktop
    ```
4.  运行脚本：
    ```bash
    python campus_login.py
    ```
5.  脚本会自动检测网络状态。如果需要登录，它会尝试自动登录并输出相应的信息。如果已经联网，它会提示无需登录。

<img width="1094" height="392" alt="image" src="https://github.com/user-attachments/assets/8a230621-b289-4183-9418-b4b641c3c40a" />


## 设置开机/登录时自动运行 (Windows 11)

### 方法一：使用启动文件夹（推荐）
按 Win + R，输入 shell:startup，回车
这会打开当前用户的启动文件夹
在该文件夹中创建一个批处理文件（如 campus_login.bat）
编辑批处理文件内容：
```bat
@echo off
python "D:\PythonFile\campus_login\campus_login.py"
```
保存文件

### 方法二：使用任务计划程序
按 Win + R，输入 taskschd.msc，回车
点击右侧"创建基本任务"
输入名称，如"Campus Login"
触发器选择"计算机启动时"
操作选择"启动程序"
在"程序或脚本"中填写：

程序/脚本：python

参数："D:\PythonFile\campus_login\campus_login.py"

完成创建

### 方法三：创建快捷方式到启动文件夹
右键点击桌面，新建快捷方式

位置输入：

```
python "D:\PythonFile\campus_login\campus_login.py"
```
命名快捷方式

将快捷方式复制到启动文件夹（通过 shell:startup 打开）

### 方法四：使用Pythonw避免命令行窗口
如果不想显示命令行窗口，可以使用 pythonw.exe：

```bat
@echo off
pythonw "D:\PythonFile\campus_login\campus_login.py"
```
验证方法
重启计算机后，可以通过以下方式验证程序是否正常运行：

检查任务管理器中的Python进程

查看程序应有的效果

**推荐使用方法一，因为它最简单且容易管理。如果程序需要管理员权限，请以管理员身份运行批处理文件。**
现在，每次你登录 Windows 账户后，系统应该会自动运行这个 Python 脚本来尝试登录校园网。

## 重要提示

* **密码安全**: 脚本中以明文形式存储了你的密码。请确保 `campus_login.py` 文件的安全，不要分享给他人。考虑使用更安全的方式管理密码（如环境变量、配置文件权限控制等）是更佳实践，但这会增加配置的复杂性。
* **学校政策**: 使用自动化脚本登录可能违反学校的网络使用规定。请自行确认并承担相应风险。
* **网络变更**: 如果学校的 Eportal 登录系统更新（如 URL、参数、认证方式改变），此脚本可能失效，需要重新分析并修改代码。
* **错误处理**: 脚本包含基本的网络错误处理，但不能覆盖所有情况。如果登录失败，请查看脚本的输出信息。

## 故障排除

* **登录失败**:
    * 检查 `campus_login.py` 文件中的 `USERNAME`, `PASSWORD`, `OPERATOR_SUFFIX` 是否完全正确。
    * 确认你的 IP 地址是否在脚本尝试登录时与登录请求中的 IP 匹配（脚本会自动检测）。
    * 校园网登录系统可能已变更，脚本需要更新。
    * 尝试手动登录一次，看是否能成功，或者登录页面是否有变化。
* **自动启动不工作**:
    * 检查任务计划程序中的 Python 解释器路径、脚本路径、起始目录是否都正确无误。
    * 尝试在任务属性中勾选“使用最高权限运行”。
    * 查看任务计划程序中的“上次运行结果”，看是否有错误代码。

## 贡献

欢迎提出 Issue 或 Pull Request 来改进此脚本。

## 许可证

MIT License。
