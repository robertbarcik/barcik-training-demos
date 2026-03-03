# Barcik Training Demos

This project contains interactive HTML demos for barcik.training,
a professional GenAI/ML/Data Science training platform.

## Project Info
- Owner: Robert Barcik, professional AI trainer
- Audience: Corporate learners (technical and non-technical)
- Hosting: S3 static website
- Live URL: http://barcik-training-demos.s3-website.eu-central-1.amazonaws.com/
  (Will move to https://demos.barcik.training/ once CloudFront is configured)

## Structure
- `/demos/` — each demo is a self-contained HTML file
- `/shared/` — shared CSS/JS assets (if any)

## Deployment
- S3 bucket: barcik-training-demos
- AWS profile: barcik-demos
- Region: eu-central-1

## Demo Standards
- Each demo is a SINGLE self-contained HTML file
- Modern, clean UI — professional training look
- Mobile-responsive (students may use phones/tablets)
- Include a header with "barcik.training" branding
- Include a brief explanation of the concept being demonstrated
- Use vanilla HTML/CSS/JS (no build step needed)
- Color scheme: use a professional blue/white palette

