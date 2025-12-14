# Google Developer Groups Event Highlights & Workshop Notes

*Date: [Insert Date of Event]*

This page serves as a detailed repository of notes, key takeaways, and resources from the recent GDG event covering AI co-pilots, Agent development, TestOps, and Server-Driven UI.

---

## Workshop 1: AI Colleagues and Co-Pilots

**Speaker:** Karl-Henrik "KH" Nilsson

This session focused on evolving our relationship with AI from simple prompting to treating them as capable teammates through structured guidance and "thinking models."

### Key Takeaways
* **Models Matter:** Different models yield different results. Use thinking models (like GPT-5.2 thinking) for planning and generation, and faster/cheaper models for validation.
* **Iterative Process:** Guiding the AI is crucial. Even "thinking" models need iterative loops to refine context and output.
* **Prompt Engineering as Code:** Treat prompts like software artifacts.
    * **Reusable Prompts:** Store them in a `prompts/` directory or a company-wide library.
    * **Config & Rules:** Clearly define constraints (e.g., "Do not commit secrets," "Prefer async/await").
* **MCP Servers (Model Context Protocol):** A standard for connecting AI models to external data sources (like file systems or web crawlers).

### The "Thinking Model" Workflow

The workshop highlighted moving beyond a single prompt into a multi-step iterative process to improve code generation quality.

```mermaid
graph TD
    A[Start: Define Task & Context] --> B{AI Drafts Solution};
    B --> C[AI Critiques Draft];
    C --> D[AI Improves based on Critique];
    D --> E[AI Generates Implementation Checklist];
    E --> F{Validation Pass};
    F -- Iteration needed --> B;
    F -- Validated --> G[Final Code Output];


Workshop 2: Agent Developer Kit (ADK)
This workshop explored moving beyond standard chatbots to building "AI Agents"—systems that can reason, plan, and execute tasks using tools.

Key Takeaways
The Agent Anatomy:

The Brain: The LLM (e.g., Gemini) used for reasoning and planning.

The Hands: Tools, functions, APIs, and databases the agent can interact with.

Orchestration: The loop that manages memory, state, and executes the plan.

Why ADK? It provides 10x more control than a simple API call, allowing for state management across multi-step tasks and connecting multiple specialized agents.

Code-First Approach: ADK allows defining agent logic in Python, Go, or Java, making it testable and version-controlled just like regular software.

Agent Orchestration Flow
How an agent processes a user request into concrete actions.

Code snippet

sequenceDiagram
    participant User
    participant Orchestrator (ADK)
    participant Brain (LLM)
    participant Tools (APIs/Functions)

    User->>Orchestrator (ADK): "What's the weather in Dubai?"
    Orchestrator (ADK)->>Brain (LLM): Sends input + available tool definitions
    Brain (LLM)-->>Orchestrator (ADK): Decides: Needs to call tool `get_weather('Dubai')`
    Orchestrator (ADK)->>Tools (APIs/Functions): Calls `get_weather('Dubai')`
    Tools (APIs/Functions)-->>Orchestrator (ADK): Returns data: "Sunny, 35°C"
    Orchestrator (ADK)->>Brain (LLM): Sends tool output back to brain
    Brain (LLM)-->>Orchestrator (ADK): Generates final natural language response
    Orchestrator (ADK)->>User: "It is currently sunny and 35°C in Dubai."
Resources & Links
ADK GitHub Repo: bukempas/adk-samples (Note: Verify exact repo link from event if different)

Workshop 3: TestOps
This session focused on best practices for modern testing pipelines, emphasizing automation, speed, and the integration of AI into QA.

Key Takeaways
Test Suites Strategy:

Smoke Suite: Is the app stable? (Fast)

Sanity Suite: Do specific recent changes work? (Targeted, AI can help select these).

Regression Suite: Did new changes break existing features? (Comprehensive).

E2E Suite: Do full app workflows function correctly?

Execution Environments: Shift-left by running tests inside the Dev CI/CD pipeline for immediate feedback, but also maintain independent QA pipelines for deeper regression against staging environments.

AI in TestOps: AI is boosting TestOps through intelligent test selection (running only what's needed based on code changes), self-healing tests that adapt to minor UI changes, and visual recognition mechanisms.

Standard TestOps Pipeline Structure
Code snippet

graph LR
    A[Code Commit/Merge] --> B(CI Trigger);
    B --> C{Build Application};
    C --> D[Unit & Integration Tests];
    D -- Fail --> E[Reject Merge];
    D -- Pass --> F[Deploy to Staging/QA Env];
    F --> G{Run Smoke/Sanity Tests};
    G -- Fail --> H[Alert Team];
    G -- Pass --> I[Run Full Regression/E2E Suite];
    I --> J[Generate & Publish QA Report];
Workshop 4: Dynamic UI with Firebase & Compose (Server-Driven UI)
This workshop addressed the friction of native app updates by introducing Server-Driven UI (SDUI) using modern tools.

Key Takeaways
The Problem: Traditional native development requires app store reviews and user downloads to update UI or logic, leading to misaligned versions across the user base.

The Solution (SDUI): Decouple the "What" from the "How."

The Server (Backend): Sends layout and data (what to display).

The Client (App): Renders the layout using native components (how to display).

The Stack:

UI Toolkit: Jetpack Compose (Declarative UI maps easily to remote JSON/XML).

Backend: Firebase Remote Config (Zero-latency updates and A/B testing capabilities).

Control Plane: AI Agents + MCP can be used to manage complex configurations.

Server-Driven UI Flow
Code snippet

sequenceDiagram
    participant App Client (Compose)
    participant Firebase Remote Config
    participant AI Admin Agent (Optional)

    Note over Firebase Remote Config: Stores UI Layout structure (JSON)
    App Client (Compose)->>Firebase Remote Config: Fetch latest UI Configuration
    Firebase Remote Config-->>App Client (Compose): Returns Layout JSON
    App Client (Compose)->>App Client (Compose): Parses JSON & dynamically renders Compose components
    Note over App Client (Compose): UI updated instantly without App Store download

    rect rgb(240, 240, 240)
    note right of AI Admin Agent (Optional): Optional AI Control Plane
    AI Admin Agent (Optional)->>Firebase Remote Config: MCP Command: `set_remote_config` (Update Layout)
    end