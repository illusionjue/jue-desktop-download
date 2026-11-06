# jue-desktop

> 基于 Tauri 2 + React 19 + Rust 的桌面应用开发框架。一套固定分层、一个脚手架、一套 AI 技能，让不熟 Rust 的人也能做出生产级桌面应用。

**产出的应用安装包约 3MB**（同功能 Electron 应用约 150MB）。

---

## 能做什么

| 能力 | 说明 |
|---|---|
| 三层后端 | Command / Service / Database，职责边界强制分开 |
| 本地数据 | SQLite（rusqlite bundled），免安装、随应用分发 |
| 真实终端 | portable-pty + xterm.js，可跑 AI CLI、vim、任意 TUI 程序 |
| 多窗口 | 主窗口标签页 + 可分离的独立窗口 |
| 系统集成 | 托盘常驻、全局快捷键、系统通知、自动更新 |
| AI 开发 | 7 个技能 + 5 个命令 + 3 个钩子，AI 按规范生成代码 |

---

## 快速开始

### 环境要求

| 依赖 | 版本 |
|---|---|
| Node.js | 20+ |
| pnpm | 9+ |
| Rust | stable（rustup 安装） |
| Visual Studio Build Tools | 含 C++ 生成工具（Windows 必需） |

### 生成项目

```bash
# 在框架仓库根目录
node cli/index.js my-app --dir ./examples

cd examples/my-app
pnpm install
pnpm tauri dev
```

一个窗口会打开，标题是你给的项目名。

### 打包

```bash
pnpm build:win            # NSIS 安装包 + 自动更新包（需签名私钥）
pnpm build:win:unsigned   # 仅安装包，本地快速验证
```

产物在 `src-tauri/target/release/bundle/nsis/`：
- `<name>_<version>_x64-setup.exe`（约 3MB）

macOS（dmg）与 Linux（deb / AppImage）由 GitHub Actions 构建。

---

## 目录结构

```
jue-desktop/
├── template/                    可复制的项目骨架
│   ├── src/                     前端
│   │   ├── api/                 invoke 封装
│   │   ├── types/               类型定义
│   │   ├── pages/               页面组件
│   │   └── styles/              设计令牌与材质
│   └── src-tauri/               后端
│       ├── src/
│       │   ├── commands/        IPC 入口
│       │   ├── services/        业务逻辑
│       │   ├── db/              数据访问
│       │   ├── models/          数据结构
│       │   ├── error.rs         统一错误
│       │   └── lib.rs           初始化与命令注册
│       └── tauri.conf.json      窗口、打包、更新配置
├── cli/                         脚手架
├── examples/                    示例应用
├── docs/                        文档
└── .claude/                     AI 开发配置（技能 / 命令 / 钩子）
```

---

## 三条铁律

违反这三条，代码结构和框架预期就不一致了：

1. **Command 层不写业务逻辑** —— 只解包状态、校验参数、转发 Service
2. **Service 层不写 SQL** —— 所有数据库语句下沉到 Database 层
3. **页面不直接 `invoke`** —— 前端一律经过 `src/api/`

细节见 [架构说明](docs/架构说明.md)。

---

## 文档

| 文档 | 内容 |
|---|---|
| [快速开始](docs/快速开始.md) | 从零到一个能跑的应用 |
| [架构说明](docs/架构说明.md) | 三层分层、数据流、约束 |
| [开发指南](docs/开发指南.md) | 新增模块、IPC、样式、调试 |
| [发布指南](docs/发布指南.md) | 打包、签名、自动更新 |
| [框架需求文档](docs/框架需求文档.md) | 12 项决策、里程碑、验收标准 |

---

## AI 开发

框架根目录的 `.claude/` 是给 AI 用的开发配置：

| 类型 | 数量 | 说明 |
|---|---|---|
| 技能 | 7 | crud-desktop / ipc-development / terminal-pty / sqlite-operations / ui-react / tauri-config / add-skill |
| 命令 | 5 | `/start` `/next` `/check` `/crud` `/exp` |
| 钩子 | 3 | 技能强制评估、危险命令拦截、nul 清理 |

使用时在框架根目录打开 Claude Code，`CLAUDE.md` 会加载项目约定。

---

## 技术栈

| 层 | 技术 |
|---|---|
| 桌面 | Tauri 2 |
| 后端 | Rust（edition 2024） |
| 前端 | React 19 + TypeScript |
| 构建 | Vite + Cargo |
| 样式 | CSS 变量令牌 + 材质类（无 UI 框架依赖） |
| 终端 | portable-pty + xterm.js |
| 数据 | SQLite（rusqlite bundled） |

---

## 许可

MIT
