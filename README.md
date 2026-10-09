# Grace: Local Holographic AI Companion

Grace is a desktop AI companion that runs primarily on your local machine. She listens to spoken conversation, replies with a natural voice, displays a cyan-blue holographic female face, expresses conversational emotions, and remembers useful information across sessions. In addition, Grace features optional vision capabilities to analyze webcam frames, understand screenshots, and read authorized project files to provide contextual assistance.

## ✨ Features

* **Voice Conversation:** Speak to Grace using your microphone, and she will transcribe your speech and respond with a natural, synthesized voice.
* **Holographic Face:** A real-time 3D holographic digital face with a deep black background, cyan particles, glowing nodes, and wireframe lines. The avatar reacts visually to listening, thinking, speaking, and idling.
* **Emotion & Personality:** Grace expresses a consistent set of emotional states that sync with her voice and conversational tone. 
* **Persistent Memory:** Stores conversation history, summaries, and user-approved long-term memories locally.
* **Local Vision (Optional):**
  * **Webcam Understanding:** Opt-in analysis of webcam frames to answer questions about your visible surroundings.
  * **Screen Understanding:** Capture and analyze user-triggered screenshots for help with UI, error messages, or charts.
  * **Work Context:** Read and analyze authorized local project files and source code for accurate debugging and assistance.
* **Privacy-First:** Processes data locally. Explicit, separate permissions for microphone, camera, and screen capture. Memory is fully controllable by the user.

## 🛠️ Technology Stack

Grace is built with a mix of web technologies, 3D rendering, and optimized local AI models.

### Application & UI
* **Desktop Shell:** Tauri v2 (or Electron)
* **Frontend:** React + TypeScript + Vite
* **3D Renderer:** Three.js / React Three Fiber (Custom shaders for holographic effects)
* **Backend:** Python + FastAPI
* **Inter-process Events:** WebSockets
* **Database (Memory):** SQLite

### Local AI Models
* **LLM (Conversation & Reasoning):** [Ollama](https://ollama.com/) running **Llama 3 (8B Instruct)** (quantized) or lightweight alternatives like Phi-3 Mini/Qwen 1.5.
* **ASR (Speech Recognition):** [whisper.cpp](https://github.com/ggerganov/whisper.cpp) using the **Whisper** Base or Small model.
* **TTS (Text-to-Speech):** [Piper TTS](https://github.com/rhasspy/piper) for fast streaming, or XTTSv2 for highly expressive voices.
* **VLM (Vision):** **LLaVA 1.5** (7B/13B) for detailed analysis, or **Moondream 2** for a blazingly fast alternative (via Ollama).
* **VAD (Voice Activity Detection):** [Silero VAD](https://github.com/snakers4/silero-vad) for rapid, accurate speech detection on the CPU.
* **Embeddings (Semantic Search):** `all-MiniLM-L6-v2` via HuggingFace sentence-transformers.

## 🚀 Architecture Overview

Grace handles inputs in a privacy-conscious, local environment. Audio is transcribed via Whisper and sent to the FastAPI orchestrator. If authorized, screenshots, webcam frames, or local files are evaluated by a Vision-Language Model.

The orchestrator pulls relevant context from SQLite memory and passes everything to the local LLM (Llama 3). The LLM generates text and emotion metadata. Text is spoken aloud via Piper TTS, and the emotion metadata alongside audio timing controls the Three.js holographic avatar's expressions and lip sync.

## 🔒 Privacy & Security

Grace is designed to keep your data under your control:
* **Local Processing:** Core features function completely offline, keeping your data on your device.
* **Granular Permissions:** Independent controls for microphone, camera, and screen access.
* **No Covert Capturing:** No continuous, hidden audio recording or video processing. Push-to-talk or visible wake-word activation only.
* **Memory Management:** You can easily inspect, correct, export, or delete any long-term memories or conversation histories.

## 💻 Hardware & Performance

Running all models simultaneously requires decent hardware. The biggest bottleneck is VRAM.
* **Recommended Setup:** Keep VAD, Embeddings, and Piper TTS on the CPU. Load the LLM into the GPU. Load the VLM into the GPU only on demand when a screenshot or camera frame needs processing.
* **Target Hardware:** Tested on an RTX 2060 (6GB VRAM) and M1 Macs. Quantized 4-bit LLM models are recommended.
