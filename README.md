# raphael-linux-fixes

Raphael（Redmi K20 Pro 至尊版，SM8150）Linux 修复版内核。

本仓库是 [GengWei1997/linux](https://github.com/GengWei1997/linux) 的 fork，保留完整上游历史。

## 修复内容

| 提交 | 修复 |
|---|---|
| `0bec0449f` | qcom_scm 等待加 30s 超时；fastrpc 用户态 invoke 加 10s 超时；fastrpc 拆除时归还 remote_heap 的 SCM 所有权 |
| `b78acea9a` | 重新应用 xiaomi_raphael_build_kernel 的 4 个设备补丁（上游构建脚本中 git apply 被注释，七月发布构建为手动应用） |
| `4c640a79b` | 移植上游 raphael-7.2 的 fastrpc_user 引用计数，修复超时后 use-after-free 内核 Oops |
| `66c83d40b` | 超时路径释放 pending 列表引用，防止循环等待泄漏 |

修复效果：

- 坏启动开机时间 4:03 → 约 25 秒
- glink 卡死不再拖慢开机
- DSP crash 后可恢复

## 构建

使用 [GavinLiuOnline/xiaomi_raphael_build_kernel](https://github.com/GavinLiuOnline/xiaomi_raphael_build_kernel) 的配方（`raphael.config` + `LLVM=-22 deb-pkg`）。

## Release 用法

`kernel-v7.0` Release 提供 4 个 deb：

- `linux-image-xiaomi-raphael.deb`（内核 + 模块 + DTB）
- `linux-headers-xiaomi-raphael.deb`
- `firmware-xiaomi-raphael.deb`、`alsa-xiaomi-raphael.deb`（沿用上游 build_kernel）

本机安装：`sudo dpkg -i linux-image-xiaomi-raphael.deb`。

或 配合 [xiaomi_raphael_build_rootfs](https://github.com/AKaNecoo/xiaomi_raphael_build_rootfs) 的 Actions， 构建时 `内核包仓库` 填 `AKaNecoo/raphael-linux-fixes`。

## 许可

GPL-2.0
