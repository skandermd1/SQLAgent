# 🤖 WatsonX SQL Agent

A natural language SQL query interface powered by IBM WatsonX's Granite model and LangChain. Ask questions about your MySQL database in plain English and get answers — no SQL required.

---

## 📋 Overview

This project connects IBM WatsonX's `granite-3-8b-instruct` foundation model to a MySQL database via LangChain's SQL agent. You can query your database using natural language prompts from the command line.

---

## 🏗️ Architecture

```
User Prompt (CLI)
      │
      ▼
LangChain SQL Agent (ZERO_SHOT_REACT_DESCRIPTION)
      │
      ├──► IBM WatsonX LLM (granite-3-8b-instruct)
      │
      └──► MySQL Database (Chinook)
```

---

## ⚙️ Prerequisites

- Python 3.8+
- Access to IBM WatsonX (`us-south.ml.cloud.ibm.com`)
- A running MySQL server with the target database
- Valid WatsonX project credentials

---

## 📦 Installation

1. **Clone the repository:**
   ```bash
   git clone <your-repo-url>
   cd <project-folder>
   ```

2. **Install dependencies:**
   ```bash
   pip install ibm-watsonx-ai ibm-watson-machine-learning langchain langchain-community mysql-connector-python
   ```

---

## 🔧 Configuration

Before running, update the following variables in the script:

| Variable | Description | Default |
|---|---|---|
| `mysql_username` | MySQL username | `root` |
| `mysql_password` | MySQL password | *(set your own)* |
| `mysql_host` | MySQL host IP or hostname | *(set your own)* |
| `mysql_port` | MySQL port | `3306` |
| `database_name` | Target database | `Chinook` |
| `project_id` | WatsonX project ID | `skills-network` |

> ⚠️ **Security Note:** Avoid hardcoding credentials. Use environment variables or a `.env` file in production:
> ```bash
> export MYSQL_PASSWORD="your_password"
> export WATSONX_PROJECT_ID="your_project_id"
> ```

---

## 🚀 Usage

Run the script with a natural language prompt:

```bash
python agent.py --prompt "How many artists are in the database?"
```

```bash
python agent.py --prompt "List the top 5 best-selling albums"
```

```bash
python agent.py --prompt "What is the total revenue from invoice lines in 2023?"
```

If no `--prompt` is provided, the script will display a usage reminder:

```
please provide a prompt using --prompt argument
```

---

## 🧠 Model Parameters

| Parameter | Value | Description |
|---|---|---|
| `MAX_NEW_TOKENS` | `1024` | Maximum tokens in the generated output |
| `TEMPERATURE` | `0.2` | Controls creativity/randomness |
| `TOP_P` | `0.95` | Nucleus sampling threshold |
| `REPETITION_PENALTY` | `1.2` | Penalizes repeated tokens |

---

## 📁 Project Structure

```
.
├── agent.py        # Main script
└── README.md       # This file
```

---

## 🛠️ How It Works

1. The script initializes a WatsonX `ModelInference` instance using the Granite 3 model.
2. The model is wrapped with `WatsonxLLM` for LangChain compatibility.
3. A `SQLDatabase` object is created from the MySQL URI.
4. LangChain's `create_sql_agent` builds a `ZERO_SHOT_REACT_DESCRIPTION` agent that can reason about and query the database.
5. The agent receives the user's natural language prompt, generates appropriate SQL, executes it, and returns the result.

---

## 🐛 Troubleshooting

**Connection refused to MySQL:**
- Verify `mysql_host` and `mysql_port` are correct and the server is reachable.

**WatsonX authentication error:**
- Ensure your `project_id` and WatsonX credentials are valid and the endpoint URL is correct.

**Parsing errors from the agent:**
- The `handle_parsing_errors=True` flag is set by default; the agent will retry on parse failures. If issues persist, try simplifying your prompt.

---

## 📄 License

This project is intended for educational and development use. Refer to IBM WatsonX and LangChain licensing terms for production usage.
