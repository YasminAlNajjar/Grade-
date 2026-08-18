# Grade+ — Automated Student Grading System

Grade+ is an AI-powered automated grading system that allows instructors to calculate student grades using natural-language grading formulas. The system fetches grade data from Google Sheets, processes it using OpenAI GPT-4.1, and writes the final results back to an output Google Sheet automatically.

---

## ✨ Features

- Paste Google Sheet links directly — no file upload needed
- Natural-language grading formula input
- AI-powered prompt analysis and structured review before processing
- Automatic column classification using GPT-4.1
- Student data validation and issue detection
- Results written directly to the output Google Sheet
- Real-time job status polling every 5 seconds
- Multi-layer error handling

---

## 🛠️ Tech Stack

| Layer        | Technology                  |
|--------------|-----------------------------|
| Backend      | Laravel 12 (PHP 8.2)        |
| Automation   | n8n                         |
| AI           | OpenAI GPT-4.1              |
| Deployment   | Railway                     |
| Sheets       | Google Sheets API           |
| Queue        | Laravel Queues              |
| Container    | Docker                      |

---

## ⚙️ Prerequisites

Before running the project, make sure you have the following:

- PHP 8.2+
- Composer
- Node.js & npm
- Docker (for running n8n)
- OpenAI API Key
- Google Sheets OAuth2 Credentials (for n8n)
- Railway account (for deployment)

---

## 📋 Usage

1. Open the Grade+ web interface
2. Paste the **input Google Sheet URL** — must be set to *Anyone with the link can view*
3. Paste the **output Google Sheet URL** — must be set to *Anyone with the link can edit*
4. Enter your grading formula in natural language, for example:
   > "Quizzes 15% (top 3 out of 5), Midterm 30%, Final Exam 40%, Assignment 15%"
5. Click **Analyze Prompt** to review the AI interpretation of your formula
6. Click **Re-analyze Prompt** if you want to revise the instructions
7. Click **Calculate Grades** to start processing
8. Wait for the results — the page polls every 5 seconds automatically
9. Click **Open Result Sheet** to view the full calculated grades

---

## 🏗️ Architecture

```
┌─────────────┐     REST API      ┌──────────────┐
│   Frontend  │ ◄───────────────► │    Laravel   │
└─────────────┘                   └──────┬───────┘
                                         │
                          ┌──────────────┼──────────────┐
                          │              │              │
                    ┌─────▼────┐  ┌─────▼────┐  ┌─────▼────┐
                    │  OpenAI  │  │   n8n    │  │ Database │
                    │ GPT-4.1  │  │Workflow  │  │          │
                    └──────────┘  └─────┬────┘  └──────────┘
                                        │
                                 ┌──────▼───────┐
                                 │Google Sheets │
                                 └──────────────┘
```

### How it works

1. The instructor fills in the form and clicks **Analyze Prompt**
2. Laravel sends the prompt and sheet headers to OpenAI GPT-4.1 for analysis
3. The instructor reviews the structured analysis and clicks **Calculate Grades**
4. Laravel dispatches a `ProcessGradingJob` to the queue
5. The job sends a webhook request to n8n with the sheet links and analysis
6. n8n fetches the CSV, classifies columns, validates data, generates grading code via GPT-4.1, calculates grades, and writes results to the output sheet
7. n8n returns a summary to Laravel
8. The frontend detects the completed job and displays the results

---

## ⚠️ Important Notes

- The input Google Sheet must have a **header row as the first row**
- The output Google Sheet must allow **editing** for anyone with the link

---

## 📄 License

This project was developed as part of an academic requirement at the Islamic University of Gaza.
