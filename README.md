<p align="center">
  <img src="assets/banner.png" alt="Claude Ads — Paid Advertising Audit Skill for Claude Code" width="100%">
</p>

# Claude Ads — Paid Advertising Audit Skill for Claude Code

Comprehensive paid advertising audit and optimization skill for Claude Code. Covers Google Ads, Meta Ads, YouTube Ads, LinkedIn Ads, TikTok Ads, Microsoft Ads, and Apple Search Ads with 190+ audit checks, industry-specific templates, and parallel subagent delegation.

[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/claude-code)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Version](https://img.shields.io/github/v/release/AgriciDaniel/claude-ads)](https://github.com/AgriciDaniel/claude-ads/releases)
[![CI](https://img.shields.io/github/actions/workflow/status/AgriciDaniel/claude-ads/ci.yml?branch=main&label=CI)](https://github.com/AgriciDaniel/claude-ads/actions)

## Contents

- [Installation](#installation)
- [Demo](#demo)
- [Quick Start](#quick-start)
- [Commands](#commands)
- [Features](#features)
- [Architecture](#architecture)
- [How It Analyzes Your Ads](#how-it-analyzes-your-ads)
- [FAQ](#faq)
- [Requirements](#requirements)
- [Uninstall](#uninstall)
- [Related Projects](#related-projects)
- [License](#license)

## Installation

### One-Command Install (Unix/macOS/Linux)

```bash
curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-ads/main/install.sh | bash
```

### One-Command Install (Windows PowerShell)

```powershell
irm https://raw.githubusercontent.com/AgriciDaniel/claude-ads/main/install.ps1 | iex
```

### Manual Install

```bash
git clone https://github.com/AgriciDaniel/claude-ads.git
cd claude-ads
./install.sh          # Unix/macOS/Linux
```

```powershell
.\install.ps1         # Windows PowerShell
```

## Demo

<p align="center">
  <img src="assets/demo.gif" alt="Claude Ads Demo" width="100%">
</p>

## Quick Start

```bash
# Start Claude Code
claude

# Run a full multi-platform audit
/ads audit

# Deep analysis for a single platform
/ads google
/ads meta
/ads linkedin

# Strategic planning by business type
/ads plan saas
/ads plan ecommerce
/ads plan local-service

# Cross-platform creative audit
/ads creative

# Budget and bidding strategy review
/ads budget
```

## Commands

| Command | Description |
|---------|-------------|
| `/ads audit` | Full multi-platform audit with parallel subagent delegation |
| `/ads google` | Google Ads deep analysis (Search, PMax, Display, YouTube, Demand Gen) |
| `/ads meta` | Meta Ads deep analysis (FB, IG, Advantage+ Shopping) |
| `/ads youtube` | YouTube Ads specific analysis (Skippable, Shorts, Demand Gen) |
| `/ads linkedin` | LinkedIn Ads deep analysis (B2B, Lead Gen, TLA) |
| `/ads tiktok` | TikTok Ads deep analysis (Creative, Shop, Smart+) |
| `/ads microsoft` | Microsoft/Bing Ads deep analysis (Copilot, Import validation) |
| `/ads creative` | Cross-platform creative quality audit and fatigue detection |
| `/ads landing` | Landing page quality assessment for ad campaigns |
| `/ads budget` | Budget allocation and bidding strategy review |
| `/ads apple` | Apple Search Ads (ASA) deep analysis (campaign structure, bids, MMP, TAP) |
| `/ads plan <type>` | Strategic ad plan with industry templates |
| `/ads competitor` | Competitor ad intelligence across all platforms |

### `/ads audit`
**Full Multi-Platform Audit**

Spawns 6 parallel subagents to analyze your ad accounts simultaneously:
- **audit-google** — 74 checks across Search, PMax, Display, YouTube, Demand Gen
- **audit-meta** — 46 checks across Pixel/CAPI, Creative, Structure, Audience
- **audit-creative** — 21 cross-platform creative quality checks
- **audit-tracking** — 7 conversion tracking health checks
- **audit-budget** — 24 budget and bidding strategy checks
- **audit-compliance** — 18 compliance and regulatory checks

Generates a unified **Ads Health Score (0-100)** with prioritized action plan.

### `/ads plan <business-type>`
**Strategic Ad Planning**

Industry-specific templates with platform mix, campaign architecture, creative strategy, targeting, budget guidelines, and KPI targets.

**Supported business types:**
- `saas` — Trial/demo focus, Google + LinkedIn primary
- `ecommerce` — Shopping/PMax, ROAS-focused, seasonal
- `local-service` — Google Search + LSA, call tracking, geo radius
- `b2b-enterprise` — LinkedIn ABM, long sales cycle, pipeline metrics
- `info-products` — Meta + YouTube, webinar/VSL funnels
- `mobile-app` — Meta + Google UAC, MMP required, LTV:CPI
- `real-estate` — Special Ad Category (housing), buyer/seller campaigns
- `healthcare` — HIPAA compliance, LegitScript, restricted targeting
- `finance` — Special Ad Category (credit), required disclosures
- `agency` — Multi-client management, reporting framework
- `generic` — Universal template with platform selection questionnaire

## Features

### 190+ Audit Checks
Comprehensive coverage across all platforms with weighted severity scoring:

| Platform | Checks | Key Areas |
|----------|--------|-----------|
| Google Ads | 74 | Search, PMax, Display, YouTube, Demand Gen |
| Meta Ads | 46 | Pixel/CAPI, Creative, Structure, Audience |
| LinkedIn Ads | 25 | B2B targeting, TLA, Lead Gen forms |
| TikTok Ads | 25 | Creative-first, Smart+, TikTok Shop |
| Microsoft Ads | 20 | Google import, Copilot, MSAN |
| Apple Search Ads | 35 | Campaign structure, bids, Creative Sets, MMP, TAP placements |

### Ads Health Score (0-100)
Weighted scoring algorithm with severity multipliers:

| Grade | Score | Action Required |
|-------|-------|-----------------|
| A | 90-100 | Minor optimizations only |
| B | 75-89 | Some improvement opportunities |
| C | 60-74 | Notable issues need attention |
| D | 40-59 | Significant problems present |
| F | <40 | Urgent intervention required |

### Industry Detection
Auto-detects business type from ad account signals (product feeds, conversion events, platform mix, targeting patterns) and loads industry-specific benchmarks and templates.

### Quality Gates
Hard rules enforced during every audit:
- Never recommend Broad Match without Smart Bidding (Google)
- 3x Kill Rule: flag CPA >3x target for immediate pause
- Budget sufficiency: Meta ≥5x CPA/ad set, TikTok ≥50x CPA/ad group
- Learning phase protection: no edits during active learning
- Compliance: auto-check Special Ad Categories (housing/credit/finance)

### Reference Data
12 built-in reference files with 2026-current benchmarks, bidding decision trees, platform specifications, compliance requirements, and conversion tracking implementation guides.

## Architecture

```
~/.claude/skills/ads/              # Main orchestrator
~/.claude/skills/ads/references/   # 12 RAG reference files
~/.claude/skills/ads-*/            # 13 sub-skills
~/.claude/skills/ads-plan/assets/  # 11 industry templates
~/.claude/agents/audit-*.md        # 6 parallel audit agents
```

### How It Works

1. **Orchestrator** (`/ads`) routes commands to specialized sub-skills
2. **Sub-skills** provide deep single-domain analysis with structured output
3. **Agents** run in parallel during full audits for maximum speed
4. **References** load on-demand (RAG pattern) — only what's needed per analysis
5. **Templates** provide industry-specific strategy frameworks

## How It Analyzes Your Ads

**Claude Ads works with data you provide** — exports, screenshots, or pasted metrics from your ad platform dashboards. It does not connect to any ad platform API automatically.

**To get accurate, account-specific recommendations:**
1. Export your account data (last 30 days recommended)
2. Run the relevant command: `/ads google`, `/ads audit`, etc.
3. Claude will ask for your industry and budget context first — provide these for relevant benchmarks
4. Paste or share your data when prompted

### Live Data Integration (Optional)

For direct API access without manual exports, install the [Google Ads MCP](https://github.com/googleads/google-ads-mcp) alongside Claude Ads. This MCP server connects Claude directly to your Google Ads account via the API.

## FAQ

**Can Claude Ads log into my ad manager automatically?**
No. Claude Ads analyzes data you provide (exports, screenshots, or pasted metrics). It doesn't connect to ad platforms automatically. See the Live Data Integration section above for Google Ads API access via MCP.

**Does it use real account data or generic benchmarks?**
Benchmarks are based on industry research (WordStream, Triple Whale, etc.) covering 16,000+ campaigns. They're averages — your results will vary by industry, budget level, and account maturity. Always provide your industry and monthly spend when running audits for the most relevant comparisons.

**Is ad posting or campaign creation still manual?**
Yes. Claude Ads is an audit and strategy tool. It finds issues, recommends fixes, and builds campaign plans — but creating, editing, or posting ads remains manual in your ad platform.

**Why do some recommendations seem off for my account size?**
Benchmarks and best practices differ significantly between a $500/month account and a $50k/month account. Always tell Claude your budget upfront: *"I spend $2k/month on Google Ads for a local plumbing business"* gives much better results than running `/ads google` without context.

**Does it support [platform] ads?**
Currently supported: Google, Meta (Facebook/Instagram), YouTube, LinkedIn, TikTok, Microsoft/Bing, and Apple Search Ads. Pinterest and other platforms are on the roadmap.

## Requirements

- Claude Code CLI
- Python 3.10+ with Playwright (optional, for live landing page analysis)

## Uninstall

### Unix/macOS/Linux

```bash
curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-ads/main/uninstall.sh | bash
```

### Windows PowerShell

```powershell
irm https://raw.githubusercontent.com/AgriciDaniel/claude-ads/main/uninstall.ps1 | iex
```

## Fork Changes (dotmd-parser integration)

This fork adds explicit dependency declarations using [dotmd-parser](https://github.com/dotmd-io/dotmd-parser) `@ref` directives, making the full dependency graph machine-readable and extensible.

### What Changed

| File | Change |
|------|--------|
| `ads/SKILL.md` | Added `@ref` directives for 17 sub-skills and 10 subagents |
| `skills/ads-audit/SKILL.md` | Added `@ref` directives for 6 subagents |
| `skills/ads-plan/SKILL.md` | Added `@ref` directives for 11 industry templates |
| `ads/research-sources/deps.yml` | New manifest for 5 research source documents |

### Dependency Graph (Before → After)

| Metric | Before | After |
|--------|--------|-------|
| Detected nodes | 31 | 42 (+11) |
| Detected edges | 71 | 106 (+35) |
| Undetected files | 20 | 9 |
| Edge types | `read-ref` only | `read-ref` + `@ref` |

### Why

- **Machine-readable**: `dotmd-parser` can now build a complete dependency graph from `ads/SKILL.md` as a single entry point (31 nodes, 30 edges, 0 warnings)
- **Extensible**: Adding new sub-skills or templates only requires adding an `@ref` directive — the graph updates automatically
- **Visualizable**: The graph can be rendered in [dotmd-io](https://github.com/dotmd-io/dotmd-io) web editor with ReactFlow

### Verify

```bash
pip install dotmd-parser
dotmd-parser ./ads/              # Full graph from orchestrator
dotmd-parser ./skills/ads-plan/  # Plan skill with 11 templates
```

## Related Projects

- [Claude SEO](https://github.com/AgriciDaniel/claude-seo) — Comprehensive SEO analysis skill for Claude Code
- [dotmd-parser](https://github.com/dotmd-io/dotmd-parser) — Dependency graph parser for .md skill files

## License

MIT License - see [LICENSE](LICENSE) for details.

---

Built for Claude Code by [@AgriciDaniel](https://github.com/AgriciDaniel) | Fork maintained by [@YUUUUKIIII1104](https://github.com/YUUUUKIIII1104)
