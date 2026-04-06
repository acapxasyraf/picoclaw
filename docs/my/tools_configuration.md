# Konfigurasi Tools

PicoClaw menyimpan konfigurasi tools dalam medan `tools` di `config.json`.

## Struktur Direktori

```json
{
  "tools": {
    "web": {
      ...
    },
    "mcp": {
      ...
    },
    "exec": {
      ...
    },
    "cron": {
      ...
    },
    "skills": {
      ...
    }
  }
}
```

## Web Tools

Web tools digunakan untuk carian web dan mengambil kandungan laman.

### Web Fetcher
Tetapan umum untuk mengambil dan memproses kandungan laman web.

| Config              | Type   | Default     | Description                                                                             |
| ------------------- | ------ | ----------- | --------------------------------------------------------------------------------------- |
| `enabled`           | bool   | true        | Aktifkan keupayaan mengambil kandungan laman web.                                       |
| `fetch_limit_bytes` | int    | 10485760    | Saiz maksimum payload laman web yang akan diambil, dalam bait (lalai 10MB).             |
| `format`            | string | "plaintext" | Format output kandungan yang diambil. Pilihan: `plaintext` atau `markdown` (disyorkan). |

### Brave

| Config        | Type   | Default | Description             |
| ------------- | ------ | ------- | ----------------------- |
| `enabled`     | bool   | false   | Aktifkan carian Brave   |
| `api_key`     | string | -       | Brave Search API key    |
| `max_results` | int    | 5       | Bilangan maksimum hasil |

### DuckDuckGo

| Config        | Type | Default | Description                |
| ------------- | ---- | ------- | -------------------------- |
| `enabled`     | bool | true    | Aktifkan carian DuckDuckGo |
| `max_results` | int  | 5       | Bilangan maksimum hasil    |

### Perplexity

| Config        | Type   | Default | Description                |
| ------------- | ------ | ------- | -------------------------- |
| `enabled`     | bool   | false   | Aktifkan carian Perplexity |
| `api_key`     | string | -       | Perplexity API key         |
| `max_results` | int    | 5       | Bilangan maksimum hasil    |

## Exec Tool

Tool exec digunakan untuk melaksanakan arahan shell.

| Config                 | Type  | Default | Description                             |
| ---------------------- | ----- | ------- | --------------------------------------- |
| `enable_deny_patterns` | bool  | true    | Aktifkan sekatan arahan berbahaya lalai |
| `custom_deny_patterns` | array | []      | Corak deny tersuai (ungkapan biasa)     |

### Fungsi

- **`enable_deny_patterns`**: Tetapkan kepada `false` untuk menyahaktifkan sepenuhnya corak sekatan arahan berbahaya lalai
- **`custom_deny_patterns`**: Tambah corak regex deny tersuai; arahan yang sepadan akan disekat

### Corak Arahan Disekat Lalai

Secara lalai, PicoClaw menyekat arahan berbahaya berikut:

- Arahan padam: `rm -rf`, `del /f/q`, `rmdir /s`
- Operasi cakera: `format`, `mkfs`, `diskpart`, `dd if=`, menulis ke `/dev/sd*`
- Operasi sistem: `shutdown`, `reboot`, `poweroff`
- Command substitution: `$()`, `${}`, backticks
- Pipe ke shell: `| sh`, `| bash`
- Peningkatan keistimewaan: `sudo`, `chmod`, `chown`
- Kawalan proses: `pkill`, `killall`, `kill -9`
- Operasi jauh: `curl | sh`, `wget | sh`, `ssh`
- Pengurusan pakej: `apt`, `yum`, `dnf`, `npm install -g`, `pip install --user`
- Container: `docker run`, `docker exec`
- Git: `git push`, `git force`
- Lain-lain: `eval`, `source *.sh`

### Had Seni Bina yang Diketahui

Pengawal exec hanya mengesahkan arahan peringkat atas yang dihantar ke PicoClaw. Ia **tidak** memeriksa secara rekursif proses anak yang dilancarkan oleh build tool atau skrip selepas arahan itu bermula.

Contoh aliran kerja yang boleh melepasi pengawal arahan terus selepas arahan awal dibenarkan:

- `make run`
- `go run ./cmd/...`
- `cargo run`
- `npm run build`

Ini bermaksud pengawal berguna untuk menyekat arahan terus yang jelas berbahaya, tetapi ia **bukan** sandbox penuh untuk pipeline build yang tidak disemak. Jika model ancaman anda merangkumi kod tidak dipercayai dalam workspace, gunakan pengasingan yang lebih kuat seperti container, VM, atau aliran kelulusan sekitar arahan build-and-run.

### Contoh Konfigurasi

```json
{
  "tools": {
    "exec": {
      "enable_deny_patterns": true,
      "custom_deny_patterns": [
        "\\brm\\s+-r\\b",
        "\\bkillall\\s+python"
      ]
    }
  }
}
```

## Cron Tool

Tool cron digunakan untuk menjadualkan tugasan berkala.

| Config                 | Type | Default | Description                                            |
| ---------------------- | ---- | ------- | ------------------------------------------------------ |
| `exec_timeout_minutes` | int  | 5       | Timeout pelaksanaan dalam minit, 0 bermaksud tiada had |

## MCP Tool

Tool MCP membolehkan integrasi dengan pelayan Model Context Protocol luaran.

### Tool Discovery (Lazy Loading)

Apabila menyambung ke berbilang pelayan MCP, mendedahkan ratusan tools secara serentak boleh menghabiskan context window LLM dan meningkatkan kos API. Ciri **Discovery** menyelesaikan perkara ini dengan mengekalkan tools MCP sebagai *tersembunyi* secara lalai.

Daripada memuatkan semua tools, LLM dibekalkan dengan tool carian ringan (menggunakan padanan kata kunci BM25 atau Regex). Apabila LLM memerlukan keupayaan tertentu, ia akan mencari dalam pustaka tersembunyi tersebut. Tools yang sepadan kemudiannya akan "dibuka" secara sementara dan disuntik ke dalam konteks untuk beberapa pusingan yang dikonfigurasikan (`ttl`).

### Konfigurasi Global

| Config      | Type   | Default | Description                                       |
| ----------- | ------ | ------- | ------------------------------------------------- |
| `enabled`   | bool   | false   | Aktifkan integrasi MCP secara global              |
| `discovery` | object | `{}`    | Konfigurasi untuk Tool Discovery (lihat di bawah) |
| `servers`   | object | `{}`    | Peta nama pelayan kepada config pelayan           |

### Discovery Config (`discovery`)

| Config               | Type | Default | Description                                                                                                                                       |
| -------------------- | ---- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `enabled`            | bool | false   | Jika true, tools MCP disembunyikan dan dimuat atas permintaan melalui carian. Jika false, semua tools dimuatkan                                   |
| `ttl`                | int  | 5       | Bilangan pusingan perbualan sesuatu tool yang ditemui kekal dibuka                                                                                |
| `max_search_results` | int  | 5       | Bilangan maksimum tools yang dipulangkan bagi setiap pertanyaan carian                                                                            |
| `use_bm25`           | bool | true    | Aktifkan tool carian bahasa semula jadi/kata kunci (`tool_search_tool_bm25`). **Amaran**: menggunakan lebih banyak sumber berbanding carian regex |
| `use_regex`          | bool | false   | Aktifkan tool carian corak regex (`tool_search_tool_regex`)                                                                                       |

> **Nota:** Jika `discovery.enabled` ialah `true`, anda MESTI mengaktifkan sekurang-kurangnya satu enjin carian (`use_bm25` atau `use_regex`), jika tidak aplikasi akan gagal bermula.

### Konfigurasi Per-Pelayan

| Config     | Type   | Required | Description                                   |
| ---------- | ------ | -------- | --------------------------------------------- |
| `enabled`  | bool   | ya       | Aktifkan pelayan MCP ini                      |
| `type`     | string | no       | Jenis pengangkutan: `stdio`, `sse`, `http`    |
| `command`  | string | stdio    | Arahan boleh laksana untuk pengangkutan stdio |
| `args`     | array  | no       | Argumen arahan untuk pengangkutan stdio       |
| `env`      | object | no       | Pemboleh ubah persekitaran untuk proses stdio |
| `env_file` | string | no       | Laluan fail persekitaran untuk proses stdio   |
| `url`      | string | sse/http | URL endpoint untuk pengangkutan `sse`/`http`  |
| `headers`  | object | no       | Header HTTP untuk pengangkutan `sse`/`http`   |

### Tingkah Laku Pengangkutan

- Jika `type` diabaikan, pengangkutan akan dikesan secara automatik:
    - `url` ditetapkan → `sse`
    - `command` ditetapkan → `stdio`
- `http` dan `sse` kedua-duanya menggunakan `url` + `headers` pilihan.
- `env` dan `env_file` hanya digunakan untuk pelayan `stdio`.

### Contoh Konfigurasi

#### 1) Pelayan stdio MCP

```json
{
  "tools": {
    "mcp": {
      "enabled": true,
      "servers": {
        "filesystem": {
          "enabled": true,
          "command": "npx",
          "args": [
            "-y",
            "@modelcontextprotocol/server-filesystem",
            "/tmp"
          ]
        }
      }
    }
  }
}
```

#### 2) Pelayan MCP SSE/HTTP jauh

```json
{
  "tools": {
    "mcp": {
      "enabled": true,
      "servers": {
        "remote-mcp": {
          "enabled": true,
          "type": "sse",
          "url": "https://example.com/mcp",
          "headers": {
            "Authorization": "Bearer YOUR_TOKEN"
          }
        }
      }
    }
  }
}
```

#### 3) Setup MCP besar dengan Tool Discovery diaktifkan

*Dalam contoh ini, LLM hanya akan melihat `tool_search_tool_bm25`. Ia akan mencari dan membuka tools Github atau Postgres secara dinamik hanya apabila diminta oleh pengguna.*

```json
{
  "tools": {
    "mcp": {
      "enabled": true,
      "discovery": {
        "enabled": true,
        "ttl": 5,
        "max_search_results": 5,
        "use_bm25": true,
        "use_regex": false
      },
      "servers": {
        "github": {
          "enabled": true,
          "command": "npx",
          "args": [
            "-y",
            "@modelcontextprotocol/server-github"
          ],
          "env": {
            "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_GITHUB_TOKEN"
          }
        },
        "postgres": {
          "enabled": true,
          "command": "npx",
          "args": [
            "-y",
            "@modelcontextprotocol/server-postgres",
            "postgresql://user:password@localhost/dbname"
          ]
        },
        "slack": {
          "enabled": true,
          "command": "npx",
          "args": [
            "-y",
            "@modelcontextprotocol/server-slack"
          ],
          "env": {
            "SLACK_BOT_TOKEN": "YOUR_SLACK_BOT_TOKEN",
            "SLACK_TEAM_ID": "YOUR_SLACK_TEAM_ID"
          }
        }
      }
    }
  }
}
```

## Skills Tool

Tool skills mengkonfigurasi penemuan dan pemasangan skill melalui registry seperti ClawHub.

### Registri

| Config                             | Type   | Default              | Description                                       |
| ---------------------------------- | ------ | -------------------- | ------------------------------------------------- |
| `registries.clawhub.enabled`       | bool   | true                 | Aktifkan registry ClawHub                         |
| `registries.clawhub.base_url`      | string | `https://clawhub.ai` | URL asas ClawHub                                  |
| `registries.clawhub.auth_token`    | string | `""`                 | Bearer token pilihan untuk had kadar lebih tinggi |
| `registries.clawhub.search_path`   | string | `/api/v1/search`     | Laluan API carian                                 |
| `registries.clawhub.skills_path`   | string | `/api/v1/skills`     | Laluan API skills                                 |
| `registries.clawhub.download_path` | string | `/api/v1/download`   | Laluan API muat turun                             |

### Contoh Konfigurasi

```json
{
  "tools": {
    "skills": {
      "registries": {
        "clawhub": {
          "enabled": true,
          "base_url": "https://clawhub.ai",
          "auth_token": "",
          "search_path": "/api/v1/search",
          "skills_path": "/api/v1/skills",
          "download_path": "/api/v1/download"
        }
      }
    }
  }
}
```

## Pemboleh Ubah Persekitaran

Semua pilihan konfigurasi boleh ditindih melalui pemboleh ubah persekitaran dengan format `PICOCLAW_TOOLS_<SECTION>_<KEY>`:

Contohnya:

- `PICOCLAW_TOOLS_WEB_BRAVE_ENABLED=true`
- `PICOCLAW_TOOLS_EXEC_ENABLE_DENY_PATTERNS=false`
- `PICOCLAW_TOOLS_CRON_EXEC_TIMEOUT_MINUTES=10`
- `PICOCLAW_TOOLS_MCP_ENABLED=true`

Nota: Konfigurasi gaya peta bersarang (contohnya `tools.mcp.servers.<name>.*`) dikonfigurasikan dalam `config.json`, bukan melalui pemboleh ubah persekitaran.
