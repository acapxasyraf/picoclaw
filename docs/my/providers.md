# 🔌 Konfigurasi Penyedia & Model

> Kembali ke [README](../../README.my.md)

### Penyedia

> [!NOTE]
> Groq menyediakan transkripsi suara percuma melalui Whisper. Jika dikonfigurasikan, mesej audio daripada mana-mana saluran akan ditranskripsikan secara automatik pada peringkat agen.

| Provider     | Tujuan                                | Dapatkan API Key                                                                                                                                               |
| ------------ | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `gemini`     | LLM (Gemini langsung)                 | [aistudio.google.com](https://aistudio.google.com)                                                                                                             |
| `zhipu`      | LLM (Zhipu langsung)                  | [bigmodel.cn](https://bigmodel.cn)                                                                                                                             |
| `volcengine` | LLM (Volcengine langsung)             | [volcengine.com](https://www.volcengine.com/activity/codingplan?utm_campaign=PicoClaw&utm_content=PicoClaw&utm_medium=devrel&utm_source=OWO&utm_term=PicoClaw) |
| `openrouter` | LLM (disyorkan, akses ke semua model) | [openrouter.ai](https://openrouter.ai)                                                                                                                         |
| `anthropic`  | LLM (Claude langsung)                 | [console.anthropic.com](https://console.anthropic.com)                                                                                                         |
| `openai`     | LLM (GPT langsung)                    | [platform.openai.com](https://platform.openai.com)                                                                                                             |
| `deepseek`   | LLM (DeepSeek langsung)               | [platform.deepseek.com](https://platform.deepseek.com)                                                                                                         |
| `qwen`       | LLM (Qwen langsung)                   | [dashscope.console.aliyun.com](https://dashscope.console.aliyun.com)                                                                                           |
| `groq`       | LLM + **Transkripsi suara** (Whisper) | [console.groq.com](https://console.groq.com)                                                                                                                   |
| `cerebras`   | LLM (Cerebras langsung)               | [cerebras.ai](https://cerebras.ai)                                                                                                                             |
| `vivgrid`    | LLM (Vivgrid langsung)                | [vivgrid.com](https://vivgrid.com)                                                                                                                             |
| `nvidia`     | LLM (NVIDIA NIM)                      | [build.nvidia.com](https://build.nvidia.com)                                                                                                                   |
| `moonshot`   | LLM (Kimi/Moonshot langsung)          | [platform.moonshot.cn](https://platform.moonshot.cn)                                                                                                           |
| `minimax`    | LLM (Minimax langsung)                | [platform.minimaxi.com](https://platform.minimaxi.com)                                                                                                         |
| `avian`      | LLM (Avian langsung)                  | [avian.io](https://avian.io)                                                                                                                                   |
| `mistral`    | LLM (Mistral langsung)                | [console.mistral.ai](https://console.mistral.ai)                                                                                                               |
| `longcat`    | LLM (Longcat langsung)                | [longcat.ai](https://longcat.ai)                                                                                                                               |
| `modelscope` | LLM (ModelScope langsung)             | [modelscope.cn](https://modelscope.cn)                                                                                                                         |

### Konfigurasi Model (`model_list`)

> **Apa yang Baharu?** PicoClaw kini menggunakan pendekatan konfigurasi yang **berpusatkan model**. Anda hanya perlu menentukan format `vendor/model` (contohnya `zhipu/glm-4.7`) untuk menambah penyedia baharu — **tanpa perubahan kod diperlukan!**

Reka bentuk ini juga membolehkan **sokongan multi-agent** dengan pemilihan penyedia yang fleksibel:

- **Agen berbeza, penyedia berbeza**: Setiap agen boleh menggunakan penyedia LLM sendiri
- **Fallback model**: Konfigurasi model utama dan sandaran untuk ketahanan
- **Load balancing**: Agihkan permintaan merentas berbilang endpoint
- **Konfigurasi berpusat**: Urus semua penyedia di satu tempat

#### 📋 Semua Vendor yang Disokong

| Vendor                  | Prefiks `model`   | API Base Lalai                                      | Protokol  | API Key                                                                                                                                                 |
| ----------------------- | ----------------- | --------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **OpenAI**              | `openai/`         | `https://api.openai.com/v1`                         | OpenAI    | [Get Key](https://platform.openai.com)                                                                                                                  |
| **Anthropic**           | `anthropic/`      | `https://api.anthropic.com/v1`                      | Anthropic | [Get Key](https://console.anthropic.com)                                                                                                                |
| **智谱 AI (GLM)**       | `zhipu/`          | `https://open.bigmodel.cn/api/paas/v4`              | OpenAI    | [Get Key](https://open.bigmodel.cn/usercenter/proj-mgmt/apikeys)                                                                                        |
| **DeepSeek**            | `deepseek/`       | `https://api.deepseek.com/v1`                       | OpenAI    | [Get Key](https://platform.deepseek.com)                                                                                                                |
| **Google Gemini**       | `gemini/`         | `https://generativelanguage.googleapis.com/v1beta`  | OpenAI    | [Get Key](https://aistudio.google.com/api-keys)                                                                                                         |
| **Groq**                | `groq/`           | `https://api.groq.com/openai/v1`                    | OpenAI    | [Get Key](https://console.groq.com)                                                                                                                     |
| **Moonshot**            | `moonshot/`       | `https://api.moonshot.cn/v1`                        | OpenAI    | [Get Key](https://platform.moonshot.cn)                                                                                                                 |
| **通义千问 (Qwen)**     | `qwen/`           | `https://dashscope.aliyuncs.com/compatible-mode/v1` | OpenAI    | [Get Key](https://dashscope.console.aliyun.com)                                                                                                         |
| **NVIDIA**              | `nvidia/`         | `https://integrate.api.nvidia.com/v1`               | OpenAI    | [Get Key](https://build.nvidia.com)                                                                                                                     |
| **Ollama**              | `ollama/`         | `http://localhost:11434/v1`                         | OpenAI    | Tempatan (tidak perlu key)                                                                                                                              |
| **OpenRouter**          | `openrouter/`     | `https://openrouter.ai/api/v1`                      | OpenAI    | [Get Key](https://openrouter.ai/keys)                                                                                                                   |
| **LiteLLM Proxy**       | `litellm/`        | `http://localhost:4000/v1`                          | OpenAI    | Key proksi LiteLLM anda                                                                                                                                 |
| **VLLM**                | `vllm/`           | `http://localhost:8000/v1`                          | OpenAI    | Tempatan                                                                                                                                                |
| **Cerebras**            | `cerebras/`       | `https://api.cerebras.ai/v1`                        | OpenAI    | [Get Key](https://cerebras.ai)                                                                                                                          |
| **VolcEngine (Doubao)** | `volcengine/`     | `https://ark.cn-beijing.volces.com/api/v3`          | OpenAI    | [Get Key](https://www.volcengine.com/activity/codingplan?utm_campaign=PicoClaw&utm_content=PicoClaw&utm_medium=devrel&utm_source=OWO&utm_term=PicoClaw) |
| **神算云**              | `shengsuanyun/`   | `https://router.shengsuanyun.com/api/v1`            | OpenAI    | -                                                                                                                                                       |
| **BytePlus**            | `byteplus/`       | `https://ark.ap-southeast.bytepluses.com/api/v3`    | OpenAI    | [Get Key](https://www.byteplus.com)                                                                                                                     |
| **Vivgrid**             | `vivgrid/`        | `https://api.vivgrid.com/v1`                        | OpenAI    | [Get Key](https://vivgrid.com)                                                                                                                          |
| **LongCat**             | `longcat/`        | `https://api.longcat.chat/openai`                   | OpenAI    | [Get Key](https://longcat.chat/platform)                                                                                                                |
| **ModelScope (魔搭)**   | `modelscope/`     | `https://api-inference.modelscope.cn/v1`            | OpenAI    | [Get Token](https://modelscope.cn/my/tokens)                                                                                                            |
| **Azure OpenAI**        | `azure/`          | `https://{resource}.openai.azure.com`               | Azure     | [Get Key](https://portal.azure.com)                                                                                                                     |
| **Antigravity**         | `antigravity/`    | Google Cloud                                        | Custom    | OAuth sahaja                                                                                                                                            |
| **GitHub Copilot**      | `github-copilot/` | `localhost:4321`                                    | gRPC      | -                                                                                                                                                       |

#### Konfigurasi Asas

```json
{
  "model_list": [
    {
      "model_name": "ark-code-latest",
      "model": "volcengine/ark-code-latest",
      "api_key": "sk-your-api-key"
    },
    {
      "model_name": "gpt-5.4",
      "model": "openai/gpt-5.4",
      "api_key": "sk-your-openai-key"
    },
    {
      "model_name": "claude-sonnet-4.6",
      "model": "anthropic/claude-sonnet-4.6",
      "api_key": "sk-ant-your-key"
    },
    {
      "model_name": "glm-4.7",
      "model": "zhipu/glm-4.7",
      "api_key": "your-zhipu-key"
    }
  ],
  "agents": {
    "defaults": {
      "model": "gpt-5.4"
    }
  }
}
```

#### Contoh Khusus Vendor

**OpenAI**

```json
{
  "model_name": "gpt-5.4",
  "model": "openai/gpt-5.4",
  "api_key": "sk-..."
}
```

**VolcEngine (Doubao)**

```json
{
  "model_name": "ark-code-latest",
  "model": "volcengine/ark-code-latest",
  "api_key": "sk-..."
}
```

**智谱 AI (GLM)**

```json
{
  "model_name": "glm-4.7",
  "model": "zhipu/glm-4.7",
  "api_key": "your-key"
}
```

**DeepSeek**

```json
{
  "model_name": "deepseek-chat",
  "model": "deepseek/deepseek-chat",
  "api_key": "sk-..."
}
```

**Anthropic (dengan API key)**

```json
{
  "model_name": "claude-sonnet-4.6",
  "model": "anthropic/claude-sonnet-4.6",
  "api_key": "sk-ant-your-key"
}
```

> Jalankan `picoclaw auth login --provider anthropic` untuk menampal token API anda.

**Anthropic Messages API (format asli)**

Untuk akses API Anthropic secara terus atau endpoint tersuai yang hanya menyokong format mesej asli Anthropic:

```json
{
  "model_name": "claude-opus-4-6",
  "model": "anthropic-messages/claude-opus-4-6",
  "api_key": "sk-ant-your-key",
  "api_base": "https://api.anthropic.com"
}
```

> Gunakan protokol `anthropic-messages` apabila:
> - Menggunakan proksi pihak ketiga yang hanya menyokong endpoint asli Anthropic `/v1/messages` (bukan `/v1/chat/completions` yang serasi OpenAI)
> - Menyambung ke perkhidmatan seperti MiniMax, Synthetic yang memerlukan format mesej asli Anthropic
> - Protokol `anthropic` sedia ada memulangkan ralat 404 (menunjukkan endpoint tidak menyokong format serasi OpenAI)
>
> **Nota:** Protokol `anthropic` menggunakan format serasi OpenAI (`/v1/chat/completions`), manakala `anthropic-messages` menggunakan format asli Anthropic (`/v1/messages`). Pilih mengikut format yang disokong oleh endpoint anda.

**Ollama (tempatan)**

```json
{
  "model_name": "llama3",
  "model": "ollama/llama3"
}
```

**Proksi/API Tersuai**

```json
{
  "model_name": "my-custom-model",
  "model": "openai/custom-model",
  "api_base": "https://my-proxy.com/v1",
  "api_key": "sk-...",
  "request_timeout": 300
}
```

**LiteLLM Proxy**

```json
{
  "model_name": "lite-gpt4",
  "model": "litellm/lite-gpt4",
  "api_base": "http://localhost:4000/v1",
  "api_key": "sk-..."
}
```

PicoClaw hanya menanggalkan prefiks luar `litellm/` sebelum menghantar permintaan, jadi alias proksi seperti `litellm/lite-gpt4` akan menghantar `lite-gpt4`, manakala `litellm/openai/gpt-4o` akan menghantar `openai/gpt-4o`.

#### Load Balancing

Konfigurasikan berbilang endpoint untuk nama model yang sama — PicoClaw akan melakukan round-robin secara automatik antara endpoint tersebut:

```json
{
  "model_list": [
    {
      "model_name": "gpt-5.4",
      "model": "openai/gpt-5.4",
      "api_base": "https://api1.example.com/v1",
      "api_key": "sk-key1"
    },
    {
      "model_name": "gpt-5.4",
      "model": "openai/gpt-5.4",
      "api_base": "https://api2.example.com/v1",
      "api_key": "sk-key2"
    }
  ]
}
```

#### Migrasi daripada Konfigurasi `providers` Lama

Konfigurasi `providers` lama kini **deprecated** tetapi masih disokong untuk keserasian ke belakang.

**Config Lama (deprecated):**

```json
{
  "providers": {
    "zhipu": {
      "api_key": "your-key",
      "api_base": "https://open.bigmodel.cn/api/paas/v4"
    }
  },
  "agents": {
    "defaults": {
      "provider": "zhipu",
      "model": "glm-4.7"
    }
  }
}
```

**Config Baharu (disyorkan):**

```json
{
  "model_list": [
    {
      "model_name": "glm-4.7",
      "model": "zhipu/glm-4.7",
      "api_key": "your-key"
    }
  ],
  "agents": {
    "defaults": {
      "model": "glm-4.7"
    }
  }
}
```

Untuk panduan migrasi terperinci, lihat [docs/migration/model-list-migration.md](docs/migration/model-list-migration.md).

### Seni Bina Penyedia

PicoClaw menghala penyedia mengikut keluarga protokol:

- Protokol serasi OpenAI: OpenRouter, gateway yang serasi OpenAI, Groq, Zhipu, dan endpoint gaya vLLM.
- Protokol Anthropic: Tingkah laku API asli Claude.
- Laluan Codex/OAuth: Laluan autentikasi token/OAuth OpenAI.

Ini memastikan runtime kekal ringan sambil menjadikan kebanyakan backend baharu yang serasi OpenAI hampir sepenuhnya bergantung kepada konfigurasi (`api_base` + `api_key`).

<details>
<summary><b>Zhipu</b></summary>

**1. Dapatkan API key dan base URL**

* Dapatkan [API key](https://bigmodel.cn/usercenter/proj-mgmt/apikeys)

**2. Konfigurasi**

```json
{
  "agents": {
    "defaults": {
      "workspace": "~/.picoclaw/workspace",
      "model": "glm-4.7",
      "max_tokens": 8192,
      "temperature": 0.7,
      "max_tool_iterations": 20
    }
  },
  "providers": {
    "zhipu": {
      "api_key": "Your API Key",
      "api_base": "https://open.bigmodel.cn/api/paas/v4"
    }
  }
}
```

**3. Jalankan**

```bash
picoclaw agent -m "Hello"
```

</details>

<details>
<summary><b>Contoh konfigurasi penuh</b></summary>

```json
{
  "agents": {
    "defaults": {
      "model": "anthropic/claude-opus-4-5"
    }
  },
  "session": {
    "dm_scope": "per-channel-peer",
    "backlog_limit": 20
  },
  "providers": {
    "openrouter": {
      "api_key": "sk-or-v1-xxx"
    },
    "groq": {
      "api_key": "gsk_xxx"
    }
  },
  "channels": {
    "telegram": {
      "enabled": true,
      "token": "123456:ABC...",
      "allow_from": ["123456789"]
    },
    "discord": {
      "enabled": true,
      "token": "",
      "allow_from": [""]
    },
    "whatsapp": {
      "enabled": false,
      "bridge_url": "ws://localhost:3001",
      "use_native": false,
      "session_store_path": "",
      "allow_from": []
    },
    "feishu": {
      "enabled": false,
      "app_id": "cli_xxx",
      "app_secret": "xxx",
      "encrypt_key": "",
      "verification_token": "",
      "allow_from": []
    },
    "qq": {
      "enabled": false,
      "app_id": "",
      "app_secret": "",
      "allow_from": []
    }
  },
  "tools": {
    "web": {
      "brave": {
        "enabled": false,
        "api_key": "BSA...",
        "max_results": 5
      },
      "duckduckgo": {
        "enabled": true,
        "max_results": 5
      },
      "perplexity": {
        "enabled": false,
        "api_key": "",
        "max_results": 5
      },
      "searxng": {
        "enabled": false,
        "base_url": "http://localhost:8888",
        "max_results": 5
      }
    },
    "cron": {
      "exec_timeout_minutes": 5
    }
  },
  "heartbeat": {
    "enabled": true,
    "interval": 30
  }
}
```

</details>

---

## 📝 Perbandingan API Key

| Perkhidmatan              | Harga                           | Kegunaan                                                                   |
| ------------------------- | ------------------------------- | -------------------------------------------------------------------------- |
| **OpenRouter**            | Percuma: 200K token/bulan       | Pelbagai model (Claude, GPT-4, dll.)                                       |
| **Volcengine CodingPlan** | ¥9.9/bulan pertama              | Terbaik untuk pengguna China, pelbagai model SOTA (Doubao, DeepSeek, dll.) |
| **Zhipu**                 | Percuma: 200K token/bulan       | Sesuai untuk pengguna China                                                |
| **Brave Search**          | $5/1000 pertanyaan              | Fungsi carian web                                                          |
| **SearXNG**               | Percuma (self-hosted)           | Metacarian fokus privasi (70+ enjin)                                       |
| **Groq**                  | Tier percuma tersedia           | Inferens pantas (Llama, Mixtral)                                           |
| **Cerebras**              | Tier percuma tersedia           | Inferens pantas (Llama, Qwen, dll.)                                        |
| **LongCat**               | Percuma: sehingga 5M token/hari | Inferens pantas                                                            |
| **ModelScope**            | Percuma: 2000 permintaan/hari   | Inferens (Qwen, GLM, DeepSeek, dll.)                                       |

---

<div align="center">
  <img src="assets/logo.jpg" alt="PicoClaw Meme" width="512">
</div>
