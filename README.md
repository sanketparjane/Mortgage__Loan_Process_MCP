# TCS_Mortgage

## AI-Assisted Property Valuation using MCP (Model Context Protocol)

### Overview

This project implements an AI-assisted property valuation system using the Model Context Protocol (MCP) architecture.

The system estimates the indicative market value of a property by combining:

* Location data
* Nearby infrastructure
* Visual surroundings
* AI-based reasoning

**Important:** GenAI is not used to directly execute actions or call APIs. All execution is strictly controlled by the MCP layer.

---

## Problem Statement

Traditional property valuation systems are either:

* Fully rule-based (fixed weights, static logic)
* Fully manual (human surveyors)

The goal is to design a system where:

* GenAI handles decision-making and reasoning
* MCP enforces execution control
* External APIs provide real-world data

This aligns with enterprise-grade AI system design.

---

## Key Design Principle

**GenAI thinks. MCP controls. Agents execute.**

---

## High-Level Architecture

```text
Client
  |
  v
FastAPI Endpoint (/mcp/start)
  |
  v
MCP Router (Orchestrator)
  |
  +--> Workflow Agent (GenAI - Planning)
  +--> Data Collector Agent (Maps & Places APIs)
  +--> Vision Agent (Street View / Imagery)
  +--> Valuation Agent (GenAI - Reasoning)
  |
  v
Final Unified Response
```

---

## MCP Workflow

### 1. Client Request

Example:

```json
{
  "address": "Nashik, Maharashtra"
}
```

### 2. MCP Entry Point

```http
POST /mcp/start
```

### 3. Workflow Agent (GenAI – Planning)

Analyzes the input context and decides:

* Whether location data is needed
* Whether visual verification is required
* Whether valuation is feasible

GenAI does not call APIs and only returns a structured plan.

### 4. MCP Router (Execution Control)

* Reads the GenAI decision
* Selectively triggers required agents
* Enforces execution order
* Prevents uncontrolled execution

### 5. Data Collector Agent (Deterministic)

Uses:

* Google Maps API
* Google Places API

Fetches:

* Latitude & longitude
* Nearby amenities (hospitals, schools, IT parks, etc.)

### 6. Vision Agent (Perception)

Uses:

* Google Street View imagery

Provides:

* Road access observations
* Surrounding environment analysis

Vision Agent does not estimate property price.

### 7. Valuation Agent (GenAI – Reasoning)

Combines structured data and vision context to produce:

* Estimated property value
* Valuation reasoning
* Confidence level

GenAI does not fetch data and does not execute APIs.

---

## Agent Types Used

| Agent                | Type                | Purpose                       |
| -------------------- | ------------------- | ----------------------------- |
| Workflow Agent       | GenAI Agent         | Planning & decision-making    |
| Data Collector Agent | Deterministic Agent | Maps & Places data collection |
| Vision Agent         | Perception Agent    | Visual surroundings analysis  |
| Valuation Agent      | GenAI Agent         | Property valuation reasoning  |

---

## Where GenAI Is Used

GenAI is used only in:

1. Workflow Agent – Planning & decision-making
2. Valuation Agent – Reasoning & explanation

All execution is handled by MCP-controlled agents.

---

## Security & Control (Why MCP)

* API keys stored in environment variables
* GenAI never sees API keys
* GenAI never executes code
* MCP enforces strict execution boundaries

This mirrors real-world enterprise AI safety practices.

---

## Environment Variables

```env
OPENAI_API_KEY=your_openai_api_key
GOOGLE_MAPS_API_KEY=your_google_maps_api_key
```

---

## How to Run

Install dependencies:

```bash
pip install -r requirements.txt
```

Start server:

```bash
uvicorn app.main:app --reload
```

Call endpoint:

```http
POST http://127.0.0.1:8000/mcp/start
```

---

## Example Output

```json
{
  "estimated_value_in_inr": 8500000,
  "valuation_reasoning": "...",
  "confidence_level": "medium"
}
```

---

## Limitations

* Government land registry APIs are not publicly available
* Vision analysis is contextual, not authoritative
* Generic addresses lead to low-confidence estimates

---

## Future Enhancements

* Integration with official land rate APIs
* Deterministic confidence scoring
* PDF valuation report generation
* Caching and rate-limit handling
