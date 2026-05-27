# 魔词浏览器 logo 资源 (ui-03)

蓝绿渐变圆形 + 白色盾牌指纹 logo（吴总 2026-05-27 提供原图 484x484 PNG）。

## 文件

| 文件 | 用途 |
|---|---|
| `product_logo_16.png` ~ `product_logo_256.png` | chrome.dll 内嵌品牌图标，多尺寸 (16/22/24/32/48/64/128/256) |
| `product_logo_22_mono.png` | 单色变种 fallback |
| `chromium.ico` | chrome.exe 主图标（Windows Explorer / 任务栏 / Alt-Tab）— 含 16/32/48/256 4 个尺寸 |
| `app_list.ico` | Windows 应用列表图标 |

## Apply 指令（patches 流程之前手动覆盖）

```bash
# 把本目录的所有文件覆盖到 chromium 源码树
cp product_logo_*.png \
   /path/to/ungoogled-chromium-windows/build/src/chrome/app/theme/chromium/
cp chromium.ico app_list.ico \
   /path/to/ungoogled-chromium-windows/build/src/chrome/app/theme/chromium/win/

# 然后正常跑 build.py (patches 自动 apply)
```

## 为什么不通过 patches 流程？

二进制文件不适合 text-based diff/patch 工具。直接覆盖文件是 chromium 内核 fork 的标准实践（Chromium / Brave / ungoogled 都用 raw asset replacement 而非 binary patch）。

## 注意

替换后 ninja 不会自动检测 .ico 文件 mtime 变化（gn dep 限制）。**必须强制 rebuild 资源文件**：
```bash
rm out/Default/obj/chrome/app/chrome_dll_resources/chrome_dll.res
rm out/Default/obj/chrome/chrome_initial/chrome_exe.res
ninja -C out/Default chrome
```
