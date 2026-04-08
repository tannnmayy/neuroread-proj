# 📖 NeuroRead | Reading, Reimagined

NeuroRead is an accessibility-first, AI-powered reading platform designed to reduce cognitive load and provide a highly personalized reading experience. It features tools for individuals with dyslexia, ADHD, and anyone who benefits from clearer, more structured reading materials.

## 🚀 Live Deployments

- **Frontend Application (Vercel):** [https://nr-three-puce.vercel.app](https://nr-three-puce.vercel.app)
- **Backend API (Render):** [https://neuroread-5icn.onrender.com](https://neuroread-5icn.onrender.com)

---

## ✨ Core Features

- **🧠 Smart Simplifier (Focus Mode):** Uses AI (Groq + LLaMA) to rewrite dense text into easily digestible content, tracking cognitive load reduction.
- **🗣️ Text-to-Speech (TTS):** Integrated audio playback for simplified text.
- **👁️ Dyslexia Support:** Includes the OpenDyslexic font, line highlighting, reading rulers, and color overlays (B/D/P/Q distinction).
- **📊 Session Analytics:** Tracks your "Cognitive Score" and reading history to visualize improvement over time.
- **🎨 Beautiful & Accessible UI:** A warm, calm, constraint-based design (defaulting to Warm Beige) that avoids harsh contrasts and is fully customizable.

---

## 📂 Folder Structure

The repository is structured as a monorepo containing both the FastAPI backend and the Vite/React frontend.

```text
neuroread/
├── backend/                 # Python FastAPI Backend
│   ├── app/                 # Core application code
│   │   ├── main.py          # Application entry point
│   │   ├── routes/          # API endpoints (e.g., assistive/simplify)
│   │   └── services/        # Business logic & AI integration (Groq client)
│   └── requirements.txt     # Python dependencies
│
├── frontend/                
│   └── frontend/            # React + Vite Frontend
│       ├── public/          # Static assets (fonts, logo)
│       ├── src/             
│       │   ├── components/  # React UI components (Navbar, Modal, etc.)
│       │   ├── pages/       # High-level route views (Dashboard, Simplifier)
│       │   ├── services/    # API calls (api.js connecting to backend)
│       │   ├── stores/      # Zustand state management
│       │   ├── index.css    # Tailwind CSS & custom design tokens
│       │   └── main.tsx     # React DOM entry
│       ├── package.json     # Node dependencies and scripts
│       ├── tailwind.config.js
│       └── vite.config.ts
│
└── README.md                # You are here
```

---

## 🛠️ Technology Stack

### Frontend
- **Framework:** React / Vite (TypeScript/JavaScript)
- **Styling:** Tailwind CSS & Custom CSS Variables (for real-time accessibility theming)
- **Animations:** Framer Motion & GSAP
- **State Management:** Zustand
- **Deployment:** Vercel

### Backend
- **Framework:** FastAPI (Python)
- **AI Integration:** Groq API (LLaMA models)
- **Machine Learning / NLP:** spaCy, textstat, NLTK
- **Deployment:** Render

---

## 💻 Local Machine Setup & Usage

To run the full application on your own machine, you need to run both the Backend and Frontend servers simultaneously in separate terminal windows.

### 1. Start the Backend

1. Open a terminal and navigate to the `backend` folder:
   ```bash
   cd backend
   ```
2. Create and activate a Python virtual environment:
   ```bash
   # On macOS/Linux:
   python3 -m venv venv
   source venv/bin/activate
   
   # On Windows:
   python -m venv venv
   venv\Scripts\activate
   ```
3. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Create a `.env` file in the `backend` folder and add your Groq API Key:
   ```env
   GROQ_API_KEY=gsk_your_groq_api_key_here
   ```
5. Start the backend server:
   ```bash
   python -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
   ```

### 2. Start the Frontend

1. Open a **new** terminal window and navigate to the `frontend/frontend` folder:
   ```bash
   cd frontend/frontend
   ```
2. Install the necessary Node packages:
   ```bash
   npm install
   ```
3. Create a `.env` file in the `frontend/frontend` folder and point it to your local backend:
   ```env
   VITE_API_URL=http://localhost:8000
   ```
4. Start the frontend development server:
   ```bash
   npm run dev
   ```
5. Open your browser and go to the link shown in the terminal (usually `http://localhost:5173`).

---

## 🌍 How to Deploy

If you want to host NeuroRead live on the internet, follow this pipeline:

### 1. Deploy the Backend to Render
1. Create a free account on [Render.com](https://render.com).
2. Connect your GitHub account and create a new **Web Service**.
3. Select your NeuroRead repository.
4. **Configuration Details:**
   - **Root Directory:** `backend`
   - **Environment:** `Python 3`
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
5. Click **Advanced** (or Environment Variables) and confidently add:
   - `GROQ_API_KEY` : `Your Groq key`
6. Click **Create Web Service**. Once it is Live, copy the generated URL (e.g., `https://neuroread-xyz.onrender.com`).

### 2. Deploy the Frontend to Vercel
1. Create an account on [Vercel.com](https://vercel.com) and import your GitHub repository.
2. In the project settings configuration:
   - **Root Directory:** `frontend/frontend`
   - **Framework Preset:** `Vite`
   - **Build Command:** `npm run build`
   - **Output Directory:** `dist`
3. Expand **Environment Variables** and add:
   - **Name:** `VITE_API_URL`
   - **Value:** `https://neuroread-xyz.onrender.com` *(The URL you copied from Render)*
4. Click **Deploy**. Vercel will build and host your frontend globally.

---

## 🧠 AI Text Simplification & Cognitive Load Scoring

### Overview
This system improves text accessibility by:
- Simplifying text using an LLM
- Measuring difficulty using a cognitive load score
- Returning structured, UI-friendly output

**Pipeline:**
Input → Analyze → Select Level → Simplify → Re-analyze → Return

---

### 1. Cognitive Load Scoring

**Score Range:** 0 (easy) → 100 (hard)

**Metrics (Weighted):**

1. **Readability (40%)**
   - Flesch Reading Ease (0–100)
   - *Formula:* `readability_load = 1 - (FRE / 100)`
   - Lower FRE → higher load

2. **Sentence Length (30%)**
   - Avg words per sentence (spaCy / regex)
   - *Formula:* `sentence_load = (avg_words - 5) / 30`
   - Clamped 0–1

3. **Complex Vocabulary (30%)**
   - difficult_words / total_words
   - *Formula:* `complex_word_load = ratio / 0.30`
   - Clamped 0–1

**Final Score:**
`cognitive_load = 100 × (0.4 × readability_load + 0.3 × sentence_load + 0.3 × complex_word_load)`

**Labels:**
- `<30` → Low
- `30–59` → Moderate
- `≥60` → High

**API Outputs:**
- `cognitive_load_score`
- `readability_score`
- `avg_sentence_length`
- `complex_word_ratio`
- `difficulty_label`

---

### 2. AI Text Simplification

**Function:** `simplify_text(text, level)`

**LLM Setup:**
- **Model:** `llama-3.3-70b-versatile` (Groq)
- **Temperature:** 0.3

**Levels:**
- **1** → Very simple
- **2** → Moderate
- **3** → Light

**Output (Strict JSON):**
```json
{
  "simplified_text": "...",
  "bullet_points": ["..."],
  "definitions": {"...": "..."},
  "step_by_step_explanation": ["..."]
}
```

---

### 3. Level Selection

If not explicitly provided:
- **High load** → Level 1
- **Moderate** → Level 2
- **Low** → Level 3

**Overrides:**
- `easy_read` / `focus` → Level 1
- `academic` → Level 3
- `beginner` → Level 1

---

### 4. Fallbacks

**API Failure:**
- Split sentences
- Keep first N
- Truncate length (more aggressive at level 1)

**Invalid JSON:**
- Clean response
- Retry parse
- Else: Use raw text with empty structured fields

---

### 5. Execution Flow

1. Analyze original text
2. Select level
3. Simplify (LLM / fallback)
4. Analyze simplified text
5. Return both scores + output

---

### 6. Important Note

- Only **one simplification pass**
- No retry loop or threshold check

**Flow:** Analyze → Simplify → Analyze → Return

---

### 7. Example Response

```json
{
  "simplified_text": "...",
  "bullet_points": ["..."],
  "definitions": {"...": "..."},
  "step_by_step_explanation": ["..."],
  "original_score": 72.5,
  "simplified_score": 38.2,
  "difficulty_reduction": 34.3
}
```

---

### TL;DR
- **Cognitive load** = readability + sentence length + vocab complexity
- **Simplification** = single LLM call based on level
- **Metrics** guide before & after, not retries

---

## 🤝 Contributing

Contributions to improve accessibility or AI features are always welcome! Please create an issue or pull request.

#   n e u r o r e a d - p r o j  
 