# ⚠️ THIS REPO IS NOT THE LIVE WEBSITE

**As of 2026-04-09, the live Stroopwafel Huis website is hosted inside the Paragu-AI (formerly Vete) multi-tenant platform.**

## Where to find the live site

- **Live URL:** https://paragu-ai.com/stroopwafel-huis
- **Tenant config:** [Ai-Whisperers/Vete/web/.content_data/stroopwafel-huis/](https://github.com/Ai-Whisperers/Vete/tree/main/web/.content_data/stroopwafel-huis)
- **Deploy:** Docker Swarm on agentzero VPS, Traefik → paragu-ai.com

## Where to make changes

| Change type                         | Where                                              |
|-------------------------------------|----------------------------------------------------|
| Content (copy, services, pricing)   | `Vete/web/.content_data/stroopwafel-huis/*.json`            |
| Brand (colors, fonts, logo)         | `Vete/web/.content_data/stroopwafel-huis/theme.json`        |
| Domain routing                      | `Vete/web/.content_data/domains.json`              |
| Custom code / components            | `Vete/web/app/[clinic]/*` (shared across tenants)  |

## What this repo is for now

This repo contains the full **business planning** for Stroopwafel Huis: business plan, recipes, menu, branding, operations, legal, finance. The **website content** (menu, page copy, brand theme) has been consolidated into the `stroopwafel-huis` tenant in Ai-Whisperers/Vete. This repo remains the source of truth for non-website business docs (recipes, operations, financial projections).

## Do not

- Do not deploy this repo standalone to production
- Do not treat this as the source of truth for website content
- Do not make content changes here expecting them to go live

---

_This repo is kept for history and non-website assets. Website-layer consolidation tracked in Ai-Whisperers/Vete PR #65 (merged)._
