# 12 — Project Structure and Architecture Decision Records (ADRs)

**Product:** ArticleCompare AI  
**Framework:** Next.js 15 (App Router)  
**Language:** JavaScript / JSX  
**UI Stack:** Tailwind CSS + Radix UI Primitives (shadcn/ui style) + Lucide Icons  

---

## 1. Target Folder & File Architecture

```text
articlecompare-ai/
├── app/
│   ├── api/
│   │   └── compare/
│   │       └── route.js                 # POST /api/compare route handler
│   ├── (marketing)/
│   │   └── page.jsx                     # Landing & product introduction
│   ├── (workspace)/
│   │   ├── compare/
│   │   │   └── page.jsx                 # Primary comparison workspace
│   │   └── history/
│   │       └── page.jsx                 # Client-side history viewer
│   ├── globals.css                      # Tailwind v4 / CSS variables & tokens
│   ├── layout.jsx                       # Root application shell & font imports
│   └── not-found.jsx                    # 404 error page
│
├── components/
│   ├── layout/
│   │   ├── Header.jsx                   # Sticky top navigation bar
│   │   ├── Footer.jsx                   # Minimalist footer & disclaimer
│   │   └── Container.jsx                # Responsive max-width wrapper
│   │
│   ├── workspace/
│   │   ├── WorkspaceShell.jsx           # Main stateful workspace orchestrator
│   │   ├── SlotCountSelector.jsx        # 2 / 3 / 4 article toggle buttons
│   │   ├── ArticleSlotCard.jsx          # Individual draft input card
│   │   ├── ArticleEditor.jsx            # Textarea with paste & char counters
│   │   ├── FileUploadButton.jsx         # Hidden file input (.txt / .md)
│   │   ├── ContextDrawer.jsx            # Collapsible context panel
│   │   ├── ProviderSelector.jsx         # Gemini / OpenAI model toggle
│   │   └── LoadingStepper.jsx           # 3-stage animated analysis loader
│   │
│   ├── report/
│   │   ├── ReportDashboard.jsx          # Master report renderer
│   │   ├── WinnerBanner.jsx             # Decisive / Near-Tie / Revision banner
│   │   ├── ExecutiveSummaryCard.jsx     # Summary and hybrid recommendation
│   │   ├── ComparisonMatrix.jsx         # 10-dimension side-by-side table
│   │   ├── ArticleDetailCard.jsx        # Tabbed deep-dive for slots A-D
│   │   ├── DimensionScoreBar.jsx        # Horizontal score progress bar
│   │   ├── SharedGapsCard.jsx           # Missing topics across all drafts
│   │   ├── ImprovementPlanCard.jsx      # Prioritized interactive checklist
│   │   ├── SeoMetadataCard.jsx          # SERP preview, title, meta, FAQ
│   │   ├── LimitationsNotice.jsx        # Editorial disclaimers & methodology
│   │   └── ExportReportModal.jsx        # JSON and Markdown download modal
│   │
│   └── ui/                              # shadcn / Radix accessible primitives
│       ├── button.jsx
│       ├── card.jsx
│       ├── badge.jsx
│       ├── tabs.jsx
│       ├── dialog.jsx
│       ├── accordion.jsx
│       ├── tooltip.jsx
│       ├── progress.jsx
│       └── textarea.jsx
│
├── lib/
│   ├── ai/
│   │   ├── providers/
│   │   │   ├── base.adapter.js          # Unified provider contract
│   │   │   ├── gemini.adapter.js        # Google Gemini API adapter
│   │   │   └── openai.adapter.js        # OpenAI API adapter
│   │   ├── provider-registry.js         # Provider configurations & allowlists
│   │   ├── prompt-builder.js            # Versioned system & user prompt strings
│   │   └── report-schema.js             # Zod schema for LLM structured output
│   │
│   ├── comparison/
│   │   ├── normalize-articles.js        # ID assignment, trim, char/word counts
│   │   ├── score-calculator.js          # Deterministic weighted math engine
│   │   ├── winner-selector.js           # 5-point threshold & tie decision logic
│   │   └── report-validator.js          # Post-LLM data verification
│   │
│   ├── validation/
│   │   └── compare-request.schema.js    # Zod schema for incoming HTTP request
│   │
│   ├── security/
│   │   └── sanitization.js              # Text escaping & injection defenses
│   │
│   └── utils/
│       ├── storage.js                   # Client-side localStorage cache helpers
│       ├── export-formatter.js          # JSON & Markdown report exporters
│       └── cn.js                        # Tailwind class merge utility (clsx + twMerge)
│
├── docs/                                # Complete project documentation pack
├── public/                              # Static icons, OG images, favicon
├── tests/
│   ├── unit/                            # Vitest unit tests (math, validation)
│   ├── integration/                     # Route handler & adapter mock tests
│   └── e2e/                             # Playwright browser end-to-end flows
│
├── .env.example                         # Environment variable placeholders
├── .gitignore                           # Git ignore rules
├── jsconfig.json                        # Path alias (@/*) configuration
├── next.config.mjs                      # Next.js production configuration
├── package.json                         # Project dependencies & scripts
└── README.md                            # Quickstart & project overview
```

---

## 2. Naming & Code Conventions

1. **React Components**: PascalCase filenames (`ArticleSlotCard.jsx`, `WinnerBanner.jsx`).
2. **Pure Logic & Utilities**: kebab-case filenames (`score-calculator.js`, `prompt-builder.js`).
3. **Route Handlers**: Strict Next.js standard `route.js`.
4. **File Extensions**: `.jsx` strictly for files rendering React components; `.js` strictly for pure logic and server modules.
5. **Constants**: UPPER_SNAKE_CASE for immutable constants (`RUBRIC_WEIGHTS`, `MAX_ARTICLE_CHARS`).
6. **Server Boundary Enforcement**: Modules accessing environment variables or provider SDKs must be isolated under `lib/ai/` and never imported by files tagged with `'use client'`.

---

## 3. Architecture Decision Records (ADRs)

### ADR-001: Next.js Modular Monolith Architecture
- **Status**: Accepted
- **Context**: We need to deliver a fast, full-stack application supporting both interactive client UI and secure server-side AI provider orchestration without infrastructure sprawl.
- **Decision**: Adopt a single Next.js 15 App Router project organizing server routes, business logic, and UI components in isolated directories.
- **Consequences**:
  - Eliminates the need for a separate backend API repo or microservices cluster.
  - Simplifies local setup, testing, and continuous deployment via Vercel.
  - Requires strict directory boundaries to prevent mixing server secrets with client state.

---

### ADR-002: JavaScript / JSX (No TypeScript)
- **Status**: Accepted
- **Context**: The product stakeholder explicitly specified that the codebase must be written in clean, modern JavaScript/JSX.
- **Decision**: Code is authored in ECMAScript 2024 JavaScript with JSX. No `.ts` or `.tsx` files are introduced.
- **Consequences**:
  - Runtime validation becomes mission-critical. We employ `Zod` at all external data boundaries (incoming HTTP requests and outgoing LLM structured responses) to guarantee complete data contract safety.
  - Development is fast, friction-free, and adheres directly to project constraints.

---

### ADR-003: Dual AI Provider Adapter Strategy
- **Status**: Accepted
- **Context**: Relying on a single AI vendor introduces availability risks, vendor lock-in, and price inflexibility.
- **Decision**: Implement a decoupled Provider Adapter interface supporting both Google Gemini (`gemini-2.5-flash`) and OpenAI (`gpt-4o`) via a unified execution signature (`runProvider`).
- **Consequences**:
  - The business can toggle between providers via environment variables without changing a single line of business logic.
  - Protects the application against single-provider outages or unexpected price hikes.

---

### ADR-004: Stateless Server Architecture (No Database for MVP)
- **Status**: Accepted
- **Context**: The MVP is designed to validate core editorial value without the overhead of user account systems, database migrations, connection pools, and data retention compliance.
- **Decision**: The MVP operates completely database-free. Comparison requests are processed in memory and returned to the client. Session history is optionally stored locally in browser `localStorage`.
- **Consequences**:
  - Zero database infrastructure costs and zero operational migration risks.
  - Maximizes user privacy; drafts are not permanently stored on any server.
  - Re-evaluating past comparisons on a different device is not supported in the MVP.

---

### ADR-005: Rubric-Based Editorial Evaluation vs. Search Ranking Predictions
- **Status**: Accepted
- **Context**: AI tools that promise "Google #1 ranking scores" are scientifically indefensible because search engine rankings depend on external authority, backlinks, page speed, and user engagement signals not present in raw text.
- **Decision**: The application evaluates drafts strictly against transparent editorial and on-page SEO rubrics.
- **Consequences**:
  - Builds deep trust with professional editors and enterprise content teams.
  - Clear disclaimers prevent misleading users about ranking guarantees.

---

### ADR-006: Human Remains the Sovereign Decision-Maker
- **Status**: Accepted
- **Context**: Editorial judgment involves brand voice, organizational politics, legal risk, and human empathy that AI cannot autonomously determine.
- **Decision**: The engine produces *recommendations, diagnostic evidence, and synthesis proposals*, but never makes autonomous publication decisions.
- **Consequences**:
  - The user interface is structured around decision-support rather than automated publishing.
  - Positions ArticleCompare AI as an indispensible co-pilot for professional editors.
