# L-BADP Implementation & Maintenance Guide

## Phase 1: Initiation
- Oracle Cloud Always Free (ARM Ampere, Ubuntu)
- Supabase tables: matches, odds, goals
- Upstash Redis setup
- Docker & Docker Compose
- Streamlit Community Cloud deployment

## Phase 2: Deployment
- Enable one league first
- Verify T-10s lock
- Scale additional leagues
- Monitor RAM usage

## Phase 3: Maintenance
- Daily Telegram alert checks
- Monthly data archival audits
- Quarterly Playwright updates
- GitHub Action health pings

## Why This Works
GUI as control plane, cloud as execution layer, all within free tiers.
