# Barcik Training Demos

This project contains interactive HTML demos for barcik.training,
a professional GenAI/ML/Data Science training platform.

## Project Info
- Owner: Robert Barcik, professional AI trainer
- Audience: Corporate learners (technical and non-technical)
- Hosting: S3 + CloudFront + ACM
- Live URL: https://demos.barcik.training/

## Structure
- `/demos/` — each demo is a self-contained HTML file
- `/shared/` — shared CSS/JS assets (if any)

## Git & GitHub
- Repo: https://github.com/robertbarcik/barcik-training-demos
- Branch: main
- After ANY file changes, ALWAYS commit and push to GitHub
- Write clear commit messages describing what changed and why

## Deployment
- S3 bucket: barcik-training-demos
- CloudFront distribution ID: E322CGNL1PIL76
- AWS profile: barcik-demos
- Region: eu-central-1

## Workflow (ALWAYS follow after making changes)
1. **Make the changes** to demo files, index, etc.
2. **Update index.html** if a demo was added or removed — keep the demo index in sync
3. **Deploy to S3:**
   ```
   aws s3 sync . s3://barcik-training-demos/ --exclude ".git/*" --exclude "CLAUDE.md" --exclude ".claude/*" --exclude ".gitignore" --exclude ".DS_Store" --profile barcik-demos --region eu-central-1
   ```
4. **Invalidate CloudFront cache** (so changes go live immediately):
   ```
   aws cloudfront create-invalidation --distribution-id E322CGNL1PIL76 --paths "/*" --profile barcik-demos
   ```
5. **Commit and push to GitHub:**
   - Stage changed files (be specific, no `git add -A`)
   - Commit with a descriptive message
   - `git push origin main`
6. **Confirm** to the user that deployment, cache invalidation, and git push all succeeded

## Index page (generated since the Riso redesign)
- `index.html` is written by `training-ops/web/riso/build.py` from `training-ops/web/riso/data/demos.json`
  (groups, titles, descriptions, cover emblem per demo). Adding or removing a demo: edit that JSON,
  run `/usr/bin/python3 ../training-ops/web/riso/build.py`, review, deploy. Do not hand-edit `index.html`.
- `assets/riso/` (riso.css, shelf.js) is copied in by the same script; the source lives in training-ops.

## Demo Standards
- Each demo is a SINGLE self-contained HTML file
- Mobile-responsive (students use phones and tablets): check 390, 820 and 1280 wide
- Include a header with "barcik.training" branding
- Include a brief explanation of the concept being demonstrated
- Use vanilla HTML/CSS/JS (no build step needed)
- **Look: Riso** (since October 2026). Paper background, navy ink, Bricolage Grotesque, square
  bordered panels with hard offset shadows, blue/pink/yellow as decoration. Paste the token block,
  the "All demos" link style and the pill style from `training-ops/web/riso/demo-kit.css`; the rules
  and the colour mapping are in `training-ops/web/riso/DEMO_RESKIN.md`; `neuron-playground.html` is
  the reference. Colours that carry meaning in a lesson keep it: red = danger/wrong, green =
  safe/correct, amber = caution; never use blue/pink/yellow for those. Exceptions by design: the two
  classroom timers stay dark (projected in a dark room) and the EU AI Act Lab keeps its
  legal-document look (Spectral, EU blue and gold).
- **AI transparency pill (mandatory since 2026-08-16):** every demo ends with a small fixed
  `<details class="ai-transparency-pill" id="ai-transparency">` before `</body>` (corner tab that
  expands into the notice: scripted simulation, nothing sent to an AI model, built with generative AI,
  reviewed by Robert who is responsible for what is published; voluntary, in the spirit of Art 50 EU AI Act).
  Add it to new demos with `python3 ../training-ops/web/ai_transparency_label.py demo demos/<file>.html`
  (idempotent). If a demo has its own bottom-right fixed element, override the pill to bottom-left
  (see timer.html). The index carries the same statement in a `.ai-notice` block above the footer.
  Demos must stay free of live AI calls; if one ever calls a model, the pill wording (and Art 50(1))
  need revisiting.

