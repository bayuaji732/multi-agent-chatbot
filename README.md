# Multi-Agent GenAI Chatbot over S&P 500 Dataset

A production-grade, multi-agent GenAI system for querying S&P 500 financial data using natural language.

## 🎯 Features

- **Multi-Agent Architecture**: Specialized agents for intent understanding, planning, data retrieval, analysis, visualization, and synthesis
- **Schema-Aware Querying**: Auto-detects CSV schema - never hallucinates column names or makes up data
- **Conversation Memory**: Handles follow-up questions with context
- **Evaluation Framework**: Built-in evaluation system with test queries and metrics
- **RESTful API**: FastAPI backend for easy integration
- **Interactive UI**: Streamlit frontend for demo and testing
- **Visualization Support**: Automatic chart generation when appropriate

## 🗃️ Architecture

```
User Query
    ↓
Intent Agent (Understands user intent, extracts entities)
    ↓
Planning Agent (Creates execution plan)
    ↓
Execution Agents:
  - Data Retrieval Agent (Executes SQL queries)
  - Analysis Agent (Performs calculations)
  - Visualization Agent (Creates charts)
    ↓
Synthesis Agent (Generates final response)
    ↓
Response + Visualization
```

## 📋 Prerequisites

- Python 3.9+
- OpenAI API key (GPT-4o-mini or GPT-4o)
- S&P 500 dataset from Kaggle

## 🚀 Installation

### 1. Clone the repository

```bash
git https://github.com/bayuaji732/multi-agent-chatbot.git
cd multi-agent-chatbot
```

### 2. Create virtual environment

```bash
# using venv
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Download dataset

- Download from: https://www.kaggle.com/datasets/paytonfisher/sp-500-companies-with-financial-information
- Extract and place `sp500_companies.csv` in `./data/` directory

### 5. Configure environment

Create a `.env` file in the root directory:

```env
OPENAI_API_KEY=your_api_key_here
DATASET_PATH=./data/sp500_companies.csv
DUCKDB_PATH=./data/sp500.duckdb
API_HOST=0.0.0.0
API_PORT=8000

# Model Configuration
LLM_MODEL=gpt-5-nano
LLM_TEMPERATURE=0.0
LLM_MAX_TOKENS=4000
```

## 📁 Project Structure

```
.
├── app/
│   ├── __init__.py
│   ├── config.py              # Configuration management
│   ├── main.py                # FastAPI application
│   ├── agents/
│   │   ├── __init__.py
│   │   ├── state.py           # State definitions
│   │   ├── intent_agent.py    # Intent understanding
│   │   ├── planner_agent.py   # Query planning
│   │   ├── data_agent.py      # Data retrieval
│   │   ├── analysis_agent.py  # Numerical analysis
│   │   ├── visualization_agent.py # Chart generation
│   │   └── synthesis_agent.py # Response generation
│   ├── api/
│   │   ├── __init__.py
│   │   └── routes.py          # API endpoints
│   ├── database/
│   │   ├── __init__.py
│   │   ├── metadata.py        # Auto-generated schema metadata
│   │   └── manager.py         # Database operations
│   ├── evaluation/
│   │   ├── __init__.py
│   │   └── evaluator.py       # Evaluation framework
│   ├── models/
│   │   ├── __init__.py
│   │   └── model.py           # Pydantic models
│   └── services/
│       ├── __init__.py
│       └── orchestrator.py    # Multi-agent orchestrator
├── data/
│   ├── sp500_companies.csv    # Your dataset
│   └── sp500.duckdb          # Auto-generated database
├── tests/
│   ├── check_columns.py       # Verify CSV columns
│   └── test_api.py           # API test suite
├── streamlit_app.py          # Streamlit frontend
├── run_evaluation.py         # Evaluation runner
├── requirements.txt          # Python dependencies
├── README.md                # This file
└── ARCHITECTURE.md          # Detailed architecture docs
```

## 🏃 Running the System

### Method 1: Using separate terminals

**Terminal 1 - Start API Server:**

```bash
uvicorn app.main:app --reload
```

You should see:

```
INFO:     Application started successfully
INFO:     Uvicorn running on http://127.0.0.1:8000
```

**Terminal 2 - Start Streamlit UI:**

```bash
streamlit run streamlit_app.py
```

Access the UI at: **http://localhost:8501**

## 📊 Dataset Schema

The system auto-detects the following columns from your CSV:

- **Symbol** (string): Stock ticker symbol
- **Name** (string): Company name
- **Sector** (string): GICS sector classification
- **Price** (float, USD): Current stock price
- **Price/Earnings** (float, ratio): P/E ratio
- **Dividend Yield** (float, percentage): Annual dividend yield
- **Earnings/Share** (float, USD): EPS
- **52 Week Low** (float, USD): 52-week low price
- **52 Week High** (float, USD): 52-week high price
- **Market Cap** (float, USD): Market capitalization
- **EBITDA** (float, USD): EBITDA
- **Price/Sales** (float, ratio): P/S ratio
- **Price/Book** (float, ratio): P/B ratio
- **SEC Filings** (string): SEC filings URL

## 📝 Example Queries

### 1. Simple Lookup Queries

```
What is Apple's market cap?
Show me Microsoft's current price
What sector is Tesla in?
Tell me Google's P/E ratio
What is Amazon's dividend yield?
```

### 2. Comparison Queries

```
Compare the market cap of Apple and Microsoft
Which has a higher P/E ratio: Tesla or Amazon?
Compare revenue between Google and Facebook
Which has more EBITDA: Apple or Microsoft?
Show me the difference in stock price between Netflix and Disney
```

### 3. Aggregation Queries

```
What's the average market cap in the technology sector?
How many companies are in the healthcare sector?
What is the total EBITDA of all energy companies?
Calculate the average P/E ratio for financial companies
What's the median stock price in the consumer sector?
```

### 4. Ranking Queries

```
Show me the top 5 companies by market cap
Which 10 companies have the highest dividend yield?
List the bottom 5 companies by stock price
Top 3 tech companies by EBITDA
Rank the top 10 companies by P/E ratio
```

### 5. Filtering Queries

```
List all companies with market cap over 1 trillion
Which tech companies have a P/E ratio less than 20?
Show me all companies with dividend yield above 3%
Find companies in healthcare sector with price above $200
List all companies with EBITDA over 50 billion
```

### 6. Statistical Analysis

```
Is there a correlation between market cap and P/E ratio?
What's the relationship between price and dividend yield?
Show me the distribution of P/E ratios across sectors
Calculate standard deviation of market caps in tech sector
```

### 7. Visualization Requests

```
Show me a chart of top 10 companies by market cap
Create a bar chart comparing EBITDA of FAANG stocks
Visualize the distribution of companies across sectors
Plot the top 15 companies by dividend yield
Show me a comparison chart of tech giants' market caps
```

### 8. Multi-Step Complex Queries

```
Compare Apple and Microsoft's P/E ratios, then show me the sector average
Find the top 5 tech companies by market cap and show their average dividend yield
List companies with market cap over 500B and compare their P/E ratios
Which sector has the highest average EBITDA? Show me the top 3 companies in that sector
```

### 9. Follow-up Contextual Queries

```
User: "What is Apple's market cap?"
Bot: [responds with Apple's market cap]
User: "Compare it to Microsoft"
Bot: [compares Apple and Microsoft market caps]
User: "What about their P/E ratios?"
Bot: [compares P/E ratios]
User: "Show me a chart"
Bot: [creates visualization]
```

### 10. Sector Analysis

```
Which sector has the most companies?
What's the average stock price in the technology sector?
Compare average market caps across all sectors
Show me healthcare companies ranked by EBITDA
Which sector has the highest average dividend yield?
```

## 🧪 Testing

### Test API Endpoints

```bash
python tests/test_api.py
```

This will test:

- Health check endpoint
- Schema endpoint
- Sample data endpoint
- Various query types (lookup, comparison, ranking, etc.)

### Run Evaluation Framework

```bash
python run_evaluation.py
```

This will:

1. Execute predefined test queries
2. Measure intent accuracy, entity extraction, and execution success
3. Save results to `data/eval_results.json`
4. Compare against baseline (if exists)

### Manual API Testing

```bash
# Health check
curl http://localhost:8000/

# Get schema
curl http://localhost:8000/schema

# Get sample data
curl http://localhost:8000/sample-data?limit=5

# Query
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"query": "What is Apple'\''s market cap?"}'

# Reset conversation
curl -X POST http://localhost:8000/reset
```

## 🔧 Configuration

Edit `.env` file or `app/config.py`:

```python
# API Configuration
api_host: str = "0.0.0.0"
api_port: int = 8000

# Model Configuration
llm_model: str = "gpt-5-nano"  # Options: gpt-4o-mini, gpt-4o, gpt-3.5-turbo
llm_temperature: float = 0.0
llm_max_tokens: int = 4000

# Data Configuration
dataset_path: str = "./data/sp500_companies.csv"
duckdb_path: str = "./data/sp500.duckdb"

# Agent Configuration
max_agent_iterations: int = 5
agent_timeout_seconds: int = 30
```

## 📈 Evaluation Metrics

The evaluation framework measures:

- **Intent Accuracy**: % of queries where intent was correctly identified
- **Entity F1 Score**: Precision and recall of entity extraction
- **Success Rate**: % of queries that executed without errors
- **Numerical Accuracy**: Whether responses contain expected values
- **Response Quality**: Length and completeness of responses
- **By Category**: Performance breakdown by query type

## 🎯 Design Decisions

### 1. Auto-Generated Schema

- **Problem**: Manual schema definition is error-prone
- **Solution**: Auto-detect schema from CSV at startup
- **Benefit**: Always matches actual data structure

### 2. Multi-Agent Architecture

- **Separation of Concerns**: Each agent has a single responsibility
- **Modularity**: Agents can be improved independently
- **Debuggability**: Easy to trace which agent caused issues

### 3. LangGraph Orchestration

- **State Management**: Centralized state flows through all agents
- **Conditional Execution**: Agents execute based on dependencies
- **Error Recovery**: Errors captured at each step

### 4. DuckDB Backend

- **Performance**: Fast analytical queries
- **Simplicity**: No external database server needed
- **SQL Compatibility**: Standard SQL with extensions

### 5. Evaluation-First Approach

- **Baseline**: Establish metrics from day one
- **Iteration**: Track improvements across versions
- **Objectivity**: Quantitative measures of quality

## 🛡️ Guardrails

- SQL injection prevention (validated queries only)
- No destructive operations (SELECT only)
- Schema validation before execution
- Error handling at each agent
- Rate limiting (configurable)

## 🐛 Debugging

Enable debug logging:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

View agent execution:

- Check console logs for each agent's decisions
- Review SQL queries in API response
- Examine `execution_plan` in state

## 🔄 Iteration Process

1. **Run Evaluation**: `python run_evaluation.py`
2. **Analyze Results**: Review `data/eval_results.json`
3. **Identify Issues**: Look at failed queries
4. **Make Improvements**: Update agents, prompts, or logic
5. **Re-evaluate**: Compare new results to baseline
6. **Document**: Track changes and impact

## 📊 Performance

### Typical Query Latency

- Simple lookup: 2-4 seconds
- Complex analysis: 4-8 seconds
- With visualization: 5-10 seconds

### Bottlenecks

- LLM API calls (dominant factor)
- Multiple sequential agents
- Large result sets

## 🚀 Future Enhancements

- [ ] Parallel agent execution
- [ ] Caching layer for common queries
- [ ] Support for time-series data
- [ ] Multi-company comparison matrices
- [ ] Export results to PDF/Excel
- [ ] User authentication
- [ ] Query history and favorites
- [ ] Natural language SQL explanation
- [ ] Support for custom datasets

## 📄 License

MIT License - see LICENSE file

## 🙏 Acknowledgments

- S&P 500 dataset from Kaggle
- OpenAI for LLM capabilities
- LangChain/LangGraph for agent orchestration
- FastAPI and Streamlit for web frameworks

---

**Built with ❤️ as a demonstration of multi-agent chatbot.**
