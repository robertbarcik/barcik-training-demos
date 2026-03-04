# Barcik Training Demos

Interactive browser demos for [barcik.training](https://barcik.training) — a professional AI, ML, and Data Science training platform for corporate teams.

**Live at:** [demos.barcik.training](https://demos.barcik.training)

## Demos

| Demo | Description |
|------|-------------|
| [Attention — Cocktail Party](https://demos.barcik.training/demos/attention_cocktail_party.html) | How the attention mechanism works, explained through a cocktail-party analogy |
| [RAG Explorer](https://demos.barcik.training/demos/rag-explorer.html) | Step-by-step walkthrough of Retrieval-Augmented Generation |
| [Function Calling](https://demos.barcik.training/demos/function-calling-demo.html) | How LLMs use function calling to interact with external tools |
| [Base vs Instruct](https://demos.barcik.training/demos/base-vs-instruct-model.html) | Side-by-side comparison of base and instruction-tuned model outputs |
| [Fine-Tuning & LoRA](https://demos.barcik.training/demos/finetune.html) | Interactive visualization of fine-tuning methods and LoRA adapters |
| [Training Timer](https://demos.barcik.training/demos/timer.html) | Classroom break/exercise timer with AI trivia |

## Architecture

```
┌─────────────┐       aws s3 sync       ┌──────────────┐
│             │ ────────────────────────▶│              │
│ Claude Code │                          │  S3 Bucket   │
│  (local)    │  cloudfront invalidate   │  (static     │
│             │ ────────────────────────▶│   hosting)   │
└──────┬──────┘                          └──────┬───────┘
       │                                        │
       │ git push                               │ origin
       ▼                                        ▼
┌─────────────┐                          ┌──────────────┐
│   GitHub    │                          │  CloudFront  │
│             │                          │  (CDN + TLS) │
└─────────────┘                          └──────┬───────┘
                                                │
                                                │ demos.barcik.training
                                                ▼
                                         ┌──────────────┐
                                         │   Students   │
                                         │  (browser)   │
                                         └──────────────┘
```

- **S3** — Static website hosting in `eu-central-1` (Frankfurt)
- **CloudFront** — CDN with HTTPS via ACM certificate
- **DNS** — CNAME pointing `demos.barcik.training` to the CloudFront distribution
- **IAM** — Dedicated deployment user with minimal permissions (S3 write + CloudFront invalidation only)

## How Demos Get Built

Demos are created with [Claude Code](https://claude.ai/code), Anthropic's agentic coding tool. The workflow:

1. **Describe** what you want in natural language
2. **Claude Code builds** a self-contained HTML file following the project standards defined in `CLAUDE.md`
3. **Claude Code deploys** it — uploads to S3, invalidates CloudFront cache, commits and pushes to GitHub
4. **It's live** at `demos.barcik.training` — typically within a few minutes from prompt to URL

## Demo Standards

- **Single file** — Each demo is one `.html` file with inline CSS and JS. No external dependencies.
- **No build step** — Open in a browser and it works. Students can View Source to inspect everything.
- **Responsive** — Works on laptops, tablets, and phones.
- **Self-explanatory** — Each demo includes a brief explanation so it's useful outside the context of a lecture.
- **Vanilla stack** — HTML, CSS, and JavaScript only. No frameworks, no bundlers.

## Repository Structure

```
barcik-training-demos/
├── index.html          # Landing page listing all demos
├── CLAUDE.md           # Project context and workflow for Claude Code
├── demos/
│   ├── attention_cocktail_party.html
│   ├── base-vs-instruct-model.html
│   ├── finetune.html
│   ├── function-calling-demo.html
│   ├── rag-explorer.html
│   └── timer.html
└── .claude/            # Claude Code configuration
```

## Why This Approach

**Why single HTML files?** No build pipeline to break, no dependencies to update. A demo written today will still work in a browser five years from now.

**Why Claude Code?** Creating interactive visualizations by hand is slow enough that it rarely happens. With Claude Code, a new demo goes from idea to live URL in minutes.

**Why S3 + CloudFront?** Simplest possible static hosting — no servers, no containers. Cost is under €1/month.

---

*Built by [Robert Barcik](https://barcik.training) with [Claude Code](https://claude.ai/code) for use in corporate AI/ML training sessions.*
