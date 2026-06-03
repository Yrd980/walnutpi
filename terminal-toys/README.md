# 终端玩具

这个目录记录 WalnutPi 上安装的纯终端 / TUI 工具。设备没有桌面环境，所以这些应用都优先选择可以直接在 SSH、串口控制台或本地终端屏幕里运行的版本。

## 启动入口

先运行：

```bash
walnut
```

主启动器已经统一到 `walnut`。这里面的工具通过 `walnut play` 打开，包含音乐、屏幕演示和一次性终端玩具。

`walnut play` 按用途分成三类：

- `Music`：音乐播放器和音乐可视化
- `Screen`：数字雨、管道、火焰、Nyancat、ASCII demo、ASCII 视频和时钟，适合本地小屏展示
- `One-shot Toys`：蒸汽小火车、大字横幅、fortune/cowsay 和 quote box

Walnut 菜单会使用 ANSI 颜色区分分类和入口。SSH / kitty 这类现代终端使用 256 色；本地 `fbterm` / Linux console 会自动降级到基础 16 色，避免小屏颜色被错误映射成一层灰雾。如果当前终端不适合显示颜色，可以用 `NO_COLOR=1 walnut play` 关闭菜单颜色。支持彩色输出的玩具会优先使用彩色模式，例如彩虹数字雨、彩色时钟、彩色管道和 `lolcat` 输出。

安装或补齐终端玩具：

```bash
sudo ./scripts/install-terminal-toys.sh
```

兼容入口：

```bash
walnut-fun
```

它现在会转发到：

```bash
walnut play
```

源码对应：

```bash
terminal-toys/walnut-fun
```

安装后的系统路径：

```bash
/usr/local/bin/walnut-fun
```

## 已安装工具

| 分类 | 工具 | 用途 | 命令 |
| --- | --- | --- | --- |
| Music | cmus | 终端音乐播放器 | `cmus` |
| Music | cava | 音乐可视化 | `cava` |
| Screen | cmatrix | 彩虹数字雨终端效果 | `cmatrix -ab -r` |
| Screen | pipes-sh | 彩色管道屏保 | `pipes -p 4 -R -K -f 60` |
| Screen | libaa-bin | ASCII 火焰 | `aafire` |
| Screen | caca-utils | 彩色火焰和 libcaca demo | `cacafire`, `cacademo` |
| Screen | nyancat | 终端 Nyancat 动画 | `nyancat` |
| Screen | tty-clock | 彩色终端时钟 | `tty-clock -c -s -C 6 -b` |
| One-shot Toys | sl | 一次性蒸汽小火车 | `sl -e` |
| One-shot Toys | toilet | 彩虹终端大字横幅 | `toilet -t --gay TEXT` |
| One-shot Toys | fortune / cowsay | 彩色随机短句和气泡输出 | `fortune -s \| cowsay \| lolcat -f` |
| One-shot Toys | boxes | 给短句加彩色 ASCII 边框 | `fortune -s \| boxes \| lolcat -f` |
| One-shot Toys | lolcat | 可选彩色输出 | `lolcat -f` |

## 音乐库

启动器会创建这个软链接，方便终端音乐播放器找到歌曲：

```bash
$HOME/Music/WalnutMusic -> $HOME/music-library
```

同时会生成 `cmus` 播放列表：

```bash
$HOME/.config/cmus/walnut-library.m3u
```

当前板子上的测试音乐库放在 `/home/pi/music-library`，包含 14 首 Walnut Demo 合成 WAV。root 侧 `/root/music-library` 会软链接到同一目录。

`walnut play` 里的 Music Visualizer 会生成 `$HOME/.config/walnut-cava/config`，默认使用第一个 ALSA capture 设备，避免在无 PulseAudio 会话的终端里启动后立即退出。

## 小屏幕说明

`cmatrix` 很适合这块内屏，因为它直接跑在终端里，不需要 X11 或 Wayland。在本地设备上，它最适合从这个仓库里已经使用的 framebuffer 终端路径启动。

Debian bookworm 的原版 `dosbox` 包不适合直接跑在这块内屏上。实测 `dosbox 0.74-3` 启动时需要至少 640x400 的视频模式，而 WalnutPi 内屏只有 480x320，会退出并显示：

```text
Could not initialize video: No video mode large enough for 640x400
```

因此不要把 DOSBox 作为 Walnut Play 的默认本地小屏工具。若要运行 DOSBox，应使用外接更高分辨率显示、远程图形环境，或选择能明确适配 480x320 framebuffer 的替代方案。
