# Getting Started

## Installation

ExoModel is LLM-agnostic. Install only the provider package you need:

```bash
pip install "exomodel[google]"      # Gemini (default)
pip install "exomodel[anthropic]"   # Claude
pip install "exomodel[openai]"      # OpenAI / Azure OpenAI
pip install "exomodel[cohere]"      # Cohere
pip install "exomodel[all]"         # all providers
```

## Configuration

Set your model and API key in a `.env` file at the root of your project:

```env
MY_LLM_MODEL=google_genai:gemini-2.5-flash-lite
MY_EMB_MODEL=google_genai:gemini-embedding-001   # optional — auto-detected from provider
GOOGLE_API_KEY=your-key-here
```

Or set them directly in code:

```python
import os
os.environ["MY_LLM_MODEL"] = "google_genai:gemini-2.5-flash-lite"
os.environ["MY_EMB_MODEL"] = "google_genai:gemini-embedding-001"
os.environ["GOOGLE_API_KEY"] = "your-key-here"
```

---

## Part 1 — Create and Update Objects from Natural Language

Inherit from `ExoModel`, define your fields, and call `.create()` with a plain-English description.
The object populates its own fields — no parsers, no manual mapping.

```python
from exomodel import ExoModel

class Proposal(ExoModel):
    client: str = ""
    project_description: str = ""
    budget: float = 0.0
    timeline_weeks: int = 0

# Populate from a single natural-language instruction
p = Proposal.create(
    "Draft a proposal for Tesla to build a real-time charging analytics dashboard. "
    "Budget around $80k, delivery in 12 weeks."
)

print(p.get_instance_json())
```

Update specific fields with a new instruction — only the changed fields are overwritten:

```python
p.update_object("Increase the budget by 10% and extend the timeline to 14 weeks")

print(p.get_instance_json())
```

---

## Part 2 — RAG Grounding

Override `get_rag_sources()` to attach documents to your class.
The object uses them as a knowledge base when populating fields and running analysis.
Sources can be local files, PDFs, or URLs.

```python
import os
from exomodel import ExoModel

rules = """# Proposal Rules
- Minimum project value is $10,000.
- Every proposal must include a 10% safety margin in the budget.
- We do not work with companies in the tobacco industry.
- All proposals over $50,000 must be flagged for legal review.
"""

RULES_PATH = os.path.abspath("proposal_rules.md")
with open(RULES_PATH, "w") as f:
    f.write(rules)

class GroundedProposal(ExoModel):
    client: str = ""
    project_description: str = ""
    budget: float = 0.0
    timeline_weeks: int = 0
    safety_margin_applied: bool = False
    flagged_for_legal: bool = False

    @classmethod
    def get_rag_sources(cls):
        return [RULES_PATH]

# The object is aware of the rules when it populates itself
p2 = GroundedProposal.create(
    "Draft a proposal for Acme Corp for a cloud data warehouse migration. "
    "Budget around $75k, 10 weeks."
)

print(p2.get_instance_json())
```

Run a self-analysis against the attached documents:

```python
analysis = p2.run_analysis()
print(analysis)
```

---

## Part 3 — Agentic Tools with `@llm_function`

Decorate any method with `@llm_function`. When you call `master_prompt()`,
ExoAgent reads the instruction, picks the right method, and calls it —
no `if/else`, no intent routing by hand.

```python
from exomodel import ExoModel, llm_function

class LeadContact(ExoModel):
    name: str = ""
    company: str = ""
    budget_signal: str = ""
    status: str = "new"
    disqualification_reason: str = ""

    @llm_function
    def qualify(self):
        """Qualify this lead — mark them as a strong prospect worth pursuing."""
        self.status = "qualified"

    @llm_function
    def disqualify(self, reason: str):
        """Disqualify this lead and record the specific reason."""
        self.status = "disqualified"
        self.disqualification_reason = reason

    @llm_function
    def request_more_info(self):
        """Mark this lead as needing more information before a decision can be made."""
        self.status = "pending_info"

# Create a lead from raw notes
lead = LeadContact.create(
    "Sarah Connor from Skynet Inc. Mentioned a budget of around $5k. Wants a simple brochure site."
)

# ExoAgent reads the instruction and dispatches to the right method automatically
lead.master_prompt("Disqualify this lead. The budget of $5k is too low for our minimum project value.")

print(lead.get_instance_json())
```

---

## Part 4 — Collections with `ExoModelList`

Generate and update a typed list of objects in a single LLM call.

```python
from exomodel import ExoModel, ExoModelList

class ProjectTask(ExoModel):
    title: str = ""
    owner: str = ""
    estimated_hours: int = 0
    priority: str = "medium"

class ProjectPlan(ExoModelList[ProjectTask]):
    pass

# Generate a full task list from one instruction
plan = ProjectPlan()
plan.create_list(
    "Break down a website redesign project into 5 tasks. "
    "Assign owners from: Alice, Bob, Carlos."
)

for task in plan.items:
    print(f"{task.title} | {task.owner} | {task.priority} | {task.estimated_hours}h")
```

Update all items in one LLM call:

```python
plan.update_list("Mark all tasks as high priority")
```

Export to CSV:

```python
print(plan.to_csv())
```

---

## Logging

ExoModel uses Python's standard `logging` module. By default all output is silent. To enable verbose output during development:

```python
import logging
logging.getLogger("exomodel").setLevel(logging.DEBUG)
```

---

**Next:** Learn the full API in [Core Usage](usage.md).
