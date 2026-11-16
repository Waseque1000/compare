# 05 — API Specification

**Product:** ArticleCompare AI  
**Specification Version:** 1.0.0 (MVP)  
**Protocol:** HTTPS / REST  
**Payload Format:** JSON (`application/json; charset=utf-8`)  
**Base Path:** `/api`  

---

## 1. Global API Conventions

### 1.1 Headers
| Header | Type | Description |
|---|---|---|
| `Content-Type` | String | Must be `application/json` for POST requests. |
| `Accept` | String | Must accept `application/json`. |
| `X-Request-Id` | String | Optional client-generated UUID. If omitted, server generates one and returns it in responses for traceability. |

### 1.2 Error Payload Standard
Every non-2xx HTTP response returns a normalized error envelope:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Article content must be between 100 and 60,000 characters.",
    "requestId": "req_9f81a7b0-c6e2-4d2b-98f5-19e4827d3b01",
    "details": [
      {
        "field": "articles[0].content",
        "message": "String must contain at least 100 character(s)"
      }
    ]
  }
}
```

---

## 2. Primary Endpoint: `POST /api/compare`

Evaluates 2, 3, or 4 submitted article drafts against the 10-dimension editorial rubric and returns a fully normalized comparison report.

### 2.1 Request Headers
```http
POST /api/compare HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Accept: application/json
```

### 2.2 Request Body Example
```json
{
  "provider": "gemini",
  "articles": [
    {
      "id": "A",
      "title": "Comprehensive Guide to Clean Code in Next.js",
      "content": "Building scalable React applications with Next.js requires strict adherence to modular design patterns and clean code principles. In this article, we explore directory structures, server-side data fetching strategies, and clean component composition. Server components allow developers to reduce bundle size while client components handle dynamic state..."
    },
    {
      "id": "B",
      "title": "Next.js Architecture: From Scratch to Production",
      "content": "Next.js has become the de facto framework for full-stack React web apps. This deep dive covers App Router fundamentals, route handlers, middleware, and rendering optimizations. When architecting your project, organizing business logic into distinct domain modules prevents spaghetti code..."
    }
  ],
  "primaryKeyword": "Next.js clean architecture",
  "searchIntent": "Informational",
  "audience": "Intermediate to senior frontend and full-stack engineers",
  "goal": "Select the most comprehensive and actionable architectural guide for publication on our tech blog",
  "language": "English",
  "tone": "Authoritative yet accessible",
  "guidelines": "Focus on Next.js 15 App Router best practices. Emphasize modular monolith architecture."
}
```

### 2.3 Request Field Specifications
| Field | Type | Required | Constraints | Description |
|---|---|---|---|---|
| `provider` | String | No | `'openai' \| 'gemini'` (default: `'gemini'`) | AI provider to execute analysis. |
| `articles` | Array | Yes | 2 to 4 objects | Array of article drafts. |
| `articles[i].id` | String | Yes | `'A' \| 'B' \| 'C' \| 'D'` | Stable slot identifier. |
| `articles[i].title` | String | No | Max 200 characters | Draft headline or title. |
| `articles[i].content` | String | Yes | 100 to 60,000 characters | Raw body text of draft. |
| `primaryKeyword` | String | No | Max 100 characters | Target SEO search query. |
| `searchIntent` | String | No | Enum: `Informational`, `Commercial`, `Transactional`, `Navigational` | Search query intent. |
| `audience` | String | No | Max 150 characters | Intended reader profile. |
| `goal` | String | No | Max 150 characters | Editorial publication target. |
| `language` | String | No | Max 50 characters (default: `'English'`) | Content language. |
| `tone` | String | No | Max 50 characters | Desired editorial style. |
| `guidelines` | String | No | Max 500 characters | Brand / style guide rules. |

---

### 2.4 Success Response (`200 OK`)
```json
{
  "provider": "gemini",
  "model": "gemini-2.5-flash",
  "meta": {
    "requestId": "req_9f81a7b0-c6e2-4d2b-98f5-19e4827d3b01",
    "articleCount": 2,
    "durationMs": 14250,
    "evaluatedAt": "2026-11-16T13:45:00.000Z"
  },
  "report": {
    "executiveSummary": "Both drafts demonstrate strong technical comprehension of the Next.js App Router. Article B is the superior foundational piece, offering significantly deeper structural advice and clearer route handler demonstrations. Article A offers superior component readability but lacks actionable deployment caveats.",
    "winner": {
      "articleId": "B",
      "title": "Next.js Architecture: From Scratch to Production",
      "reason": "Article B scored 85 compared to Article A's 78 (+7 points). It provides superior architectural depth, realistic directory structures, and clear separation of server/client boundaries.",
      "confidence": "High"
    },
    "hybridRecommendation": "Adopt Article B as the core narrative backbone. Transpose Section 3 of Article A (Modular Component Composition) into Section 4 of Article B, and incorporate Article A's cleaner code snippets for custom hooks.",
    "articles": [
      {
        "articleId": "A",
        "title": "Comprehensive Guide to Clean Code in Next.js",
        "overallScore": 78,
        "scores": {
          "searchIntent": 82,
          "accuracyEvidence": 80,
          "depthCompleteness": 72,
          "readability": 88,
          "structure": 78,
          "originality": 70,
          "examples": 82,
          "onPageSEO": 75,
          "topicalCoverage": 74,
          "publicationReadiness": 80
        },
        "strengths": [
          "Exceptionally clear breakdown of React Server Component (RSC) mental models with clean syntax examples.",
          "High readability with conversational, accessible phrasing suited for intermediate developers."
        ],
        "weaknesses": [
          "Superficial treatment of directory structure best practices in large codebases.",
          "Fails to address route handler error boundaries or environment variable isolation."
        ],
        "contentGaps": [
          "Missing guidance on caching behavior and revalidation tags (`next/cache`).",
          "No mention of middleware execution order or edge runtime constraints."
        ],
        "editorialIssues": [
          "Section 2 contains an unverified claim regarding server component memory usage."
        ],
        "seoSuggestions": [
          "Incorporate H2 subheadings targeting 'App Router folder structure' to capture secondary search intent."
        ]
      },
      {
        "articleId": "B",
        "title": "Next.js Architecture: From Scratch to Production",
        "overallScore": 85,
        "scores": {
          "searchIntent": 88,
          "accuracyEvidence": 85,
          "depthCompleteness": 87,
          "readability": 82,
          "structure": 86,
          "originality": 84,
          "examples": 85,
          "onPageSEO": 82,
          "topicalCoverage": 86,
          "publicationReadiness": 84
        },
        "strengths": [
          "Exemplary modular monolith folder tree demonstrating domain separation.",
          "Detailed, practical code examples for route handlers with Zod request validation.",
          "Accurate, up-to-date coverage of Next.js 15 server action error handling."
        ],
        "weaknesses": [
          "Dense introductory paragraphs that could cause reader drop-off before reaching practical code.",
          "Some long code blocks lack explanatory annotations."
        ],
        "contentGaps": [
          "Does not compare App Router caching strategies with legacy Pages Router approaches."
        ],
        "editorialIssues": [
          "Paragraph 4 in Section 2 is overly dense and needs bulleted decomposition."
        ],
        "seoSuggestions": [
          "Shorten title to under 60 characters for optimal SERP display: 'Next.js Architecture: Production Guide'."
        ]
      }
    ],
    "comparisonMatrix": [
      {
        "criterion": "Search-Intent Satisfaction",
        "scores": { "A": 82, "B": 88 },
        "analysis": "Article B addresses both theoretical structure and deployment patterns, providing greater intent fulfillment than Article A's component-focused approach."
      },
      {
        "criterion": "Accuracy & Evidence Quality",
        "scores": { "A": 80, "B": 85 },
        "analysis": "Both provide accurate Next.js 15 examples, but Article B includes defensible error boundary patterns."
      },
      {
        "criterion": "Depth & Completeness",
        "scores": { "A": 72, "B": 87 },
        "analysis": "Article B covers middleware, route handlers, and modular domains; Article A leaves out backend boundaries."
      },
      {
        "criterion": "Readability & Audience Fit",
        "scores": { "A": 88, "B": 82 },
        "analysis": "Article A has superior cadence and readability, scoring 6 points higher in this category."
      },
      {
        "criterion": "Structure & Scannability",
        "scores": { "A": 78, "B": 86 },
        "analysis": "Article B leverages subheadings, code callouts, and lists more effectively."
      },
      {
        "criterion": "Originality & Differentiation",
        "scores": { "A": 70, "B": 84 },
        "analysis": "Article B provides custom domain architecture models rather than standard tutorial examples."
      },
      {
        "criterion": "Practical Examples & Utility",
        "scores": { "A": 82, "B": 85 },
        "analysis": "Both offer good snippets; Article B provides complete, runnable route handler patterns."
      },
      {
        "criterion": "On-Page SEO Fundamentals",
        "scores": { "A": 75, "B": 82 },
        "analysis": "Article B aligns headings more closely with target search keyword clusters."
      },
      {
        "criterion": "Topical Coverage",
        "scores": { "A": 74, "B": 86 },
        "analysis": "Article B captures broader Next.js ecosystem concerns including caching and deployment."
      },
      {
        "criterion": "Publication Readiness",
        "scores": { "A": 80, "B": 84 },
        "analysis": "Article B requires minimal structural adjustment before publishing."
      }
    ],
    "sharedGaps": [
      "Neither article discusses automated testing strategies (e.g., Vitest or Playwright) for Next.js App Router applications.",
      "Neither draft explains bundle analysis tools (such as `@next/bundle-analyzer`) to diagnose client component bloat."
    ],
    "priorityImprovements": [
      {
        "priority": "High",
        "task": "Add a dedicated section on caching and revalidation tags (`revalidatePath` and `revalidateTag`) to Article B.",
        "reason": "Caching is the most common pitfall in Next.js 15; omitting it leaves readers vulnerable to stale data bugs."
      },
      {
        "priority": "Medium",
        "task": "Refactor dense paragraphs in Article B's Introduction into a bulleted summary of key takeaways.",
        "reason": "Improves early reader engagement and reduces initial cognitive load."
      },
      {
        "priority": "Low",
        "task": "Insert a short note cautioning developers against exposing private server utilities in client components.",
        "reason": "Promotes defensive security practices."
      }
    ],
    "seo": {
      "title": "Next.js Architecture Guide: Clean Code for Production",
      "metaDescription": "Master Next.js App Router architecture. Learn modular monolith folder structures, server components, and route handlers for scalable production apps.",
      "slug": "nextjs-clean-architecture-guide",
      "primaryKeyword": "Next.js clean architecture",
      "secondaryKeywords": [
        "Next.js App Router folder structure",
        "Next.js server components best practices",
        "modular monolith Next.js"
      ],
      "faqIdeas": [
        "What is the best folder structure for Next.js App Router?",
        "How do you separate server and client components cleanly?",
        "When should you use route handlers instead of server actions?"
      ]
    },
    "limitations": [
      "All scores and evaluations reflect text-only editorial analysis based on the submitted drafts.",
      "Scores do not guarantee Google or Bing search rankings.",
      "Factual assertions have not been verified against external authoritative databases."
    ]
  }
}
```

---

## 3. Error Status Codes and Payloads

| Status Code | Error Code | Description & Cause |
|---|---|---|
| `400 Bad Request` | `VALIDATION_ERROR` | Request payload failed schema validation (e.g., $< 2$ or $> 4$ articles, content $< 100$ characters). |
| `400 Bad Request` | `INVALID_PROVIDER` | Selected provider is not supported or allowlisted on this server. |
| `413 Payload Too Large` | `PAYLOAD_TOO_LARGE` | Total submitted content exceeds maximum character ceiling (180,000 characters). |
| `429 Too Many Requests` | `RATE_LIMITED` | Application or AI provider rate limit exceeded. Retry after specified delay. |
| `502 Bad Gateway` | `AI_PROVIDER_ERROR` | External AI provider returned an unparseable response or upstream API failure. |
| `503 Service Unavailable` | `PROVIDER_NOT_CONFIGURED` | Required provider API key (`GEMINI_API_KEY` or `OPENAI_API_KEY`) is missing on the server. |
| `504 Gateway Timeout` | `ANALYSIS_TIMEOUT` | AI provider failed to complete analysis within the 60-second execution window. |
| `500 Internal Error` | `INTERNAL_SERVER_ERROR` | Unexpected server exception. |

---

## 4. Future Roadmap Endpoints (Phase 2 & Phase 3)

The following endpoints are specified for future phases when optional cloud persistence, user accounts, and deep URL/document parsing are introduced:

| Method | Endpoint | Release | Description |
|---|---|---|---|
| `POST` | `/api/v1/comparisons` | Phase 2 | Creates a draft comparison entity with assigned UUID. |
| `GET` | `/api/v1/comparisons` | Phase 2 | Returns paginated list of user's saved comparisons. |
| `GET` | `/api/v1/comparisons/:id` | Phase 2 | Retrieves comparison details and latest normalized report. |
| `DELETE` | `/api/v1/comparisons/:id` | Phase 2 | Permanently deletes a saved comparison record. |
| `POST` | `/api/v1/import/url` | Phase 2 | Safely scrapes and extracts clean body text from a verified web URL. |
| `POST` | `/api/v1/import/file` | Phase 2 | Parses `.docx` and `.pdf` files via isolated sandbox parsing service. |
| `GET` | `/api/v1/usage` | Phase 3 | Returns API credit balance, token consumption, and monthly quota status. |
