# 🧠 NeuroRead: AI Adaptive Reading & Accessibility

**NeuroRead** is a production-grade AI accessibility platform designed to empower readers with dyslexia and ADHD. It combines state-of-the-art LLM simplification with an adaptive learning engine to transform cognitively demanding content into accessible, natural, and meaningful language.

---

## 🚀 Key Impressive Features

### 1. **SSV2: Smart Simplifier V2 (LLM-First Architecture)**
Unlike traditional word-swapping tools, **SSV2** uses a multi-layered LLM pipeline (Groq + Llama-3.3-70B) to rewrite complex text. It prioritizes semantic integrity while aggressively reducing cognitive load.
*   **ACM (Adaptive Cognitive Modeling)**: Real-time analysis of "Cognitive Intensity" using readability indices and sentence-transformer embeddings.
*   **Meaning-First Validation**: A 0.90 semantic similarity threshold ensures that simplification never leads to information loss.

### 2. **VFC: Visual Flow Control**
Designed specifically for the dyslexic eye, **VFC** implements:
*   **OpenDyslexic Integration**: Specialized high-contrast typography.
*   **Dynamic Spacing**: Automated paragraph chunking (`\n\n` formatting) to prevent visual crowding and "text-swimming."

### 3. **EPD: Error Pattern Detection (Learning Mode)**
For younger learners, the system tracks specific phonological bottlenecks (e.g., *b/d* or *p/q* confusion). 
*   **Adaptive Content Engine**: Dynamically scales exercise difficulty and content focus based on real-time error rates.

### 4. **Multimodal Support**
*   **Reading Voice (TTS)**: High-quality text-to-speech for auditory reinforcement.
*   **Accessibility Dashboard**: Visualizes long-term progress in reading speed, comprehension, and XP-based milestones.

---

## 🏗️ System Architecture & Flow

### **AI Simplification Pipeline (V2)**
```mermaid
graph TD
    Input[User Text] --> L0[Layer 0: Preprocessing & Normalization]
    L0 --> L1[Layer 1: Groq LLM Primary Simplification]
    L1 --> L2[Layer 2: Post-Processing & Paragraph Chunking]
    L2 --> L3[Layer 3: 3-Tier Validation\nSimilarity + length + Cognitive Load]
    L3 -- Fail --> L4[Layer 4: Qwen/HF Fallback Engine]
    L3 -- Pass --> Cache[Thread-Safe TTL Cache]
    L4 --> Cache
    Cache --> Output[Final Simplified Text]
```

---

## 📂 Project Structure

### **Backend (FastAPI)**
The engine room, built for high-concurrency and low-latency ML inference.
*   `app/services/simplification_engine.py`: The core V2 pipeline logic.
*   `app/services/learning/`: Adaptive logic for error patterns and content scaling.
*   `app/routes/`: RESTful endpoints for Assistive and Learning modes.
*   `app/models/`: Database schemas for user profiles and progress tracking.

### **Frontend (React + Vite)**
A premium, "Moss & Clay" themed interface optimized for focus and accessibility.
*   `src/components/SimplifierModal.jsx`: The main interface for SSV2.
*   `src/components/learning/`: Interactive modules for Phonics and Spelling.
*   `src/components/ProgressDashboard.tsx`: Data visualization for cognitive metrics.
*   `src/components/AssistiveMode.jsx`: Unified accessibility toggle system.

---

## 🛠️ Technology Stack

| Layer | Technology |
| :--- | :--- |
| **Inference** | Groq (Llama-3.3-70B-Versatile), HuggingFace (Qwen-72B) |
| **Semantic AI** | `sentence-transformers` (all-MiniLM-L6-v2) |
| **Backend** | Python 3.10+, FastAPI, SQLAlchemy |
| **Database** | PostgreSQL |
| **Frontend** | React 19, Vite, TypeScript, Tailwind CSS |
| **Animations** | Framer Motion |

---

## 📦 Quick Start

### **1. Backend Setup**
```bash
cd backend
pip install -r requirements.txt
# Set your GROQ_API_KEY in .env
python -m uvicorn app.main:app --reload
```

### **2. Frontend Setup**
```bash
cd frontend
npm install
npm run dev
```

---

## 🧠 Final Objective
NeuroRead targets the **linguistic quality gap**. By moving away from "robotic" word-swapping and toward **natural human tone**, we provide a reading experience that is not just easier, but more enjoyable for neurodivergent minds.

**"Reading, Reimagined."**



# 🛠️ Developer Setup & Initialization

To get NeuroRead running locally, follow these steps:

## 1. Prerequisites
Ensure you have the following installed:

- Git  
- Python 3.10+  
- Node.js (LTS)  

## 2. Clone the Repository

```bash
git clone https://github.com/tannnmayy/NeuroRead.Mainsite.git
cd NeuroRead.Mainsite
```

## 3. Backend Setup (FastAPI)

Navigate to the backend folder:

```bash
cd backend
```

Create and activate a virtual environment:

```bash
# Windows
python -m venv venv
.\venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Configure Environment Variables:  
Create a `.env` file in the `/backend` directory:

```env
GROQ_API_KEY=your_api_key_here
```

Run the Server:

```bash
python -m uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

## 4. Frontend Setup (React + Vite)

Navigate to the frontend folder:

```bash
cd ../ai-accessibility-assistant-frontend-main
```

Install packages:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

## 5. Access the Platform

- Frontend: http://localhost:5173  
- Interactive API Docs (Swagger): http://127.0.0.1:8000/docs  

