# Miku UI Vendor — marble 适配版

Miku UI 官方 `platform_vendor_miku` 的 fork，含 marble（POCO F5, SM7475/ukee）专属适配（构建任务/内核打包）。

## 平台声明

- **平台**: Android 15（trunk_staging / Baklava, userdebug）
- **适配分支**: `miku-a15`（本仓库适配主分支，2026-08-12 从 Vampire_v3 分出）
- **官方跟踪**: `Vampire_v3`（本地保留，跟踪 Miku-UI 官方上游；ROM 版本名保持 Vampire v3）
- **版本标记**: tag `a15`（2026-08-12 打标）
- **基线**: Miku UI Vampire v3（A15 线）
- **A16 迁移**: 下一轮切 Android 16 时本分支冻结，新平台另建 `miku-a16` 分支

## 相对上游的适配（全部独立 commit，可回退）

| commit | 内容 |
|--------|------|
| `463c984` | **kernel.mk: DTBS_TECHPACK staging 内联进 dtb.img 规则**（相机修复核心）——只 stage 非 camera dtbo + `ukee-camera.dtbo` + `marble*-camera-sensor.dtbo`，排除 mtp/cdp/qrd reference-design（board-id 0x10008 重叠会坏 phandle fixups）；内联保证每次构建重 stage（standalone target 会因 runtime 依赖失效） |
| `b0dac15` | kernel.mk: toybox xargs 兼容（`-I{}` → command substitution，toybox 0.8.11 不支持） |

> 坑（详见 miku_docs round7_operation.md）：ninja target 是 `dtbimage/dtboimage` 不是 `dtb.img`；kati 会吃掉 recipe 行尾 `\;`；mkbootimg 用 `system/tools/mkbootimg/mkbootimg.py`（fragment 组须以 --vendor_ramdisk_fragment 结尾）。

## 使用

```shell
git remote -v  # origin_miku = linuxredmipower-arch/platform_vendor_miku (fork)
# 编译前确认在 miku-a15 分支（含 463c984）
```
