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

## 🛠️ Setup Instructions (Step-by-Step)

### 🔹 Step 1: Clone the Repository
```bash
git clone https://github.com/Bisu7/challenge_1b
cd adobe_docker
```

### 🔹 Step 2: (Optional) Create and Activate a Virtual Environment
#### On Windows:
```bash
python -m venv .venv
.venv\Scripts\activate
```
#### On macOS/Linux:
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 🔹 Step 3: Install Python Dependencies
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

✅ If `sentence-transformers` fails to install or causes compatibility issues, use these versions:
- `sentence-transformers==2.2.2`
- `huggingface-hub==0.10.1`
- `scikit-learn==1.3.0`
- `PyMuPDF==1.22.5`
- `numpy==1.24.4`

---

## 📥 Input Format

Place your input files inside the `input/` folder.

### Sample `persona.json`
```json
{
  "persona": "A legal associate",
  "task": "Extract all sections discussing compliance and regulatory risks"
}
```

### Sample `document.pdf`
Add any document to be semantically parsed alongside the persona.

---

## ▶️ Step-by-Step: Run the Project

### 🔹 Step 1: Ensure `input/` contains:
- `persona.json`
- At least one `.pdf` document

### 🔹 Step 2: Run the main script
```bash
python main.py
```

This will:
- Load the persona and task
- Extract paragraphs from all PDF(s) in `input/`
- Rank each paragraph by semantic relevance
- Save the result to `output/challenge1b_output.json`

---

## 📦 Output Format

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

## 🐳 Step-by-Step: Docker Support

### 🔹 Step 1: Build the Docker Image
```bash
docker build --network host -t pdf-ranker-app .  
```

### 🔹 Step 2: Run the Docker Container

#### On Windows (PowerShell)
```bash
docker run --rm -v "${PWD}\input:/app/input" -v "${PWD}\output:/app/output" pdf-ranker-app
```

#### On Linux/macOS (or Git Bash)
```bash
docker run --rm -v "$(pwd)/input:/app/input" -v "$(pwd)/output:/app/output" pdf-ranker-app

```

✅ Tip: Temporarily remove `--network none` to allow HuggingFace model downloads when first running the container.

---

## 📌 Notes

- The `input/` folder must contain both `persona.json` and at least one `.pdf` file.
- For offline Docker usage, ensure the model is downloaded in advance.
- Output is generated in the `output/` folder as `challenge1b_output.json`.
