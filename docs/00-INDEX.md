# ArticleCompare AI — Complete Project Documentation Pack

**Product:** ArticleCompare AI  
**Document Set:** Product, Engineering, AI Engine, UI/UX, Security, Quality, Operations, Roadmap & Design System  
**Baseline Technology:** Next.js 15 App Router | JavaScript/JSX | Tailwind CSS | OpenAI & Google Gemini | No Database (MVP)  
**Comparison Capacity:** 2–4 articles per comparison  

---

## 1. Executive Summary & Purpose

**ArticleCompare AI** is an AI-assisted editorial decision-support platform designed for editors, content strategists, SEO specialists, and publishers. It solves a core editorial bottleneck: comparing multiple draft versions or competing articles fairly, consistently, and rigorously before publication.

Rather than acting as a generic conversational chatbot or rewriting drafts blindly, ArticleCompare AI executes an evidence-backed evaluation against a structured 10-dimension rubric. It scores each article, visualizes a direct comparison matrix, identifies missing content gaps, checks publication readiness, and generates prioritized editorial improvements alongside a hybrid recommendation synthesizing the strongest elements of each draft.

> [!IMPORTANT]
> **Defensible Decision-Support Principle:**  
> ArticleCompare AI is an editorial decision-support tool. It does **not** predict Google/Bing rankings, establish external factual truth without citations, or replace human editorial judgment. The AI recommends; the editor decides.

---

## 2. Complete Documentation Map

| Document | Purpose & Core Focus |
|---|---|
| [01-PRD.md](file:///d:/Compare/docs/01-PRD.md) | **Product Requirements Document**: Vision, user personas, problem statement, core journey, 10 rubric dimensions, success metrics, and MVP acceptance criteria. |
| [02-SRS.md](file:///d:/Compare/docs/02-SRS.md) | **Software Requirements Specification**: Functional requirements (FR-001–FR-015), non-functional requirements (NFR-001–NFR-008), input limits, and data validation rules. |
| [03-Architecture.md](file:///d:/Compare/docs/03-Architecture.md) | **System Architecture**: Modular monolith structure, component diagrams, request lifecycle, AI provider adapter pattern, synchronous/asynchronous transition path. |
| [05-API-Specification.md](file:///d:/Compare/docs/05-API-Specification.md) | **API Specification**: `POST /api/compare` endpoint contract, request/response JSON schemas, error status codes, error payload schemas, and future roadmap endpoints. |
| [06-AI-Analysis-Engine.md](file:///d:/Compare/docs/06-AI-Analysis-Engine.md) | **AI Analysis Engine**: 10-dimension weighted scoring rubric, deterministic calculation formula, winner decision rules, prompt engineering rules, and JSON schema validation. |
| [07-UI-UX-Specification.md](file:///d:/Compare/docs/07-UI-UX-Specification.md) | **UI/UX Specification**: Visual identity (Blue/White/Enterprise), screen inventory (Screens A–G), comparison workspace mechanics, and accessibility standards (WCAG 2.2 AA). |
| [08-Security-Privacy.md](file:///d:/Compare/docs/08-Security-Privacy.md) | **Security & Privacy**: Threat model, server-only secret handling, prompt injection defense, SSRF protection, and privacy/retention policies. |
| [09-Testing-QA-Plan.md](file:///d:/Compare/docs/09-Testing-QA-Plan.md) | **Testing & QA Plan**: Layered testing strategy, test case catalog (TC-001–TC-015), AI quality/consistency benchmarks, and release gating criteria. |
| [10-Deployment-Operations.md](file:///d:/Compare/docs/10-Deployment-Operations.md) | **Deployment & Operations**: Hosting on Vercel/Node, environment variable configuration, logging, cost management, observability, and runbooks. |
| [11-Development-Roadmap.md](file:///d:/Compare/docs/11-Development-Roadmap.md) | **Development Roadmap**: Phased milestones (Phase 0: Baseline Review to Phase 5: Research & Intelligence), work breakdown structure, and exit criteria. |
| [12-Project-Structure-ADR.md](file:///d:/Compare/docs/12-Project-Structure-ADR.md) | **Project Structure & ADRs**: Target file tree for Next.js App Router (JavaScript/JSX), component directory breakdown, and 6 Architecture Decision Records. |
| [13-Glossary-and-Open-Decisions.md](file:///d:/Compare/docs/13-Glossary-and-Open-Decisions.md) | **Glossary & Decisions Log**: Domain terminology definitions, confirmed architectural defaults, open decisions, risks, and Definition of Done. |

---

## 3. Dedicated Frontend & UI/UX Design System Pack

Developed using the `ui-ux-pro-max`, `ui-styling`, and `banner-design` frameworks:

| Design System Document | Focus Area |
|---|---|
| [01-tokens.md](file:///d:/Compare/docs/design-system/01-tokens.md) | **Design Tokens & Theme Architecture**: 3-layer token system (Primitive, Semantic, Component), CSS variables, Tailwind configuration, color palettes, typography scale, spacing, elevation. |
| [02-components.md](file:///d:/Compare/docs/design-system/02-components.md) | **Component Specification & Architecture**: shadcn/ui + Tailwind primitives, props, state contracts, and accessible behaviors for all workspace and report widgets. |
| [03-screens-and-wireframes.md](file:///d:/Compare/docs/design-system/03-screens-and-wireframes.md) | **Screen Layouts & High-Fidelity Wireframes**: Responsive layouts for Desktop (1440px), Tablet (768px/1024px), and Mobile (360px/390px) across all 7 product screens. |
| [04-interactions-and-states.md](file:///d:/Compare/docs/design-system/04-interactions-and-states.md) | **Interaction States & Motion Guidelines**: Complete state matrix (Default, Hover, Focus, Active, Disabled, Loading, Error, Success), loading sequence stepper, and error recovery flows. |
| [05-banners-and-creatives.md](file:///d:/Compare/docs/design-system/05-banners-and-creatives.md) | **Marketing Banners & Creative Assets**: Website hero banner, OpenGraph preview cards (1200×630), Twitter headers (1500×500), Product Hunt assets, and AI visual prompts. |

---

## 4. Key Architectural & Product Decisions

1. **No Database (MVP)**: The application is stateless on the server. Comparison requests are processed in memory and returned to the client. Session comparison history is optionally stored locally in browser `localStorage`.
2. **JavaScript/JSX Only**: Strictly adhering to the project preference. No TypeScript/TSX. Type safety and validation are enforced at runtime boundaries via `Zod`.
3. **Modular Monolith**: One Next.js 15 application cleanly separating UI components, API route handlers, domain logic, and provider adapters.
4. **Dual AI Provider Integration**: Plug-and-play adapter architecture supporting OpenAI (`gpt-4o` / configurable) and Google Gemini (`gemini-2.5-flash` / `gemini-1.5-pro` / configurable) without vendor lock-in.
5. **Deterministic Score Calculation**: While dimension scores (0–100) are generated by the LLM based on textual evidence, the overall weighted score is calculated deterministically on the server to prevent arithmetic hallucinations.
