# 13 — Glossary and Open Decisions

**Product:** ArticleCompare AI  
**Scope:** Terminology, Decision Log, Risk Mitigations & Definition of Done  

---

## 1. Domain Glossary

| Term | Definition in ArticleCompare AI |
|---|---|
| **Article Draft** | A single piece of content (title + body text) submitted for comparative evaluation. Assigned a stable slot letter (`A`, `B`, `C`, or `D`). |
| **Comparison Session** | A single analysis run evaluating 2, 3, or 4 submitted article drafts against shared context parameters. |
| **Editorial Rubric** | The immutable 10-dimension evaluation framework applied identically to every submitted article. |
| **Dimension Score** | An integer score from 0 to 100 assigned by the AI engine to an article for a single rubric dimension based on observed text. |
| **Overall Score** | The deterministic, mathematically weighted sum of all 10 dimension scores ($0–100$), rounded to the nearest integer. |
| **Decisive Winner** | The leading draft when its overall score surpasses the runner-up by $\ge 5\text{ points}$ without unmitigated critical accuracy flaws. |
| **Close Race / Near Tie** | A comparison where the score delta between the top two drafts is $< 5\text{ points}$, indicating that both drafts have distinct, viable strengths. |
| **Shared Content Gap** | A relevant subtopic, user question, definition, or explanation that is missing across *all* submitted article drafts. |
| **Hybrid Recommendation** | Concrete editorial synthesis instructions explaining how an editor can combine the strongest sections of different drafts into a superior final piece. |
| **Textual Evidence** | An exact excerpt, heading citation, or verifiable structural observation from the submitted text backing an asserted score or weakness. |
| **Provider Adapter** | A decoupled server-side module that translates standardized prompt payloads into provider-specific API calls (OpenAI or Google Gemini). |
| **Structured Output** | A capability of modern LLMs where output is guaranteed to conform to a provided JSON Schema at the token sampling level. |

---

## 2. Production Decisions Log

| Decision Area | MVP Default | Production Target | Rationale |
|---|---|---|---|
| **Product Working Name** | ArticleCompare AI | Confirm trademark & domain | Simple, descriptive, and SEO-friendly working name. |
| **User Authentication** | None (Stateless) | NextAuth / Better Auth (Phase 3) | Eliminates login friction during MVP product validation. |
| **Data Retention** | Ephemeral memory only | User-managed cloud projects (Phase 3) | Maximum user privacy; zero liability for stored confidential drafts. |
| **Max Article Length** | 60,000 characters (~10k words) | Configurable by user tier | Sufficient for 99% of long-form blog articles and technical guides. |
| **Export Formats** | JSON + Markdown | Add PDF / Word export (Phase 2) | Lightweight, structured, and developer-friendly formats first. |
| **Default AI Provider** | Google Gemini (`gemini-2.5-flash`) | Configurable via dashboard | High speed, large context window, and exceptional structured output reliability. |
| **URL Import Capability** | Deferred to Phase 2 | Safe proxy extraction with SSRF defense | Scraping external URLs introduces complex security risks best tackled after MVP core engine is proven. |

---

## 3. Risk & Mitigation Matrix

| Risk Event | Severity | Likelihood | Architectural Mitigation |
|---|---|---|---|
| **Probabilistic Score Inconsistency** | High | Medium | Enforce low temperature ($0.2$); standardize system prompts; use deterministic server math for overall score; benchmark against human editorial sets. |
| **Fabricated Fact Claims** | High | Medium | UI prominently states that text-only analysis does not verify external factual accuracy; engine instructs model to flag unverified claims as caveats. |
| **API Cost Spikes / Abuse** | High | Low | Hard ceiling of 60,000 characters per article; 60s request timeout; pre-flight token gating; provider billing budget alerts. |
| **Upstream Provider Outages** | Medium | Medium | Pluggable dual-provider architecture; switch between OpenAI and Gemini instantly via environment variables or UI toggle. |
| **Prompt Injection via Drafts** | Medium | Low | XML delimiter boundary tagging; explicit system instructions commanding LLM to treat article content as inert data. |
| **User Data Leakage** | Critical | Low | Zero database storage on server; memory freed immediately after HTTP response; operational logger strictly masks article text. |
| **Misleading Ranking Guarantees** | High | Low | Explicit product disclaimers clarify that scores represent editorial quality, not search engine ranking predictions. |

---

## 4. Definition of Done (DoD)

A user story, feature, or phase milestone is considered **Done** only when all of the following conditions are met:
1. **Requirements Fulfilled**: All functional requirements and acceptance criteria in the PRD/SRS are satisfied.
2. **Deterministic Math Verified**: Overall score calculation verified to match weighted formula with zero calculation drift.
3. **Zod Validation Passes**: 100% of incoming payloads and outgoing report objects validate against Zod schemas.
4. **Comprehensive Error Handling**: All network dropouts, rate limits, and provider timeouts display user-friendly error banners without erasing user inputs.
5. **Cross-Browser & Responsive**: Tested and visually verified across Desktop (1440px), Tablet (768px/1024px), and Mobile (360px/390px).
6. **Accessibility Verified**: Meets WCAG 2.2 AA standards with full keyboard navigation and high-visibility focus states.
7. **Automated Tests Pass**: Unit tests, integration tests, and Playwright E2E flows pass cleanly.
8. **Zero Secret Leakage**: Verified that API keys are strictly server-side and excluded from client bundles.
9. **Documentation Updated**: Architecture diagrams, API specs, and environment variable examples reflect current implementation.
