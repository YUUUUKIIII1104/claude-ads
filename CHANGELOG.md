# Changelog

All notable changes to claude-ads are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.1-fork] - 2026-03-25

### Added
- **dotmd-parser `@ref` directives** across orchestrator and sub-skills for machine-readable dependency graph
  - `ads/SKILL.md`: 17 sub-skills + 10 subagents declared via `@ref`
  - `skills/ads-audit/SKILL.md`: 6 audit subagents declared via `@ref`
  - `skills/ads-plan/SKILL.md`: 11 industry templates declared via `@ref`
  - `ads/research-sources/deps.yml`: manifest for 5 research source documents
- **README**: "Fork Changes" section with dependency graph analysis, usage examples, and dotmd-parser/dotmd-io showcase
- **README**: Table of Contents link to Fork Changes section

### Changed
- Sub-skill and subagent listings in `ads/SKILL.md` converted from prose lists to `@ref` directives (functionally equivalent, now parseable by dotmd-parser)
- Industry template listings in `skills/ads-plan/SKILL.md` converted from backtick-quoted names to `@ref` directives

### Dependency Graph Impact
- Detected nodes: 31 → 42 (+35%)
- Detected edges: 71 → 106 (+49%)
- Undetected files: 20 → 9
- `dotmd-parser ./ads/` now returns full graph (31 nodes, 30 edges, 0 warnings) from single entry point

> Fork of [AgriciDaniel/claude-ads](https://github.com/AgriciDaniel/claude-ads) with [dotmd-parser](https://github.com/dotmd-io/dotmd-parser) integration.

## [1.2.0] - 2026-03-12

### Added
- **Apple Search Ads sub-skill** (`/ads apple`): 35 checks across campaign structure (BOFU/MOFU/Search Match), bid health (CPT vs install rate, CPA Goals), Creative Sets (Custom Product Pages), MMP attribution (AppsFlyer/Adjust/SKAdNetwork), budget pacing, TAP placement coverage (Today/Search/Product Pages), and goal CPA benchmarks by app category and country tier
- **Context Intake** step in orchestrator: Claude now asks for industry, monthly ad spend, primary goal, and active platforms before any audit — ensures benchmarks and recommendations match the user's actual situation instead of defaulting to generic industry averages
- **Google Ads MCP reference** in README: links to [google-ads-mcp](https://github.com/googleads/google-ads-mcp) for users who want live API-connected audits
- **FAQ section** in README: addresses top community questions (API login, benchmark accuracy, manual ad posting, budget context, platform support)
- **"How It Analyzes Your Ads"** section in README: clearly explains manual data input model and data export workflow

### Fixed
- `install.ps1`: PowerShell 5.1 crash on git clone — git progress writes to stderr which PS 5.1 treated as a terminating error under `$ErrorActionPreference = "Stop"`. Fixed by temporarily setting `Continue` around clone call and using `2>&1 | Out-Null`
- `uninstall.ps1`: Parse failure on non-UTF-8-BOM systems — Unicode `→` and `✓` characters in double-quoted strings caused `TerminatorExpectedAtEndOfString`. Replaced with ASCII equivalents
- `ads-google/SKILL.md`: Negative keyword guidance now enforces Exact Match `[kw]` and Phrase Match `"kw"` types by default — never Broad Match negatives. Negatives must be sourced from Search Terms Report data and grouped into themed Shared Lists. Includes over-blocking review step
- `ads/SKILL.md`: Removed unsupported `allowed-tools` frontmatter field per Anthropic skill spec
- `ads/SKILL.md`: Added `apple` to `argument-hint` subcommand list
- Install scripts: Updated sub-skill count from 12 → 13 to reflect new ads-apple addition

## [1.1.1] - 2026-02-11

### Fixed
- M-CR2 vs M37 frequency threshold ambiguity: clarified M-CR2 is ad set level (<3.0) and M37 is campaign level (<4.0)
- Ecommerce template PMax image count aligned to G31 audit check (15 → 20 images per asset group)
- Real estate template budget percentages widened to bracket 100% (was 90-105%, now 80-110%)
- Info products template TikTok allocation note: added minimum $50/day campaign budget caveat
- Duplicate step numbering in ads-tiktok (two step 7s) and ads-creative (two step 6s)

### Added
- `argument-hint` field on orchestrator skill for CLI subcommand hints

## [1.1.0] - 2026-02-11

### Fixed
- Audit check count corrected from 186 to 190 (actual total: Google 74 + Meta 46 + LinkedIn 25 + TikTok 25 + Microsoft 20)
- TikTok budget sufficiency threshold aligned to authoritative checklist (Pass ≥50x CPA, Warning 20-49x, Fail <20x)
- Benchmarks typo: Local Services CPC `$7.85-$15-$30` → `$7.85-$15.00`
- Call Campaigns context note: clarified creation vs serving deadlines (Feb 2026 / Feb 2027)
- Flexible Ads context note: corrected launch date from 2025 to 2024
- Scoring system weighting rationale: corrected "20-25%" to "25-30%" to match actual platform weights
- G59 mobile speed: LCP now measured on mobile viewport (375x812) instead of desktop
- G61 schema check: validates Product/FAQ/Service types per audit reference (not any schema)
- Removed unused beautifulsoup4 and lxml from requirements.txt

### Added
- `uninstall.ps1` for Windows parity (Unix already had `uninstall.sh`)
- `.gitattributes` to fix GitHub language detection (Markdown, not PowerShell)
- Research context notes in google-audit.md (ECPC deprecation, Call Campaigns sunset, Power Pack, AI Max)
- Research context notes in meta-audit.md (detailed targeting removal, Flexible Ads, Financial Products SAC)
- Research context notes in linkedin-audit.md (Connected TV, BrandLink, Live Event Ads, Accelerate campaigns)
- Weighting rationale section in scoring-system.md explaining grading band design
- Scoring system reference added to ads-tiktok and ads-creative process steps
- Missing `.gitignore` patterns for creative, landing, budget, and competitor reports

### Changed
- Removed non-spec `color` field from all 6 agent frontmatter files
- Agent frontmatter now uses only official Claude Code spec fields (name, description, model, maxTurns, tools)

## [1.0.0] - 2026-02-11

### Added
- Main orchestrator skill (`/ads`) with industry detection and quality gates
- 12 sub-skills: audit, google, meta, youtube, linkedin, tiktok, microsoft, creative, landing, budget, plan, competitor
- 6 parallel audit agents: audit-google, audit-meta, audit-creative, audit-tracking, audit-budget, audit-compliance
- 12 reference files with 2026 benchmarks, bidding decision trees, platform specs, compliance requirements
- 11 industry templates: saas, ecommerce, local-service, b2b-enterprise, info-products, mobile-app, real-estate, healthcare, finance, agency, generic
- 190 audit checks across all platforms (Google 74, Meta 46, LinkedIn 25, TikTok 25, Microsoft 20)
- Ads Health Score (0-100) with weighted severity scoring
- install.sh and uninstall.sh for Unix/macOS/Linux
- install.ps1 for Windows PowerShell
- Agent frontmatter uses model sonnet, maxTurns 20, with example blocks
- Sub-skills set user-invocable false to avoid menu clutter
- Reference files follow RAG pattern (loaded on-demand per analysis)
- Quality gates: Broad Match safety, 3x Kill Rule, budget sufficiency, learning phase protection
