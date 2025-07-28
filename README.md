# Persona-Driven Document Intelligence  
**Adobe India Hackathon 2025 – Round 1B Submission**

This project implements a semantic document intelligence system that extracts, ranks, and returns the most relevant content from one or more PDF documents based on a given **persona** and **task** ("job-to-be-done").

---

## 🧠 Problem Statement (Round 1B Summary)

Build a core system that powers intelligent document understanding and semantic linking. The system should:
- Understand a user’s persona and task.
- Read and extract paragraphs from one or more documents.
- Rank and return the **top-K** most relevant sections.
- Output the results in a structured JSON format.

---

## 📁 Project Structure

```
.
├── .venv/                       # (optional) Python virtual environment
├── input/                      # Input folder (PDFs + persona config)
│   ├── persona.json            # Persona and task definition
│   └── document.pdf            # Example document
├── output/                     # Output JSON is generated here
│   └── challenge1b_output.json
├── utils/                      # Helper modules
│   ├── config_loader.py        # Loads persona/task config
│   ├── pdf_reader.py           # Parses and extracts PDF text
│   ├── relevance_ranker.py     # Ranks sections using semantic similarity
│   └── __pycache__/            # Python bytecode cache
├── main.py                     # Entry point: runs the entire pipeline
├── requirements.txt            # Python dependencies
├── Dockerfile                  # Docker container setup
└── README.md                   # This file
```

---

## 🛠️ Setup Instructions

### 1. Clone the Repo & Navigate
```bash
git clone https://github.com/Bisu7/challenge_1b
cd adobe_docker  # or your folder name
```

### 2. (Optional) Create a Virtual Environment
```bash
python -m venv .venv
# Activate it:
# On Windows:
.venv\Scripts\activate
# On macOS/Linux:
source .venv/bin/activate
```

### 3. Install Python Dependencies
```bash
pip install -r requirements.txt
```

If `sentence-transformers` fails with newer versions, stick with:
- `sentence-transformers==2.2.2`
- `huggingface-hub==0.10.1`
- `scikit-learn==1.3.0`
- `PyMuPDF==1.22.5`
- `numpy==1.24.4`

---

## 📥 Input Format

Place your input files in the `input/` folder.

### Sample `persona.json`
```json
{
  "persona": "A legal associate",
  "task": "Extract all sections discussing compliance and regulatory risks"
}
```

### Sample `document.pdf`
Add any PDF document to be semantically parsed based on the persona and task.

---

## ▶️ Run the Project

```bash
python main.py
```

This will:
- Load the persona and task
- Extract paragraphs from PDF(s)
- Rank each paragraph’s relevance to the persona + task
- Save output to: `output/challenge1b_output.json`

---

## 📦 Output Format

The final JSON will look like this:

```json
{
  "persona": "A legal associate",
  "job_to_be_done": "Extract all sections discussing compliance and regulatory risks",
  "document": "document.pdf",
  "top_sections": [
    {
      "page": 3,
      "text": "This document ensures compliance with legal obligations under clause 19...",
      "score": 0.872
    }
  ]
}
```

---

## 🐳 Docker Support

### 1. Build the Docker Image

```bash
docker build --platform linux/amd64 -t persona-insight:round1b .
```

### 2. Run the Docker Container

```bash
docker run --rm ^
  -v ${PWD}\input:/app/input ^
  -v ${PWD}\output:/app/output ^
  --network none ^
  persona-insight:round1b
```

> On **PowerShell**, use `${PWD}`  
> On **Git Bash/macOS/Linux**, use `$(pwd)` instead.

---

## 📌 Notes

- Make sure `input/` contains both `persona.json` and at least one `.pdf`.
- You can use multiple PDFs in the `input/` folder for batch processing.
- If running in a no-network Docker environment, you should **pre-download the transformer model locally** or temporarily allow the container to access the internet.
