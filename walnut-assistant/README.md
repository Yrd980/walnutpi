# Walnut Home

Walnut Home 是这台无桌面 WalnutPi 终端的命令中心。

它把原本分散的 Linux 命令收拢成一个入口：

```bash
walnut
```

## 命令

```bash
walnut                 # 交互式菜单
walnut ai              # 打开 WalnutAI 聊天
walnut ai TEXT         # 向 WalnutAI 提一个一次性 agent 请求
walnut notes           # 笔记子菜单
walnut play            # 打开 Music、Screen 和 One-shot Toys 分类
walnut status          # 系统、网络、服务、Docker、蓝牙状态
walnut video [mode]    # 播放 ASCII 视频演示：color 或 gray
walnut voice           # 打开语音键盘 CLI
walnut note TEXT       # 追加一条日记笔记
walnut today           # 显示今天的笔记
```

较少直接使用的命令：

- `walnut status`：设备、网络和服务检查
- `walnut ai TEXT`：一次性 agent 回合；能由核桃派本地查询或执行的请求，应先用本地能力处理，再总结给用户
- `walnut video color|gray`：直接播放 ASCII 视频
- `walnut note TEXT` 和 `walnut today`：快速记笔记
- `walnut voice`：语音键盘 CLI；主菜单暂时隐藏，等接入麦克风和 STT 凭据后再测试
- `voice-keyboard-walnutpi.service`：保留安装能力，但默认不自启

## 设计

这个入口故意做得小而稳定：

- 纯 CLI，不依赖桌面环境
- 只使用 Python 标准库
- 所有主要入口统一在 `walnut`
- 维护类操作收进菜单里，不再外露太多顶层命令

## 笔记

日记笔记会写到当前用户的 home 目录：

```bash
~/walnut-memory/daily/YYYY-MM-DD.md
```

这是 WalnutPi 本地记忆层的第一步。

## 安装

```bash
cp walnut /usr/local/bin/walnut
chmod +x /usr/local/bin/walnut
```

## 运行时布局

- 源码仓库：`~/projects/WalnutPi`，或者 `WALNUT_PROJECT_ROOT` 指向的位置
- 主启动器：`/usr/local/bin/walnut`
- 兼容启动器：`/usr/local/bin/walnut-fun` -> `walnut play`
- 已安装的 WalnutAI 运行时：`/opt/walnut-ai`
- 已安装的语音运行时：`/opt/walnut-voice-keyboard`

路径解析是可覆盖的：

- `WALNUT_PROJECT_ROOT` 可覆盖源码仓库位置
- `WALNUT_MEMORY_DIR` 可覆盖笔记位置
- `WALNUT_MUSIC_DIR` 可覆盖本地音乐库位置

源码修改请保留在 Git 仓库里，安装时再把启动器复制到 `/usr/local/bin`。
