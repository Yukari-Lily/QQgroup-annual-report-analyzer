# QQ群年度报告分析器

QQ 群聊天记录分析工具，可以生成精美的年度报告

## ✨ 核心特性

- 📊 **智能词频统计**：基于 jieba 分词的高级文本分析
- 🔍 **新词发现**：自动识别群聊专属新词
- 📈 **多维度排行榜**：发言量、活跃度、表情包、夜猫子等多个维度
- 🎨 **精美可视化报告**：自动生成 HTML/PNG 格式年度报告
- 🤖 **AI 智能点评**：集成 OpenAI API，提供 AI 年度总结（可选）
- 🎯 **交互式选词**：Web 界面支持从热词列表中自主选择展示词汇
- 💾 **数据持久化**：MySQL 数据库永久存储报告（Web 模式）
- 📜 **历史记录管理**：随时查看、搜索、删除历史报告（Web 模式）
- 📱 **响应式设计**：完美适配各种设备
- ⚙️ **高度可定制**：丰富的配置参数，满足不同需求

## 🚀 快速开始

本项目提供两种使用方式：

### 方式一：Web 界面模式（推荐）

通过浏览器访问可视化界面进行操作，提供更友好的交互体验和数据管理功能。可以本地运行，也可以部署到服务器作为 Web 应用。

#### 📋 环境要求

使用前请确保已安装以下软件：

1. **Python 3.8+** （必需）
   - Windows: 从 [python.org](https://www.python.org/downloads/) 下载安装
   - Mac: `brew install python3`
   - Linux: `sudo apt install python3 python3-venv python3-pip`

2. **Node.js 16+** （必需）
   - 从 [nodejs.org](https://nodejs.org/) 下载安装
   - 或使用 nvm 等版本管理工具

3. **MySQL 5.7+ / 8.0+** （可选，仅在需要多用户或生产环境时）
   - 默认使用 JSON 文件存储，**无需安装 MySQL**
   - 如需使用 MySQL，请安装并配置：
     - Windows: 从 [mysql.com](https://dev.mysql.com/downloads/mysql/) 下载安装
     - Mac: `brew install mysql`
     - Linux: `sudo apt install mysql-server`
   - 启动 MySQL 服务：
     - Windows: 在服务管理器中启动 MySQL 服务
     - Mac: `brew services start mysql`
     - Linux: `sudo systemctl start mysql`

#### 🎯 一键启动（强烈推荐）

项目提供了一键启动脚本，可自动完成环境配置和服务启动。

**Windows 系统：**
```bash
# 双击运行或在命令行执行
start.bat
```

**Linux / Mac 系统：**
```bash
# 添加执行权限（仅首次需要）
chmod +x start.sh stop.sh

# 启动服务
./start.sh

# 停止服务
./stop.sh
```

**首次运行注意事项：**
- 脚本会自动创建 `backend/.env` 配置文件
- **默认使用 JSON 文件存储，无需配置 MySQL**
- 如需使用 MySQL，请编辑 `backend/.env` 设置 `STORAGE_MODE=mysql` 并配置数据库密码
- 脚本会自动安装所有依赖并初始化存储

**启动流程说明：**

脚本会自动执行以下步骤：
1. ✅ 检查 Python 和 Node.js 环境
2. ✅ 检查/创建配置文件（backend/.env）
3. ✅ 创建虚拟环境并安装依赖
4. ✅ 初始化存储系统（默认为 JSON 文件存储）
5. ✅ 启动后端服务（Flask，端口5000）
6. ✅ 启动前端服务（Vite，端口5173）

**访问应用：**
- 前端界面：http://localhost:5173
- 后端API：http://localhost:5000

#### ⚙️ 配置说明

在首次运行时，脚本会自动创建 `backend/.env` 文件。

**默认配置（无需 MySQL）：**

```env
# 存储模式（默认使用 JSON 文件存储）
STORAGE_MODE=json

# Flask 配置
FLASK_SECRET_KEY=your_secret_key
FLASK_PORT=5000
DEBUG=false

# CORS 配置
ALLOWED_ORIGINS=http://localhost:5173,http://localhost:5000

# OpenAI API（用于AI功能，可选）
OPENAI_API_KEY=sk-...
OPENAI_BASE_URL=https://api.openai.com/v1
OPENAI_MODEL=gpt-3.5-turbo
```

**如需使用 MySQL（多用户/生产环境）：**

```env
# 切换到 MySQL 存储模式
STORAGE_MODE=mysql

# MySQL 数据库配置
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_password    # ⚠️ 修改为你的MySQL密码
MYSQL_DATABASE=qq_reports
```

**存储模式说明：**
- `STORAGE_MODE=json`（默认）：使用 JSON 文件存储在 `runtime_outputs/reports_db/` 目录
  - ✅ 无需安装 MySQL
  - ✅ 适合个人本地使用
  - ✅ 数据以文件形式存储，易于备份和迁移
  
- `STORAGE_MODE=mysql`：使用 MySQL 数据库存储
  - ✅ 适合多用户环境
  - ✅ 适合生产部署
  - ✅ 支持高并发访问
  - ⚠️ 需要安装和配置 MySQL

#### 🔧 手动启动

如果你想手动控制启动过程：

```bash
# 1. 配置后端
cd backend
cp .env.example .env
# 编辑 .env 填入数据库配置

# 2. 安装后端依赖
pip install -r requirements.txt

# 3. 启动后端（会自动初始化 JSON 存储）
python app.py

# 4. 在新终端启动前端
cd ../frontend
npm install
npm run dev
```

**如需使用 MySQL：**

```bash
# 编辑 backend/.env，设置 STORAGE_MODE=mysql 并配置数据库信息
# 然后初始化数据库
cd backend
python init_db.py
```

#### 🌐 部署到服务器

如果你想将应用部署到服务器作为在线服务，请参考 [DEPLOYMENT.md](./DEPLOYMENT.md) 了解详细的生产环境部署指南。

### 方式二：命令行模式

直接通过终端运行分析脚本，适合快速批量分析或自动化场景。

#### 1. 安装依赖

```bash
git clone https://github.com/ZiHuixi/QQgroup-annual-report-analyzer.git
cd QQgroup-annual-report-analyzer
pip install -r requirements.txt
```

#### 2. 准备聊天记录

使用 [qq-chat-exporter](https://github.com/Yiyuery/qq-chat-exporter) 导出 QQ 群聊天记录为 JSON 格式。

#### 3. 配置

```bash
# 复制配置模板
cp config.example.py config.py

# 编辑 config.py，设置输入文件路径
# INPUT_FILE = "path/to/your/chat.json"
```

#### 4. 运行分析

```bash
python main.py
```

生成的报告在 `runtime_outputs` 目录下。

## 🔍 故障排查

### 问题1：存储服务初始化失败

**默认 JSON 存储模式：**
- JSON 存储几乎不会出现问题，如遇到错误，检查 `runtime_outputs/reports_db/` 目录权限

**MySQL 存储模式错误：**

**错误信息：** "MySQL 初始化失败"

**解决方案：**
1. 如果不需要 MySQL，可以在 `backend/.env` 中设置 `STORAGE_MODE=json` 使用默认的 JSON 存储
2. 如果需要使用 MySQL：
   - 确认 MySQL 服务已启动
   - 检查 `backend/.env` 中的数据库配置
   - 确认 MySQL 用户有创建数据库的权限
   - 尝试手动连接测试：
     ```bash
     mysql -u root -p
     ```

### 问题2：端口已被占用

**错误信息：** "Address already in use"

**解决方案：**
1. 检查端口5000和5173是否被占用
   - Windows: `netstat -ano | findstr :5000`
   - Linux/Mac: `lsof -i :5000`
2. 停止占用端口的进程或更改配置端口

### 问题3：Python依赖安装失败

**解决方案：**
1. 确认网络连接正常
2. 尝试使用镜像源：
   ```bash
   pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
   ```
3. 或手动安装关键依赖：
   ```bash
   pip install flask flask-cors pymysql python-dotenv jieba
   ```

### 问题4：前端依赖安装失败

**解决方案：**
1. 清理缓存：
   ```bash
   cd frontend
   rm -rf node_modules package-lock.json
   npm install
   ```
2. 使用国内镜像：
   ```bash
   npm install --registry=https://registry.npmmirror.com
   ```

## 📖 系统架构

### Web 模式架构

```
┌─────────────┐
│  前端 Vue 3  │ ← 用户界面（上传、选词、查看）
└──────┬──────┘
       │ HTTP/REST API
┌──────▼──────┐
│  后端 Flask  │ ← 分析引擎、API 服务
└──────┬──────┘
       │
       ▼
   ┌─────┐
   │MySQL│ ← 数据存储
   └─────┘
```

### 工作流程

**Web 模式完整流程：**

1. 用户在前端选择 QQ 群聊 JSON 文件
2. 前端上传文件到后端
3. 后端分析文件并提取热词和统计数据
4. 分析完成后删除本地临时文件
5. 将分析结果保存到 MySQL 数据库，返回热词列表
6. 用户从热词列表中选择想要展示的词汇
7. 后端根据选词生成 AI 点评
8. 完整报告数据保存到数据库，前端动态渲染展示
9. 用户可随时查看历史报告

**命令行模式流程：**

1. 配置输入文件路径
2. 运行 main.py 脚本
3. 自动完成分析并生成报告文件
4. 报告保存在 runtime_outputs 目录

## 📊 生成的报告包含

- 📈 **基础统计**：消息总数、时间范围、参与人数
- 🔥 **年度热词**：群聊最热门的词汇（Web 模式支持自定义选择）
- 👑 **多维度排行榜**：
  - 发言量排行
  - 活跃度排行
  - 表情包达人
  - 夜猫子/早起人
  - 语音达人
  - 图片分享达人
- 📅 **时间分析**：活跃时段
- 🎭 **趣味统计**：最长发言、撤回次数等

## 💾 数据存储

### JSON 文件存储（默认）
- 数据存储在 `runtime_outputs/reports_db/` 目录
- 每个报告一个 JSON 文件
- 包含索引文件 `index.json` 用于快速查询
- 适合个人使用，数据易于备份和迁移

### MySQL 数据库存储（可选）
- 报告数据存储在 MySQL 数据库
- 支持高效查询和多用户访问
- 适合生产环境部署

## 🛠️ 技术栈

### Web 模式
- **后端**：Flask, pymysql, python-dotenv
- **前端**：Vue 3, Vite, Axios
- **数据库**：MySQL 5.7+

### 命令行模式
- Python 3.8+
- jieba（中文分词）
- Jinja2（模板引擎）
- Playwright（网页渲染）
- OpenAI API（可选）

## ⚙️ 详细配置

### 命令行模式配置（config.py）

```python
# 分词和词频统计
TOP_N = 200                    # 提取前 N 个高频词
MIN_FREQ = 1                   # 最小词频
MIN_WORD_LEN = 1              # 最小词长
MAX_WORD_LEN = 10             # 最大词长

# 新词发现参数
PMI_THRESHOLD = 2.0           # 互信息阈值
ENTROPY_THRESHOLD = 0.5       # 信息熵阈值
NEW_WORD_MIN_FREQ = 20        # 新词最小频次

# 词组合并参数
MERGE_MIN_FREQ = 30           # 合并最小频次
MERGE_MIN_PROB = 0.3          # 合并条件概率阈值

# AI 功能（需要配置 OpenAI API）
OPENAI_API_KEY = "sk-..."    # OpenAI API Key
OPENAI_MODEL = "gpt-3.5-turbo"  # 使用的模型
AI_COMMENT_MODE = 'ask'      # 'always', 'never', 'ask'

# 图片导出
ENABLE_IMAGE_EXPORT = True    # 是否导出图片
IMAGE_GENERATION_MODE = 'ask' # 图片生成模式
```

## 📝 开发计划

- [x] 基础词频统计
- [x] 新词发现算法
- [x] 多维度排行榜
- [x] HTML 报告生成
- [x] PNG 图片导出
- [x] AI 智能点评
- [x] Web 界面开发
- [x] 用户选词功能
- [x] 历史记录管理
- [x] 一键启动脚本
- [ ] 报告分享功能优化
- [ ] 数据可视化增强
- [ ] 移动端优化

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

AGPL-3.0 License

本项目采用 GNU Affero General Public License v3.0 开源协议。

**重要提示**：如果您修改本软件并通过网络提供服务，必须向用户提供修改后的完整源代码。

## 🙏 致谢

- [qq-chat-exporter](https://github.com/Yiyuery/qq-chat-exporter) - QQ 聊天记录导出工具
- [jieba](https://github.com/fxsjy/jieba) - 中文分词库

## 📮 联系方式

- GitHub: [@ZiHuixi](https://github.com/ZiHuixi) & [@Jingkun Yu](https://github.com/yujingkun1)
- 项目地址: https://github.com/ZiHuixi/QQgroup-annual-report-analyzer

## 🌟 Star History

如果这个项目对你有帮助，请给个 Star ⭐️

---

**注意**：本项目仅供学习和个人使用，请遵守相关法律法规和平台服务条款。
