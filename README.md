# BugLie Premium AI Assistant

BugLie is an offline-first, highly interactive, intelligent desktop productivity companion built on Electron. Powered by a local LLM (`phi3:mini` via Ollama), BugLie goes beyond standard chat to act as a deep system integrator—capable of managing files, launching applications, sending WhatsApp messages, calculating prayer times, and generating interactive educational quizzes.

## 🌟 Key Features

### 1. Local AI Intelligence
- **100% Offline Processing:** Powered by `phi3:mini` running via local Ollama, ensuring total privacy and zero API costs.
- **Intent Routing:** A smart prompt router determines whether your command requires a conversation, a system action, or an app launch, and handles it accordingly in less than 500ms.
- **Contextual Memory:** Maintains a rolling conversation history so you can use pronouns (like "it", "him", "her") and BugLie understands the context.

### 2. Deep File System Search
- **Recursive Drive Indexing:** Automatically scans and indexes all connected drives (e.g., C:, D:) in seconds.
- **Smart Exclusions:** Ignores system junk, `node_modules`, and hidden folders to maintain performance.
- **Disambiguation UI:** If multiple files share the same name (e.g., `main.py` in different projects), BugLie presents an interactive UI overlay allowing you to pick exactly which file to open.

### 3. Interactive Quiz Engine
- **Dynamic Multi-Question Quizzes:** Ask BugLie to "Make a 5 question quiz about Python" and it will generate unique Multiple Choice Questions dynamically.
- **Gamified UI:** Clickable A/B/C/D buttons right in the chat interface. Includes screen-shake animations for wrong answers and randomized celebrations for correct ones.
- **Score Tracking:** Automatically grades your performance and provides a final score summary.

### 4. Communication Integrations
- **WhatsApp Automation:** Logs securely into your WhatsApp account using `whatsapp-web.js` (Puppeteer).
  - Can send quick messages to contacts (e.g., `whatsapp John: Hello!`).
  - Sends automatic system notifications (like prayer reminders) to a dedicated "BugLie" WhatsApp group.
  - Features an auto-reply mechanism to gracefully handle incoming messages when you're away.
- **Email Delivery:** Integrated with `nodemailer` to dispatch alerts directly to your inbox.

### 5. Automated Prayer Reminders
- **Adhan Integration:** Uses coordinates to calculate highly accurate daily prayer times.
- **Multi-Channel Alerts:** When it's time to pray, BugLie notifies you simultaneously via:
  - Windows System Notification
  - Desktop UI visual updates
  - Automated WhatsApp message
  - Direct Email to your inbox

### 6. Advanced Voice Command Pipeline (Speech-To-Text)
BugLie doesn't just read text—it listens. The integrated voice command system allows you to dictate complex instructions, ask questions, or launch system applications completely hands-free.

**How the STT (Speech-To-Text) Architecture Works:**
1. **Frontend Audio Capture:** When you press and hold the microphone icon (🎙️), the Electron frontend utilizes the native WebRTC `MediaRecorder` API to capture your audio stream via your default microphone.
2. **Buffer Processing:** As soon as you release the button, the raw audio chunks are packaged into an array buffer and bridged via secure IPC handlers (`preload.js` -> `main.js`).
3. **Python STT Engine Engine:** `main.js` saves a temporary `temp_audio.wav` file and spawns a background Python process (`stt.py`). 
4. **Google Speech Recognition:** The Python script uses the `SpeechRecognition` library (`sr.recognize_google`) to transcribe the audio file locally. It is completely resilient, handling unknown values and network errors gracefully.
5. **Hardware Constraints & Cloud Fallback:** Originally intended to use local OpenAI Whisper for complete offline dictation, the system detects hardware/VRAM limitations. To maintain fast performance without exhausting local memory, BugLie falls back to the lightweight Google Speech Recognition API (`recognize_google`) to transcribe the audio.
6. **Execution Pipeline:** The resulting transcript is fed directly into BugLie's AI Intent Router exactly as if you typed it.

**Examples of Voice Actions:**
- **App Launcher:** Hold the mic and say *"Open notepad"* or *"Launch chrome"*. The voice input is parsed, bypassing the AI, and instantly executes via `child_process.exec`.
- **Hands-Free Quizzing:** Say *"Quiz me about machine learning"*.
- **Dictation Chat:** Speak long paragraphs or complex coding questions instead of typing them out.

---

## 🛠️ Technology Stack

- **Frontend:** HTML, Vanilla CSS, JavaScript (Custom glassmorphism UI with micro-animations)
- **Backend:** Node.js, Electron (Main/Preload/Renderer Architecture)
- **AI/LLM:** Ollama, `node-fetch`, Prompt Engineering
- **File System:** PowerShell integration, `glob`, `fs`
- **Integrations:** `whatsapp-web.js`, `nodemailer`, `adhan`
- **Voice Pipeline:** Python (Wav processing)

---

## 🚀 Setup & Installation

### Prerequisites
1. **Node.js:** Ensure Node.js is installed.
2. **Ollama:** Install Ollama locally and pull the required model (`ollama run phi3:mini`).
3. **Python:** Required for the Speech-to-Text module.

### Installation
1. Clone the repository to your local machine.
2. Navigate to the project directory:
   ```bash
   cd BugsLie
   ```
3. Install dependencies:
   ```bash
   npm install
   ```
4. Start the application:
   ```bash
   npm start
   ```

### Configuration Notes
- **WhatsApp:** On first launch, check the terminal for the WhatsApp Web QR code to link your account. Create a group named **BugLie** on your phone to receive automated push notifications.
- **Email:** The application requires a Gmail App Password set in `backend/emailService.js` to send outbound alerts.

---

## 📝 Usage Guide

- **Fast Paths:** Some commands bypass the AI for instant execution.
  - `open [app]` (e.g. `open calculator`)
  - `whatsapp [name]: [message]`
  - `quiz me about [topic]`
- **Voice:** Click and hold the microphone icon in the chat bar to record a voice command.
- **File Search:** Type `find index.html` and let BugLie scan your computer to open it.

---

> **Built with ❤️ as a Next-Generation Offline Productivity Companion.**
