# Neural-AI-Red-Team-Range
A hands-on, HackTheBox-style training lab for red teaming AI systems.

<p align="center">
  <img src="assets/banner.svg" alt="Neural Range — AI Red Team Training Lab" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-9fef00" alt="MIT license">
  <img src="https://img.shields.io/badge/build-no%20build%20step-2ee6d6" alt="No build step">
  <img src="https://img.shields.io/badge/dependencies-zero-5cb2ff" alt="Zero dependencies">
  <img src="https://img.shields.io/badge/OWASP%20LLM-2025-ffb000" alt="OWASP LLM Top 10 2025">
  <img src="https://img.shields.io/badge/MITRE-ATLAS-b48cff" alt="MITRE ATLAS">
</p>

# Neural Range

A hands-on, HackTheBox-style training lab for red teaming AI systems. You attack simulated-but-realistic targets in the browser, capture flags, generate a commercial-grade report from what you found, and prepare for the HTB Certified Offensive AI Expert (COAE). It runs as a single static site — no backend, no build step, no dependencies.

Coverage spans LLM/GenAI offensive testing and adversarial ML, mapped to the **OWASP Top 10 for LLM Applications (2025)** and **MITRE ATLAS**.

> [!WARNING]
> **Authorized use only.** Every target in this app is a local simulation. The techniques here are for systems you own or have explicit written permission to test. Unauthorized access to computer systems is a crime in most jurisdictions.

---

## Live demo

After deploying to GitHub Pages, the app is served at:

```
https://<your-username>.github.io/neural-range/
```

Replace `<your-username>` with your GitHub handle. See [Deploy to GitHub Pages](#deploy-to-github-pages). The repository ships anonymized — no name or email is baked into the code, and the report generator defaults to a generic "Red Team Operator" tester.

---

## What it is

Most LLM security material is a wall of prose. This is a range you operate. Each module pairs a short brief with a live target you attack, then checks your understanding. Captured flags, quiz scores, and report metadata save to your browser's local storage.

Four ways to practise, plus two career tools, all self-contained:

- **Simulated attack sandbox** — a vulnerable target per module. Type payloads into a terminal and watch it leak, break, or execute. The indirect-injection and agent labs render a tool log; the output-handling lab renders your payload into a fake victim app and fires the XSS / SQLi / markdown beacon.
- **Guided capture-the-flag** — 24 flags across 9 modules, escalating from nuisance to real impact, with progressive hints and a full walkthrough on every lab.
- **Knowledge checks** — a scored quiz per module with an explanation on every answer.
- **The Armory** — a copy-paste payload library, a 14-step engagement checklist, and real commands for production tooling (Garak, PyRIT, Promptfoo, DeepTeam).
- **Report Builder** — turns your captured flags into a commercial-grade report with CWE, CVSS v3.1, root-cause analysis and remediation per finding. Export to Markdown or HTML, or print to PDF.
- **COAE Exam Prep** — a practical field guide to the 7-day exam: prerequisites, a day-by-day plan, and how to hit the reporting bar.

---

## Curriculum

| # | Module | OWASP / Area | Flags | What you practise |
|---|--------|--------------|:-----:|-------------------|
| 01 | Recon & Threat Modeling | Pre-engagement | 3 | Fingerprint the model, map the attack surface, find guardrail boundaries |
| 02 | Prompt Injection (Direct) | LLM01 | 3 | Instruction override, role reassignment, secret leakage |
| 03 | System Prompt Extraction | LLM07 | 2 | Leak the hidden instruction layer and the secrets stuffed into it |
| 04 | Jailbreaks & Guardrail Bypass | LLM01 | 3 | Role-play, framing, and obfuscation bypasses against a benign target |
| 05 | Indirect Prompt Injection | LLM01 | 2 | Plant a payload in content an agent reads; exfiltrate with victim privileges |
| 06 | Sensitive Information Disclosure | LLM02 | 2 | Cross-user leakage, RAG over-retrieval, training-data regurgitation |
| 07 | Improper Output Handling | LLM05 | 3 | XSS, markdown-image exfiltration, and SQLi through model output |
| 08 | Excessive Agency & Agentic Attacks | LLM06 | 3 | Tool enumeration, confused-deputy SSRF, tool-chaining to RCE |
| 09 | Adversarial ML — Evasion | AI integrity | 3 | FGSM, targeted attacks, and an interactive evasion bench with live gradients |

Reference and career sections sit alongside the modules: **Methodology & Reporting**, a **Frameworks Map** (OWASP LLM Top 10, MITRE ATLAS, NIST AI RMF), the **Report Builder**, and **COAE Exam Prep**.

### Module 09 — the evasion bench

Adversarial ML is the part of the COAE most people find steep. Module 09 includes an interactive bench built on a small linear classifier with exact gradients, so you can see — pixel by pixel — why a prediction flips. Run untargeted FGSM, force a chosen target class, and run the random-noise control to prove that the gradient *direction*, not the perturbation *size*, is what breaks the model. The brief carries the PyTorch FGSM and PGD code you need to build the same attacks against real models.

---

## Quick start (run locally)

The app is plain static files.

**Just open it.** Double-click `index.html`. It works from the filesystem; progress saves to local storage.

**Or serve it (recommended)** — mirrors how GitHub Pages serves it and avoids any `file://` quirks:

```bash
python -m http.server 8000     # then open http://localhost:8000
# or:  npx serve .
```

No `npm install`, no bundler, no framework. The only requirement is a modern browser.

---

## Deploy to GitHub Pages

### Path A — deploy from a branch (simplest)

1. Create a repo named `neural-range` and push these files to `main`:
   ```bash
   git init
   git add .
   git commit -m "Neural Range: AI red team training lab"
   git branch -M main
   git remote add origin https://github.com/<your-username>/neural-range.git
   git push -u origin main
   ```
2. Repo **Settings → Pages**.
3. **Build and deployment → Source → Deploy from a branch**.
4. Branch `main`, folder `/ (root)`. Save.
5. Wait a minute, then open `https://<your-username>.github.io/neural-range/`.

The included `.nojekyll` makes Pages serve the `assets/` folder as-is instead of running Jekyll.

### Path B — GitHub Actions (automatic on every push)

This repo ships `.github/workflows/deploy-pages.yml`. Push to `main`, then set **Settings → Pages → Source** to **GitHub Actions**. Every push to `main` publishes automatically; watch the **Actions** tab.

> Asset paths are relative, so the site works under any repo name or sub-path without edits.

---

## Project structure

```
neural-range/
├── index.html                      # Lean HTML shell + boot screen; loads css and js
├── assets/
│   ├── banner.svg                  # README/header artwork
│   ├── css/
│   │   └── range.css               # All styles (HackTheBox-style dark/neon theme)
│   └── js/
│       ├── data.js                 # CONTENT: modules, labs, quizzes, armory, frameworks,
│       │                           #          findings DB (CWE/CVSS), exam-prep guide
│       └── app.js                  # LOGIC: state, router, renderers, lab engine,
│                                   #        evasion bench, report builder, quiz, boot
├── .github/workflows/deploy-pages.yml
├── .nojekyll
├── .gitignore
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

Content and logic are separate: you add training material in `data.js` and never touch `app.js`.

---

## How it works

The app is data-driven. `app.js` is the engine; `data.js` is the content.

- **Router** swaps views into `#main` and re-renders the sidebar — dashboard, each module, armory, methodology, frameworks, report builder, exam prep.
- **Modules.** Each entry in `MODULES` carries a `brief` (HTML), a `lab`, and a `quiz`.
- **Chat labs.** Each target is a function that takes your input and returns a reply, any flags captured, and optional side-panel content (a rendered DOM or a tool log). Targets match input against technique patterns rather than executing anything real.
- **Evasion bench** (`lab.mode === 'evasion'`). A linear classifier with exact gradient math — FGSM untargeted/targeted and a noise control — rendered as probability bars and pixel grids.
- **Report builder.** Maps each captured flag to a finding in the `FINDINGS` knowledge base (CWE, CVSS v3.1 vector, root cause, remediation) and assembles a full report, exportable as Markdown/HTML or printable.
- **Progress** persists in `localStorage` under one key, with an in-memory fallback if storage is blocked.

Nothing leaves the browser. No telemetry, no network calls.

---

## Extending it — add a module

Append an object to the `MODULES` array in `assets/js/data.js`. A chat-lab module:

```js
{
  id:'newmod', num:'10', code:'CODE', title:'Title', diff:'Medium',
  owasp:'LLM0x:2025', atlas:'AML.Txxxx',
  tagline:'One-line summary.',
  brief:`<p class="lead">Theory as HTML. Reuse .kmap, .danger-note, pre.code, .prose.</p>`,
  lab:{
    scenario:'Target', target:'TARGET-ID', termTitle:'target://id',
    placeholder:'Input hint', intro:'Lab intro', firstBot:'Optional first message',
    objectives:[{id:'nm-1', title:'Objective', flag:'HTB{flag}', pts:20}],
    hints:['Hint 1','Hint 2','Hint 3'],
    solution:`<p>Walkthrough.</p>`,
    engine:function(t,L){ var x=_lc(t); if(_any(x,[/pattern/])) return {reply:'Pwned.',flags:['nm-1']}; return {reply:'Default.'}; }
  },
  quiz:[{q:'Question?', a:['A','B','C','D'], correct:1, why:'Why B.'}]
}
```

To make a captured flag appear in the Report Builder, add a matching entry to the `FINDINGS` map keyed by the objective id (`nm-1`), with `cwe`, `cvss`, `sev`, `rootCause`, `remediation` and friends. Engine helpers: `_lc`, `_any`, `_esc`, `renderToolLog`. The dashboard, sidebar, flag totals and rank update automatically.

---

## Frameworks referenced

- **OWASP Top 10 for LLM Applications (2025)** — the vulnerability vocabulary. The 2025 revision split System Prompt Leakage (LLM07) and Vector & Embedding Weaknesses (LLM08) into their own entries and broadened DoS into Unbounded Consumption (LLM10).
- **MITRE ATLAS** — adversary tactics and techniques for ML systems, structured like ATT&CK, with heavy 2025 coverage of generative AI and AI agents.
- **NIST AI RMF** — the governance frame (Govern, Map, Measure, Manage) that red-team output feeds.
- **CWE / CVSS v3.1** — the Report Builder tags findings with CWE (including CWE-1427 for LLM prompt injection and CWE-1039 for adversarial perturbations) and CVSS v3.1 vectors.

OWASP names the bugs, ATLAS names the TTPs, NIST frames how the business manages them, CWE/CVSS make findings reportable.

---

## Roadmap

Ideas, not promises. Pull requests welcome.

- A multi-turn "crescendo" lab where the target tracks conversation state across messages.
- Deeper adversarial-ML labs (data poisoning / backdoor triggers, model extraction, membership inference).
- Export/import of progress as JSON.
- A defender mode that pairs each attack lab with the mitigating control.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Short version: add content in `data.js`, keep `app.js` generic, test by opening `index.html`, and keep every lab target a harmless simulation.

---

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, teach with it.

---

## References

- [OWASP Top 10 for LLM Applications 2025](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/)
- [MITRE ATLAS](https://atlas.mitre.org/)
- [CWE-1427 — Improper Neutralization of Input Used for LLM Prompting](https://cwe.mitre.org/data/definitions/1427.html)
- [CWE-1039 — Adversarial Input Perturbations](https://cwe.mitre.org/data/definitions/1039.html)
- [HTB Certified Offensive AI Expert (COAE)](https://academy.hackthebox.com/news/the-new-htb-certified-offensive-ai-expert-htb-coae-is-officially-here)
- [Microsoft PyRIT](https://github.com/Azure/PyRIT) · [NVIDIA Garak](https://github.com/NVIDIA/garak) · [Promptfoo](https://www.promptfoo.dev/docs/red-team/) · [DeepTeam](https://www.trydeepteam.com/)

---

<p align="center"><sub>Built for learning. Hack only what you are allowed to hack.</sub></p>
