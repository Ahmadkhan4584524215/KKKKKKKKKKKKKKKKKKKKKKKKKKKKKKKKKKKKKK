# Resume ATS Analyzer

A simple Streamlit app that lets a user upload a resume and uses Gemini Flash to provide an **estimated ATS-readiness score**, strengths, missing/weak keywords, ATS warnings, and practical resume improvements.

> **Important:** The score is an AI estimate, not a score from a real employer's ATS. ATS systems differ between companies and job platforms.

## Features

- Upload **PDF, DOCX, or TXT** resumes.
- Optional **job description** for a more targeted keyword-match score.
- Estimated ATS score from 0–100.
- Score breakdown for keywords, experience, skills, clarity, and ATS format readiness.
- Strengths and improvement suggestions.
- Missing/weak keyword suggestions.
- ATS warnings.
- Suggested rewrites for existing resume bullets without inventing facts.
- Download the analysis as JSON.
- Uses Gemini through Google's current `google-genai` Python SDK.

## Project files

```text
resume-ats-analyzer/
├── app.py
├── requirements.txt
└── README.md
```

## 1. Get a Gemini API key

Create a Gemini API key in Google AI Studio, then keep it private. Do **not** paste the key into `app.py` or commit it to GitHub.

The app reads the key from either:

- Streamlit secrets: `GEMINI_API_KEY`
- Environment variable: `GEMINI_API_KEY`

The default model is `gemini-3.8-flash`. You can override it with the `GEMINI_MODEL` environment variable if your API project uses another available Flash model.

## 2. Run locally

Use Python 3.11 or 3.12 for a straightforward local setup.

Create a virtual environment:

### Windows

```powershell
python -m venv .venv
.venv\Scripts\activate
```

Install dependencies:

```powershell
pip install -r requirements.txt
```

Create this file:

```text
.streamlit/secrets.toml
```

Put your key in it:

```toml
GEMINI_API_KEY = "YOUR_GEMINI_API_KEY"
```

Then start the app:

```powershell
streamlit run app.py
```

Open the local URL shown by Streamlit, normally `http://localhost:8501`.

## 3. Push the project to GitHub

From the project folder:

```powershell
git init
git add app.py requirements.txt README.md
git commit -m "Initial resume ATS analyzer"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/resume-ats-analyzer.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username and create the GitHub repository first.

### Important security step

Do **not** commit `.streamlit/secrets.toml`. Add it to `.gitignore` before using `git add .` in a normal workflow:

```text
.streamlit/secrets.toml
.venv/
__pycache__/
```

If you accidentally publish an API key, revoke/rotate that key immediately.

## 4. Deploy on Streamlit Community Cloud

1. Push `app.py`, `requirements.txt`, and `README.md` to GitHub.
2. Sign in to Streamlit Community Cloud.
3. Connect your GitHub account.
4. Choose **Deploy an app**.
5. Select your repository and the `main` branch.
6. Set the main file to `app.py`.
7. Deploy.
8. Open the deployed app's **Settings / Secrets** area.
9. Add:

```toml
GEMINI_API_KEY = "YOUR_GEMINI_API_KEY"
```

10. Save the secret and restart/redeploy if requested.

Do not put the API key in GitHub or inside `app.py`.

## 5. Test the app

Test these cases after deployment:

- PDF resume + job description
- DOCX resume + job description
- TXT resume without a job description
- No uploaded file
- Missing API key
- Empty/scanned PDF with no extractable text
- A resume with no matching keywords

## How the score works

The app asks Gemini to produce an estimated 0–100 score based on:

- Keyword alignment
- Experience relevance
- Skills relevance
- Clarity
- ATS format readiness

When a job description is supplied, keyword matching is evaluated against that job description. Without one, the app gives a general ATS-readiness assessment.

## Limitations

- A scanned/image-only PDF may not contain extractable text. Use a text-based PDF or DOCX/TXT for best results.
- Visual formatting is only partially assessable because the app analyzes extracted text.
- The score is not an official ATS score.
- AI suggestions should be reviewed by the user before adding them to a resume.
- The app does not store resumes in a database.

## Suggested next features

After the basic version works, you can add:

- Resume vs. job-description keyword heatmap
- Downloadable improved resume
- Multiple resume versions
- Login/user accounts
- History dashboard
- More detailed section-by-section checks
- OCR for scanned resumes
