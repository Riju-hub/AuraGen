# AuraGen: Self-Healing Generative UI via Cognitive Load

> Developed as part of the Generative AI Internship at **Infotact Solutions Pvt. Ltd.**

AuraGen is an adaptive interface framework that bridges the gap between client-side behavioral telemetry and real-time generative UI. Standard interfaces are rigid, forcing confused users to decipher static forms or manually consult side chatbots. AuraGen monitors real-time cognitive friction (rage clicks, hesitation intervals, erratic mouse movements) and automatically morphs complex, high-friction UIs into simplified, context-preserving step-by-step wizards on the fly.

---

## ⚡ Core Architecture

The system operates across a closed loop between client telemetry, edge analysis, and generative compilation:

```
[ User Interaction ] 
       │ (Cursor velocity, rage clicks, pauses)
       ▼
[ Friction Engine (React Client) ]
       │ WebSocket Telemetry Stream
       ▼
[ WebSocket Server & Cognitive Scorer (Node.js) ]
       │ Threshold Breach (High Cognitive Friction)
       ▼
[ Code-Gen Agent (LangChain + GPT-4o) ]
       │ Injects Design System + Current DOM State
       ▼
[ AST Injector & Security Linter (Babel / Node.js) ]
       │ Validated, Sandboxed React Component
       ▼
[ Dynamic Renderer (Next.js + Framer Motion) ]
       │ In-Browser Compilation & Smooth Morph
       ▼
[ Simplified Conversational Wizard ]
```

---

## 🧩 Key Modules

| Module | Stack | Description |
|---|---|---|
| **Friction Engine** | React, WebSockets | Tracks cursor velocity, cursor entropy, hesitation durations, and click-error rates to compute a normalized **Cognitive Load Score**. |
| **Code-Gen Agent** | LangChain, GPT-4o | Prompt pipeline bound to the design system (Tailwind CSS, component schemas) to synthesize syntactically strict, functional React components. |
| **AST Injector** | Babel, Node.js | Traverses the generated code's Abstract Syntax Tree (AST), validates imports, strips unsafe globals (`window`, `eval`, arbitrary fetch), and verifies JSX structures. |
| **Dynamic Renderer** | Next.js, Framer Motion | Uses dynamic imports, React Suspense, and in-browser runtime evaluation to hot-swap components without losing previously entered form state. |

---

## 🗓️ 4-Week Development Roadmap

### Week 1: Telemetry & Baseline Code Generation
- [ ] **Frontend:** Implement `useCognitiveLoad` hook to track cursor entropy, hesitation intervals, and rage-clicks.
- [ ] **Backend:** Establish bidirectional WebSocket telemetry transport and build LangChain prompting scaffolds with Tailwind CSS design constraints.

### Week 2: Compilation, AST Safety & Dynamic Shell
- [ ] **Backend:** Implement Babel-based AST validation to sanitize and lint LLM output before delivery.
- [ ] **Frontend:** Build the Next.js runtime injection container using dynamic compilation and React Suspense fallbacks.
- [ ] **Milestone Check:** Verify consistent React code generation and test spike detection during simulated confusion.

### Week 3: State Preservation & Fluid Morphing
- [ ] **Backend:** Pass active DOM form states into the generation pipeline so user inputs are preserved across component swaps.
- [ ] **Frontend:** Implement shared-layout morphing using `framer-motion` to transition between static layouts and conversational wizard flows.

### Week 4: Streaming, Optimization & Graceful Degradation
- [ ] **Backend & Client:** Implement token streaming directly into client compilation layers to bring UI generation latency below 2 seconds.
- [ ] **Error Boundaries:** Build safe fallbacks and circuit breakers to prevent broken layouts upon generation failure.
- [ ] **Milestone Check:** Complete end-to-end audit for the Final Project Review.

---

## 🛠️ Project Structure (Target Layout)

```text
auragen/
├── client/                     # Next.js frontend
│   ├── src/
│   │   ├── components/         # Standard UI components & Design System
│   │   ├── hooks/
│   │   │   └── useCognitiveLoad.ts   # Mouse tracking & rage-click telemetry
│   │   ├── dynamic/            # Runtime compiler and morphing shells
│   │   └── pages/ or app/      # App routes and financial demo form
├── server/                     # Node.js backend
│   ├── src/
│   │   ├── telemetry/          # WebSocket telemetry ingestion & scorer
│   │   ├── agent/              # LangChain pipelines & prompts
│   │   ├── security/           # Babel AST sanitizer & validation rules
│   │   └── index.ts            # Entrypoint
├── .gitignore
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- Node.js (v18.0.0 or higher)
- npm or pnpm
- OpenAI API Key

### Setup
```bash
# Clone the repository
git clone https://github.com/your-username/AuraGen.git
cd AuraGen

# Setup environment files
cp server/.env.example server/.env
# Add OPENAI_API_KEY to server/.env
```

---

## 🔒 Security & Code Integrity
- LLM outputs are treated as untrusted input.
- Babel parses all LLM responses into an AST; execution is rejected if unauthorized dependencies, DOM exploits, or unlisted globals are detected.