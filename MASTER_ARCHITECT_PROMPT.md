# Architecting the Ligi-Bigi Autonomous Data Pipeline (L-BADP)

## Objective
Design a highly modular, distributed, and autonomous data collection system for Betika’s "Ligi Bigi" virtual sports. The system must operate 24/7 on a $0 budget using Oracle Cloud (Compute), Supabase (PostgreSQL), Upstash (Redis), and Streamlit (GUI).

## System Architecture

### 1. Orchestrator Layer
A central coordinator using a Multi-League Swarm pattern. It discovers leagues via the `.league-countries` flags and spawns independent League Clusters (English, Kenyan, etc.) concurrently.

### 2. Agent Factory (Modular Workers)

- **Chronos Agent**  
  Monitors the `<ul class="time">` container. Broadcasts the "Global Pulse" and triggers "Match Start/End" events.

- **Odds-Stalk Agent**  
  Scrapes market prices (1X2, GG, etc).  
  **Integrity Gate:** Must lock/stop scraping at T-minus 10 seconds to ensure pre-match integrity.

- **Shadow Agent**  
  While the current match is live, pre-scrapes the *next* match’s odds in a separate browser context.

- **Live-Pulse Agent**  
  Uses Mutation Observers on `.match-resulting`. Captures goals (via `.blink`), goal minutes, and HT states.

- **Historian Agent**  
  Parses the "Results" tab to audit and verify Live-Pulse data.

- **Janitor Agent**  
  Compresses daily raw data and offloads logs to free cloud storage to remain under Supabase’s 500MB limit.

- **Sentinel Agent**  
  Monitors agent health, triggers tiered retries, and manages circuit breakers.

## Logic Requirements
- Concurrency via Playwright browser contexts  
- Human jitter engine (Gaussian delays)  
- Strict 10-second pre-match lock  
- Redis for state, Supabase for persistence

## GUI Requirements
- League toggles
- Integrity threshold controls
- Live monitoring grid

## Error Handling
- Tier 1: 100ms DOM retries  
- Tier 2: Circuit breaker  
- Tier 3: Dead letter queue
