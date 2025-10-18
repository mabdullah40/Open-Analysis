[![PyPI version](https://img.shields.io/pypi/v/talking_data.svg?color=7c5cff&label=PyPI%20Version)](https://pypi.org/project/talking_data/)
[![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Downloads](https://static.pepy.tech/badge/talking_data)](https://pepy.tech/project/talking_data)
[![Docs](https://img.shields.io/badge/Docs-View%20Full%20Documentation-9f8cff.svg)](https://your-docs-url.com)

---
## 🚀 talking_data
**`talking_data`** is a lightweight, multi-provider **AI Data Analysis** library that performs SQL-based reasoning directly on your `pandas.DataFrame` using **OpenAI**, **Groq**, or **Google Gemini** models.

It automatically:
- 🧩 Generates SQL queries from plain English questions
- 🚀 Executes them locally using **DuckDB**
- 🧠 Summarizes results as insightful text or HTML
- ⚙ Installs missing dependencies automatically

---

## 🚀 Installation

```bash
pip install talking_data
```

Once installed, import it in your Python project:

```python
from talking_data import open_analysis
```

---

## ⚡ Quickstart Example

```python
from talking_data import open_analysis
import pandas as pd

# Sample DataFrame
df = pd.DataFrame({
    "region": ["East", "West", "North", "South"],
    "sales": [1200, 800, 950, 1100],
    "profit": [200, 100, 150, 180]
})

result = open_analysis(
    df=df,
    model_provider="openai",
    api_key="YOUR_API_KEY",
    question="Which region performs best by profit margin?",
    query_context="Profit margin = profit / sales * 100",
    log_level="detailed"
)

print("SQL Query:", result["sql_query"])
print("Query Result:")
print(result["query_result"])
print("Insights:")
print(result["plain_text_output"])
```

---

## 🧩 Supported Providers

| Provider | Default Model | SDK Dependency |
|----------|---------------|----------------|
| 🧠 OpenAI | `gpt-4o` | `openai` |
| ⚙ Groq | `llama-3.3-70b-versatile` | `groq` |
| 🌐 Google Gemini | `gemini-2.0-flash` | `google-genai` |

---

## 🧠 Function Reference

```python
open_analysis(
    df: pd.DataFrame,
    model_provider: str = "openai",
    model: str = None,
    api_key: str = None,
    question: str = "Do a data analysis on the dataframe and give me insights?",
    temperature_one: float = 0.2,
    temperature_two: float = 0.7,
    max_completion_tokens: int = 1024,
    output_layer_context: str = None,
    query_context: str = None,
    log_level: str = "basic"
) -> dict
```

The function performs a two-phase process:

1. **SQL Generation**: The model creates a valid SQL query from your DataFrame preview and question.
2. **Insight Summarization**: It summarizes query results in both HTML and text formats.

---

## 🧾 Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `df` | `pandas.DataFrame` | The dataset to analyze. |
| `model_provider` | `str` | One of `"gemini"`, `"groq"`, or `"openai"`. |
| `model` | `str` | Optional model name override. |
| `api_key` | `str` | API key for the respective provider. |
| `question` | `str` | Natural-language query for analysis. |
| `temperature_one` | `float` | LLM creativity for SQL generation (default: `0.2`). |
| `temperature_two` | `float` | LLM creativity for summarization (default: `0.7`). |
| `max_completion_tokens` | `int` | Token limit for completions (for Groq/OpenAI). |
| `output_layer_context` | `str` | Extra info or context for insights. |
| `query_context` | `str` | KPI definitions or SQL hints. |
| `log_level` | `str` | `"none"`, `"basic"`, `"detailed"`, or `"debug"`. Controls log verbosity. |

---

## 📤 Return Structure

```python
{
    "provider": "openai",
    "model": "gpt-4o",
    "sql_query": "SELECT region, SUM(profit)/SUM(sales)*100 AS margin FROM df GROUP BY region ORDER BY margin DESC",
    "query_result": "<pandas.DataFrame>",
    "html_output": "<section>...</section>",
    "plain_text_output": "East region has the highest profit margin (16.7%)",
    "logs": [
        "Initializing provider: openai",
        "Starting SQL generation...",
        "Generated SQL: SELECT ...",
        "Starting insights generation...",
        "Summary generated successfully."
    ]
}
```

---

## 🪵 Logging Levels

| Level | Description |
|-------|-------------|
| `none` | No logs returned. |
| `basic` | Key initialization and success/failure steps. |
| `detailed` | Includes SQL queries and key phases. |
| `debug` | Includes tracebacks and raw prompts. |

Logs are stored in `result["logs"]`.

---

## 🧮 Example with Gemini

```python
result = open_analysis(
    df=df,
    model_provider="gemini",
    api_key="YOUR_GEMINI_API_KEY",
    question="Find the top 2 regions by total sales."
)

print(result["plain_text_output"])
```

---

## ⚙ Example with Groq

```python
result = open_analysis(
    df=df,
    model_provider="groq",
    api_key="YOUR_GROQ_API_KEY",
    question="Compare profit and sales correlation by region."
)

print(result["plain_text_output"])
```

---

## 💡 How It Works

### 1. SQL Generation Layer
- The model analyzes the DataFrame preview and generates a valid SQL query.
- DuckDB executes the SQL locally.

### 2. Summarization Layer
- The model summarizes the query output.
- Returns both human-readable plain text and styled HTML.

---

## 🧠 Example Output

**Plain Text:**
```
Region East has the highest profit margin of 16.7%, followed by South at 16.3%.
```

**Generated SQL:**
```sql
SELECT region, SUM(profit)/SUM(sales)*100 AS margin FROM df GROUP BY region ORDER BY margin DESC;
```

**HTML Output:**
```html
<section>
  <h3>Regional Profit Margin Insights</h3>
  <ul>
    <li>East region leads with a 16.7% margin</li>
    <li>South follows closely at 16.3%</li>
  </ul>
</section>
```

---

## 🪄 Features

- Multi-provider LLM support (Gemini · Groq · OpenAI)
- Automatic dependency installation
- Returns structured results (SQL + DataFrame + insights)
- Full logging control
- Zero manual SQL required

---

## 🔁 Version History

| Version | Highlights |
|---------|------------|
| 1.5.0 | Added log levels, lazy import, unified provider handling |
| 1.4.0 | Logs returned in results |
| 1.3.0 | Added Gemini and Groq support |
| 1.0.0 | Initial OpenAI-based release |

---

## 🛠 Requirements

- Python 3.8+
- Internet connection
- API key for your chosen provider

---

## 🧩 Frequently Asked Questions

**Q: Does it modify my DataFrame?**  
A: No, it registers it temporarily in DuckDB for safe querying.

**Q: Can it work offline?**  
A: Not yet — all providers (Gemini, Groq, OpenAI) are cloud APIs.

**Q: Do I need to install dependencies manually?**  
A: No. The library installs missing ones automatically on first use.

**Q: Can I get raw HTML output?**  
A: Yes, available via `result["html_output"]`.

---

## 🪪 License

This project is licensed under the MIT License.

```
MIT License

Copyright (...)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

---

## 🌐 Links

- 📦 **PyPI**: [https://pypi.org/project/talking_data](https://pypi.org/project/talking_data)
- 💻 **GitHub**: [https://github.com/yourusername/talking_data](https://github.com/yourusername/talking_data)
- 📖 **Documentation**: [https://your-docs-url.com](https://your-docs-url.com)

---

Made with ❤ by Mohammad Abdullah
