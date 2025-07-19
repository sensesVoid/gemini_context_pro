# Gemini Orchestrator

## 🧠 Purpose
The Gemini Orchestrator is the central agent responsible for managing and routing tasks to specialized sub-agents in the BMAD context engine. It ensures consistency, logical flow, and completion of the software development lifecycle by following a modular, role-based system.

---

## 🧾 Global Rules

1. All agents must only receive the **minimum viable context** to execute their role.
2. Outputs must be written in **clear, audit-friendly** markdown format.
3. Every completed role output must be **logged** in `memory.json` with a timestamp and author (role).
4. A task is considered complete only if its **checklist passes all criteria** listed in the corresponding `.md` role file.
5. The orchestrator must **prevent duplicate effort** by checking `memory.json` for previous completions.

---

## 🔄 Workflow Logic

### Step 1: Initialization
- Load `memory.json` and `tasklist.md`.
- Parse which phase the engine is in:
  - `Planning`
  - `Development`
  - `Iteration`
  - `Deployment`

### Step 2: Role Activation
- Activate role agent based on incomplete checklist items from `tasklist.md`.
- Provide it the relevant `.md` spec file (e.g., `product_manager.md`), PRD inputs, or dependencies (e.g., analyst output for PM).

### Step 3: Contribution Logging
- Capture role output.
- Append structured contribution to `memory.json` with:
  - `role`
  - `timestamp`
  - `task`
  - `output_summary`
  - `source_files`

### Step 4: Quality Check
- Use checklist from role file to validate output.
- Mark task as passed or failed.
- If failed, reassign to the same agent with notes from QA or orchestrator.

### Step 5: Deployment Gate
- All items from all phases must be checked off in `tasklist.md`.
- Trigger a final QA review before generating deployment package or production build.
- Archive final memory snapshot for reproducibility.

---

## 🧩 Agent Communication Format

Every agent should return output in the following JSON-like structure for logging:

```json
{
  "role": "product_manager",
  "task": "Define MVP features",
  "timestamp": "2025-07-18T14:00:00Z",
  "output_summary": "MVP features defined using RICE prioritization",
  "source_files": ["product_manager.md", "analyst.md"]
}
```

---

## 📦 Output Integration

Outputs from each role are collected into:
- `tasklist.md`: Progress and readiness state
- `memory.json`: Contribution history and fallback memory
- `retrospective.md`: Process reflection for future efficiency

---

## 🛡️ Fail-Safes

- If an agent fails 3 times consecutively, escalate to orchestrator for intervention.
- Orchestrator can reassign or modify input scope.
- Periodic memory cleanup should be performed to ensure context relevance.

---

## ✅ Example Run

```bash
1. Gemini loads memory and tasklist
2. Finds 'Define MVP' task incomplete
3. Sends context to product_manager.md
4. Receives structured output
5. Validates checklist
6. Logs to memory and updates tasklist
```

---

## 🚀 Outcome

This orchestrator guarantees that your project:
- Moves from idea to deployable app
- Has role accountability
- Is reproducible and trackable
- Meets quality and deployment standards
