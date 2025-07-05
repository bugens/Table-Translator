# Table-Translator
用于Excel、csv表格的内容翻译


# 表格翻译器 - 使用说明

## 📌 工具简介
本工具是一款基于AI API的自动化文件翻译程序，支持Excel（.xlsx/.xls）和CSV（.csv）格式文件的批量翻译。通过调用指定的AI模型API，可实现高效、准确的多语言翻译，适用于数据处理、文档本地化等场景。

---

## 📁 配置文件参数说明（`AI_config.json`）

以下是 `AI_config.json` 中每个参数的详细说明：

| 参数名 | 类型 | 说明 |
|--------|------|------|
| `api_key` | string | AI模型API的访问密钥，用于身份验证。 |
| `model_name` | string | 使用的AI模型名称，指定翻译模型。 |
| `api_url` | string | AI模型API的请求地址。 |
| `api_timeout` | int | API请求超时时间（秒），防止长时间无响应。 |
| `api_delay` | float | 每次API请求之间的延迟时间（秒），用于控制请求频率。 |
| `max_tokens` | int | 单次翻译的最大token数，控制输出长度。 |
| `temperature` | float | 控制生成文本的随机性，值越高生成结果越随机，值越低越确定。 |
| `top_p` | float | 控制生成文本的多样性，使用核采样方法。 |
| `top_k` | int | 控制生成文本的采样范围，限制从top_k个词中选择。 |
| `frequency_penalty` | float | 控制重复内容的惩罚力度，值越高越少重复。 |
| `enable_thinking` | boolean | 是否启用模型的“思考”模式（如推理模式）。 |
| `default_source_lang` | string | 默认源语言代码（如 `en` 表示英文）。 |
| `default_target_lang` | string | 默认目标语言代码（如 `zh` 表示中文）。 |
| `default_file` | string | 默认输入文件名，若未指定则使用此文件。 |
| `default_column` | int | 默认翻译列号（从1开始），若未指定则翻译该列。 |
| `default_batch_size` | int | 默认每次翻译的行数，控制批量处理大小。 |
| `max_batch_size` | int | 最大允许的批量翻译行数，防止内存溢出。 |
| `max_retries` | int | API请求失败时的最大重试次数。 |
| `retry_delay` | int | 每次重试之间的延迟时间（秒）。 |
| `translation_prompt` | object | 翻译提示模板，包含以下字段： |
| `translation_prompt.instruction` | string | 翻译指令，说明翻译任务。 |
| `translation_prompt.requirements` | list | 翻译要求列表，如保留术语、格式等。 |
| `translation_prompt.output_format` | string | 翻译输出格式说明，确保输出符合预期。 |

---

## 📦 功能特性

- ✅ 支持Excel和CSV文件格式
- ✅ 支持自定义源语言与目标语言
- ✅ 支持批量翻译，提升效率
- ✅ 自动插入翻译列，保留原始数据
- ✅ 支持API失败重试机制
- ✅ 可配置API参数（如超时、重试次数等）
- ✅ 支持虚拟环境自动切换

---

## 🛠 使用方法

### 1. 配置虚拟环境(可选）

建议使用虚拟环境（venv）运行本工具。

```bash
# 进入目标目录（以/path/to/folder为例）
cd /path/to/folder

# 创建虚拟环境（环境目录名为venv）
python -m venv venv  # 或 python3 -m venv venv

# 激活环境
# Linux/macOS:
source venv/bin/activate
# Windows:
.\venv\Scripts\activate
```

### 2. 安装依赖

```bash
pip install pandas openpyxl requests tqdm
```

---

### 2. 命令行参数说明

```bash
python translator.py [参数]
```

| 参数 | 说明 | 示例 |
|------|------|------|
| `-F`, `--file` | 输入文件路径（支持 `.xlsx`, `.xls`, `.csv`）（默认从配置读取）  | `-F data.xlsx` |
| `-C`, `--col` | 待翻译列号（从1开始）（默认从配置读取）  | `-C 2` |
| `-S`, `--source` | 源语言代码（如 `en`）（默认从配置读取）  | `-S en` |
| `-T`, `--target` | 目标语言代码（如 `zh`）（默认从配置读取）  | `-T zh` |
| `-G`, `--config` | 配置文件路径（默认为 `AI_config.json`） | `-G config.json` |
| `-B`, `--batch` | 每次翻译的行数（默认从配置读取）  | `-B 50` |
| `-R`, `--retries` | API失败重试次数（默认从配置读取） | `-R 5` |

---

### 3. 示例命令

```bash
# 使用默认配置翻译 data.xlsx 第2列，从英文翻译为中文
python translator.py

# 自定义文件、列号、语言、批量大小和重试次数
python translator.py -F data.xlsx -C 3 -S en -T zh -B 50 -R 5
```

---

## 📁 输出说明

- 翻译结果将保存为新文件，文件名格式为：`原文件名_translate.扩展名`
- 翻译列将插入在原列右侧，并标注为：`翻译(源语言→目标语言)`

---

## ⚠️ 注意事项

- 确保API密钥有效，且API服务可用。
- 翻译内容需符合API的输入限制（如最大token数）。
- 若翻译失败，程序将自动重试，重试次数可在配置中设置。
- 翻译过程中会自动添加延迟（`api_delay`），避免触发API限流。

---

## 📚 支持语言

请参考所使用的AI模型支持的语言列表。默认配置中支持从英文（en）翻译为中文（zh）。

---

## 📝 版本信息

- 当前版本：1.0.0
- 作者：bugens

---

## 📄 协议

本工具遵循 MIT License，欢迎自由使用和修改。

---

## 🚀 未来计划

- 暂无

---

