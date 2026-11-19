# CLAUDE.md — Resume Repository Context

## What This Repo Is

A **LaTeX-based resume** that auto-compiles to PDF and deploys to GitHub Pages via GitHub Actions. The source of truth is `resume.tex`. There is no npm, no framework, no package.json — just LaTeX + shell CI.

**Live URL:** https://shobhnik13.github.io/resume/resume.pdf  
**Repo:** https://github.com/Shobhnik13/resume

## Architecture

```
resume.tex  (source)
    ↓
GitHub Actions push to main
    ↓
Docker container: danteev/texlive
    ↓
pdflatex → resume.pdf
    ↓
Copy resume.pdf + index.html → build/
    ↓
Deploy build/ to gh-pages branch
    ↓
Served at shobhnik13.github.io/resume/
```

`index.html` is a thin JS redirect: `location.href = "...resume.pdf"` — it just forwards browser visitors to the PDF.

---

## File Map

| File | Purpose | Served at |
|------|---------|-----------|
| `resume/software-resume.tex` | Software/backend resume | `resume.shobhnik.site/resume/software-resume.pdf` → `shobhnik.site/resume` |
| `resume/product-resume.tex` | Product resume | `resume.shobhnik.site/resume/product-resume.pdf` → `shobhnik.xyz/resume` |
| `cover-letter/software-cover-letter.tex` | Software cover letter | `resume.shobhnik.site/cover-letter/software-cover-letter.pdf` → `shobhnik.site/cover-letter` |
| `cover-letter/product-cover-letter.tex` | Product cover letter | `resume.shobhnik.site/cover-letter/product-cover-letter.pdf` → `shobhnik.xyz/cover-letter` |
| `index.html` | Browser redirect to the software resume PDF | |
| `.github/workflows/build.yml` | CI: compile all four PDFs, deploy to gh-pages | |

The portfolio sites (`Portfolio` and `portfolio-product` repos) proxy these PDFs with Vercel rewrites, so edit the `.tex` here and push; the sites pick up the new PDF with no change on their side. Legacy `resume.pdf` and `product-resume.pdf` are still copied to the root of gh-pages so older links keep working.

---

## Fork History & Attribution

This repo is a fork of **[@jai-dewani](https://github.com/jai-dewani)**'s resume template. Attribution is in `README.md`. The original README line mentioning "Abir" is stale/irrelevant — it was from an intermediate fork and can be updated or removed.

### How to Detach the Fork (Make It a Standalone Repo)

GitHub doesn't provide a UI button to un-fork. Two options:

**Option A — Duplicate (recommended):**
1. Create a **new** empty repo on GitHub named `resume` (delete old one first, or use a temp name)
2. Locally: `git remote set-url origin https://github.com/Shobhnik13/resume.git`
3. `git push -u origin main --force`
4. The new repo will have no fork relationship

**Option B — GitHub Support:**
- Email support@github.com or use the support form and ask to "detach fork network" for `Shobhnik13/resume`

---

## Resume Content (as of April 2026)

**Personal info in `resume.tex`:**
- Name: Souvik De (line 163)
- Location: India (line 168)
- Email: souvikde.tech@gmail.com (line 172)
- Phone: +91 9073302976 (line 176)
- Website: https://souvik.de/ (line 180)
- LinkedIn: https://www.linkedin.com/in/souvik-de-a2b941169/ (line 184)
- GitHub: https://github.com/Souvikns/ (line 188)

**Experience (4 roles):**
1. Software Engineer II — XANE.AI, July 2025–Present (GenAI pipelines, RAG, Maruti Suzuki)
2. Lead Software Engineer — CodeMate.AI, Dec 2024–May 2025 (RAG search, CI/CD, VS Code extension)
3. Software Engineer — Postman, Feb 2022–June 2024 (AsyncAPI CLI, Bundler 16k+ downloads, GSoC mentor)
4. Backend Developer Intern — Mage, Nov 2021–Feb 2022 (Go microservices, React, Docker/K8s)

**Projects:**
- Kitsu — GitHub Action for AI-powered PR summaries
- Notion-Board — GitHub Action to sync GitHub Issues to Notion
- AsyncAPI-RAG — FastAPI RAG app over AsyncAPI docs

**Technologies:** JS/TS, Go, Python, SQL | Node.js, FastAPI, Gin | RAG, Qdrant, LLM, Langchain, Ollama | MySQL, Firebase | Docker, GitHub Actions

**Education:** Chandigarh University, BE Computer Science, June 2018–June 2022

---

## Common Tasks

### Update Resume Content
Edit `resume.tex` only. Push to main — GitHub Actions compiles and deploys automatically. Do NOT manually edit or commit `resume.pdf`.

### Add/Edit an Experience Entry
```latex
\begin{twocolentry}{
    Month Year – Month Year
}
    \textbf{Job Title}, Company -- City\end{twocolentry}

\vspace{0.10 cm}
\begin{onecolentry}
    \begin{highlights}
        \item Achievement bullet point here.
    \end{highlights}
\end{onecolentry}
```

### Add/Edit a Project Entry
```latex
\begin{twocolentry}{
    \href{https://github.com/Souvikns/project-name}{Project Name}
}
    \textbf{One-line description.}\end{twocolentry}

\vspace{0.10 cm}
\begin{onecolentry}
    \begin{highlights}
        \item Detail point.
    \end{highlights}
\end{onecolentry}
```

### Update "Last Updated" Text
Line 143 in `resume.tex`:
```latex
\small\color{gray}\textit{Last updated in Month Year}
```

### Update the Redirect URL
`index.html` line 14:
```js
location.href = "https://shobhnik13.github.io/resume/resume.pdf";
```

---

## Known Issues / Things to Fix

1. **README is stale** — still mentions "Abir" from an old intermediate fork. Should be updated to say this is Souvik De's resume.
2. **"Last updated" timestamp** (resume.tex line 143) — says September 2024, needs updating after content changes.
3. **Fork status** — repo still shows "forked from jai-dewani/resume" on GitHub. See detach instructions above.

---

## CI/CD Notes

- Workflow file: `.github/workflows/build.yml`
- Trigger: push to `main` (PDF files are ignored to prevent loops)
- LaTeX compiler: `xu-cheng/latex-action@v2` running inside `danteev/texlive:latest`
- Deploy action: `JamesIves/github-pages-deploy-action@3.7.1`
- Target branch: `gh-pages` (auto-created/overwritten on each deploy)
- Uses `GITHUB_TOKEN` (no extra secrets needed)
