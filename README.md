# A1WJUN-KBD47 ZMK 键盘固件配置

基于 [ZMK](https://zmk.dev) 的 47 键无线机械键盘固件配置（主控：nice!nano / nRF52840）。

## 目录结构

```
├── build.yaml                          # GitHub Actions 构建矩阵（固件 + 设置重置固件）
├── .github/workflows/build.yml         # 调用 ZMK 官方构建工作流
├── config/
│   ├── A1WJUN-KBD47.conf               # 核心配置（蓝牙/电源/鼠标仿真/去抖等）
│   └── west.yml                        # west 清单（跟随 zmk main）
├── boards/shields/A1WJUN-KBD47/
│   ├── A1WJUN-KBD47.keymap             # 按键映射（层/行为/宏/组合键）
│   ├── A1WJUN-KBD47.dtsi               # 物理布局（ZMK Studio 用）
│   ├── A1WJUN-KBD47.overlay            # 矩阵/编码器硬件定义
│   └── Kconfig.defconfig               # 板级默认配置（键盘名/DCDC 等）
└── zephyr/module.yml                   # Zephyr 模块声明
```

## 层布局概览

| 层 | 名称 | 激活方式 | 说明 |
|---|---|---|---|
| 0 | Base | 默认 | 主键盘；ESC 轻点=ESC/按住=静音，TAB 轻点=TAB/按住=Caps |
| 1 | Number | 按住空格 | 数字/符号/鼠标移动 |
| 2 | Function | 按住空格 | F1-F12 |
| 3 | Bluetooth | 按住左 Ctrl | 蓝牙切换/账号密码宏/输出切换/刷机 |
| 4 | Symbols | 按住右 Alt | 符号（预留） |
| 6 | Navigation | 按住 DEL | 导航（预留） |
| 8 | System | 按住 ↑ | 系统（预留） |
| 10+ | Layer10-31 | - | 预留扩展层 |

## 构建

**推荐：GitHub Actions 自动构建**——推送任意提交后在 Actions 页面下载 `.uf2` 固件。

本地构建（需 ZMK 开发环境）：

```sh
west build -p -b nice_nano//zmk -- -DSHIELD=A1WJUN-KBD47
```

## 自定义约定

- **组合键超时三档规则**：50ms = 纯快速组合 / 80ms = 含修饰键 / 100ms = 涉及 Tap Dance 键
- **hold-tap 参数顺序**：`&行为 按住动作 轻点动作`（参数 1=按住，参数 2=轻点）
- **宏安全约定**：密码类宏只存前缀，剩余部分手动输入，完整密码不入固件

## 支持特性

蓝牙多设备（5 个）、ZMK Studio 在线配置（带锁）、鼠标仿真与滚轮加速、
编码器（滚动/媒体/蓝牙切换）、软关机、电池电量上报。
