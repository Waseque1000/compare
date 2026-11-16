# 09 — Testing and QA Plan

**Product:** ArticleCompare AI  
**Quality Objective:** Zero critical bugs, 100% schema compliance, and reproducible editorial evaluation  
**Test Stack:** Vitest (Unit/Integration) | React Testing Library (Components) | Playwright (E2E) | Zod (Contracts)  

---

## 1. Test Strategy & Quality Pyramid

ArticleCompare AI employs a multi-tiered testing strategy designed to validate deterministic code, UI components, API endpoints, and probabilistic AI evaluation quality:

```
            /  E2E Tests  \          -> Playwright (Full workflow 2-4 articles)
           / Component UI  \         -> React Testing Library (Workspace & Report)
          / Integration API \        -> Vitest (Route handler & Provider adapters)
         / Unit & Logic Tests\       -> Vitest (Scoring math, winner rules, schemas)
        / AI Quality Baseline \      -> Benchmark Suite (Human-evaluated consistency)
```

---

## 2. Core Functional Test Catalog

| ID | Test Scenario | Execution Step | Expected Result | Pass Criteria |
|---|---|---|---|---|
| **TC-001** | Submit 2 Valid Articles | Input 2 articles ($\ge 100$ chars) and submit. | Status `200 OK`. Report returned with articles `A` and `B` evaluated across all 10 dimensions. | All matrix rows contain scores for `A` and `B`. |
| **TC-002** | Submit 3 Valid Articles | Configure 3 slots, input valid text, and submit. | Status `200 OK`. Report returned containing articles `A`, `B`, and `C`. | Matrix rows contain scores for `A`, `B`, and `C`. |
| **TC-003** | Submit 4 Valid Articles | Configure 4 slots, input valid text, and submit. | Status `200 OK`. Report returned containing articles `A`, `B`, `C`, and `D`. | Matrix rows contain scores for `A`, `B`, `C`, and `D`. |
| **TC-004** | Submit 1 Article | Attempt to submit payload with only 1 article. | Status `400 Bad Request`. Request rejected before calling AI provider. | Validation error message displayed. |
| **TC-005** | Submit 5 Articles | Attempt to submit payload with 5 articles. | Status `400 Bad Request`. Request rejected before calling AI provider. | Validation error message displayed. |
| **TC-006** | Article Under Min Length | Input article with 60 characters. | Status `400 Bad Request`. Ingestion blocked client-side & server-side. | Inline validation error displayed. |
| **TC-007** | Article Exceeds Max Length | Input article with 65,000 characters. | Status `400 Bad Request`. Input blocked. | Error message: "Max length 60,000 characters". |
| **TC-008** | Empty Article Title | Submit article with valid content but empty title. | Status `200 OK`. Server assigns fallback title (e.g., "Draft A"). | Valid report rendered without missing title crashes. |
| **TC-009** | Unsupported Provider | Dispatch request with `"provider": "anthropic"`. | Status `400 Bad Request`. Request rejected by provider allowlist. | Error code: `INVALID_PROVIDER`. |
| **TC-010** | Missing Provider API Key | Simulate request when server has no API key set. | Status `503 Service Unavailable`. Safe configuration error. | No server stack trace exposed. |
| **TC-011** | Malformed AI Response | Mock upstream provider returning truncated JSON. | Server attempts repair; returns `502 Bad Gateway` if unrecoverable. | UI renders helpful "Analysis failed" error banner. |
| **TC-012** | Provider Timeout | Simulate provider hanging for $> 60$ seconds. | Request aborted at 60s; returns `504 Gateway Timeout`. | User text preserved in workspace inputs. |
| **TC-013** | Rate Limit (429) | Simulate upstream provider returning 429 quota error. | Server translates to normalized 429 response. | UI displays: "Provider rate limit reached. Retry in 30s." |
| **TC-014** | Report Export (JSON) | Click "Export JSON" on rendered report. | Browser triggers download of schema-valid `.json` report file. | File contains valid JSON conforming to report schema. |
| **TC-015** | Duplicate Submit Prevention | Rapidly double-click "Compare Articles" button. | UI locks button immediately on first click; single HTTP call sent. | Exactly 1 API call registered in network log. |

---

## 3. AI Quality & Evaluation Benchmarks

Because LLM outputs are probabilistic, the test suite includes a deterministic benchmark test harness (`tests/ai-benchmark/`) evaluated against human-reviewed editorial baselines:

### 3.1 Score Consistency Across Repeated Runs
- **Test**: Run the same pair of benchmark articles 5 times with temperature $0.2$.
- **Criteria**: The overall score variance must not exceed $\pm 3\text{ points}$, and the winner decision must remain identical across all 5 runs.

### 3.2 Length Bias Resistance
- **Test**: Compare a concise, high-density 800-word article against a 2,500-word padded version with repeated phrases and superficial filler.
- **Criteria**: The concise article must score higher in Readability, Structure, and Search-Intent Satisfaction. Overall score must favor substance over length.

### 3.3 Keyword Stuffing Penalty
- **Test**: Compare an authentic, natural draft against a draft unnaturally stuffed with the primary keyword in every paragraph.
- **Criteria**: The stuffed draft must receive an explicit penalty in Readability ($< 60$) and On-Page SEO ($< 65$), with a weakness citing keyword stuffing.

### 3.4 Close-Race / Tie Detection
- **Test**: Submit two high-quality articles with virtually identical strengths and overall scores within 2 points of each other.
- **Criteria**: The engine must declare a **"Close Race / Near Tie"** rather than forcing a low-confidence winner, outlining comparative trade-offs.

### 3.5 Zero Hallucinated Ranking Claims
- **Test**: Inspect 50 generated reports across diverse niches.
- **Criteria**: Exactly 0 reports may state or imply that the article will achieve a specific Google or Bing search ranking.

---

## 4. Accessibility & UI Quality Gates

### 4.1 Automated Accessibility Tests (axe-core / Playwright)
- Zero WCAG 2.2 Level AA violations across all 7 product screens.
- All interactive controls navigable via `Tab`, `Shift+Tab`, `Space`, and `Enter`.
- All text meets minimum contrast ratio of $4.5:1$.

### 4.2 Cross-Browser & Viewport Matrix
- Desktop: Chrome (1440px), Edge (1440px), Firefox (1440px), Safari (1440px).
- Tablet: iPad / Safari (768px), Chrome Tablet (1024px).
- Mobile: iPhone 14 / Safari (390px), Pixel 7 / Chrome (412px).

---

## 5. Release Gating Checklist

Before any production deployment or PR merge to `main`, the build must pass the following release gates:
- [ ] 100% of unit and integration tests passing (`npm run test`).
- [ ] 100% of Playwright E2E tests passing (`npm run test:e2e`).
- [ ] ESLint passes with zero errors (`npm run lint`).
- [ ] Production build succeeds cleanly (`npm run build`).
- [ ] Zero secrets or API keys detected in git history or client bundle files.
- [ ] Schema validation gate: 100% of test reports validate against Zod schemas.
