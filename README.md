# 悉数 · Xishu

[![微信小程序](https://img.shields.io/badge/微信小程序-云开发-07C160?logo=wechat&logoColor=white)](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/getting-started.html)
[![JavaScript](https://img.shields.io/badge/客户端-JavaScript-F7DF1E?logo=javascript&logoColor=222222)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
[![Node.js](https://img.shields.io/badge/云函数-Node.js-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Python](https://img.shields.io/badge/AI%20实验-Python-3776AB?logo=python&logoColor=white)](https://www.python.org/)

> 面向个人用户的资产与生活资产管理微信小程序。

悉数将银行卡、现金与生活资产放在同一个视图中，帮助用户了解资产总量、记录资产变化，并通过汇率、风险分析和目标管理做出更有依据的规划。

这是一个基于**微信小程序云开发**的全栈示例项目：前端使用原生 WXML / WXSS / JavaScript，后端使用微信云函数和云数据库，并预留 AI 分析与数据同步能力。

> **项目状态：开发中**<br>
> 当前版本适合学习、演示和个人使用。涉及真实资产数据前，请先完成权限、隐私和数据备份评估。

## 目录

- [产品亮点](#产品亮点)
- [功能概览](#功能概览)
- [技术架构](#技术架构)
- [快速开始](#快速开始)
- [云函数清单](#云函数清单)
- [数据与安全说明](#数据与安全说明)
- [开发与贡献](#开发与贡献)

## 产品亮点

### 统一的资产视图

- 管理多张银行卡及不同币种账户
- 按实时汇率换算并汇总人民币资产
- 查看账户明细、资产构成和变化趋势
- 导出资产数据，便于留存和复盘

### 可执行的财务工具

- 设置目标并追踪完成进度
- 使用风险雷达和压力测试了解资产风险
- 配置汇率提醒规则，记录提醒历史
- 通过 AI 获取组合建议和汇率分析（需完成相应服务配置）

### 生活资产管理

- 维护资产与心愿两个列表
- 记录购入成本、使用情况和估算残值
- 使用 OCR 辅助录入资产
- 从云端图标库选择图标，并同步多端数据

### 适合持续迭代的工程结构

- 前端页面、云函数和 AI 实验代码分层组织
- 云函数按用户、资产、汇率、风险和 AI 能力拆分
- 内置诊断页，便于验证云端读取、同步和分析链路
- 关键外部服务通过环境变量配置，避免将密钥写入代码

## 功能概览

| 模块 | 能力 |
| --- | --- |
| 首页 | 总资产概览、目标进度、风险提示 |
| 银行卡 | 多账户、多币种、余额及账户详情 |
| 汇率 | 实时汇率、历史记录、汇率提醒 |
| 风险分析 | 风险雷达、压力测试、组合建议 |
| 悉数 | 资产与心愿、成本追踪、残值估算 |
| 数据管理 | 云端同步、数据导出、诊断页面 |

## 技术架构

```text
微信小程序（miniprogram）
        │
        ├── 微信云函数（cloudfunctions）
        │       ├── 用户资料与资产同步
        │       ├── 汇率、风险与提醒服务
        │       └── OCR、AI 分析与图标库
        │
        └── 微信云数据库
                ├── 用户配置与资产数据
                └── 汇率历史数据
```

### 技术栈

- **客户端**：微信小程序原生 WXML、WXSS、JavaScript
- **服务端**：微信云函数、Node.js、`wx-server-sdk`
- **数据层**：微信云数据库
- **图表**：ECharts
- **AI 实验**：Python、PyTorch、Tushare 数据接口

## 项目结构

```text
.
├── miniprogram/          # 小程序页面、组件、样式与工具
├── cloudfunctions/       # 微信云函数
├── ai/                   # 汇率数据处理、训练与推理示例
├── project.config.json   # 微信开发者工具项目配置
└── README.md
```

## 快速开始

### 环境要求

- 微信开发者工具
- 已注册并认证的小程序 AppID
- 已开通微信云开发的环境
- Node.js（用于安装和部署云函数依赖）
- Python 3.9+（仅在运行 `ai/` 目录脚本时需要）

### 打开项目

1. 克隆仓库，并使用微信开发者工具打开仓库根目录。
2. 在 `project.config.json` 中确认 `appid`、`miniprogramRoot` 和 `cloudfunctionRoot` 与当前环境一致。
3. 在微信开发者工具中创建或绑定云开发环境。
4. 按需在对应云函数目录安装依赖，并上传 `cloudfunctions/` 下的函数代码。
5. 编译运行小程序，先创建测试账户和资产，再通过“诊断”页面验证读取、同步、汇率、风险分析和导出流程。

> 不同云函数依赖的第三方服务可能不同。首次部署前，请阅读对应函数目录中的代码和 `package.json`，补齐环境变量、API Token 及权限配置。

### 外部服务配置

| 配置项 | 用途 | 使用位置 |
| --- | --- | --- |
| `TUSHARE_TOKEN` | 导入历史汇率数据 | `tushareImport`、`ai/` |
| `SILICONFLOW_API_KEY` | OCR 与视觉模型调用 | `aiParseAsset` |
| `SILICONFLOW_API_URL` | 自定义视觉模型 API 地址（可选） | `aiParseAsset` |
| `SILICONFLOW_VISION_MODEL` | 视觉模型候选列表（可选） | `aiParseAsset` |
| `ALERT_TEMPLATE_ID` | 微信提醒模板 ID（可选） | `alertMonitor` |
| `ALERT_PAGE_PATH` | 提醒点击后的页面路径（可选） | `alertMonitor` |
| `ICON_LIBRARY_JSON` | 云端图标库配置（可选） | `getIconLibrary` |

请在微信云开发的云函数环境变量中配置生产密钥；本地调试使用脱敏测试值，不要通过命令行参数、截图或提交记录泄露密钥。

### AI 示例

`ai/` 目录提供离线汇率数据处理、LSTM 训练、推理和 ONNX 导出示例。详细步骤见 [ai/README.md](./ai/README.md)。

```bash
pip install -r ai/requirements.txt
python ai/data_prep.py --api fx_daily --token "$TUSHARE_TOKEN" \
  --start 20230101 --end 20231231 --out ai/data.csv
python ai/train_model.py --csv ai/data.csv --datecol trade_date \
  --pricecol CNY --seq 20 --epochs 20 --out ai/model.pth
```

## 云函数清单

| 分类 | 云函数 |
| --- | --- |
| 用户与同步 | `getUserProfile`、`syncUserProfile`、`getXishuAssets`、`syncXishuAssets` |
| 汇率与风险 | `getRealtimeFx`、`riskAnalyzer`、`alertMonitor`、`aiFxMonitor` |
| AI 能力 | `aiPortfolioAdvisor`、`aiParseAsset`、`aiEstimateResale` |
| 数据与工具 | `tushareImport`、`getIconLibrary` |

## 数据与安全说明

- 本项目面向个人资产记录和开发学习场景，不构成投资、税务或法律建议。
- 不要将真实银行卡号、API Token、云环境密钥或其他敏感信息提交到 Git。
- `project.private.config.json`、`.env`、依赖目录和本地模型文件默认不纳入版本控制。
- 建议使用脱敏测试数据完成开发、录屏和演示，并在生产环境中配置最小权限。
- AI 预测、风险分析和残值估算结果仅供参考，不能替代专业判断。

## 开发建议

- 修改页面后，优先使用微信开发者工具进行编译和真机验证。
- 修改云函数后，在对应函数目录安装依赖，再从开发者工具上传部署。
- 涉及网络、权限、弱网和导出的流程，应至少在真机上完成一次验收。
- 新增云函数时，同步补充本 README 的云函数清单和部署说明。

## 开发与贡献

欢迎提交 Issue、改进建议和 Pull Request。为了让问题更容易复现，提交 Issue 时请尽量提供：

1. 微信开发者工具版本和基础库版本；
2. 运行平台（开发者工具、Android 或 iOS）；
3. 可复现问题的步骤；
4. 期望行为与实际行为；
5. 脱敏后的日志或截图。

提交代码前建议完成以下检查：

- 使用微信开发者工具完成编译，确认 WXML、WXSS 和 JavaScript 无报错；
- 在诊断页面验证受影响的云函数链路；
- 不提交真实资产数据、密钥、私有配置和依赖目录；
- 如果新增云函数或环境变量，同步更新本文档。

## 路线图

- [ ] 完善自动化测试与云函数本地调试流程
- [ ] 增加更清晰的首次使用引导和空状态
- [ ] 完善数据备份、恢复与批量导入能力
- [ ] 进一步拆分 AI 服务配置，降低部署门槛

## 许可证

当前仓库未声明开源许可证。若要公开分发或二次开发，请先补充许可证文件并确认第三方资源的授权范围。

## 反馈

欢迎通过 [Issues](https://github.com/Memgin/xishu/issues) 提交问题、功能建议或使用反馈。
