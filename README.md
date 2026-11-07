# OpenList for Synology DSM 7

把 [OpenListTeam/OpenList](https://github.com/OpenListTeam/OpenList) 的最新版本封装成群晖 DSM 7 套件（SPK），由 GitHub Actions 定时构建并发布。

OpenList 是一个支持多存储的文件列表程序，基于 AList 项目继续开发，可用于统一浏览和管理网盘、对象存储、本地目录等。

## 特性

- 不依赖 Docker，直接运行上游原生二进制
- 跟随上游 Release 自动构建，通常在上游发版后数小时内产出新包
- 同时提供 x86_64 与 ARM 两种架构
- 构建过程固定时间戳，同版本产物可重现

## 安装

### 手动安装

在 [Releases](../../releases) 下载对应架构的安装包，然后到套件中心 →「手动安装」：

| 架构 | 文件 |
| --- | --- |
| x86_64 机型 | `openlist_<version>_x86_64.spk` |
| ARM 机型 | `openlist_<version>_armv8.spk` |

若不确定机型对应的架构，可在 DSM「控制面板 → 信息中心」查看，或参考 [群晖机型列表](https://kb.synology.com/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have)。

### 通过套件源安装

把本仓库的 Release 汇总到自建套件源后，即可在套件中心直接搜索安装。仓库本身不提供在线套件源。

## 配置

默认管理员账号 `admin`，密码 `openlist`，首次登录后请立即修改。

套件数据目录：`/var/packages/openlist/var`

## 构建

```bash
python build.py
```

产物输出到 `dist/`。构建流程：

1. 查询上游最新 Release
2. 按架构匹配对应资产并下载
3. 组装 `package.tgz` 与 `INFO`，打成 SPK
4. 生成 `dist/manifest.json` 与发布说明

仓库结构：

```
build.py               构建脚本
spk_builder.py         SPK 封装实现
app.json               套件元数据与上游资产匹配规则
template/              INFO、conf、scripts、UI 等套件框架文件
.github/workflows/     自动构建与发布
```

最低 DSM 版本：7.1-42661

## 免责声明

本项目是社区维护的封装脚本，与上游项目及群晖科技均无隶属关系。软件本身版权归上游作者所有，使用前请阅读上游项目的许可证。
