# 02 — Software Requirements Specification (SRS)

**Product:** ArticleCompare AI  
**Specification Version:** 1.0.0 (MVP)  
**Target Platform:** Web Application (Next.js 15 App Router)  
**Programming Language:** JavaScript / JSX (ECMAScript 2024)  
**Database Requirement:** None (Stateless Architecture / Client-side Cache)  

---

## 1. System Context & Overview

ArticleCompare AI operates as a lightweight, database-free web application. The frontend (Next.js React client components) interacts with Next.js server-side Route Handlers (`app/api/compare/route.js`). The server acts as an orchestrator: it ingests article drafts, sanitizes and validates input data, constructs an editorial prompt, securely invokes third-party AI LLMs (OpenAI or Google Gemini), validates the structured response against a formal JSON schema, deterministically computes overall scores, and returns the final report payload.

```
┌────────────────────────────────────────────────────────┐
│               Client Browser (Desktop/Mobile)          │
│  - React 19 / Next.js 15 Client Components             │
│  - Tailwind CSS + Lucide Icons                         │
│  - Optional localStorage Comparison Cache              │
└───────────────────────────┬────────────────────────────┘
                            │ HTTPS / JSON Payload
                            ▼
┌────────────────────────────────────────────────────────┐
│           Next.js 15 Server (Modular Monolith)          │
│  - Route Handler: POST /api/compare                    │
│  - Zod Request Validator                               │
│  - Text Normalization & Token Budget Check             │
│  - Prompt Orchestrator & Provider Adapter              │
│  - Deterministic Score Calculator                      │
│  - Zod Report Response Validator                       │
└───────────────┬────────────────────────┬───────────────┘
                │                        │
       Server-to-Server API     Server-to-Server API
                │                        │
                ▼                        ▼
     ┌────────────────────┐   ┌────────────────────┐
     │     OpenAI API     │   │  Google Gemini API │
     │   (gpt-4o / JSON)  │   │(gemini-2.5-flash)  │
     └────────────────────┘   └────────────────────┘
```

---

## 2. User Roles & Permission Model

### 2.1 MVP Release Scope
- **Anonymous Session User**: Any visitor can initiate, customize, execute, and export a comparison in the active browser session.
- **Zero Account Overhead**: No authentication (AuthN), authorization (AuthZ), session cookies, or user profile records are managed by the server.

### 2.2 Future Roadmap (Phase 2 & Phase 3)
- **Guest**: Rate-limited anonymous access.
- **Member (Authenticated)**: Can save comparison history to cloud workspaces.
- **Team Editor**: Can share reports and collaborate on final drafts.
- **Workspace Admin**: Can manage team seats, provider quotas, and API billing.

---

## 3. Functional Requirements

### 3.1 Comparison Setup & Input Management
- **FR-001 (Article Capacity)**: The system shall allow the user to select and configure between 2 and 4 article slots dynamically (default: 2 slots).
- **FR-002 (Draft Ingestion)**:
  - Each article slot shall accept an optional `title` (text, max 200 characters) and a mandatory `content` (body text).
  - The system shall support direct text pasting and file drag-and-drop upload for `.txt` and `.md` files.
  - File upload shall read file contents locally via the browser `FileReader` API and populate the respective editor without server roundtrip.
- **FR-003 (Input Constraints)**:
  - Minimum content length per article: **100 characters** (trimmed).
  - Maximum content length per article: **60,000 characters** (trimmed).
  - Maximum total comparison payload: **180,000 characters** across all articles.
- **FR-004 (Context Parameters)**: The system shall provide an optional context configuration panel accepting:
  - `primaryKeyword`: String (max 100 chars).
  - `searchIntent`: Enum (`Informational`, `Commercial`, `Transactional`, `Navigational`).
  - `audience`: String (max 150 chars).
  - `goal`: String (max 150 chars).
  - `language`: String (default: `English`).
  - `tone`: String (e.g., `Authoritative`, `Conversational`, `Technical`, `Journalistic`).
  - `guidelines`: String (max 500 chars, custom brand or publication guidelines).

### 3.2 Provider Orchestration & Analysis Engine
- **FR-005 (Provider Selection)**:
  - The system shall support OpenAI and Google Gemini via a unified adapter abstraction.
  - The UI shall permit the user to toggle between providers if multiple provider keys are active on the server.
  - If a provider is not configured or missing an API key, the system shall disable the option and display a clear configuration badge.
- **FR-006 (Rubric-Based Evaluation)**:
  - The system shall evaluate all submitted articles against the 10 standard dimensions:
    1. Search-Intent Satisfaction (15%)
    2. Accuracy & Evidence Quality (15%)
    3. Depth & Completeness (12%)
    4. Readability & Audience Fit (10%)
    5. Structure & Scannability (10%)
    6. Originality & Differentiation (8%)
    7. Practical Examples & Utility (8%)
    8. On-Page SEO Fundamentals (8%)
    9. Topical Coverage (8%)
    10. Publication Readiness (6%)
  - Each dimension shall be assigned an integer score from **0 to 100**.
  - The system shall calculate the weighted overall score deterministically on the server.
- **FR-007 (Winner Determination)**:
  - The system shall declare a decisive winner only when the leading article surpasses the runner-up by $\ge 5\text{ points}$ without unmitigated critical flaws.
  - If the score margin is $< 5\text{ points}$, the system shall declare a **"Close Race / Near Tie"** and present a comparative trade-off summary.
  - If all articles score $< 60$ or suffer from critical accuracy failures, the system shall declare **"No Clear Winner — Revisions Required"**.
- **FR-008 (Gap Detection)**:
  - The system shall generate article-specific content gaps.
  - The system shall synthesize a dedicated **Shared Gaps** list identifying vital topics missing across *all* submitted drafts.
- **FR-009 (Actionable Revision Plan)**:
  - The system shall generate a list of prioritized editorial improvement tasks.
  - Each task shall specify: `priority` (`High`, `Medium`, `Low`), `task` (concise action), and `reason` (textual justification).
- **FR-010 (On-Page SEO Generation)**:
  - The system shall generate suggested SEO metadata: `title` (max 60 chars), `metaDescription` (max 160 chars), `slug` (URL-safe string), `secondaryKeywords` (array), and `faqIdeas` (array of 3–5 search-aligned questions).

### 3.3 Presentation, State & Export
- **FR-011 (Structured Report Display)**:
  - The system shall render the evaluation as a modular, responsive dashboard:
    1. Executive Summary & Winner Banner (with confidence tag).
    2. Side-by-side Score Matrix (comparing all submitted articles A–D per dimension).
    3. Hybrid Recommendation Card (synthesizing the best sections).
    4. Article Detail Cards (strengths, weaknesses, gaps, editorial issues, and evidence citations).
    5. Prioritized Action Checklist (with interactive client-side completion checkboxes).
    6. SEO Metadata Preview card.
    7. Methodological Limitations & Verification Disclaimers.
- **FR-012 (Report Export)**:
  - The system shall provide single-click JSON export generating a clean, schema-valid `.json` file formatted for automated pipelines or archival.
  - The system shall support Markdown (`.md`) export formatted with markdown tables and GitHub-flavored callouts.
- **FR-013 (Error Feedback)**:
  - The system shall catch and translate all API, network, and validation failures into friendly, actionable UI banners.
  - Failed submissions shall preserve all user-entered text and context settings in the active inputs.
- **FR-014 (Client-Side History)**:
  - The system may save the last 5 completed comparison summaries to browser `localStorage`.
  - The user shall have explicit controls to view, reload, or clear local comparison history.

---

## 4. Non-Functional Requirements (NFRs)

### 4.1 NFR-001: Performance & Responsiveness
- **UI Responsiveness**: The main thread must never freeze during input typing or file reading. Input debouncing should ensure 60fps interaction.
- **Progress Visibility**: During analysis execution, the UI shall render a staged animated stepper indicating:
  1. *Validating inputs & tokens...*
  2. *Analyzing articles against 10-dimension rubric...*
  3. *Verifying evidence & computing final report...*
- **Request Timeout**: The server-side provider call shall enforce a maximum timeout of **60 seconds** before aborting and returning a `504 Gateway Timeout`.

### 4.2 NFR-002: Reliability & Fault Tolerance
- **Transient Retries**: The provider adapter shall implement bounded exponential backoff for transient HTTP errors (`429`, `503`), retrying at most twice.
- **Fail-Safe Processing**: If an AI provider returns malformed JSON, the server shall attempt one corrective parse; if parsing still fails, it returns a normalized `502 Bad Gateway` error rather than crashing the Node process.

### 4.3 NFR-003: Security
- **Server-Side Key Isolation**: OpenAI and Gemini API keys must reside exclusively in server environment variables (`OPENAI_API_KEY`, `GEMINI_API_KEY`). Never expose keys to client bundles.
- **Input Sanitization**: User-supplied text and titles must be treated strictly as untrusted data strings. No raw HTML rendering (`dangerouslySetInnerHTML`) without strict sanitization.
- **Prompt Injection Hardening**: Prompts must utilize strict boundary delimiters (e.g., `<article_content id="A">...</article_content>`) and explicit system rules instructing the LLM to ignore embedded commands.

### 4.4 NFR-004: Privacy & Data Retention
- **Zero Server-Side Storage**: The MVP server shall not write article content, drafts, or reports to disk or databases. All data remains in volatile Node.js memory for the duration of the HTTP request.
- **Provider Confidentiality Notice**: The UI must display an informative notice detailing which third-party AI provider processes the drafts.

### 4.5 NFR-005: Accessibility (WCAG 2.2 AA Compliance)
- **Contrast Ratios**: All text must achieve $\ge 4.5:1$ contrast against adjacent backgrounds; large headers and interactive buttons must achieve $\ge 3:1$.
- **Color Independence**: Score indications and winner badges must include explicit textual labels and numerical values; color alone must never communicate state.
- **Keyboard Navigation**: All interactive elements (tabs, file upload buttons, context accordion, checkboxes, export buttons) must be operable via `Tab`, `Enter`, and `Space` with visible focus rings.
- **Screen Reader Support**: Meaningful ARIA attributes (`aria-expanded`, `aria-live`, `aria-label`, `role="status"`) must accompany dynamic UI updates.

### 4.6 NFR-006: Code Quality & Maintainability
- **Strict JavaScript / JSX**: All code written in clean, modern JavaScript with JSX. No TypeScript compiler dependencies.
- **Runtime Type Contracts**: Complex payload boundaries (API requests, LLM outputs, client reports) must be validated with `Zod`.
- **Modular Monolith Architecture**: Strict decoupling between Presentation (`components/`), API Orchestration (`app/api/`), Domain Logic (`lib/comparison/`), and AI Adapters (`lib/ai/`).

### 4.7 NFR-007: Compatibility
- Fully functional on all modern evergreen browsers: Google Chrome, Microsoft Edge, Mozilla Firefox, Apple Safari (desktop and mobile).
- Responsive viewports: Mobile (360px–420px), Tablet (768px–1024px), Desktop (1280px–1920px).

### 4.8 NFR-008: Observability
- Server route handlers shall emit structured JSON operational logs including: `requestId`, `timestamp`, `provider`, `model`, `articleCount`, `totalCharacters`, `durationMs`, and `status`.
- Article content and API keys must be explicitly masked or omitted from application logs.

---

## 5. Data Validation Schemas (Zod Specification)

### 5.1 Request Validation Schema (`lib/validation/compare-request.schema.js`)
```javascript
import { z } from 'zod';

export const ArticleInputSchema = z.object({
  id: z.enum(['A', 'B', 'C', 'D']),
  title: z.string().trim().max(200, 'Title cannot exceed 200 characters').optional().default(''),
  content: z.string().trim()
    .min(100, 'Article content must be at least 100 characters')
    .max(60000, 'Article content cannot exceed 60,000 characters')
});

export const CompareRequestSchema = z.object({
  provider: z.enum(['openai', 'gemini']).optional().default('gemini'),
  articles: z.array(ArticleInputSchema)
    .min(2, 'At least 2 articles are required for comparison')
    .max(4, 'At most 4 articles can be compared simultaneously'),
  primaryKeyword: z.string().trim().max(100).optional(),
  searchIntent: z.enum(['Informational', 'Commercial', 'Transactional', 'Navigational']).optional(),
  audience: z.string().trim().max(150).optional(),
  goal: z.string().trim().max(150).optional(),
  language: z.string().trim().max(50).optional().default('English'),
  tone: z.string().trim().max(50).optional(),
  guidelines: z.string().trim().max(500).optional()
});
```

### 5.2 Report Validation Schema (`lib/ai/report-schema.js`)
```javascript
import { z } from 'zod';

const DimensionScoreSchema = z.number().int().min(0).max(100);

export const ArticleEvaluationSchema = z.object({
  articleId: z.enum(['A', 'B', 'C', 'D']),
  title: z.string(),
  overallScore: DimensionScoreSchema,
  scores: z.object({
    searchIntent: DimensionScoreSchema,
    accuracyEvidence: DimensionScoreSchema,
    depthCompleteness: DimensionScoreSchema,
    readability: DimensionScoreSchema,
    structure: DimensionScoreSchema,
    originality: DimensionScoreSchema,
    examples: DimensionScoreSchema,
    onPageSEO: DimensionScoreSchema,
    topicalCoverage: DimensionScoreSchema,
    publicationReadiness: DimensionScoreSchema
  }),
  strengths: z.array(z.string().min(5)).min(1),
  weaknesses: z.array(z.string().min(5)).min(1),
  contentGaps: z.array(z.string().min(5)).min(1),
  editorialIssues: z.array(z.string().min(5)).default([]),
  seoSuggestions: z.array(z.string().min(5)).default([])
});

export const ComparisonMatrixRowSchema = z.object({
  criterion: z.string(),
  scores: z.record(z.enum(['A', 'B', 'C', 'D']), DimensionScoreSchema),
  analysis: z.string().min(10)
});

export const PriorityImprovementSchema = z.object({
  priority: z.enum(['High', 'Medium', 'Low']),
  task: z.string().min(5),
  reason: z.string().min(10)
});

export const CompareReportSchema = z.object({
  executiveSummary: z.string().min(20),
  winner: z.object({
    articleId: z.enum(['A', 'B', 'C', 'D']).nullable(),
    title: z.string().nullable(),
    reason: z.string().min(10),
    confidence: z.enum(['High', 'Medium', 'Low'])
  }),
  hybridRecommendation: z.string().min(20),
  articles: z.array(ArticleEvaluationSchema).min(2).max(4),
  comparisonMatrix: z.array(ComparisonMatrixRowSchema).min(10),
  sharedGaps: z.array(z.string().min(5)).min(1),
  priorityImprovements: z.array(PriorityImprovementSchema).min(1),
  seo: z.object({
    title: z.string(),
    metaDescription: z.string(),
    slug: z.string(),
    primaryKeyword: z.string(),
    secondaryKeywords: z.array(z.string()),
    faqIdeas: z.array(z.string())
  }),
  limitations: z.array(z.string()).min(1)
});
```
