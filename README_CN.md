# LLM 委员会

![llmcouncil](header.jpg)

这个项目的核心理念是：与其向单个 LLM 提供商（如 OpenAI GPT 5.1、Google Gemini 3.0 Pro、Anthropic Claude Sonnet 4.5、xAI Grok 4 等）提问，不如将它们组成"LLM 委员会"。本项目是一个简单的本地 Web 应用，界面类似 ChatGPT，但它使用 OpenRouter 将您的查询发送给多个 LLM，然后让它们互相评审和排名，最后由主席 LLM 生成最终回答。

更详细地说，当您提交一个查询时会发生以下过程：

1. **阶段 1：初步意见**。用户查询被单独发送给所有 LLM，并收集它们的回答。这些单独的回答会以"标签页"形式展示，用户可以逐一查看。
2. **阶段 2：交叉评审**。每个 LLM 会收到其他 LLM 的回答。在后台，LLM 的身份会被匿名化，这样它们在评判输出时不会有偏袒。LLM 会被要求根据准确性和洞察力对这些回答进行排名。
3. **阶段 3：最终回答**。指定的 LLM 委员会主席会综合所有模型的回答，编译成一个最终答案呈现给用户。

## 开发说明

这个项目 99% 是作为周六的趣味编程实验快速开发的，因为我想在[与 LLM 一起阅读书籍](https://x.com/karpathy/status/1990577951671509438)的过程中并排探索和评估多个 LLM。能够并排查看多个回答，以及所有 LLM 对彼此输出的交叉意见，非常有用。我不会以任何方式提供支持，这里仅供其他人参考和启发，我不打算改进它。现在代码是短暂的，库已经过时了，让您的 LLM 以任何您喜欢的方式修改它吧。

## 设置指南

### 1. 安装依赖

本项目使用 [uv](https://docs.astral.sh/uv/) 进行项目管理。

**后端：**
```bash
uv sync
```

**前端：**
```bash
cd frontend
npm install
cd ..
```

### 2. 配置 API 密钥

在项目根目录创建 `.env` 文件：

```bash
OPENROUTER_API_KEY=sk-or-v1-...
```

在 [openrouter.ai](https://openrouter.ai/) 获取您的 API 密钥。确保购买所需的积分，或注册自动充值。

### 3. 配置模型（可选）

编辑 `backend/config.py` 来自定义委员会：

```python
COUNCIL_MODELS = [
    "openai/gpt-5.1",
    "google/gemini-3-pro-preview",
    "anthropic/claude-sonnet-4.5",
    "x-ai/grok-4",
]

CHAIRMAN_MODEL = "google/gemini-3-pro-preview"
```

## 运行应用

**方式 1：使用启动脚本**
```bash
./start.sh
```

**方式 2：手动运行**

终端 1（后端）：
```bash
uv run python -m backend.main
```

终端 2（前端）：
```bash
cd frontend
npm run dev
```

然后在浏览器中打开 http://localhost:5173。

## 技术栈

- **后端：** FastAPI（Python 3.10+）、async httpx、OpenRouter API
- **前端：** React + Vite、react-markdown 用于渲染
- **存储：** JSON 文件存储在 `data/conversations/`
- **包管理：** Python 使用 uv，JavaScript 使用 npm
