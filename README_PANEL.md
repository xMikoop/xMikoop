# 🤖 Multi-Agent AI Orchestration Framework

**A scalable framework for automated strategic analysis, market research, and competitive intelligence using multiple LLMs.**

---

### 🚨 The Problem
Marketing teams and executives often use LLMs like ChatGPT or Claude for strategy, research, or content. But a single prompt usually returns safe, generic, or biased answers. Single models don't challenge their own assumptions well, leading to shallow insights.

### 💡 The Solution
A **Cross-Examining Multi-Agent System**. Instead of one AI doing all the work, this framework orchestrates multiple specialized AI personas. 
- **Agent 1 (e.g., Market Analyst)**: Focuses on trends and hard data.
- **Agent 2 (e.g., Financial / Economist)**: Focuses on budgets, ROI, and costs.
- **Agent 3 (e.g., Consumer Behavior)**: Focuses on user psychology and risks.
- **Moderator**: Orchestrates the debate, forces agents to ask each other hard questions, highlights contradictions, and outputs a final, highly rigorous synthesis.

### 🛠️ Tech Stack & Architecture
- **Environment:** Local CLI execution (easily adaptable to Next.js API Routes / n8n nodes).
- **Models:** Provider-agnostic. Can mix models in the same workflow (e.g., *Claude 3.5 Sonnet* for deep analysis, *GPT-4o* for rapid moderation, *Gemini* for structured data).
- **Data Flow:** Modular file-based routing & context injection to prevent context pollution between agents.

### 📈 Business Impact (Marketing & Growth)
- **Campaign Validation:** Before spending $10k on a campaign, 3 AI experts try to "break" the idea and find flaws.
- **Competitor Analysis:** Agents simulate different competitors reacting to a new product launch.
- **Time Saved:** Turns a 3-day strategic research sprint into a 5-minute automated pipeline.

---

### 📂 Repository Structure (Demo Use Case)
As a proof of concept, this repository contains a completed run simulating a complex geopolitical/economic panel. **The exact same architecture drops directly into Marketing Tech.**

- `prompts/` - Individual system prompts injecting deep expertise into each agent.
- `01-` to `03-` - Independent agent outputs (isolated context).
- `04-pytania-krzyzowe.md` - The Moderator forcing agents to find flaws in each other's logic.
- `05-synteza.md` - Final executive summary with actionable indicators.
- `JAK-URUCHOMIC-RECZNIE-I-ROZNE-MODELE.md` - Technical documentation on orchestrating cross-provider models.

---
*Built with a focus on pragmatism—no overengineered frameworks (like heavy Langchain setups), just pure prompts, context management, and fast execution.*
