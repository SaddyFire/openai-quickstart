# AI Agent 环境配置指南

## 项目信息
- **项目名称**: openai-quickstart (LangChain LCEL 课程项目)
- **项目路径**: `/Users/deer/workspace/jk_code/openai-quickstart`

## 环境配置

### Python 环境管理
- **包管理器**: uv (非 pip)
- **Python 版本**: 3.13.14
- **虚拟环境**: `.venv` (通过 conda + uv 创建)

### 激活环境命令
```bash
# 进入项目目录
cd /Users/deer/workspace/jk_code/openai-quickstart

# 激活虚拟环境（推荐方式）
source .venv/bin/activate

# 或者直接使用 .venv/bin/python 运行脚本
.venv/bin/python your_script.py
```

### 安装/更新依赖
```bash
# 使用 uv 安装依赖（不要使用 pip）
uv pip install -r requirements.txt

# 安装单个包
uv pip install package_name
```

## API 配置

### DeepSeek API（主要使用）
- **API Key**: 配置在 `.env` 文件中 (`DEEPSEEK_API_KEY`)
- **Base URL**: `https://api.deepseek.com`
- **模型**: `deepseek-chat`

### .env 文件配置
```env
DEEPSEEK_API_KEY=sk-xxx
DEEPSEEK_BASE_URL=https://api.deepseek.com
```

## 重要注意事项

### 代理配置
- 项目使用 socks5 代理 (`http://127.0.0.1:7892`)
- **禁止**设置 `all_proxy` 或 `ALL_PROXY` 环境变量，会导致 httpx 报错 `missing socksio`
- 如需代理，请使用 `https_proxy` 或 `HTTPS_PROXY`

### 环境变量继承问题
- VS Code 的 Jupyter 内核**不会**继承 `.zshrc` 中的环境变量
- `.env` 文件中的变量会被 `dotenv` 正确加载
- 如果需要在 Jupyter 中使用环境变量，请在 notebook 中显式加载

### HuggingFace 模型离线加载
- 模型已缓存在本地: `~/.cache/huggingface/hub/`
- 使用离线模式避免网络请求:
```python
os.environ["HF_HUB_OFFLINE"] = "1"
os.environ["TRANSFORMERS_OFFLINE"] = "1"
model_path = os.path.expanduser("~/.cache/huggingface/hub/models--shibing624--text2vec-base-chinese/snapshots/183bb99aa7af74355fb58d16edf8c13ae7c5433e")
```

### Embedding 模型
- **模型名称**: `shibing624/text2vec-base-chinese`
- **维度**: 768
- **用途**: 中文文本向量化

## 依赖包说明

### 核心包
- `langchain` / `langchain-core` / `langchain-openai`: LangChain 框架
- `langchain-huggingface`: HuggingFace 集成
- `chromadb`: 向量数据库
- `sentence-transformers`: 文本嵌入模型

### 版本兼容性注意
- `langchain-core` 已升级到 0.3.86 (兼容 `langchain-huggingface==0.3.1`)
- 如遇版本冲突，优先升级 `langchain-core`

## 运行 Notebook

### 推荐方式
1. 在 VS Code 中打开 `.ipynb` 文件
2. 选择 Python 解释器: `.venv/bin/python`
3. 重启内核并运行所有单元格 (Restart & Run All)

### 常见问题排查
- **httpx 报错**: 检查是否设置了 `all_proxy` 环境变量
- **模型加载失败**: 确认离线模式环境变量已设置
- **ImportError**: 运行 `uv pip install -r requirements.txt` 更新依赖
