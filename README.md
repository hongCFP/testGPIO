# testGPIO — Lesson 0 点灯练习

## 这块板

- 板子：f103 工控板 V1，MCU **STM32F103C8T6**（LQFP48）
- 外部晶振 12 MHz，系统时钟 72 MHz
- **LED2 = PA15，低电平点亮**
- **SW1 = PA0，按下接地**（板上 R17 已上拉，不用开内部上拉）
- 蜂鸣器 = PB5
- PC13 只引到排针，板上没接任何东西
- 下载器：ST-Link V2（0483:3748）

## 程序干什么

按下 SW1 → LED2 亮；松开 → 灭。逻辑在 `Core/Src/main.c` 的 `while (1)` 里。

## 怎么编译烧录

VS Code 打开本目录 → CMSIS 视图构建 → Debug Adapter 选 pyocd → Load & Run。

命令行（PowerShell 或 cmd，**不要**用 git-bash）：

```bat
set A=C:\Users\30958\.vcpkg\artifacts\2139c4c6
set PATH=%A%\tools.kitware.cmake\3.31.12\bin;%A%\tools.ninja.build.ninja\1.13.2;%A%\tools.open.cmsis.pack.cmsis.toolbox\2.15.0\bin;%PATH%
set AC6_TOOLCHAIN_6_22_0=%A%\compilers.arm.armclang\6.22.0\bin
cd /d D:\RoboMaster\workspace\testGPIO\MDK-ARM
cbuild testGPIO.csolution.yml
pyocd load -t stm32f103c8 out\testGPIO\testGPIO\testGPIO.axf
```

`AC6_TOOLCHAIN_6_22_0` 必须指向编译器 bin 目录，否则 cbuild 报
`no compiler registered`。

## 目录

| 路径 | 内容 |
|---|---|
| `testGPIO.ioc` | CubeMX 配置源头 |
| `Core/Src/main.c` | 主逻辑（只写 USER CODE BEGIN/END 之间） |
| `Core/Src/gpio.c` | `MX_GPIO_Init`：开时钟、配引脚 |
| `Core/Src/system_stm32f1xx.c` | 时钟树配置 |
| `MDK-ARM/startup_stm32f103xb.s` | 启动文件，上电第一段代码 |
| `MDK-ARM/testGPIO_testGPIO.sct` | 分散加载脚本（Flash/RAM 地址分配） |
| `Drivers/` | ST 官方 HAL 库，不要改 |

## 状态

2026-10-23 Lesson 0 验收通过：按下 SW1 → LED2 亮，松开 → 灭。
