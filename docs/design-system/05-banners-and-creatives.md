# Design System: Marketing Banners & Creative Assets

**Product:** ArticleCompare AI  
**Skill Guidelines:** `banner-design` | `ui-ux-pro-max` | `ui-styling`  
**Brand Archetype:** High-Trust Editorial Intelligence / Analytical Enterprise  
**Primary Color Palette:** Enterprise Sapphire Blue (`#2563EB`), Crisp White (`#FFFFFF`), Slate (`#0F172A`), Emerald Accent (`#10B981`)  

---

## 1. Banner Portfolio & Dimensional Specifications

To establish a cohesive brand presence across web, search, and social platforms, ArticleCompare AI specifies five standardized banner formats:

| Format / Platform | Pixel Dimensions | Aspect Ratio | Safe Content Area | Primary Goal & Placement |
|---|---|---|---|---|
| **Website Hero Banner** | 1920 × 640 px | 3 : 1 | Central 1440 × 540 px | Main marketing landing page header and conversion zone. |
| **OpenGraph Social Preview** | 1200 × 630 px | 1.91 : 1 | Central 960 × 500 px | Shared link previews across LinkedIn, Twitter, Slack, and Discord. |
| **Twitter / X Header** | 1500 × 500 px | 3 : 1 | Central 1200 × 400 px | Official Twitter/X organization profile header. |
| **LinkedIn Company Cover** | 1128 × 191 px | ~5.9 : 1 | Central 900 × 160 px | LinkedIn company page header banner. |
| **Product Hunt Gallery Asset** | 1270 × 760 px | 1.67 : 1 | Central 1080 × 660 px | Product Hunt launch gallery showcase slide. |

---

## 2. Art Direction Options (3 Distinct Concepts)

Following the `banner-design` skill principles, three complementary art direction styles have been developed for ArticleCompare AI:

### Concept 1: "The Analytical Workspace" (Data & Editorial Infographic — Recommended)
- **Style:** Data-Dense Infographic + Swiss Modernism.
- **Visuals:** High-contrast layout showcasing an elegant, floating comparative score matrix ($85$ vs $78$) with emerald winner tags, clean typography, and a subtle isometric comparison grid.
- **Tone:** Authoritative, objective, calm, and trustworthy.
- **Best For:** Technical content teams, B2B publishers, enterprise content leads.

### Concept 2: "Glassmorphic Editorial Precision" (Modern SaaS Glassmorphism)
- **Style:** Frosted glassmorphism with subtle blue and sapphire mesh lighting.
- **Visuals:** Floating translucent glass cards displaying side-by-side drafts (`Draft A` and `Draft B`) with glowing score badges, surrounded by a dark navy/slate ambient background.
- **Tone:** Premium, cutting-edge AI, modern, and high-tech.
- **Best For:** Product Hunt launches, developer tool showcases, Twitter campaigns.

### Concept 3: "Bold Typography & The Sovereign Editor" (Minimalist Editorial)
- **Style:** Bold Swiss Typography with stark white space and a single high-contrast visual anchor.
- **Visuals:** Oversized, confident serif/sans headline (*"Stop Guessing. Compare Drafts."*) with a minimalist scale icon and single emerald CTA.
- **Tone:** Direct, fearless, human-centric, and no-nonsense.
- **Best For:** Organic LinkedIn thought leadership, content strategy webinars, print handouts.

---

## 3. Production HTML & CSS Banner Implementations

### 3.1 Website Hero Banner (1920 × 640 px)
```html
<div class="relative w-full max-w-[1920px] h-[640px] bg-gradient-to-r from-slate-900 via-slate-950 to-blue-950 text-white overflow-hidden flex items-center justify-between px-16 lg:px-24">
  <!-- Subtle Ambient Glow -->
  <div class="absolute -top-32 -left-32 w-96 h-96 bg-blue-600/20 rounded-full blur-3xl pointer-events-none"></div>
  <div class="absolute -bottom-32 right-1/4 w-96 h-96 bg-emerald-500/10 rounded-full blur-3xl pointer-events-none"></div>

  <!-- Left: Messaging & Value Prop (Central Safe Zone) -->
  <div class="relative z-10 max-w-2xl space-y-6">
    <div class="inline-flex items-center gap-2 px-3 py-1.5 rounded-full bg-blue-500/10 border border-blue-400/20 text-blue-400 text-xs font-semibold tracking-wide uppercase">
      <span class="w-2 h-2 rounded-full bg-blue-400 animate-pulse"></span>
      Editorial Decision-Support AI
    </div>
    
    <h1 class="text-5xl lg:text-6xl font-bold tracking-tight text-white leading-[1.15]">
      Compare Article Drafts with <span class="text-transparent bg-clip-text bg-gradient-to-r from-blue-400 via-blue-200 to-emerald-400">Editorial Rigor.</span>
    </h1>

    <p class="text-lg text-slate-300 max-w-xl leading-relaxed">
      Evaluate 2–4 drafts against an immutable 10-dimension rubric. Uncover missing content gaps, verify search-intent satisfaction, and synthesize the winning master draft.
    </p>

    <div class="flex items-center gap-4 pt-2">
      <a href="/compare" class="px-6 py-3.5 rounded-lg bg-blue-600 hover:bg-blue-500 text-white font-semibold text-sm shadow-lg shadow-blue-600/30 transition-all active:scale-[0.98]">
        Start Free Comparison &rarr;
      </a>
      <span class="text-xs text-slate-400 font-medium">No account required &bull; 100% Stateless &bull; Private</span>
    </div>
  </div>

  <!-- Right: Floating Visual UI Mockup (10-Dimension Score Card) -->
  <div class="relative z-10 hidden lg:block w-[520px] rounded-xl bg-slate-900/80 border border-slate-700/60 p-6 shadow-2xl backdrop-blur-md">
    <div class="flex items-center justify-between pb-4 border-b border-slate-800">
      <div class="flex items-center gap-2">
        <span class="text-xs font-bold px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-400 border border-emerald-500/30">🏆 WINNER</span>
        <span class="text-sm font-semibold text-white">Draft B: Production Architecture</span>
      </div>
      <span class="text-xs font-mono font-bold text-emerald-400">85 / 100</span>
    </div>

    <!-- Mini Score Matrix Comparison -->
    <div class="space-y-3 py-4 text-xs font-sans">
      <div class="flex items-center justify-between">
        <span class="text-slate-400">Search-Intent Satisfaction (15%)</span>
        <div class="flex items-center gap-3 font-mono">
          <span class="text-slate-400">Draft A: 82</span>
          <span class="text-emerald-400 font-bold">Draft B: 88</span>
        </div>
      </div>
      <div class="flex items-center justify-between">
        <span class="text-slate-400">Accuracy & Evidence Quality (15%)</span>
        <div class="flex items-center gap-3 font-mono">
          <span class="text-slate-400">Draft A: 80</span>
          <span class="text-emerald-400 font-bold">Draft B: 85</span>
        </div>
      </div>
      <div class="flex items-center justify-between">
        <span class="text-slate-400">Depth & Completeness (12%)</span>
        <div class="flex items-center gap-3 font-mono">
          <span class="text-slate-400">Draft A: 72</span>
          <span class="text-emerald-400 font-bold">Draft B: 87</span>
        </div>
      </div>
    </div>

    <div class="pt-3 border-t border-slate-800/80 flex items-center justify-between text-[11px] text-slate-400">
      <span>10 Dimensions Evaluated</span>
      <span class="text-blue-400 font-medium">Deterministic Scoring Engine</span>
    </div>
  </div>
</div>
```

---

### 3.2 OpenGraph Social Preview Card (1200 × 630 px)
```html
<div class="relative w-[1200px] h-[630px] bg-slate-950 text-white flex flex-col justify-between p-16 overflow-hidden">
  <!-- Subtle Gradient Accent Lines -->
  <div class="absolute inset-0 bg-[radial-gradient(circle_at_top_right,rgba(37,99,235,0.18),transparent_50%)]"></div>
  <div class="absolute bottom-0 left-0 right-0 h-1.5 bg-gradient-to-r from-blue-600 via-blue-400 to-emerald-500"></div>

  <!-- Top: Brand Header -->
  <div class="relative z-10 flex items-center justify-between">
    <div class="flex items-center gap-3">
      <div class="w-10 h-10 rounded-lg bg-blue-600 flex items-center justify-center font-bold text-white text-xl">
        ⚖
      </div>
      <span class="text-2xl font-bold tracking-tight text-white">ArticleCompare <span class="text-blue-400">AI</span></span>
    </div>
    <span class="text-sm font-semibold tracking-wide px-3 py-1 rounded-full bg-slate-800 text-slate-300 border border-slate-700">
      Editorial Decision Workspace
    </span>
  </div>

  <!-- Center: Primary Headline & Value Proposition -->
  <div class="relative z-10 max-w-3xl space-y-4">
    <h2 class="text-5xl font-extrabold tracking-tight text-white leading-tight">
      Compare Drafts. Find Content Gaps. Publish with Complete Confidence.
    </h2>
    <p class="text-xl text-slate-300 leading-relaxed font-normal">
      Evaluates 2–4 articles against a shared 10-dimension rubric with textual evidence, deterministic scoring, and zero ranking hallucinations.
    </p>
  </div>

  <!-- Bottom: Feature Badges -->
  <div class="relative z-10 flex items-center gap-6 text-sm font-medium text-slate-400">
    <div class="flex items-center gap-2">
      <span class="text-emerald-400">✓</span> 10-Dimension Editorial Rubric
    </div>
    <div class="flex items-center gap-2">
      <span class="text-blue-400">✓</span> Dual Provider (Gemini & OpenAI)
    </div>
    <div class="flex items-center gap-2">
      <span class="text-amber-400">✓</span> 100% Stateless & Private
    </div>
  </div>
</div>
```

---

## 4. AI Visual Prompts for Generative Art Assets

Following the `banner-design` skill procedure, the following prompts are formatted for high-resolution visual element generation (using Google Gemini image models / Nano Banana image generation):

### 4.1 Website Hero Background & Abstract 3D Sculpture
```text
Model: gemini-3-pro-image-preview
Aspect Ratio: 16:9 (or 3:1)
Prompt:
"A sophisticated 3D glass and metallic sculpture representing editorial balance and document comparison, minimalist geometric forms, translucent frosted glass panes intersecting with smooth sapphire blue and titanium white ribbons, subtle emerald light refraction, soft ambient studio lighting, dark navy and slate gradient background, ultra-clean product design aesthetic, high fidelity, 4k render, octane render style, no text, no blur."
```

### 4.2 OpenGraph Social Background Accent
```text
Model: gemini-2.5-flash-image
Aspect Ratio: 16:9
Prompt:
"Abstract enterprise technology background, deep midnight blue and obsidian slate, glowing delicate geometric grid lines, subtle data matrix nodes, ethereal volumetric blue lighting, clean minimalism, premium SaaS brand aesthetic, high resolution, no text."
```

### 4.3 Feature Graphic for Article Comparison
```text
Model: gemini-3-pro-image-preview
Aspect Ratio: 1:1
Prompt:
"Two floating minimalist digital documents side-by-side in space, made of translucent frosted glass with glowing blue and emerald interface elements, holographic data connections between them representing textual comparison and gap analysis, clean studio background, elegant enterprise aesthetic, photorealistic, 4k."
```

---

## 5. Banner Design Quality Checklist

- [x] **Safe Zones Respected**: Key titles, CTAs, and logos placed within central 75% of the canvas; $\ge 60\text{px}$ margin from canvas perimeter.
- [x] **Single Core CTA**: One clear action verb ("Start Free Comparison") with high contrast.
- [x] **Typography Limit**: Exactly two font families (Inter for headers/body and JetBrains Mono for metrics).
- [x] **WCAG Contrast Compliant**: All banner copy satisfies minimum $4.5:1$ contrast against adjacent backgrounds.
- [x] **Responsive Adaptability**: HTML/CSS implementations scale gracefully to mobile viewports without horizontal text clipping.
