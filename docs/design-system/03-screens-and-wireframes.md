# Design System: Screen Layouts & High-Fidelity Wireframes

**Product:** ArticleCompare AI  
**Design Intelligence:** `ui-ux-pro-max` | `ui-styling`  
**Layout Standards:** Desktop (1440px), Tablet (768px–1024px), Mobile (360px–412px)  

---

## 1. Master Layout & Grid Framework

ArticleCompare AI utilizes a 12-column responsive grid system centered within a maximum container width of `1440px` (`max-w-7xl`):
- **Desktop ($\ge 1280\text{px}$)**: 12 columns, 24px gutters, 32px side margins. Side-by-side multi-column comparison.
- **Tablet ($768\text{px} - 1024\text{px}$)**: 8 columns, 16px gutters, 24px side margins. 2-column or tabbed slot layouts.
- **Mobile ($360\text{px} - 412\text{px}$)**: 4 columns, 12px gutters, 16px side margins. Single-column stacked cards with sticky action bars.

---

## 2. Screen B: Comparison Workspace (Desktop Wireframe 1440px)

```text
+----------------------------------------------------------------------------------------------------+
|  [Logo] ArticleCompare AI   [v1.0 MVP]               [How It Works]   [History (3)]   [Gemini API] |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  HERO HEADER                                                                                       |
|  Compare Article Drafts with Editorial Intelligence                                                |
|  Evaluate 2–4 drafts against a 10-dimension rubric. Uncover content gaps and verify intent fit.     |
|                                                                                                    |
|  +----------------------------------------------------------------------------------------------+  |
|  |  SLOT CONTROLLER:  [ [2 Articles] (Active) ]   [ [3 Articles] ]   [ [4 Articles] ]           |  |
|  +----------------------------------------------------------------------------------------------+  |
|                                                                                                    |
|  +--------------------------------------------+    +--------------------------------------------+  |
|  | ARTICLE DRAFT A               [Clear Slot] |    | ARTICLE DRAFT B               [Clear Slot] |  |
|  +--------------------------------------------+    +--------------------------------------------+  |
|  | Title: [ Clean Code Architecture in Next.js]    | Title: [ Next.js 15: Production Guide      ]  |
|  |                                            |    |                                            |  |
|  | Ingestion: [ Paste Text ]  [ Upload File ] |    | Ingestion: [ Paste Text ]  [ Upload File ] |  |
|  |                                            |    |                                            |  |
|  | +----------------------------------------+ |    | +----------------------------------------+ |  |
|  | | Building scalable React applications   | |    | | Next.js has become the de facto        | |  |
|  | | requires strict adherence to modular  | |    | | framework for full-stack apps. In this | |  |
|  | | patterns. In this guide, we explore   | |    | | deep dive, we explore App Router       | |  |
|  | | server components and route handlers.. | |    | | directory layouts, server actions...   | |  |
|  | |                                        | |    | |                                        | |  |
|  | |                                        | |    | |                                        | |  |
|  | +----------------------------------------+ |    | +----------------------------------------+ |  |
|  | [OK] 1,420 words | 8,950 chars (Min 100)   |    | [OK] 1,850 words | 11,200 chars (Min 100)  |  |
|  +--------------------------------------------+    +--------------------------------------------+  |
|                                                                                                    |
|  +----------------------------------------------------------------------------------------------+  |
|  | [v] OPTIONAL EDITORIAL CONTEXT & SEO GOALS                                   (3 Configured)  |  |
|  +----------------------------------------------------------------------------------------------+  |
|  |  Primary Keyword: [ Next.js clean architecture          ]   Search Intent: [ Informational v ]  |  |
|  |  Target Audience: [ Senior frontend & fullstack devs    ]   Content Tone:  [ Authoritative v ]  |  |
|  |  Editorial Goal:  [ Select definitive guide for blog   ]   Language:      [ English (US)  v ]  |  |
|  |  Brand Guidelines: [ Emphasize Next.js 15 App Router best practices. Modular monolith.     ]  |  |
|  +----------------------------------------------------------------------------------------------+  |
|                                                                                                    |
|                                  [ COMPARE ARTICLES (Ctrl+Enter) -> ]                              |
|                          Server-side analysis | 10 Dimensions | ~15s execution                     |
+----------------------------------------------------------------------------------------------------+
```

---

## 3. Screen C: Analysis Loading Stepper (Wireframe)

```text
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|                               +-------------------------------------+                              |
|                               |     ANALYZING EDITORIAL DRAFTS      |                              |
|                               +-------------------------------------+                              |
|                               |                                     |                              |
|                               |  [v] Step 1: Input Validation       |                              |
|                               |      Sanitizing text & tokens...    |                              |
|                               |                                     |                              |
|                               |  [*] Step 2: 10-Dimension Evaluation|                              |
|                               |      (Evaluating drafts via Gemini) |                              |
|                               |      ||||||||||||||...... [65%]     |                              |
|                               |                                     |                              |
|                               |  [ ] Step 3: Synthesis & Evidence   |                              |
|                               |      Verifying score calculations...|                              |
|                               |                                     |                              |
|                               |  Evaluating in parallel.            |                              |
|                               |  Average duration: 12-20 seconds.   |                              |
|                               +-------------------------------------+                              |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 4. Screen D: Results Overview & Winner Banner (Wireframe)

```text
+----------------------------------------------------------------------------------------------------+
|  🏆 WINNER RECOMMENDATION                                                 [ Confidence: HIGH ]      |
|  ------------------------------------------------------------------------------------------------  |
|  Article B: Next.js 15: Production Guide                                                           |
|  Overall Score: 85/100 (+7 point decisive advantage over Article A)                               |
|                                                                                                    |
|  Editorial Rationale:                                                                              |
|  Article B delivers superior structural depth, complete route handler implementations, and        |
|  actionable error boundary examples. Article A has higher readability but omits critical backend   |
|  architecture patterns needed for senior engineers.                                                |
|                                                                    [ Re-run Analysis ] [ Export v ] |
+----------------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------------+
|  📊 EXECUTIVE SUMMARY & HYBRID SYNTHESIS STRATEGY                                                  |
|  ------------------------------------------------------------------------------------------------  |
|  Executive Summary:                                                                                |
|  Both drafts demonstrate solid technical execution. Article B provides the best structural        |
|  framework and production code patterns. Article A offers superior component readability.         |
|                                                                                                    |
|  Hybrid Recommendation:                                                                            |
|  Use Article B as the foundational draft. Transpose Section 3 (Component Composition) from         |
|  Article A into Article B's architecture section to create the definitive guide.                  |
+----------------------------------------------------------------------------------------------------+
```

---

## 5. Screen E: 10-Dimension Comparative Matrix (Wireframe)

```text
+----------------------------------------------------------------------------------------------------+
|  📋 10-DIMENSION COMPARATIVE MATRIX                                      [ Download Full Report ]  |
+-----------------------------------+------------+------------+--------------------------------------+
|  Evaluation Criterion             |  Draft A   |  Draft B   |  Comparative Editorial Rationale     |
+-----------------------------------+------------+------------+--------------------------------------+
|  Search-Intent Satisfaction (15%) |     82     |   [88] *   |  B answers deployment intent directly|
|  Accuracy & Evidence (15%)        |     80     |   [85] *   |  B includes verified error boundaries|
|  Depth & Completeness (12%)       |     72     |   [87] *   |  B covers middleware & cache borders |
|  Readability & Fit (10%)          |   [88] *   |     82     |  A has superior conversational flow  |
|  Structure & Scannability (10%)   |     78     |   [86] *   |  B leverages visual code callouts    |
|  Originality (8%)                 |     70     |   [84] *   |  B introduces custom domain patterns |
|  Practical Examples (8%)          |     82     |   [85] *   |  Both strong; B provides full routes |
|  On-Page SEO Fundamentals (8%)    |     75     |   [82] *   |  B aligns headings with keywords     |
|  Topical Coverage (8%)            |     74     |   [86] *   |  B covers broader ecosystem aspects  |
|  Publication Readiness (6%)       |     80     |   [84] *   |  B ready with minor Intro edits      |
+-----------------------------------+------------+------------+--------------------------------------+
|  OVERALL SCORE (Weighted Sum)     |     78     |   [85] *   |  Decisive Advantage: Draft B (+7 pts)|
+-----------------------------------+------------+------------+--------------------------------------+
|  * Brackets denote highest scoring draft in category.                                              |
+----------------------------------------------------------------------------------------------------+
```

---

## 6. Screen F: Detailed Article Diagnostic Tabs (Wireframe)

```text
+----------------------------------------------------------------------------------------------------+
|  ARTICLE DIAGNOSTIC DEEP-DIVE                                                                      |
|  +--------------------+  +--------------------+                                                    |
|  | [Article A (78)]   |  | [Article B (85)] * |                                                    |
|  +--------------------+--+--------------------+--------------------------------------------------+ |
|                                                                                                    |
|  Article B: Next.js 15: Production Guide                                                           |
|  Overall Score: 85/100 | Publication Readiness: 84/100                                              |
|                                                                                                    |
|  DIMENSION SCORES:                                                                                 |
|  Search Intent:      [========================] 88/100                                             |
|  Accuracy/Evidence:  [======================= ] 85/100                                             |
|  Depth/Completeness: [========================] 87/100                                             |
|  Readability:        [======================  ] 82/100                                             |
|  Structure:          [======================= ] 86/100                                             |
|                                                                                                    |
|  [v] Key Strengths (3 Identified)                                                                  |
|      - Exemplary modular folder structure demonstrating domain isolation.                          |
|      - Comprehensive route handler implementation with Zod schema validation.                      |
|      - Up-to-date Next.js 15 server action error handling patterns.                                |
|                                                                                                    |
|  [!] Weaknesses & Areas for Polish (2 Identified)                                                  |
|      - Dense introductory paragraphs may cause reader fatigue before code sections.                |
|      - Some complex code blocks lack explanatory annotations.                                      |
|                                                                                                    |
|  [?] Content Gaps (1 Identified)                                                                   |
|      - Omits comparison between App Router caching and legacy Pages Router patterns.               |
|                                                                                                    |
|  [#] Verified Textual Citations:                                                                   |
|      "Section 2: Domain Architecture" - Demonstrates valid module boundaries.                      |
+----------------------------------------------------------------------------------------------------+
```

---

## 7. Screen G: Action Plan & SEO SERP Preview (Wireframe)

```text
+----------------------------------------------------------------------------------------------------+
|  PRIORITIZED EDITORIAL ACTION PLAN                         Filter: [ All ] [ High ] [ Med ] [ Low ]|
+----------------------------------------------------------------------------------------------------+
|  [ ] HIGH PRIORITY                                                                                 |
|      Task: Add dedicated section on caching & revalidation tags (`next/cache`).                    |
|      Reason: Caching is the most common Next.js 15 pitfall; omitting it leaves readers vulnerable. |
|                                                                                                    |
|  [ ] MEDIUM PRIORITY                                                                               |
|      Task: Break down dense paragraphs in Article B's Introduction into bulleted takeaways.        |
|      Reason: Improves early engagement and reduces cognitive bounce rates.                         |
|                                                                                                    |
|  [ ] LOW PRIORITY                                                                                  |
|      Task: Insert security warning cautioning against exposing server utilities in client code.   |
|      Reason: Reinforces defensive engineering best practices.                                      |
+----------------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------------+
|  GOOGLE SERP SIMULATOR & SEO METADATA                                                              |
+----------------------------------------------------------------------------------------------------+
|  Search Snippet Preview:                                                                           |
|  https://yourdomain.com > nextjs-clean-architecture-guide                                          |
|  Next.js Architecture Guide: Clean Code for Production                                             |
|  Master Next.js App Router architecture. Learn modular monolith folder structures, server         |
|  components, and route handlers for scalable production apps.                                      |
|                                                                                                    |
|  Recommended Title (54 chars): [ Next.js Architecture Guide: Clean Code for Production          ]  |
|  Meta Description (148 chars): [ Master Next.js App Router architecture. Learn modular monolith..]  |
|  URL Slug:                     [ nextjs-clean-architecture-guide                                ]  |
|                                                                                                    |
|  Secondary Keywords: [Next.js App Router structure] [Server components] [Modular monolith Next.js] |
+----------------------------------------------------------------------------------------------------+
```

---

## 8. Mobile Wireframe (390px Viewport)

```text
+--------------------------------------+
| [=] ArticleCompare AI   [v1.0] [Gem] |
+--------------------------------------+
| Compare Article Drafts               |
| 10-dimension editorial intelligence. |
|                                      |
| Slots: [ 2 (Active) ] [ 3 ] [ 4 ]    |
|                                      |
| Article Slot: [ Tab A (Active) ] [B] |
| +----------------------------------+ |
| | Title: [ Next.js Architecture  ] | |
| | [ Paste ]  [ Upload File ]       | |
| | +------------------------------+ | |
| | | Building scalable apps with  | | |
| | | Next.js requires clean...    | | |
| | +------------------------------+ | |
| | [OK] 1,420 words | 8,950 chars   | |
| +----------------------------------+ |
|                                      |
| [>] Optional Context (3 Set) [v]     |
|                                      |
| +----------------------------------+ |
| | [ COMPARE ARTICLES (14s) -> ]    | |
| +----------------------------------+ |
|                                      |
| --- RESULTS (When Finished) ---      |
|                                      |
| +----------------------------------+ |
| | 🏆 WINNER: Article B (85/100)    | |
| | (+7 point advantage)             | |
| | Rationale: Superior structure &  | |
| | practical route handlers.        | |
| +----------------------------------+ |
|                                      |
| Matrix: [<- Scrollable Table ->]     |
| +----------------------------------+ |
| | Criterion   | A  | B  | Winner   | |
| | Search (15%)| 82 | 88 | B (+6)   | |
| | Depth (12%) | 72 | 87 | B (+15)  | |
| | Read (10%)  | 88 | 82 | A (+6)   | |
| +----------------------------------+ |
|                                      |
| [ Export Full JSON Report ]          |
+--------------------------------------+
```
