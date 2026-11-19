---
name: drm-gpu
description: "修改 Linux 内核里的 DRM、KMS、amdgpu 或其他 GPU 驱动时使用。用户提到 drm、kms、gem、amdgpu、显示模式、GPU reset 或 dma-buf 时也要使用。"
---

# DRM 和 GPU 驱动

这棵树是 Linux 5.0-rc1。DRM 核心在 `drivers/gpu/drm/`，AMD 驱动在 `drivers/gpu/drm/amd/`。用户态头文件在 `include/uapi/drm/`。

- 先读同目录已有的 init、fini、suspend 和 reset，新代码走同一套生命周期。
- 已经走 atomic 的代码，不要绕过 atomic state 直接改显示寄存器。本来就是 legacy 路径的，就继续留在 legacy 路径。
- GEM、VRAM 和 GTT 的释放要和提交、fence 对齐。复位路径要能处理 GPU 已经挂死、fence 不会再响的情况。
- 用户态 ioctl 结构放在 uapi。加字段时保持大小和填充兼容。
- 寄存器头文件很大。只引用这次用到的寄存器，不要整文件重排。
- 日志用 `DRM_DEBUG`、`DRM_ERROR` 或驱动自己的宏。热路径不要无条件 printk。
