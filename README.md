# PatentSight

## ✨ Overview

**Patent Innovation Predictor** is an intelligent multi-agent system designed to automate the analysis, retrieval, and forecasting of patent innovations. Built with **CrewAI**, **LangChain**, **Ollama**, and **OpenSearch**, the system enables powerful **Retrieval-Augmented Generation (RAG)** workflows tailored for patent data. It dynamically analyzes any research domain (e.g., lithium batteries, hydrogen storage, quantum computing) to uncover trends and predict future innovations.



## 📊 Key Features

* ✨ **Agentic Workflow:** Uses four CrewAI agents with well-defined roles.
* 🔮 **Retrieval-Augmented Generation (RAG):** Real-time access to OpenSearch-powered patent database.
* 📆 **Custom Research Domains:** Works with any field the user inputs at runtime.
* 🧑‍💼 **LLM Reasoning with Data:** Merges patent metadata with LLM-driven insights.
* ⚡ **Ollama for Local LLMs:** Ensures privacy, speed, and no external dependencies.
* 🔹 **Modular Design:** Easily extensible with new tools, agents, or data connectors.

---

## 🛠️ Tech Stack

| Component         | Technology         | Purpose                                         |
| ----------------- | ------------------ | ----------------------------------------------- |
| Language          | Python 3.9+        | Core logic and orchestration                    |
| Agents            | CrewAI             | Multi-agent workflow manager                    |
| Prompt/LLM Chains | LangChain          | Prompt templates, chaining, and LLM abstraction |
| LLM Server        | Ollama + LangChain | Local LLM execution (LLaMA3, Mistral, etc.)     |
| Search Engine     | OpenSearch         | Patent indexing, keyword & vector search        |
| Embeddings        | nomic-embed-text   | Patent vector embeddings via Ollama             |
| Data API          | SerpAPI            | Real-time patent data scraping                  |
| Containerization  | Docker             | Runs Ollama & OpenSearch locally                |
| Storage           | JSON + OpenSearch  | Indexed patents and search results              |

---

## 🪤 Agents & Tasks

| Agent                    | Purpose                                                | Tools Used                               |
| ------------------------ | ------------------------------------------------------ | ---------------------------------------- |
| 💼 Research Director     | Define research goals, key focus areas, time ranges    | LLM only                                 |
| 🧰 Patent Retriever      | Query OpenSearch, retrieve & organize patents          | `search_patents`, `search_by_date_range` |
| 📊 Data Analyst          | Detect trends, tech evolution, and innovation hotspots | `analyze_patent_trends`                  |
| 🌌 Innovation Forecaster | Predict future breakthroughs and recommend R\&D areas  | LLM only                                 |

---

## 🛩️ How It Works

1. **Patent Collection**:

   * Run `information_collector.py` to fetch patent data from SerpAPI.
   * Save as JSON and load into OpenSearch using `opensearch_client.py`.

2. **Embedding Indexing**:

   * Generate vector embeddings using `embedding.py` via `nomic-embed-text`.
   * Store embeddings in OpenSearch for hybrid (keyword + vector) search.

3. **Main Pipeline Execution**:

   * Execute `agentic_rag.py`.
   * Prompts user for research area (e.g., "Hydrogen Storage") and LLM model (e.g., "llama3").
   * Checks if Ollama and OpenSearch are running.
   * Instantiates agents and their tools.
   * Agents sequentially execute tasks, passing data forward.
   * Final report is saved to `/results/`.

---

## ⚙️ Setup Guide

### 1. Clone the Repository

```bash
git clone <repo-url>
cd Ai_Agent
```

### 2. Set Up Virtual Environment

```powershell
python -m venv .venv
.\.venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Environment Variables

Create a `.env` file with your SerpAPI key:

```
SERPAPI_API_KEY=your_key_here
```

### 5. Start Services

#### OpenSearch

Follow instructions from [OpenSearch Docs](https://opensearch.org/docs/latest/install-and-configure/install-opensearch/)
Ensure it's running at `localhost:9200`.

#### Ollama

```bash
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
docker exec -it ollama ollama pull llama3
```

For embeddings:

```bash
docker exec -it ollama ollama pull nomic-embed-text
```

### 6. Index Patent Data

```bash
python information_collector.py  # collect
python opensearch_client.py      # index into OpenSearch
python embedding.py              # add embeddings
```

### 7. Run Main Pipeline

```bash
python agentic_rag.py
```

---

## 🗂️ File Structure

```
Ai_Agent/
├── agentic_rag.py           # Main CLI pipeline
├── patent_crew.py           # Agents and tasks setup
├── opensearch_client.py     # Index management
├── embedding.py             # Embedding generator
├── information_collector.py # Patent scraper (SerpAPI)
├── helper.py                # Utility functions
├── .env                     # Secrets (API keys)
├── requirements.txt         # Python dependencies
├── files/                   # Raw patent data
├── results/                 # Analysis output
└── README.md                # Project documentation
```

---

## ⚡ Advanced Usage & Customization

* **Switch LLM Model**: At runtime, specify any model available in Ollama (e.g., "mistral").
* **Change Research Domain**: Just input a new research field at runtime.
* **Add New Agents**: Extend `patent_crew.py` to include critics, validators, or enrichment agents.
* **Output Formats**: Change agent outputs to JSON/CSV for dashboards or API use.
* **Web Interface**: Integrate with FastAPI or Streamlit for interactive UI.

---

## 🔎 Example

```
Enter the research area to analyze (default: Lithium Battery): Hydrogen Storage
Enter the Ollama model to use (default: llama2): llama3

[...Agents running...]

Analysis completed and saved to: /results/patent_analysis_20250704_153000.txt
```

---

## 🚀 Future Improvements

* Add Agent Memory for long-term context.
* Introduce Critic Agent for validation.
* Integrate visual analytics (charts/timelines).
* Expose as API or Web App.

---




## Pipeline of Patent Innovation Predictor

1. **User Input**
   - User starts the CLI (`python agentic_rag.py`).
   - User enters the research area (e.g., "Lithium Battery") and selects the Ollama LLM model (e.g., "llama3").
     
   ![image](https://github.com/user-attachments/assets/9fb41f22-e27c-4b74-a3d5-6533a728e29b)


2. **Environment & Service Checks**
   - Script checks if Ollama (LLM server) and OpenSearch (vector DB) are running and accessible.
   - Ensures required models are available in Ollama.

3. **Patent Data Collection & Indexing**
   - `information_collector.py` fetches patent data from SerpAPI for the specified research area.
   - Data is saved as JSON files in the `files/` or `results/` directory.
   - `embedding.py` generates vector embeddings for patent abstracts using Ollama’s embedding model.
   - `opensearch_client.py` creates the OpenSearch index (if not present) and ingests the patent data with embeddings.

4. **Agentic RAG Workflow (CrewAI Orchestration)**
   - Four agents are instantiated:
     - **Research Director:** Defines the research plan for the chosen area.
     - **Patent Retriever:** Uses OpenSearch tools to retrieve and organize relevant patents.
     - **Data Analyst:** Analyzes trends, patterns, and company focus from the retrieved data.
     - **Innovation Forecaster:** Predicts future breakthroughs and R&D priorities.
   - Each agent executes its task in sequence, passing outputs to the next agent.

5. **Patent Search & Retrieval**
   - Agents use CrewAI tools to query OpenSearch for patents matching the research area and time window.
   - Results are grouped, summarized, and prepared for analysis.
   
    ![image](https://github.com/user-attachments/assets/a36b6fea-ea1d-447d-be68-c5af2dfad9d9)


6. **Trend Analysis & Forecasting**
   - Data Analyst agent identifies innovation trends, key companies, and emerging technologies.
   - Innovation Forecaster agent predicts future directions, R&D priorities, and disruptive trends.

7. **Reporting**
   - The final output (comprehensive analysis and forecast) is saved to a timestamped `.txt` file in the project directory.

8. **User Review**
   - User reviews the saved report for insights, trends, and recommendations.
   
    ![image](https://github.com/user-attachments/assets/0c23db9e-3fb4-4be5-ace1-546de25f1458)


---

**Technologies Involved at Each Step:**

- **Python**: Orchestration, agent logic, and CLI.
- **CrewAI**: Multi-agent workflow and task management.
- **LangChain**: LLM prompt management and chaining.
- **Ollama**: Local LLM and embedding model serving (via Docker).
- **OpenSearch**: Patent data storage, keyword and vector search.
- **SerpAPI**: Patent data collection from Google Patents.
- **Docker**: Containerization for Ollama and OpenSearch services.

---

**Summary Diagram:**

```
User Input
   ↓
Service Checks (Ollama, OpenSearch)
   ↓
Patent Data Collection (SerpAPI → JSON)
   ↓
Embedding Generation (Ollama)
   ↓
Indexing in OpenSearch
   ↓
CrewAI Agentic Pipeline:
   Research Plan → Patent Retrieval → Trend Analysis → Innovation Forecast
   ↓
Report Generation (.txt)
   ↓
User Review#
