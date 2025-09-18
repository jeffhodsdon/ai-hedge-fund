# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Core Commands
- **Run hedge fund CLI**: `poetry run python src/main.py --ticker AAPL,MSFT,NVDA`
- **Run backtester**: `poetry run python src/backtester.py --ticker AAPL,MSFT,NVDA`
- **Install dependencies**: `poetry install`

### Web Application
- **Quick start**: `./run.sh` (Mac/Linux) or `run.bat` (Windows)
- **Manual backend**: `cd app/backend && poetry run uvicorn main:app --reload`
- **Manual frontend**: `cd app/frontend && npm run dev`

### Code Quality
- **Format code**: `poetry run black src/ app/`
- **Sort imports**: `poetry run isort src/ app/`
- **Lint code**: `poetry run flake8 src/ app/`

## Architecture Overview

### Multi-Agent Trading System
The system employs multiple AI investment agents that work together to make trading decisions:

1. **Investment Strategy Agents** (12 total):
   - Famous investors: Warren Buffett, Charlie Munger, Ben Graham, Peter Lynch, etc.
   - Each agent embodies the investment philosophy of their namesake investor
   - Located in `src/agents/` with individual files for each agent

2. **Analysis Agents** (4 total):
   - Valuation Agent: Calculates intrinsic value and generates trading signals
   - Sentiment Agent: Analyzes market sentiment
   - Fundamentals Agent: Analyzes fundamental data
   - Technicals Agent: Analyzes technical indicators

3. **Risk Management**: `src/agents/risk_manager.py` - Calculates risk metrics and position limits

4. **Portfolio Management**: `src/agents/portfolio_manager.py` - Makes final trading decisions

### Data Flow Architecture
- **State Management**: `src/graph/state.py` - Defines the shared state structure across agents
- **LangGraph Workflow**: Agents are connected in a directed graph using LangGraph
- **Data Sources**: Financial data fetched through `src/tools/api.py`
- **Caching**: `src/data/cache.py` for performance optimization

### Web Application Stack
- **Backend**: FastAPI application in `app/backend/`
- **Frontend**: React/Vite application in `app/frontend/`
- **Database**: SQLAlchemy with Alembic migrations
- **API Routes**: RESTful endpoints for hedge fund operations, backtesting, and configuration

### Key Configuration Files
- **Environment**: `.env` file required with API keys (OPENAI_API_KEY, FINANCIAL_DATASETS_API_KEY, etc.)
- **Poetry**: `pyproject.toml` defines Python dependencies and dev tools
- **Code Style**: Black formatter with 420 character line length, isort for import sorting

### CLI Features
- **Analyst Selection**: Interactive selection of which investment agents to use
- **LLM Model Selection**: Support for OpenAI, Anthropic, Groq, DeepSeek, Ollama (local)
- **Date Range**: Configurable start/end dates for analysis periods
- **Portfolio Tracking**: Supports both long and short positions with margin requirements

### Important Implementation Notes
- All agents return trading signals that flow through risk management before portfolio decisions
- The system supports backtesting with historical data and performance metrics
- No actual trades are executed - this is for educational/research purposes only
- Data for AAPL, GOOGL, MSFT, NVDA, TSLA is free; other tickers require FINANCIAL_DATASETS_API_KEY