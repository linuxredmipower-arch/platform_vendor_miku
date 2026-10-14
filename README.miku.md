# Miku UI Vendor — marble 适配版

> 上游说明见同目录 [`README.md`](README.md)，未改动。
> 本文件是 Miku UI TDA 的**适配声明**。

Miku UI 官方 `platform_vendor_miku` 的 fork，含 marble（POCO F5, SM7475/ukee）专属适配（构建任务 / 内核打包）。

## 平台声明

- **平台**: Android 17（Cinderella = android-17.0.0_r1，LineageOS 24.0 基线铺底）
- **适配分支**: `miku-a17`（当前线）
- **基线**: Miku UI `Cinderella`
- **A16 线**: `miku-a16`（Blooming_v2 基线，2026-09-27 收工、刷机验证通过）；**A17 自上游另开，不从 A16 接续**
- **A15 线**: `miku-a15` 已冻结

## 分支

- `miku-a17` — Android 17 适配分支（当前线）
- `miku-a16` — Android 16 适配分支（Blooming_v2）
- `miku-a15` — Android 15 适配分支（已冻结）
- 上游跟踪：`Cinderella`（Miku-UI 官方）

## 相对上游的适配（全部独立 commit，可回退）

| commit | 内容 |
|--------|------|
| `42b4a48` | **`kernel.mk`: DTBS_TECHPACK 相机 staging 修复**（重放 A16 `5c5cb97` / A15 `463c984`）——只 stage 非 camera dtbo + `ukee-camera.dtbo` + `marble*-camera-sensor.dtbo`，排除 mtp/cdp/qrd reference-design（board-id 重叠会坏 phandle fixups）；**内联进 `dtb.img` 规则**保证每次构建重 stage |

> 与 Cinderella 的差异小注：上游 kernel.mk 有 `Stop warnings for prebuilt kernel platform` / `Rip out GCC support` 两处前进；重放未冲突，后续构建中如遇改动需复核。

### 相关坑（来自 A15 线，A16 同构，详见 `miku_docs` round7 记录）

- ninja target 名是 `dtbimage` / `dtboimage`，**不是** `dtb.img`
- kati 会吃掉 recipe 行尾的 `\;`
- mkbootimg 用 `system/tools/mkbootimg/mkbootimg.py`（fragment 组必须以 `--vendor_ramdisk_fragment` 结尾）

## 使用

```shell
git remote add miku-fork https://github.com/linuxredmipower-arch/platform_vendor_miku.git
git fetch miku-fork miku-a17 && git checkout -B miku-a17 miku-fork/miku-a17
```
