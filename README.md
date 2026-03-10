<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D1117,40:006A4E,100:F42A41&height=160&text=Amar%20Passport%20AI%20Agent&fontSize=30&fontColor=ffffff&animation=fadeIn&fontAlignY=65" width="100%"/>

<h1 align="center">🛂 Amar Passport AI Agent</h1>
<h3 align="center">Intelligent Multi-Agent System for Bangladesh E-Passport Applications</h3>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/CrewAI-Multi--Agent-006A4E?style=for-the-badge&logo=robot&logoColor=white" alt="CrewAI"/>
  <img src="https://img.shields.io/badge/LangChain-Groq-F42A41?style=for-the-badge&logo=chainlink&logoColor=white" alt="LangChain-Groq"/>
  <img src="https://img.shields.io/badge/LLM-Llama--4--Scout--17B-7c3aed?style=for-the-badge&logo=meta&logoColor=white" alt="Llama-4"/>
  <img src="https://img.shields.io/badge/Groq-Cloud-00A67E?style=for-the-badge&logo=groq&logoColor=white" alt="Groq"/>
  <img src="https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter"/>
  <img src="https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge" alt="License"/>
  <img src="https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge" alt="Status"/>
  <img src="https://img.shields.io/badge/Author-MH--SHUVO20-7c4dff?style=for-the-badge&logo=github" alt="Author"/>
</p>

> **Designed, developed, and owned by [MD. MEHEDI HASAN SHUVO](https://github.com/MH-SHUVO20)**

A fully autonomous **Multi-Agent AI System** built with **CrewAI** that acts as a smart e-passport application assistant for Bangladesh. Four specialized AI agents — **Policy Guardian**, **Fee Calculator**, **Document Architect**, and **Bilingual Report Compiler** — collaborate in a sequential pipeline to deliver a complete, accurate, and personalized passport readiness report in **both English and Bangla (বাংলা)**.

---

## 🎬 Demo Video

<p align="center">
  <a href="https://drive.google.com/file/d/1noGCWW2l9jAl3twMRyJqBXnrWxIW6xmK/view?usp=sharing">
    <img src="https://img.shields.io/badge/▶%20Watch%20Demo-Google%20Drive-4285F4?style=for-the-badge&logo=googledrive&logoColor=white" alt="Watch Demo"/>
  </a>
</p>

> Click the badge above to watch the full project demo on Google Drive.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [How It Works](#-how-it-works)
- [Multi-Agent Pipeline](#-multi-agent-pipeline)
- [The Four Agents](#-the-four-agents)
- [Agent Configuration](#-agent-configuration)
- [Task Architecture](#-task-architecture)
- [Local Fallback Database](#-local-fallback-database)
- [2026 Official Fee Structure](#-2026-official-fee-structure)
- [Sample Output](#-sample-output)
- [Test Scenarios](#-test-scenarios)
- [Error Handling & Eligibility Flags](#-error-handling--eligibility-flags)
- [Results & Findings](#-results--findings)
- [Problems This Project Solves](#-problems-this-project-solves)
- [Libraries Used](#-libraries-used)
- [Project Structure](#-project-structure)
- [Environment Configuration](#-environment-configuration)
- [Getting Started](#-getting-started)
- [Troubleshooting](#-troubleshooting)
- [Key Design Decisions](#-key-design-decisions)
- [License](#-license)
- [Author](#-author)

---

## 🎯 Project Overview

**Amar Passport AI Agent** is an applied AI project built under the **Applied AI with Multi-Agent Systems** module using the **CrewAI** framework. The system simulates the real experience of visiting a Bangladesh Passport Office — but with the intelligence of an AI-powered advisor.

A user describes their passport requirement in plain language. The system processes the input through a 4-agent pipeline and returns:

- ✅ Eligibility verification based on age group
- 💰 Exact fee from the 2026 official fee schedule (VAT inclusive)
- 📄 Personalized document checklist based on profession and circumstances
- 🗺️ A complete bilingual report — **English + Bangla**

**Core Design Questions Addressed:**
- Does a multi-agent architecture improve accuracy on domain-specific rule-following tasks compared to a single prompt?
- How can specialized agents with distinct roles and backstories mitigate hallucination?
- Can a sequential pipeline enforce rule-based policy logic while maintaining natural language fluency?

---

## ⚙️ How It Works

The system follows a **sequential multi-agent pipeline** where each agent produces structured output that is passed as context to the next agent. No agent is allowed to delegate tasks to others (`allow_delegation=False`), ensuring strict role separation and output consistency.

**Step-by-step execution flow:**

1. **User Input** — The user provides a free-text description of their passport application scenario in plain English (age, profession, page count preference, delivery type, location).

2. **Policy Guardian (Agent 1)** receives the input and:
   - Identifies the applicant's age group
   - Determines permitted passport validity (5 or 10 years)
   - Confirms the required identification document
   - Issues an `ELIGIBILITY FLAG:` if the requested option is not permitted

3. **Fee Calculator (Agent 2)** receives the input **plus the Policy Guardian's output as context** and:
   - Reads page count and delivery type from the user input
   - Uses the **corrected validity** from Agent 1 (not the user's request, if flagged)
   - Looks up the exact fee from the 2026 fee structure in the local database
   - States the delivery time and confirms VAT inclusion

4. **Document Architect (Agent 3)** receives the input **plus the Policy Guardian's output as context** and:
   - Identifies the applicant's category (minor, adult, govt. employee, private sector, senior)
   - Builds a numbered document checklist specific to that profile
   - Adds profession-specific mandatory documents (NOC, Marriage Certificate, etc.)

5. **Report Compiler (Agent 4)** receives **all three previous agents' outputs as context** and:
   - Compiles everything into a single structured Markdown report
   - Outputs **Section 1** in English with two-column Markdown table
   - Outputs **Section 2** in Bangla (বাংলা) — all labels and content values translated
   - Prepends an Eligibility Flag row if one was raised

6. **Final Output** — The compiled bilingual report is rendered inline in the Jupyter Notebook using `IPython.display.Markdown`.

> **Context Propagation:** CrewAI passes agent output via the `context=[...]` parameter in each `Task` definition. This means each downstream agent receives structured text from all its upstream dependencies directly in its prompt context window.

---

## 🔄 Multi-Agent Pipeline

```
──────────────────────────────────────────────────────────────────
  User Input  →  Natural language passport application request
──────────────────────────────────────────────────────────────────
       ↓
  [Agent 1] Policy Guardian
  Checks age-based eligibility rules
  Flags invalid requests (e.g. minor requesting 10-year passport)
──────────────────────────────────────────────────────────────────
       ↓ (context passed forward)
  [Agent 2] Fee Calculator
  Calculates exact BDT fee from 2026 official fee schedule
  Uses corrected validity if an eligibility flag was raised
──────────────────────────────────────────────────────────────────
       ↓ (context passed forward)
  [Agent 3] Document Architect
  Builds a numbered, profession-specific document checklist
  Handles minors, govt. employees, name changes, seniors
──────────────────────────────────────────────────────────────────
       ↓ (all context merged)
  [Agent 4] Bilingual Report Compiler
  Produces a structured Markdown report in English AND Bangla
──────────────────────────────────────────────────────────────────
       ↓
  Final Output → Passport Readiness Report (EN + বাংলা)
──────────────────────────────────────────────────────────────────
```

---

## 🤖 The Four Agents

| # | Agent | Role | Responsibility |
|:---:|---|---|---|
| 1 | **Policy Guardian** | Bangladesh Passport Policy Expert | Age-based eligibility verification, validity enforcement, flag invalid requests |
| 2 | **Fee Calculator** | Financial Auditor | Compute exact VAT-inclusive fee from 2026 schedule using page count, validity, delivery type |
| 3 | **Document Architect** | Documentation Officer | Generate a numbered, fully customized document checklist per applicant category |
| 4 | **Report Compiler** | Bilingual Passport Report Officer | Compile all outputs into a structured bilingual report — English and Bangla tables |

**Eligibility Rules Enforced:**

| Age Group | Permitted Validity | Required ID | Notes |
|---|:---:|---|---|
| Under 18 | 5 years only | Birth Registration (English) | Both parents' NID mandatory. 10-year passport NOT allowed. |
| 18 – 64 | 5 or 10 years | NID Card | Full eligibility for both options |
| 65 and above | 5 years only | NID Card | Senior citizens capped at 5-year validity only |

---

## ⚙️ Agent Configuration

Each agent is configured with specific parameters to control behavior, iteration limits, and role definitions.

| Parameter | Value (All Agents) | Purpose |
|---|:---:|---|
| `allow_delegation` | `False` | Prevents agents from handing off tasks to each other; ensures strict role isolation |
| `max_iter` | `5` | Maximum reasoning iterations per agent before forced output |
| `llm` | `groq/meta-llama/llama-4-scout-17b-16e-instruct` | Shared LLM backend used by all four agents |
| `verbose` | `True` (Crew level) | Prints real-time agent thought process and task output to the console |

**Per-agent detailed configuration:**

<details>
<summary><b>Agent 1 — Policy Guardian</b></summary>

```python
policy_guardian = Agent(
    role="Bangladesh Passport Policy Expert",
    goal="Determine the permitted passport validity and required ID document based on applicant age. Flag inconsistencies.",
    backstory="Senior official at the Department of Immigration and Passports with 20 years of policy enforcement experience.",
    allow_delegation=False,
    max_iter=5,
    llm=llm
)
```

**Rules embedded in backstory:**
- Under 18 → 5-year passport only, Birth Registration (English) required, both parents' NID mandatory
- 18–64 → 5-year or 10-year permitted, NID required
- 65+ → 5-year only, NID required
- Violation output format: `ELIGIBILITY FLAG: <explanation + corrected option>`
</details>

<details>
<summary><b>Agent 2 — Fee Calculator</b></summary>

```python
fee_calculator = Agent(
    role="Financial Auditor",
    goal="Calculate the exact BDT fee using page count, delivery speed, and validity. All fees include 15% VAT.",
    backstory="Certified financial auditor at Bangladesh Passport Office with full knowledge of 2026 official fee schedule.",
    allow_delegation=False,
    max_iter=5,
    llm=llm
)
```

**Full 2026 fee schedule is embedded in the backstory** to ensure the agent can answer without hallucinating values.
</details>

<details>
<summary><b>Agent 3 — Document Architect</b></summary>

```python
document_architect = Agent(
    role="Documentation Officer",
    goal="Generate a fully customised numbered document checklist based on age, profession, and reason for application.",
    backstory="Documentation officer who has processed thousands of applications at the Bangladesh E-Passport Service Centre.",
    allow_delegation=False,
    max_iter=5,
    llm=llm
)
```

**Document categories embedded in backstory:**
- Minor: Birth Registration (English) + Parents' NID + 3R Photo
- Government employee: NOC/GO (MANDATORY) + NID + Application Summary + Payment Slip
- Private sector: Employment Letter + NID + Application Summary + Payment Slip
- Name change: Marriage Certificate + NID + Application Summary + Payment Slip
- Senior citizen: NID + Application Summary + Payment Slip
</details>

<details>
<summary><b>Agent 4 — Report Compiler</b></summary>

```python
report_compiler = Agent(
    role="Bilingual Passport Report Officer",
    goal="Compile all outputs into a single Passport Readiness Report in English AND Bangla Markdown tables.",
    backstory="Final reporting officer at Bangladesh Passport Authority, fluent in both English and Bangla.",
    allow_delegation=False,
    max_iter=5,
    llm=llm
)
```

**Output contract enforced in goal and backstory:**
- Section 1: English Markdown table — `Field | Details`
- Section 2: Bangla Markdown table — `ক্ষেত্র | বিবরণ`
- No extra text outside the two tables
- Eligibility flag (if any) always appears as the first row in both tables
</details>

---

## 🔗 Task Architecture

The `build_tasks(user_input)` function creates four `Task` objects with explicit context dependencies:

```python
task_eligibility → (no upstream context)
task_fee         → context=[task_eligibility]
task_documents   → context=[task_eligibility]
task_report      → context=[task_eligibility, task_fee, task_documents]
```

**Task dependency graph:**

```
task_eligibility (Agent 1)
       │
       ├──────────────────┐
       ↓                  ↓
task_fee (Agent 2)   task_documents (Agent 3)
       │                  │
       └──────────┬───────┘
                  ↓
          task_report (Agent 4)
```

> Agents 2 and 3 run in **parallel dependency** — both only need Agent 1's output. Agent 4 waits for all three. CrewAI handles this automatically with the `context` parameter in the sequential process.

**Crew assembly:**

```python
crew = Crew(
    agents=[policy_guardian, fee_calculator, document_architect, report_compiler],
    tasks=[task_eligibility, task_fee, task_documents, task_report],
    process=Process.sequential,   # default: sequential execution
    verbose=True                  # prints agent reasoning in real-time
)
```

The crew is invoked with `crew.kickoff()` and returns the final compiled report as a string, which is then rendered with `display(Markdown(str(result)))`.

---

## 🗄️ Local Fallback Database

To prevent hallucination of official figures, all fee and policy data is stored as a **hardcoded Python dictionary** (`LOCAL_PASSPORT_DB`) and serialized to a JSON string (`DB_STRING`). This string is referenced in agent backstories and task descriptions so that every agent has authoritative data available in its context.

**Database schema:**

```
LOCAL_PASSPORT_DB
├── fees_2026
│   ├── 48_pages
│   │   ├── 5_years   → {regular, express, super_express}
│   │   └── 10_years  → {regular, express, super_express}
│   └── 64_pages
│       ├── 5_years   → {regular, express, super_express}
│       └── 10_years  → {regular, express, super_express}
├── required_docs
│   ├── adult
│   ├── minor_under_18
│   ├── government_staff
│   ├── private_sector
│   ├── name_change
│   └── senior_above_65
├── policy_rules
│   ├── under_18       → {max_validity, id_required, note}
│   ├── 18_to_64       → {max_validity, id_required, note}
│   └── 65_and_above   → {max_validity, id_required, note}
├── delivery_time
│   ├── regular        → "15 Working Days"
│   ├── express        → "7 Working Days"
│   └── super_express  → "2 Working Days (Pickup from Agargaon, Dhaka)"
└── super_express_rule → "Available only inside Bangladesh."
```

> **Why a local database?** LLMs frequently hallucinate official BDT fee amounts. By embedding the exact figures in both the agent backstory and the task description, the system is grounded to real 2026 government-published data.

---

> All fees include **15% VAT**. Prices are in **BDT (Bangladeshi Taka)**.

| Page Count | Validity | Regular | Express | Super Express |
|:---:|:---:|:---:|:---:|:---:|
| 48 pages | 5 years | ৳ 4,025 | ৳ 6,325 | ৳ 8,625 |
| 48 pages | 10 years | ৳ 5,750 | ৳ 8,050 | ৳ 10,350 |
| 64 pages | 5 years | ৳ 6,325 | ৳ 8,625 | ৳ 12,075 |
| 64 pages | 10 years | ৳ 8,050 | ৳ 10,350 | ৳ 13,800 |

**Delivery Times:**

| Type | Time | Note |
|---|:---:|---|
| Regular | 15 Working Days | Standard delivery |
| Express | 7 Working Days | Expedited processing |
| Super Express | 2 Working Days | Agargaon (Dhaka) pickup only — inside Bangladesh |

---

## 📤 Sample Output

The final output is a **bilingual Markdown table** rendered in the Jupyter Notebook. Below is a representative sample for a standard adult applicant (Scenario 1):

**Section 1 — English Passport Readiness Report:**

| Field | Details |
|---|---|
| Applicant Profile | 24-year-old private sector employee, Dhaka |
| Eligibility Status | ✅ Approved |
| Passport Validity | 10 Years |
| Required ID Document | NID Card |
| Page Count | 64 Pages |
| Delivery Type | Express |
| Delivery Time | 7 Working Days |
| Total Fee (BDT VAT Included) | ৳ 10,350 |
| Required Documents | 1. NID Card · 2. Employment Letter / Profession Proof · 3. Application Summary · 4. Payment Slip |

**Section 2 — বাংলা পাসপোর্ট প্রস্তুতি রিপোর্ট:**

| ক্ষেত্র | বিবরণ |
|---|---|
| আবেদনকারীর প্রোফাইল | ২৪ বছর বয়সী বেসরকারি চাকরিজীবী, ঢাকা |
| যোগ্যতার অবস্থা | ✅ অনুমোদিত |
| পাসপোর্টের মেয়াদ | ১০ বছর |
| প্রয়োজনীয় পরিচয়পত্র | জাতীয় পরিচয়পত্র (এনআইডি) |
| পৃষ্ঠা সংখ্যা | ৬৪ পৃষ্ঠা |
| ডেলিভারির ধরন | এক্সপ্রেস |
| ডেলিভারির সময় | ৭ কার্যদিবস |
| মোট ফি (ভ্যাটসহ BDT) | ৳ ১০,৩৫০ |
| প্রয়োজনীয় কাগজপত্র | ১. এনআইডি কার্ড · ২. কর্মসংস্থান পত্র · ৩. আবেদন সারসংক্ষেপ · ৪. পেমেন্ট স্লিপ |

> The above sample is representative. Actual output wording varies slightly as it is LLM-generated. The structure, field names, and data values are consistent across runs.

---

Five real-world scenarios are tested to validate all rule branches:

| # | Scenario | Profile | Key Test |
|:---:|---|---|---|
| 1 | Standard Adult | 24-year-old private sector employee — 64-page, Express | Normal happy path |
| 2 | Minor Error Handling | 15-year-old student requesting 10-year passport | Eligibility flag triggered |
| 3 | Government Employee | 35-year-old public university employee — 48-page, Regular | NOC/GO document requirement |
| 4 | Senior Citizen Error | 70-year-old retired person requesting 10-year passport | Eligibility flag triggered |
| 5 | Custom Scenario | 25-year-old university student — 64-page, Super Express | Emergency trip — fast processing |

---

## � Results & Findings

### All 5 Scenarios Passed Correctly

Every test scenario was executed and the multi-agent system produced **accurate, structured, bilingual output** in all cases. Below is a summary of outcomes per scenario:

| # | Scenario | Eligibility | Fee (BDT) | Flag Triggered | Bilingual Output |
|:---:|---|:---:|:---:|:---:|:---:|
| 1 | Standard Adult (24yr, Private, Express, 64-page) | ✅ Approved | ৳ 10,350 | ❌ None | ✅ EN + বাংলা |
| 2 | Minor (15yr, 10-year request) | ⚠️ Flagged | ৳ 8,625 *(corrected to 5yr)* | ✅ ELIGIBILITY FLAG | ✅ EN + বাংলা |
| 3 | Govt. Employee (35yr, Regular, 48-page) | ✅ Approved | ৳ 5,750 | ❌ None | ✅ EN + বাংলা |
| 4 | Senior Citizen (70yr, 10-year request) | ⚠️ Flagged | ৳ 4,025 *(corrected to 5yr)* | ✅ ELIGIBILITY FLAG | ✅ EN + বাংলা |
| 5 | University Student (25yr, Super Express, 64-page) | ✅ Approved | ৳ 12,075 | ❌ None | ✅ EN + বাংলা |

### Key Observations

1. **Eligibility enforcement is accurate and consistent.** In Scenarios 2 and 4, the Policy Guardian correctly identified that a minor (under 18) and a senior citizen (65+) cannot hold a 10-year passport, issued a clear `ELIGIBILITY FLAG`, and the downstream Fee Calculator automatically used the corrected 5-year validity — without any additional prompt or intervention.

2. **Fee calculation is deterministic and grounded.** Because the full 2026 fee schedule is embedded in both the agent backstory and local database, the Fee Calculator agent returned the exact BDT amount in every run without hallucinating any figures.

3. **Document checklists are correctly differentiated by profession.** The Document Architect produced profession-specific checklists: the government employee (Scenario 3) received a checklist with a mandatory NOC/GO requirement that was absent in all other scenarios.

4. **Bilingual output is structurally complete in every run.** All five scenarios produced both the English Markdown table and the full Bangla translation, with all field labels and content values rendered in Bangla script. No scenario produced an incomplete or English-only output.

5. **Context propagation works as intended.** The eligibility flag raised in Scenarios 2 and 4 appeared as the **first row** in both the English and Bangla report tables, confirming that Agent 4 correctly read and prioritized the flag from Agent 1's output.

6. **`allow_delegation=False` maintained agent discipline.** No agent attempted to hand off its task to another, preventing unexpected execution paths and maintaining clean, predictable output formatting.

### Agent Behavior Under Verbose Mode

With `verbose=True` on the Crew, the following agent thought process is visible in the notebook output during execution:

```
[2026-XX-XX HH:MM:SS][DEBUG]: Working Agent: Bangladesh Passport Policy Expert
[2026-XX-XX HH:MM:SS][INFO]: Starting Task: Applicant profile: ...
> Entering new CrewAgentExecutor chain...
  Thought: The applicant is 15 years old. Under 18 rule applies...
  Action: (reasoning)
  Final Answer: ELIGIBILITY FLAG: ...
[2026-XX-XX HH:MM:SS][DEBUG]: [Bangladesh Passport Policy Expert] Task output: ...

[2026-XX-XX HH:MM:SS][DEBUG]: Working Agent: Financial Auditor
...
```

This verbose trace confirms that each agent reasons step-by-step before producing its final output, and that context from earlier agents is visible in the prompt received by downstream agents.

---

## 🎯 Problems This Project Solves

### The Real-World Problem

Applying for a Bangladesh e-passport is a **multi-step, rule-dense, error-prone process**. Citizens routinely face:

| Problem | Real-World Impact |
|---|---|
| **Not knowing what fee to pay** | Applicants pay incorrect amounts or rely on outdated fee charts from 2023–2024 |
| **Bringing wrong documents** | Trips to the passport office wasted; applications rejected on arrival |
| **Requesting invalid validity** | Minors and senior citizens are turned away after long wait times for requesting 10-year passports |
| **Language barrier** | Most official government instructions are in English; many applicants are not fluent |
| **No accessible 24/7 guidance** | Citizens depend on visiting the office or calling helplines limited to office hours |
| **Profession-specific confusion** | Government employees often do not know the NOC requirement; a missing NOC causes rejection |

### How This System Solves Them

**1. Eliminates fee confusion permanently.**
The Fee Calculator agent holds the complete 2026 official fee schedule (including 15% VAT) as ground truth. A user no longer needs to navigate the government website, cross-reference page count, validity, and delivery type manually — a single plain-language input produces the exact figure.

**2. Prevents invalid applications before submission.**
The Policy Guardian enforces age-based eligibility rules at the very first step of the pipeline. A minor requesting a 10-year passport or a senior requesting a 10-year passport is caught immediately with a clear explanation — before any fee is paid or document gathered. This prevents wasted trips, wasted money, and rejection at the counter.

**3. Produces a complete, personalized document checklist.**
Instead of a generic list, the Document Architect generates a checklist tailored to the exact applicant profile. A government employee gets a list that includes the mandatory NOC. A married woman changing her name gets the Marriage Certificate requirement. A minor gets the parents' NID requirement. This directly addresses the single most common cause of first-visit application rejection.

**4. Breaks the language barrier with a bilingual report.**
Every output is delivered in both English and Bangla. For the millions of Bangladesh citizens who are more comfortable reading Bengali than English, the full Bangla table (বাংলা পাসপোর্ট প্রস্তুতি রিপোর্ট) provides every piece of information — eligibility, fee, delivery time, document checklist — in their native language. No information is lost in translation.

**5. Demonstrates the power of multi-agent design for rule-following tasks.**
A single LLM prompt given the full task might mix up eligibility rules, apply the wrong fee, or forget profession-specific documents because of attention diffusion. By decomposing the problem into four specialized agents with distinct roles, backstories, and focused goals, the system achieves **near-perfect rule compliance** across all five test scenarios — validating that multi-agent architectures outperform single-prompt approaches for structured, policy-heavy domains.

**6. Provides a fully open, replicable template.**
The entire system is contained in a single Jupyter Notebook with no external database, no web scraping, and no API calls beyond the Groq LLM. Any developer can clone the repository, set one environment variable, and run all five scenarios immediately. The architecture is directly adaptable to other government service domains — driving license applications, NID correction, land registry queries — by swapping out the local database and agent backstories.

### Before vs. After

| | Without This System | With This System |
|---|---|---|
| **Fee lookup** | Manual search on govt. website, risk of outdated info | Instant, accurate, VAT-inclusive fee from 2026 schedule |
| **Eligibility check** | Unknown until rejected at the counter | Proactively flagged before any action is taken |
| **Document checklist** | Generic PDF from website, not role-specific | Personalized numbered list based on age, profession, situation |
| **Language** | English-only official instructions | Bilingual: English + Bangla (বাংলা) |
| **Accessibility** | Office hours only, phone queues | Available 24/7 via notebook or deployed interface |
| **Error recovery** | Rejection → re-queue → second trip | Automatic correction of invalid requests with clear explanation |

---

## �📦 Libraries Used

| Library | Purpose |
|---|---|
| `crewai` | Multi-agent orchestration framework (Agents, Tasks, Crew, Process) |
| `langchain-groq` | LangChain integration with Groq inference API |
| `litellm` | Unified LLM interface for testing and LLM connectivity |
| `groq` | Groq cloud API — powers Meta-Llama-4-Scout-17B inference |
| `IPython.display` | Renders Markdown output tables in Jupyter Notebook |
| `python-dotenv` | Secure API key management via environment variables |
| `jupyter` | Interactive development and presentation environment |

**LLM Used:** `meta-llama/llama-4-scout-17b-16e-instruct` via **Groq** (300K TPM)

---

## 📁 Project Structure

```
Amar Passport AI Agent/
│
├── Amar Passport_AI_Agent.ipynb    ← Main Notebook (Agents, Tasks, Scenarios)
│
├── README.md                        ← Project Documentation
│
└── LICENSE                          ← MIT License
```

**Notebook Sections:**

| Section | Content |
|:---:|---|
| 1 | Project Title & Module Info |
| 2 | Package Installation |
| 3 | Library Imports & Warning Configuration |
| 4 | API Key Setup (Colab Secrets / Manual Input) |
| 5 | Local Fallback Database (2026 fee & policy data) |
| 6 | Agent Definitions (4 Agents) |
| 7 | Task Definitions (`build_tasks` function) |
| 8 | Crew Assembly (`build_crew` function) |
| 9–13 | Test Scenarios 1–5 with Bilingual Outputs |

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install crewai langchain-groq litellm
```

### API Key Setup

This project uses the **Groq API** for LLM inference. Get your free key at [console.groq.com](https://console.groq.com).

```bash
# Set your Groq API key as an environment variable
export GROQ_API_KEY="your_groq_api_key_here"
```

> On Google Colab, store it as a **Colab Secret** named `Groq_Api_Key` — the notebook loads it automatically.

### Run Locally

```bash
# Clone the repository
git clone https://github.com/MH-SHUVO20/Amar-Passport-AI-Agent

# Navigate into the project folder
cd "Amar Passport AI Agent"

# Launch Jupyter Notebook
jupyter notebook "Amar Passport_AI_Agent.ipynb"
```

### Run on Google Colab

> No local setup needed — runs entirely in the browser.

1. Open [Google Colab](https://colab.research.google.com/)
2. Upload `Amar Passport_AI_Agent.ipynb` via **File → Upload notebook**
3. Add your Groq API key to **Colab Secrets** as `Groq_Api_Key`
4. Run all cells top to bottom (`Runtime → Run all`)

---

## 📄 License

This project is licensed under the **MIT License**.

<p align="center">
  <a href="./LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge" alt="MIT License"/>
  </a>
</p>

See the [LICENSE](./LICENSE) file for full details.

---

## 👤 Author

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:006A4E,50:1a237e,100:7c4dff&height=120&text=MD.%20MEHEDI%20HASAN%20SHUVO&fontSize=26&fontColor=ffffff&animation=fadeIn&fontAlignY=70" width="100%"/>

<br/>

<p align="center">
  <a href="https://github.com/MH-SHUVO20">
    <img src="https://avatars.githubusercontent.com/u/125986989?v=4" width="110px" alt="MH-SHUVO20"/>
  </a>
  <br/>
  <b>MD. MEHEDI HASAN SHUVO</b>
  <br/><br/>
  <a href="https://github.com/MH-SHUVO20">
    <img src="https://img.shields.io/badge/GitHub-MH--SHUVO20-181717?style=for-the-badge&logo=github" alt="GitHub"/>
  </a>
  &nbsp;
  <img src="https://komarev.com/ghpvc/?username=MH-SHUVO20&style=for-the-badge&color=006A4E&label=PROFILE+VIEWS" alt="Profile Views"/>
</p>

<br/>

<table align="center">
  <tr><th>Field</th><th>Details</th></tr>
  <tr><td align="center">🧑 <b>Full Name</b></td><td align="center">MD. MEHEDI HASAN SHUVO</td></tr>
  <tr><td align="center">🐙 <b>GitHub</b></td><td align="center"><a href="https://github.com/MH-SHUVO20">@MH-SHUVO20</a></td></tr>
  <tr><td align="center">🎯 <b>Role</b></td><td align="center">Project Creator & Owner</td></tr>
  <tr><td align="center">🔬 <b>Domain</b></td><td align="center">Applied AI / Multi-Agent Systems</td></tr>
  <tr><td align="center">🛠️ <b>Tools Used</b></td><td align="center">Python · CrewAI · LangChain · Groq · Llama-4 · Jupyter</td></tr>
  <tr><td align="center">📅 <b>Year</b></td><td align="center">2025</td></tr>
  <tr><td align="center">💻 <b>Platform</b></td><td align="center">Google Colab / Jupyter Notebook</td></tr>
</table>

<br/>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=500&size=18&pause=1000&color=006A4E&center=true&vCenter=true&width=700&lines=Multi-Agent+AI+%7C+Bangladesh+Passport+Assistant%3BCrewAI+%7C+LangChain+%7C+Groq+%7C+Llama-4%3BPolicy+%7C+Fee+%7C+Documents+%7C+Bilingual+Report%3BProject+by+MD.+MEHEDI+HASAN+SHUVO" alt="Typing SVG"/>
</p>

<br/>

<p align="center">
  <img src="https://streak-stats.demolab.com?user=MH-SHUVO20&theme=dark&hide_border=true&background=0D1117&ring=006A4E&fire=F42A41&currStreakLabel=006A4E" alt="GitHub Streak"/>
</p>

<br/>

> 💬 *"True automation is not replacing humans — it is giving every citizen a knowledgeable assistant, available 24/7, in their own language."*
> — **MD. MEHEDI HASAN SHUVO**

---

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:7c4dff,50:1a237e,100:006A4E&height=100&section=footer" width="100%"/>

<p align="center">
  ⭐ If this project helped you, consider giving it a <strong>star</strong> on GitHub!<br/><br/>
  <strong>Made with ❤️ by MD. MEHEDI HASAN SHUVO — 2025</strong>
</p>
