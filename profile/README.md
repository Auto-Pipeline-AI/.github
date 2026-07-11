<div align="center">

<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/logo-light-bg.png#gh-light-mode-only" width="520" alt="AutoPipelineAI">
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/logo-dark-bg.png#gh-dark-mode-only" width="520" alt="AutoPipelineAI">

### Turn a plain-English sentence into a validated CI/CD pipeline.

[![Latest release](https://img.shields.io/github/v/release/Auto-Pipeline-AI/AutoPipeline?label=latest%20release&color=0E0E10&logo=github)](https://github.com/Auto-Pipeline-AI/AutoPipeline/releases/latest)
[![License: MIT](https://img.shields.io/badge/license-MIT-1F9D5B)](https://github.com/Auto-Pipeline-AI/AutoPipeline/blob/main/LICENSE)
[![Paper](https://img.shields.io/badge/paper-arXiv%3A2606.06662-3457D5)](https://arxiv.org/abs/2606.06662)
[![Tests](https://img.shields.io/badge/tests-459%20passing-1F9D5B)](#-testing)
[![Eval](https://img.shields.io/badge/eval-0%20hallucinations%20%2F%20240%20reqs-3457D5)](#-evaluation)

</div>

---

## 👋 About

**AutoPipelineAI** is a graduation project from the **Faculty of Computers and
Artificial Intelligence (FCAI), Cairo University**, developed under the
technical mentorship of **Siemens Egypt** and the supervision of
**Dr. Mohammad El-Ramly**.

Writing CI/CD configuration by hand is slow and easy to get wrong: you have to
learn platform-specific YAML, guess at the right actions and versions, and
debug failures one push at a time. AutoPipelineAI does that work for you.
Point it at a real codebase, describe what you want in plain English, and a
multi-agent LLM system analyzes your project, plans a pipeline, generates
GitHub Actions or GitLab CI YAML, validates it with real linters, and
**self-corrects** until it is valid. The whole process streams to you live,
and you approve every shell command and file write along the way.

It is delivered as a **three-tier desktop product** (an Electron / Next.js
client, a NestJS backend-for-frontend, and a FastAPI multi-agent engine),
split across the three repositories in this organization and composed into one
runnable app by the [`AutoPipeline`](https://github.com/Auto-Pipeline-AI/AutoPipeline)
launcher.

<div align="center">
<sub>with thanks to</sub><br/><br/>

<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/logo-cairo-university.png" height="80" alt="Cairo University">
&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/logo-fcai.png" height="80" alt="FCAI">
&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/logo-siemens.png" height="50" alt="Siemens">

</div>

---

## 🚀 Start here

You don't need to touch the three repos individually. The
[**`AutoPipeline`**](https://github.com/Auto-Pipeline-AI/AutoPipeline) repo is a
one-command launcher that clones all three, installs everything, and runs them
together as a single desktop app:

```bash
git clone https://github.com/Auto-Pipeline-AI/AutoPipeline.git
cd AutoPipeline
npm run setup   # clone the 3 app repos + install dependencies
npm start       # launch agent + backend + frontend + the desktop window
```

You pick a provider and paste a key inside the app itself, encrypted in your OS
keychain.

<div align="center">
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/screenshot-home.png" width="47%" alt="Project picker (dark theme)">
&nbsp;
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/screenshot-workspace-light.png" width="47%" alt="Session workspace (light theme)">
</div>

---

## 🏗️ Architecture

A deterministic Supervisor (zero LLM calls) routes between specialized agents
(Planner, Analyzer, Researcher, Generator, Validator, Writer) on a LangGraph
graph. The backend is a thin proxy and persistence layer, and the frontend
never talks to the agent directly.

<div align="center">
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/architecture.svg" width="640" alt="AutoPipeline three-tier architecture">
</div>

## 📦 Repositories

| Repo | Role | Port |
|---|---|---|
| [**AutoPipeline**](https://github.com/Auto-Pipeline-AI/AutoPipeline) | One-command launcher: clones and runs the other three as one app | n/a |
| [**agent**](https://github.com/Auto-Pipeline-AI/agent) | Multi-agent LLM engine (Python/FastAPI): analyzes, plans, generates, validates, self-corrects | `:8000` |
| [**backend**](https://github.com/Auto-Pipeline-AI/backend) | Backend-for-frontend (NestJS): sessions, persistence, NDJSON proxy, model catalog | `:3333` |
| [**frontend**](https://github.com/Auto-Pipeline-AI/frontend) | Desktop client (Next.js + Electron): streaming chat UI, HITL prompts, key storage | `:3001` |

---

## ✨ Features

- **Multi-agent engine**: Planner, Analyzer, Researcher, Generator, Validator, and Writer agents orchestrated by a deterministic, zero-LLM Supervisor.
- **Repository-aware**: the Analyzer inspects your actual build files, test runners, and deploy configs so pipelines are grounded in reality, not guesses.
- **Self-correcting reflexion loop**: generated YAML is linted with `actionlint` (GitHub Actions) or the GitLab CI Lint API, and regenerated automatically on failure.
- **Live web research**: a ReAct Researcher agent looks up current action versions and syntax rather than relying on stale training data.
- **Multi-platform**: GitHub Actions and GitLab CI, from the same natural-language prompt.
- **Multi-provider LLMs**: OpenAI, Anthropic, Google Gemini, Groq, or any OpenAI-compatible endpoint (Ollama, vLLM, LM Studio); provider, model, and key are supplied per-request.
- **Real-time NDJSON streaming**: see reasoning tokens, tool calls, plans, and YAML drafts as they happen, not just the final result.
- **Human-in-the-loop**: explicit approval gates before running shell commands or writing files to disk.
- **Security by design**: commands are classified safe / modifying / dangerous before execution, and streamed payloads are scanned with entropy-based secret detection.
- **Desktop-native**: Electron provides a project folder picker, OS-keychain-encrypted API key storage, and a file-save bridge.

---

## 🧰 Tech stack

| Tier | Technologies |
|---|---|
| **Agent** (intelligence) | Python 3.11+, FastAPI, LangGraph + LangChain, LiteLLM, Pydantic v2, `actionlint`, `detect-secrets` |
| **Backend** (BFF) | Node.js 20+, NestJS 11, TypeScript, Prisma 7, SQLite |
| **Frontend** (client) | Next.js 16, React 19, Electron 41, TypeScript, Tailwind CSS 4 |
| **Testing & tooling** | pytest, Jest, Vitest, ESLint, GitHub Actions CI, PlantUML/Kroki |

---

## 📊 Evaluation

The pipeline-generation capability was evaluated end-to-end on **six
open-source repositories** across four languages (Click and Peewee in Python,
Hugo in Go, p-limit and Vite in JS/TS, Rustlings in Rust) using **four LLMs**
(GLM, Qwen, Kimi, MiniMax). Each project was judged against 10 repository-specific
functional requirements (240 requirements in total), asking a concrete question
of every one: does the generated pipeline actually do what was asked?

<div align="center">
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/eval-pass-rate.png" width="520" alt="Overall functional pass rate by model">
</div>

<table align="center">
<tr><th>Model</th><th>Pass rate</th><th>Errors</th><th>Hallucinations</th></tr>
<tr><td>GLM</td><td><b>78.3%</b></td><td>19</td><td>0</td></tr>
<tr><td>Qwen</td><td>75.0%</td><td>18</td><td>0</td></tr>
<tr><td>Kimi</td><td>71.7%</td><td>20</td><td>0</td></tr>
<tr><td>MiniMax</td><td>60.0%</td><td>29</td><td>0</td></tr>
</table>

Reliability varies by project and model, so the per-project breakdown matters:

<div align="center">
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/eval-functional-by-project.png" width="640" alt="Functional pass rate by project and model">
</div>

### Why zero hallucinations?

Across all 240 evaluated requirements, **not one model hallucinated**. The
error breakdown makes the point clearly: every failure was a Configuration
Error or an Omission (a missing step), never an invented action, tool, or fact.

<div align="center">
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/eval-error-distribution.png" width="600" alt="Error-type distribution per model; the Hallucination row is zero everywhere">
</div>

That result is by design, not luck. No single component is trusted to be
correct on its own; every stage either grounds the model in something real or
checks its output against something real before the run moves forward. The
Analyzer reads your actual repo instead of guessing, the Researcher pulls live
docs instead of relying on stale training data, and structured-output schemas
keep responses on rails instead of free-form text. Most importantly, the
Generator's YAML is never taken on faith: it is linted by **real tools**
(`actionlint`, the GitLab CI Lint API), and on failure the concrete errors are
fed back into a bounded reflexion loop that regenerates until the output is
valid or the retry limit is hit. Nothing reaches the Writer, and nothing is
written to disk, until it has survived that gate.

<div align="center">
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/agent-graph.svg" width="820" alt="Multi-agent orchestration graph: a deterministic Supervisor routes between LLM and non-LLM nodes">
</div>

See the [associated paper](https://arxiv.org/abs/2606.06662) for the full
per-project results.

---

## ✅ Testing

Each service ships its own hermetic, boundary-mocked test suite that runs in
CI. Together they add up to **459 automated tests**:

| Service | Framework | Tests | Coverage |
|---|---|---|---|
| Agent | pytest | 310 (265 unit + 45 integration) | 84% |
| Backend | Jest | 106 unit | 87% |
| Frontend | Vitest | 43 unit | 86% |

---

## 🔒 Security

LLM API keys are supplied per-request and stored encrypted in the OS keychain,
and are never committed or logged. Shell commands are classified
safe / modifying / dangerous and gated behind an explicit human approval
prompt, and file writes require the same. All streamed payloads pass through
entropy-based secret redaction before reaching the UI.

---

## 🎓 Team & paper

Built by Youssef Mohamed, Mohamed Ahmed Hemdan, Mahmoud Saleh Saad, Ahmed
Mohamed Tolba, and Seif Gamal Abdelmonem, supervised by
**Dr. Mohammad El-Ramly** (FCAI, Cairo University).

The project is also written up as a research paper,
*"AutoPipelineAI: Context-Aware CI/CD Pipeline Generation from Natural
Language"*, available on [arXiv (2606.06662)](https://arxiv.org/abs/2606.06662).

---

<div align="center">
<sub>All three services are MIT licensed.</sub>
</div>

---

## 🎬 See it in action

One prompt turns into a validated GitHub Actions pipeline — the agent analyzes
the repo, plans, streams the reasoning and YAML live, self-corrects, and asks
for approval before writing the file:

<div align="center">
<img src="https://raw.githubusercontent.com/Auto-Pipeline-AI/.github/main/profile/assets/demo.gif" width="820" alt="AutoPipelineAI generating a CI/CD pipeline end to end">
</div>

<div align="center">
<img src="https://count.getloli.com/@auto-pipeline-ai?theme=rule34" alt="visitor counter">
</div>
