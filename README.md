# CareerPilot AI — Resume Intelligence Dashboard

A hackathon-ready Streamlit app for comparing one or many text-based PDF resumes against a job description. It provides an explainable heuristic screening score, detected skill coverage, candidate comparisons, local screening history, a resume coach, interview practice questions, and a learning roadmap.

## Features

- **Dashboard:** total resumes screened, average screening score, average skill coverage, job profiles, recent score chart, and latest reports.
- **Batch screening:** upload multiple PDF resumes against one target role and compare results in a ranked table.
- **Explainable percentage:** `Screening score = 70% × detected skill coverage + 30% × TF-IDF text similarity`.
- **Skill-gap analysis:** matching skills, detected gaps, and skill lists from resume/job text.
- **Optional Groq AI assessment:** richer explanation when `GROQ_API_KEY` is configured; local matching still works without it.
- **Resume Coach:** evidence-based strengths, improvements, keywords, project ideas, and a suggested professional summary.
- **Interview Prep:** role- and skill-specific practice questions with answer-drafting fields.
- **Learning Roadmap:** turns detected gaps into practical learning steps.
- **Past Reports:** searchable screening history persisted in a local SQLite database, with JSON export and optional history deletion.
- **Exports:** CSV candidate comparison, JSON reports, and text-based interview/roadmap downloads.

## Setup on Windows (PowerShell)

From the extracted project folder, run:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
Copy-Item .env.example .env
notepad .env
```

Add your Groq key to `.env` if you want AI features:

```env
GROQ_API_KEY=your_actual_key_here
GROQ_MODEL=openai/gpt-oss-120b
```

Then run:

```powershell
python -m streamlit run app.py
```

## How scores work

- **Text similarity** uses TF-IDF + cosine similarity. It measures wording overlap and may be low even when relevant skills exist.
- **Detected skill coverage** is the percentage of skills recognised by the built-in vocabulary that are present in both the job description and resume.
- **Screening score** is a transparent heuristic: 70% detected skill coverage + 30% text similarity. It is designed to help organise human review, not estimate the probability of getting hired or job performance.
- **AI assessment** can make mistakes. Check suggestions against the source resume and job description; never invent skills, qualifications, or experience.

## Important limitations and responsible use

- Scanned/image-only PDFs require OCR; OCR is not included.
- The skill vocabulary is a starter list and can miss synonyms, context, and domain-specific terms.
- A missing keyword does not prove someone lacks a skill. Review resumes in context and do not use the score as the sole basis for employment decisions.
- Screening history is stored locally in `careerpilot_history.db`. The database and `.env` are ignored by Git; do not publish private resumes or API keys.
- This prototype is single-user/local, not a secure multi-user production hiring system. Obtain permission to process candidate data and follow applicable privacy rules.
