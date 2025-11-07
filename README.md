# Anthropic API Proxy for Gemini & OpenAI Models 🔄

**Use Anthropic clients (like Claude Code) with Gemini, OpenAI, or direct Anthropic backends.** 🤝

A proxy server that lets you use Anthropic clients with Gemini, OpenAI, or Anthropic models themselves (a transparent proxy of sorts), all via LiteLLM. 🌉


![Anthropic API Proxy](pic.png)

## Quick Start ⚡

### Prerequisites

- OpenAI API key 🔑
- Google AI Studio (Gemini) API key (if using Google provider) 🔑
- [uv](https://github.com/astral-sh/uv) installed.

### Setup 🛠️

#### From source

1. **Clone this repository**:
   ```bash
   git clone https://github.com/hangts/claude-code-proxy.git
   cd claude-code-proxy
   ```

2. **Install uv** (if you haven't already):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```
   *(`uv` will handle dependencies based on `pyproject.toml` when you run the server)*

3. **Configure Environment Variables**:
   Copy the example environment file:
   ```bash
   cp .env.example .env
   ```
   Edit `.env` and fill in your API keys and model configurations:

   - ONEAPI_BASE_URL: (Optional) ONEAPI URL, default: http://oneapi.blockbeat.hk/v1
   - SAVING_MODEL: (Optional) Claude will automatically call the haiku (claude-haiku-4-5-20251001) model to perform simple tasks; replace it here, default: deepseek/deepseek-v3.2-exp
   - HEIGHT_MODEL: (Optional) Claude will automatically call the sonnet (claude-sonnet-4-5-20250929) model to perform tasks; replace it here, default: anthropic/claude-sonnet-4.5

4. **Run the server**:
   ```bash
   uv run uvicorn server:app --host 0.0.0.0 --port 8082 --reload
   ```
   *(`--reload` is optional, for development)*

### Using with Claude Code 🎮

1. **Install Claude Code** (if you haven't already):
   ```bash
   npm install -g @anthropic-ai/claude-code
   ```

2. **Connect to your proxy**:
   ```bash
   ANTHROPIC_BASE_URL=http://localhost:8082 claude
   ```
3. 配置 claude code settings:
   ```json
   {
      "env": {
         "ANTHROPIC_BASE_URL": "your proxy url",
         "ANTHROPIC_AUTH_TOKEN": "your oneapi auth token",
         "ANTHROPIC_MODEL": "oneapi/{your model name}",
         "ANTHROPIC_SMALL_FAST_MODEL": "oneapi/{your model name}",
         "API_TIMEOUT_MS": "3000000",
         "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "I"
      }
   }
   ```

3. **That's it!** Your Claude Code client will now use the configured backend models (defaulting to Gemini) through the proxy. 🎯


## How It Works 🧩

This proxy works by:

1. **Receiving requests** in Anthropic's API format 📥
2. **Translating** the requests to OpenAI format via LiteLLM 🔄
3. **Sending** the translated request to OpenAI 📤
4. **Converting** the response back to Anthropic format 🔄
5. **Returning** the formatted response to the client ✅

The proxy handles both streaming and non-streaming responses, maintaining compatibility with all Claude clients. 🌊

## Contributing 🤝

Contributions are welcome! Please feel free to submit a Pull Request. 🎁
