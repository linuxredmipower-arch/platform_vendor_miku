# Miku UI Vendor — marble 适配版

> 上游说明见同目录 [`README.md`](README.md)，未改动。
> 本文件是 Miku UI TDA 的**适配声明**。

Miku UI 官方 `platform_vendor_miku` 的 fork，含 marble（POCO F5, SM7475/ukee）专属适配（构建任务 / 内核打包）。

## 平台声明

- **平台**: Android 16（Blooming_v2 = android-16.0.0_r4 + LineageOS 23.2, userdebug）
- **适配分支**: `miku-a16`（当前线）
- **版本标记**: tag `a16`（2026-09-27 打标）
- **基线**: Miku UI `Blooming_v2` @ `defe0c6`
- **A15 线**: `miku-a15` 已冻结（A15 出包验证通过）。两线**已分叉**——`miku-a16` 自上游另开，不是从 `miku-a15` 接续（领先 52 个提交 / 落后 3 个）

## 分支

- `miku-a16` — Android 16 适配分支（当前线）
- `miku-a15` — Android 15 适配分支（已冻结；本地称 `Vampire_v3` 线的适配分支）
- 上游跟踪：`Blooming_v2`（Miku-UI 官方）

## 相对上游的适配（全部独立 commit，可回退）

| commit | 内容 |
|--------|------|
| `5c5cb97` | **`kernel.mk`: DTBS_TECHPACK 相机 staging 修复**（移植 A15 `463c984`）——只 stage 非 camera dtbo + `ukee-camera.dtbo` + `marble*-camera-sensor.dtbo`，排除 mtp/cdp/qrd reference-design（board-id `0x10008` 重叠会坏 phandle fixups）；**内联进 `dtb.img` 规则**保证每次构建重 stage（standalone target 会因 runtime 依赖失效） |

> A15 侧的配套修复（`b0dac15` toybox xargs 兼容）**在 A16 已不需要**：A16 的 `kernel.mk` 不含 `-I{}` 用法。

### 相关坑（来自 A15 线，A16 同构，详见 `miku_docs` round7 记录）

- ninja target 名是 `dtbimage` / `dtboimage`，**不是** `dtb.img`
- kati 会吃掉 recipe 行尾的 `\;`
- mkbootimg 用 `system/tools/mkbootimg/mkbootimg.py`（fragment 组必须以 `--vendor_ramdisk_fragment` 结尾）

## 使用

```shell
git remote add miku-fork https://github.com/linuxredmipower-arch/platform_vendor_miku.git
git fetch miku-fork miku-a16 && git checkout miku-a16
```
