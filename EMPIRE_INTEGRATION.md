# M3ta a3y3 Int3llig3nc3 3mpir3 — System Integration & Architecture

This document records the complete wiring, active services, endpoints, and architecture connecting **OpenBot**, **Google Antigravity Harness**, and **M3ta-0s (OB1 Open Brain)**.

---

## 1. Executive Summary

- **Repository**: [openbot-tmw](https://github.com/m3ta-a3y3-inT3LLiG3nc3/openbot-tmw)
- **Active Harnesses**:
  - **Google Antigravity** (`agent-antigravity`): Live on port `4214` (AG-UI protocol, Gemini `gemini-3.6-flash`).
  - **Google ADK** (`agent-adk`): Port `4208` (LiteLLM / Gemini).
  - **DeepSeek Harness** (`deepseek-harness-tmw`): Native & OpenAI-compatible support.
  - **Google Research Harness2** (`harness2`): Evaluation framework.
- **Cognitive Memory & Brain**:
  - **M3ta-0s Open Brain** (`OB1-m3ta-0s`): Live on port `8000` via Streamable HTTP Model Context Protocol (MCP).

---

## 2. Live Service Endpoints & Verification

| Service | Port / URI | Protocol | Health Check | Verification Output |
| :--- | :--- | :--- | :--- | :--- |
| **Google Antigravity Agent** | `http://localhost:4214` | AG-UI over HTTP | `GET /health` | `{"ok": true, "harness": "antigravity"}` |
| **M3ta-0s Open Brain MCP** | `http://localhost:8000` | Streamable HTTP MCP | `GET /health` | `{"ok": true, "service": "M3ta-0s Open Brain"}` |
| **OpenBot API Server** | `http://localhost:3001` | REST / SSE | `GET /api/health` | Deployment runtime |
| **OpenBot Web Client** | `http://localhost:3010` | React / Vite | `GET /` | Local web app UI |

---

## 3. Wiring Contracts & Tool Specifications

### A. M3ta-0s Open Brain MCP Tools (Port 8000)
1. `ping`: Health verification.
2. `capture_thought`: Save persistent thought, fact, or instruction to Open Brain memory.
3. `search_thoughts`: Semantic and keyword search across stored memories.
4. `recall_agent_memory`: Scoped memory retrieval prior to task execution.
5. `writeback_agent_memory`: Operational trace and summary writeback post-task.

### B. OpenBot Tenant Package Integration
- **`examples/fintech/skills.yaml`**: Registered skill `m3ta-os-memory`:
  ```yaml
  - slug: m3ta-os-memory
    title: M3ta-0s Open Brain Memory
    summary: Search and store persistent memory, notes, and evidence in M3ta-0s Open Brain.
    tools:
      - open-brain/search_thoughts
      - open-brain/capture_thought
      - open-brain/recall_agent_memory
      - open-brain/writeback_agent_memory
  ```
- **`examples/fintech/agents.yaml`**: Assigned `m3ta-os-memory` to `general-assistant` and `picked-harness`.
- **`.env` Configuration**:
  ```env
  OPENBOT_INTELLIGENCE_MEMORY=on
  M3TA_OS_URL=http://localhost:8000
  M3TA_OS_MCP_KEY=openbot-m3ta-os-key
  MANAGED_AGENT_AG_UI_URL=http://localhost:4214
  ```

---

## 4. Multi-Tier Empire Repositories

The 267 repositories under `m3ta-a3y3-inT3LLiG3nc3` are organized across 6 functional layers:
- **Layer 0 (Infrastructure & Memory)**: `OB1-m3ta-0s`, `ODS-th3-m3ta-way`, `gbrain`, `supermemory-tmw`
- **Layer 1 (Orchestration & Workspaces)**: `openbot-tmw`, `ag-ui-tmw`, `agent-desktop`, `BrowserOStmw`, `Herald-OS`
- **Layer 2 (Agent Framework Harnesses)**: `agent-antigravity`, `deepseek-harness-tmw`, `harness2`, `Mobile-Harness-tmw`
- **Layer 3 (Autonomous Agents)**: `hermes-agent-TMW`, `agent-browser-tmw`, `OpenManus`, `openclaw-tmw`
- **Layer 4 (MCP & Tooling)**: `agentphone-mcp`, `enhanced-mcp`, `DesktopCommanderMCP`
- **Layer 5 (Skills Engine)**: `superpowers-tmw`, `agentskills`, `skills-tmw`, `skills-codex-tmw`

---

## 5. Verification Commands

```bash
# Verify Google Antigravity Agent
curl -s http://localhost:4214/health

# Verify M3ta-0s Open Brain Server
curl -s http://localhost:8000/health

# Query MCP tools list from M3ta-0s
curl -s -X POST http://localhost:8000/ \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -H "x-brain-key: openbot-m3ta-os-key" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```
