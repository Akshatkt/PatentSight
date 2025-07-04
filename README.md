# Patent Innovation Predictor: Agentic RAG System for Patent Analysis

## Overview

**Patent Innovation Predictor** is an advanced, agentic RAG (Retrieval-Augmented Generation) system for automated patent analysis, trend forecasting, and innovation prediction. It leverages CrewAI for multi-agent orchestration, OpenSearch for vector and keyword search, and Ollama for running local LLMs (e.g., Llama3, Mistral) with LangChain integration. The system is designed for deep-dive analysis of patent data in any research area (e.g., lithium batteries, hydrogen storage, quantum computing).

---

## Features

- **Multi-Agent Workflow:** Four specialized agents (Research Director, Patent Retriever, Data Analyst, Innovation Forecaster) coordinate to analyze, retrieve, and forecast patent trends.
- **Retrieval-Augmented Generation:** Combines LLM reasoning with real patent data from OpenSearch.
- **Flexible Research Area:** User can specify any research domain; all tasks dynamically adapt.
- **OpenSearch Integration:** Fast, scalable search over patent abstracts, titles, and metadata with vector (embedding) and keyword support.
- **Ollama LLMs:** Run local LLMs for privacy, speed, and cost efficiency.
- **Extensible Tools:** Easily add new tools for data enrichment, validation, or external APIs.
- **Automated Reporting:** Saves analysis and forecasts to timestamped files for audit and review.

---

## Architecture Diagram

```mermaid
flowchart TD
    A[User Input: Research Area & Model] --> B[Main Script]
    B --> C[Check Ollama & OpenSearch]
    C --> D[Create CrewAI Agents & Tools]
    D --> E[Task 1: Research Plan (Director)]
    E --> F[Task 2: Patent Retrieval (Retriever)]
    F --> G[Task 3: Trend Analysis (Analyst)]
    G --> H[Task 4: Innovation Forecast (Forecaster)]
    H --> I[Save Results to File]
    F --> J[OpenSearch Patent DB]
    D --> K[Ollama LLM (via LangChain)]
```

---

## Agents & Tasks

| Agent                | Role/Goal                                                                 | Tools Used                      | Task Description                                                                                  |
|----------------------|---------------------------------------------------------------------------|----------------------------------|---------------------------------------------------------------------------------------------------|
| Research Director    | Define research plan, scope, and focus areas                              | LLM only                        | Outlines key tech areas, time periods, and aspects for analysis                                   |
| Patent Retriever     | Find and organize relevant patents from OpenSearch                        | search_patents, search_by_date   | Retrieves patents, groups by sub-tech, summarizes companies and innovations                       |
| Data Analyst         | Analyze trends, patterns, and company focus                               | analyze_patent_trends           | Identifies growing/declining areas, tech evolution, company focus, emerging sub-technologies      |
| Innovation Forecaster| Predict future breakthroughs, R&D priorities, and disruptive technologies | LLM only                        | Forecasts next 2-3 years, recommends R&D, predicts leaders and disruptive trends                  |

---

## Setup Instructions

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd Ai_Agent
```

### 2. Create and Activate Virtual Environment

```powershell
python -m venv .venv
.\.venv\Scripts\activate
```

### 3. Install Python Dependencies

```bash
pip install -r requirements.txt
```

**Sample `requirements.txt`:**
```
crewai
langchain
langchain_ollama
opensearch-py
requests
python-dotenv
```

### 4. Set Up Environment Variables

Create a `.env` file in the project root:

```
SERPAPI_API_KEY=your_serpapi_key_here
```

### 5. Start OpenSearch

- Install OpenSearch (see [OpenSearch docs](https://opensearch.org/docs/latest/install-and-configure/install-opensearch/)).
- Start OpenSearch on `localhost:9200`.

### 6. Start Ollama (LLM Server)

- Install Docker Desktop (if not already).
- Pull and run Ollama:

```bash
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

- Pull your desired model (e.g., llama3):

```bash
docker exec -it ollama ollama pull llama3
```

- For embeddings, pull:

```bash
docker exec -it ollama ollama pull nomic-embed-text
```

### 7. Index Patent Data

- Use `information_collector.py` to fetch and save patent data from SerpAPI.
- Use `opensearch_client.py` to create the OpenSearch index and ingest data.

---

## Running the Main Application

```bash
python agentic_rag.py
```

You will be prompted for:
- Research area (e.g., "Lithium Battery", "Hydrogen Storage")
- Ollama model (e.g., "llama3", "mistral")

The system will:
- Run all agent tasks in sequence
- Save the final report to a timestamped `.txt` file

---

## File Structure

```
Ai_Agent/
│
├── agentic_rag.py           # Main CLI application
├── patent_crew.py           # CrewAI agent/task definitions
├── opensearch_client.py     # OpenSearch client and index setup
├── embedding.py             # Embedding API wrapper (Ollama)
├── information_collector.py # Fetches patent data from SerpAPI
├── helper.py                # Utility functions for SerpAPI
├── requirements.txt         # Python dependencies
├── .env                     # Environment variables (not committed)
├── files/                   # Patent data files
├── results/                 # Output and intermediate results
└── README.md                # This file
```

---

## Customization & Extensibility

- **Add New Agents:** Define new roles in `patent_crew.py` for validation, enrichment, or domain-specific tasks.
- **Add Tools:** Extend `BaseTool` to connect to other APIs, databases, or analytics engines.
- **Change LLM Model:** Use any Ollama-supported model by changing the input or default in the script.
- **Switch Research Area:** The system is fully dynamic; just enter a new research area at runtime.

---

## Troubleshooting

- **Ollama Not Running:** Ensure Docker container is up (`docker ps`), and model is pulled.
- **OpenSearch Connection Error:** Check OpenSearch is running on `localhost:9200` and index is created.
- **Missing Packages:** Run `pip install -r requirements.txt` inside your `.venv`.
- **No Patent Data:** Use `information_collector.py` to fetch and index new data.

---

## Example Usage

```
(.venv) PS C:\Users\Akshat\OneDrive\Desktop\Ai_Agent> python agentic_rag.py

Enter the research area to analyze (default: Lithium Battery): Hydrogen Storage
Enter the Ollama model to use (default: llama2): llama3

# ...agents run, tasks execute...

Analysis completed and saved to patent_analysis_20250704_153000.txt
```

---

## Advanced Tips

- **Add Memory/Context:** Pass outputs between agents for richer context.
- **Structured Outputs:** Modify agents to output JSON/CSV for dashboards.
- **Evaluation:** Add a Critic agent for output validation.
- **Scale:** Deploy as a REST API or web app using FastAPI/Streamlit.

---

## License

MIT License

---

## Credits

- [CrewAI](https://github.com/joaomdmoura/crewai)
- [LangChain](https://github.com/langchain-ai/langchain)
- [Ollama](https://ollama.com/)
- [OpenSearch](https://opensearch.org/)
- [SerpAPI](https://serpapi.com/)

---

## Contact

For questions or contributions, open an issue or contact