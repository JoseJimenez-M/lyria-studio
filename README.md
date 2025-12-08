\# Lyria Studio V2: Hybrid Audio Architecture



\## Overview

This repository hosts the V2 architecture for the Athena Audio Engine. It transitions from a monolithic script to a Client-Server architecture, decoupling the generation logic (Python/FastAPI) from the user experience (React/Next.js).



This hybrid approach allows for a "Consumer-Grade" chat interface for rapid ideation, while retaining a "Pro-Grade" studio environment for granular DSP editing.



---



\## Quick Start (Execution Guide)



To run the full suite, you need 3 separate terminals running concurrently.



\### Terminal 1: The Backend Core (API)

Handles the connection to Vertex AI and exposes the generation endpoints.



cd backend

\# Ensure your environment is active (e.g., conda activate stitch\_test)

uvicorn api:app --reload --port 8000



\### Terminal 2: The Frontend Client (UI)

Launches the Next.js Chat Interface (Consumer View).



cd frontend

npm run dev

\# Running on http://localhost:3000



\### Terminal 3: The Advanced Studio (DSP Engine)

Launches the Deep Editor for segmentation and stitching.



cd backend

streamlit run studio.py --server.port 8501



\*\*Usage Flow:\*\*

1\. Open http://localhost:3000

2\. Generate a track using the Chat UI.

3\. Click "Open in Studio" to transfer the asset to the advanced editor.



---



\## Installation \& Setup



\### Prerequisites

\* Python 3.10+ (Recommended: Anaconda/Miniconda)

\* Node.js 18+ (LTS Version)

\* FFmpeg installed and added to system PATH.



\### 1. Backend Setup

Initialize the Python environment for signal processing and API management.



cd backend

pip install fastapi uvicorn python-multipart streamlit pydub websockets



Note: Ensure GOOGLE\_API\_KEY is set in your environment variables.



\### 2. Frontend Setup

Install the React dependencies and UI component libraries.



cd frontend

npm install



---



\## System Architecture \& File Roles



\### Backend Layer (/backend)

Contains the proprietary logic for audio manipulation and external model orchestration.



\* api.py

&nbsp;   \* Role: REST Interface \& Entry Point.

&nbsp;   \* Function: Handles CORS policies and routes JSON requests from the Frontend to the generation engine. Exposes specific headers for cross-origin file handling.

\* studio.py

&nbsp;   \* Role: Advanced DSP Visualizer.

&nbsp;   \* Function: A specialized environment for non-linear editing. Handles the "Session State" when importing assets from the chat and executes the final rendering pipeline.

\* audio\_utils.py

&nbsp;   \* Role: Signal Processing Middleware.

&nbsp;   \* Function: Contains the core algorithms for "Smart Stitching," crossfade calculation, and temporary file management. This is the DSP engine of the project.

\* lyria\_generator.py

&nbsp;   \* Role: Model Gateway.

&nbsp;   \* Function: Manages the asynchronous WebSocket connection to the Google Lyria model, handling stream buffering and binary decoding.



\### Frontend Layer (/frontend)

A modern, responsive client built with Next.js 14 (App Router).



\* src/app/page.tsx

&nbsp;   \* Role: Client State Manager.

&nbsp;   \* Function: Manages the chat history, audio playback states, and the logic that constructs the deep links to open the Studio with the correct context.

\* src/app/layout.tsx

&nbsp;   \* Role: Global Context.

&nbsp;   \* Function: Handles font optimization and suppresses hydration warnings for browser extensions compatibility.



---



\## Notes for Deployment

\* Storage: Currently uses local ephemeral storage for low-latency processing during the MVP phase. Production migration would require an S3/GCS bucket implementation.

\* Security: The API currently allows all origins for development ease. This must be restricted to the specific frontend domain in production.

EOF

