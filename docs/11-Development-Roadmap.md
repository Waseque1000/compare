# 11 — Development Roadmap

**Product:** ArticleCompare AI  
**Current Phase:** Phase 0 $\to$ Phase 1 (MVP Delivery)  
**Lifecycle Status:** Active Implementation  

---

## 1. Roadmap Overview & Phasing

```mermaid
gantt
    title ArticleCompare AI — Phased Implementation Timeline
    dateFormat  YYYY-MM-DD
    section Phase 0 (Baseline)
    Review Starter & Align Model Defaults     :done, p0_1, 2026-11-01, 3d
    Zod Schemas & Validation Contracts        :done, p0_2, 2026-11-04, 3d
    Deterministic Scoring Math                :done, p0_3, 2026-11-07, 2d
    section Phase 1 (MVP)
    Workspace UI & 2-4 Slot Controls         :active, p1_1, 2026-11-10, 5d
    Provider Adapters & Timeout Logic        :active, p1_2, 2026-11-15, 4d
    Results UI & Comparison Matrix           :p1_3, 2026-11-19, 5d
    Export JSON & A11y Polish                :p1_4, 2026-11-24, 3d
    QA Test Suite & Release Verification     :p1_5, 2026-11-27, 4d
    section Phase 2 (Imports & Polish)
    Markdown Export & LocalStorage History   :p2_1, 2026-12-05, 5d
    URL Ingestion with SSRF Protection       :p2_2, 2026-12-10, 7d
    PDF/DOCX Document Parsers                :p2_3, 2026-12-17, 7d
    section Phase 3 (Optional Cloud)
    Optional Auth & Cloud Workspace (If Req) :p3_1, 2027-01-10, 14d
```

---

## 2. Detailed Phase Breakdown & Milestones

### Phase 0 — Baseline Review & Stabilization (Completed)
- **Objective**: Stabilize existing starter code and establish strict contracts.
- **Key Deliverables**:
  - Audit existing Next.js starter structure.
  - Correct Gemini and OpenAI model identifiers.
  - Define formal Zod schemas for request validation and report contracts.
  - Implement server-side deterministic scoring logic.
  - Isolate provider calls into decoupled adapters.

---

### Phase 1 — MVP Completion (Current Focus)
- **Objective**: Deliver a production-ready, reliable, database-free comparison workspace.
- **Key Deliverables**:
  - **M-1.1**: Polish 2–4 slot article editor with live character counters and file upload buttons.
  - **M-1.2**: Implement collapsible context drawer (Keyword, Intent, Audience, Goal, Tone).
  - **M-1.3**: Wire dual provider adapters (`gemini-2.5-flash`, `gpt-4o`) with 60-second timeouts.
  - **M-1.4**: Render Executive Summary, Winner Banner, and 10-Dimension Comparative Matrix.
  - **M-1.5**: Implement deep-dive Article Cards (Strengths, Weaknesses, Gaps, Evidence).
  - **M-1.6**: Implement Prioritized Action Plan and SEO Metadata preview.
  - **M-1.7**: One-click JSON report export.
  - **M-1.8**: Accessibility review (WCAG 2.2 AA) and responsive layout optimization.
- **Exit Criteria**: All 11 PRD acceptance criteria satisfied and QA test suite passing.

---

### Phase 2 — Ingestion & Usability Enhancements
- **Objective**: Expand ingestion options and streamline report sharing.
- **Key Deliverables**:
  - **M-2.1**: Browser-local comparison history via `localStorage` (last 5 reports).
  - **M-2.2**: Formatted Markdown (`.md`) report export.
  - **M-2.3**: Side-by-side synchronized reader for reviewing original drafts.
  - **M-2.4**: Safe URL ingestion service with strict SSRF defense (DNS verification, private IP blocking).
  - **M-2.5**: Client-side preview and inline editor for extracted URL/file text.
- **Exit Criteria**: URL ingestion passes SSRF security test suite with zero IP bypass vulnerabilities.

---

### Phase 3 — Optional Cloud Expansion (Only if Requirements Change)
- **Objective**: Support multi-device cloud persistence and user accounts if required by business expansion.
- **Key Deliverables**:
  - User authentication (NextAuth / Better Auth) with email and OAuth providers.
  - Cloud database (PostgreSQL via Neon / Supabase).
  - Cloud project folders, tag management, and shared report URLs.
- **Strict Rule**: Do not begin Phase 3 unless authenticated persistence becomes a confirmed business requirement.

---

### Phase 4 — Team Workspaces & Enterprise Governance
- **Objective**: Enable agency and publication team collaboration.
- **Key Deliverables**:
  - Organization accounts with Role-Based Access Control (Admin, Editor, Contributor).
  - Editorial review comments and threaded discussion on comparison reports.
  - Workspace usage metering and team API quota enforcement.
  - Audit logging for enterprise compliance.

---

### Phase 5 — Research & Editorial Intelligence
- **Objective**: Expand from text-only evaluation to evidence-assisted research.
- **Key Deliverables**:
  - Live search engine retrieval integration (Google Search API / Perplexity / Tavily) to verify factual assertions.
  - Automated claim-to-source mapping with external web citations.
  - Hybrid final-draft interactive editor synthesizing winning sections directly inside the app.
  - Direct CMS publishing hooks (WordPress REST API, Webflow CMS API).
