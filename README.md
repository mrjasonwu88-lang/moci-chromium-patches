# moci-chromium-patches

魔词浏览器（MoCi Browser）Chromium 内核 patches。基于 [ungoogled-chromium-windows](https://github.com/ungoogled-software/ungoogled-chromium-windows) + Brave Shields 移植 + 自写指纹/品牌 patches。

## 内容

`patches/moci-v3/` — 7 个 patches，覆盖 25 个 chromium 源文件 + 2 个新文件，共 ~1910 行：

| Patch | 行数 | 功能 |
|---|---|---|
| `moci-infra-newfiles.patch` | 170 | `chrome/common/moci/fingerprint_config.{h,cc}` — 单例 + JSON 解析 + 派生种子 |
| `moci-infra.patch` | 237 | fp-01 `--fingerprint-config` switch + fp-02 ChromeMainDelegate hook + fp-03b propagate to child processes |
| `moci-blink-noise.patch` | 846 | fp-03 Canvas + fp-04 Audio + fp-05 WebGL vendor/renderer + fp-10 ClientRects + fp-15 Screen pool + fp-17 WebGPU adapter + fp-26 hardwareConcurrency + fp-26b deviceMemory + fp-27 outer dimensions + fp-28 history.length cap + fp-23 media queries + runtime_enabled_features.json5 |
| `moci-blink-disable.patch` | 315 | fp-09 webdriver=false + fp-12 mediaDevices + fp-13 speech + fp-19 battery + fp-20 bluetooth + fp-21 permissions + fp-29 storage quota + fp-32 maxTouchPoints + fp-34 PressureObserver/IdleDetector |
| `moci-network-spoof.patch` | 207 | fp-14 Client Hints + fp-16 timezone + fp-22 navigator.connection |
| `moci-phase3-brand.patch` | 285 | BRANDING + chromium_strings.grd (魔词浏览器 / 魔词) + version.py UTF-8 fix |
| `moci-fp24-perf-precision.patch` | 56 | fp-24 Performance.now() 1ms / 0.1ms 粗化 + performance.memory 强制 bucketized — 抵 WASM 编译时间指纹 / V8 heap 探测 |

## 应用

```bash
# 基础设置 ungoogled-chromium-windows
git clone https://github.com/ungoogled-software/ungoogled-chromium-windows.git
cd ungoogled-chromium-windows
# 把本 repo 的 patches/moci-v3 拷到 ungoogled patches 目录
cp -r /path/to/moci-chromium-patches/patches/moci-v3 patches/
# 在 patches/series 末尾追加 6 行：
#   moci-v3/moci-infra-newfiles.patch
#   moci-v3/moci-infra.patch
#   moci-v3/moci-blink-noise.patch
#   moci-v3/moci-blink-disable.patch
#   moci-v3/moci-network-spoof.patch
#   moci-v3/moci-phase3-brand.patch
# 按 ungoogled 标准流程跑 build
```

## License

- Chromium source: BSD-3-Clause
- ungoogled-chromium patches: CC0-1.0
- MoCi patches (本仓): MIT

166 字段指纹生成算法（按 environment_id 派生噪声种子的具体算法 + 设备库）**不在本仓**，属于商业秘密。

## 不接受外部 PR

为防供应链攻击，本仓不接受任何外部 Pull Request。如发现安全漏洞请通过 `security@kuajing168.com` 报告。

## 关于魔词浏览器

[moci.com.cn](https://moci.com.cn)。面向跨境电商卖家的反指纹浏览器。本 patches 仓仅含内核改动，**不含**业务代码 / 166 字段指纹库 / Electron 客户端。
