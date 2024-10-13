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

安装完成，可以开始使用 Asahi Linux 并享受支持 Steam 的游戏体验了

## 附加内容：卸载步骤

#以下是卸载 Asahi Linux 步骤的代码：

```markdown
# 卸载 Asahi Linux 指南

## 1. 查看磁盘列表

首先，使用以下命令查看当前磁盘分区列表：

```bash
diskutil list
```

你会看到类似以下输出的内容：

```text
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.3 GB   disk0
   1:             Apple_APFS_ISC Container disk1         524.3 MB   disk0s1
   2:                 Apple_APFS Container disk4         390.0 GB   disk0s2
   3:                 Apple_APFS Container disk3         2.5 GB     disk0s3
   4:                        EFI EFI - FEDOR             524.3 MB   disk0s4
   5:           Linux Filesystem                         1.1 GB     disk0s5
   6:           Linux Filesystem                         100.3 GB   disk0s6
   7:        Apple_APFS_Recovery Container disk2         5.4 GB     disk0s7
```

> **说明**: 需要检查自己的输出序号，找到对应的 Asahi Linux 分区（例如上面示例中的 3, 4, 5, 6）。

## 2. 删除 Asahi Linux 的引导程序和分区

接下来，执行以下命令来删除 Asahi Linux 的引导程序和分区：

1. 删除引导程序：

    ```bash
    diskutil apfs deleteContainer disk0s3
    ```

2. 清除 Linux 分区：

    ```bash
    diskutil eraseVolume free free disk0s4
    diskutil eraseVolume free free disk0s5
    diskutil eraseVolume free free disk0s6
    ```

> **注意**: 这些命令将会清空对应的 Linux 分区和 EFI 分区。

## 3. 合并空闲空间

再次运行 `diskutil list`，确认清除后的空闲分区已被释放。你会看到输出的内容有一个(free space)的条目：

```text
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.3 GB   disk0
   1:             Apple_APFS_ISC Container disk1         524.3 MB   disk0s1
   2:                 Apple_APFS Container disk4         390.0 GB   disk0s2
                    (free space)                         104.4 GB   -
   3:        Apple_APFS_Recovery Container disk2         5.4 GB     disk0s7
```

接下来，开始合并所有空闲分区到 macOS：

```bash
diskutil apfs resizeContainer disk0s2 0
```

> **说明**: 这将合并所有存在的空闲分区到 `disk0s2`（macOS 的主分区）。

## 4. 完成

```text
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.3 GB   disk0
   1:             Apple_APFS_ISC Container disk1         524.3 MB   disk0s1
   2:                 Apple_APFS Container disk4         494.4 GB   disk0s2
   3:        Apple_APFS_Recovery Container disk2         5.4 GB     disk0s7
```
至此，Asahi Linux 已经被完全卸载，磁盘空间成功合并到 macOS 中。

```text
以上操作参考了：https://asahilinux.org/2024/10/aaa-gaming-on-asahi-linux/
以及官方wiki：https://github.com/AsahiLinux/docs/wiki/Partitioning-cheatsheet
```

# 附加内容：扩展Asahi Linux的磁盘分区大小

```markdown
# 获取macOS磁盘分区信息 分配15GB的空闲空间给Asahi Linux使用

以下是示例输出的 `/dev/disk0` 的分区信息：

| #   | 类型                              | 名称                 | 大小         | 标识符       |
|-----|-----------------------------------|----------------------|--------------|--------------|
| 0   | GUID_partition_scheme             | -                    | 500.3 GB     | disk0        |
| 1   | Apple_APFS_ISC Container          | disk1                | 524.3 MB     | disk0s1      |
| 2   | Apple_APFS Container              | disk5                | 330.0 GB     | disk0s2      |
| 3   | Apple_APFS Container              | disk3                | 2.5 GB       | disk0s3      |
| 4   | Apple_APFS Container              | disk2                | 2.5 GB       | disk0s4      |
| 5   | EFI                               | EFI - FEDOR          | 524.3 MB     | disk0s5      |
| 6   | Linux Filesystem                  | -                    | 1.1 GB       | disk0s6      |
| 7   | Linux Filesystem                  | -                    | 157.8 GB     | disk0s7      |
| 8   | Apple_APFS_Recovery Container     | disk4                | 5.4 GB       | disk0s8      |
```
要从现有的 APFS 容器（330 GB 空间的 disk0s2）中分出 15 GB 空间

```bash
sudo diskutil apfs resizeContainer disk5 315g
```
> ⚠️ 这条命令会将 APFS 容器 disk5 的大小缩小到 315 GB，从而分配出 15 GB 的空闲空间。你可以调整 315g 为你实际需要的大小

## 1. 添加空闲分区到现有 Btrfs 文件系统

以上操作需要在Asahi Linux的终端中操作 分区信息可通过如下命令获取
```bash
sudo lsblk -f
```

假设现有的主 Btrfs 文件系统位于 `/dev/nvme0n1p7`，空闲分区为 `/dev/nvme0n1p9`。使用以下命令将 `nvme0n1p9` 添加到 `nvme0n1p7` 的 Btrfs 文件系统中：

```bash
sudo btrfs device add /dev/nvme0n1p9 /
```

> ⚠️ 注意：请将 `/` 替换为实际挂载的目录路径。如果 nvme0n1p7 被挂载到其他位置（例如 /mnt/data），则替换成对应的挂载点。

## 2. 重新分配 Btrfs 文件系统

添加分区后，因为Btrfs 文件系统不会自动扩展到新的设备上，因此你需要手动运行 btrfs balance 命令来重新分配空间：

```bash
sudo btrfs balance start /
```

> ⚠️ 注意：请将 `/` 替换为实际挂载的目录路径。如果 nvme0n1p7 被挂载到其他位置（例如 /mnt/data），则替换成对应的挂载点。
这个命令将确保新添加的分区中的可用空间能够被整个 Btrfs 文件系统使用

## 3. 验证合并是否成功

### 查看 Btrfs 文件系统的状态

合并完成后，你可以使用以下命令查看 Btrfs 文件系统的状态，确认新空间是否已添加

```bash
sudo btrfs filesystem usage /
```

你应该在其中看到类似如下的输出条目：

```text
Overall:
    Device size:                 180.00GiB
    Device allocated:            165.00GiB
    Device unallocated:          15.00GiB

Unallocated:
   /dev/nvme0n1p7                15.00GiB
```

> ⚠️ 注意：此项操作存在风险 可能会导致重启后卡在进入KDE Plasma的动画

## 参考命令

- 移除分区（如果需要恢复单一分区 比如移除 nvme0n1p9 并将其空间合并回 nvme0n1p7）
  ```bash
  sudo btrfs device remove /dev/nvme0n1p9 /
  ```

通过以上步骤，你可以将空闲分区逻辑合并到已有的 Btrfs 文件系统中且它们在系统重启后自动挂载。

```text
以上操作参考了Google的一些内容但不推荐尝试
重装其实是最快的办法
```
