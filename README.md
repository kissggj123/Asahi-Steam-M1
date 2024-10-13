# Asahi-Steam-M1
在MacBook M1上通过Fedora Asahi Remix游玩Steam游戏


```markdown
# Asahi Linux 安装指南

## 1. 执行安装命令

首先，执行以下命令开始安装：

```bash
curl https://alx.sh | sh
```

> **注意**: 需要开启代理，路由器走规则或者给终端设置代理。可以通过以下命令设置代理：

```bash
export https_proxy=http://127.0.0.1:7897 http_proxy=http://127.0.0.1:7897 all_proxy=socks5://127.0.0.1:7897
```

## 2. 配置磁盘空间分配

根据提示一路回车，直到需要分配空间时，输入的值按照以下公式计算：

```text
New size（后续给 Linux 的空间） = Total size（总空间） - [Minimum new size（给 macOS 保留的空间，需要大于此值或不调整保持默认） + Available space（从当前空闲空间中拆分，或选择默认 min 选项）]
```
<img width="468" alt="image" src="https://github.com/user-attachments/assets/bd0db942-e8ca-49d2-94f8-2967907d66f1">

> **说明**: `free up` 的值就是通过如上公式你分配给 Linux 的空间。

## 3. 系统选择与下载

<img width="369" alt="image" src="https://github.com/user-attachments/assets/0dcd1576-c07c-487c-8bde-dc510ffd5d62">

在系统选择步骤中，直接选择 `1`。然后等待系统下载完成。

## 4. 启动到 Asahi Linux 的前序步骤
<img width="468" alt="image" src="https://github.com/user-attachments/assets/f887ea5b-7f08-4a63-96c2-747cff9f70b6">

当出现上图提示安装过程结束 你需要在回车后等待系统完全关机后，按住电源键，直到出现启动选项。选择 Fedora Linux（或者你设置的名称）。进入恢复界面后，按照提示输入你的 macOS 本地账户的账号和密码（需要输入两次）。输入成功后，系统会自动重启到 Asahi Linux。

## 5. 后续操作（在 Asahi Linux 终端进行）

以下操作需要在 Asahi Linux 系统的终端中进行：

1. 修改 root 用户的密码：

    ```bash
    sudo passwd root
    ```

2. 切换到 root 用户：

    ```bash
    su
    # 输入上一步修改的 root 密码
    ```

3. 更新系统并重启：

    ```bash
    dnf upgrade --refresh && reboot
    ```

4. 安装 Steam：

    ```bash
    dnf install steam
    ```

## 6. 完成

现在安装完成，可以开始使用 Asahi Linux 并享受 Steam 带来的游戏体验了！
