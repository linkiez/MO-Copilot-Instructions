# MO Copilot Instructions

> A curated, living set of instructions, guidelines, and best practices for working effectively with GitHub Copilot (and other AI coding assistants).  
> Contributions are **warmly welcome** — whether you’re fixing a typo, adding examples, or proposing entirely new sections.

---

## 🌟 Why This Repository Exists

Modern development involves collaborating not only with people, but also with AI tools. This repository aims to:

- Document repeatable prompting patterns and interaction styles.
- Encourage responsible and ethical AI-assisted development.
- Provide reusable snippets, templates, and workflows.
- Evolve through community input and real-world usage.

If you’ve ever thought “I figured out a great way to ask Copilot for X — others should know this,” this is the place to share it.

---

## 🧭 Table of Contents
1. [Highlights](#-highlights)
2. [Getting Started](#-getting-started)
3. [Repository Structure](#-repository-structure)
4. [Usage Ideas](#-usage-ideas)
5. [Contributing](#-contributing)
6. [Pull Request Workflow](#-pull-request-workflow)
7. [Prompt Crafting Guide](#-prompt-crafting-guide)
8. [AI Usage & Attribution](#-ai-usage--attribution)
9. [Quality & Style Guidelines](#-quality--style-guidelines)
10. [Roadmap](#-roadmap)
11. [FAQ](#-faq)
12. [Community & Support](#-community--support)
13. [License](#-license)
14. [Acknowledgements](#-acknowledgements)

---

## ✨ Highlights

- ✅ Open to all contributors — first-time PRs encouraged.
- 🧪 Experiment-friendly: propose new instruction formats.
- 🧠 Focused on practical Copilot (and similar LLM) workflows.
- 🔄 Designed to be iterative: small improvements are valuable.
- 🛡️ Emphasis on clarity, safety, and responsible usage.

---

## 🚀 Getting Started

Clone the repository:

```bash
git clone https://github.com/linkiez/MO-Copilot-Instructions.git
cd MO-Copilot-Instructions
```

Optionally create a feature branch:

```bash
git checkout -b feat/add-new-prompt-pattern
```

Explore existing instruction files (if present) and follow the contribution guidelines below to add your enhancements.

---

## 🗂 Repository Structure

Below is a suggested structure (update this section to reflect the actual layout as the repo evolves):

```
/prompts/              -> Curated prompt patterns
/patterns/             -> Pattern explanations (e.g. "Refactor Loop", "Explain Commit")
/workflows/            -> Multi-step AI-assisted workflows
/examples/             -> Before/after transformations or generated outputs
/guides/               -> Narrative documents (ethics, best practices, limits)
/meta/                 -> Governance, templates, style decisions
README.md
CONTRIBUTING.md        -> (Optional: can be extracted from this README)
CODE_OF_CONDUCT.md     -> (Recommended)
LICENSE
```

If a directory doesn’t exist yet, feel free to propose and create it in a PR along with rationale.

---

## 💡 Usage Ideas

You can use this repository to:

- Copy/paste refined prompts for repeatable tasks.
- Learn phrasing strategies for better AI responses.
- Build internal team guidelines by forking.
- Track evolution of prompt engineering approaches.
- Compare output quality from different prompting styles.

---

## 🤝 Contributing

All contributions are welcome — including very small ones. Examples:

- Fixing typos or formatting
- Adding a new prompt pattern
- Documenting an edge case or failure mode
- Improving clarity or structure
- Adding a comparison example (bad prompt vs improved prompt)
- Translating content (open an issue first to coordinate)

If unsure whether something belongs: open a draft PR or issue to discuss.

---

## 🔁 Pull Request Workflow

1. Fork or create a branch (e.g., `feat/<short-description>`).
2. Make focused changes (small PRs merge faster).
3. Ensure:
   - Files use consistent tone (see Style section).
   - New patterns include: intent, structure, example, limitations.
4. Reference related issues if applicable (e.g., `Closes #12`).
5. Mark as Draft if still experimenting.
6. Await feedback — maintainers will:
   - Suggest edits
   - Merge
   - Or request clarification

### Example PR Description Template

```
### Summary
Adds a new prompt pattern for batch refactoring.

### Why
This is a common workflow when migrating style conventions.

### Additions
- /patterns/batch-refactor.md
- Example prompt + output

### Checklist
- [x] Includes limitations
- [x] Provides realistic before/after
```

---

## 🧪 Prompt Crafting Guide

When adding a prompt pattern, try to include:

| Section        | Description |
|----------------|-------------|
| Name           | Short, descriptive (e.g., "Iterative Refactor Loop") |
| Use Case       | When to use it |
| Template       | A reusable scaffold |
| Example        | Concrete input + output |
| Variations     | Optional tweaks |
| Failure Modes  | How it may go wrong |
| Mitigations    | How to recover |

Sample Format:

```
# Pattern: Iterative Refactor Loop

Use Case:
Improve legacy function clarity without changing behavior.

Template:
"Refactor the following code for readability, keeping logic identical. 
Return: 1) Improved code 2) Explanation of changes 3) Potential edge cases not handled."

Example Input:
<code>

Example Output:
<code>

Failure Modes:
- Drops edge-case checks
- Renames externally used symbols

Mitigations:
- Add explicit 'Do not rename public API symbols'
```

---

## 🧾 AI Usage & Attribution

To promote transparency:
- If significant content was AI-generated, note it (e.g., footer: “Portions generated with AI and reviewed by humans.”).
- Avoid copy/pasting raw AI output without review.
- Do not include proprietary or sensitive data in prompts.
- Report hallucinated facts so we can annotate them.

---

## 🧷 Quality & Style Guidelines

- Tone: clear, neutral, instructive.
- Avoid hype terms unless contextually relevant.
- Prefer examples over abstract description.
- Use American English (or note variation).
- Wrap lines at ~100–120 chars where practical for diff readability.
- Use Markdown headings semantically (do not skip levels).
- Use fenced code blocks with proper language tags.

---

## 🗺 Roadmap (Proposed)

| Phase | Focus | Status |
|-------|-------|--------|
| 1     | Core structure + seed patterns | In Progress |
| 2     | Add failure case catalog        | Planned |
| 3     | Introduce multilingual prompts  | Planned |
| 4     | Tool-specific sub-guides        | Planned |
| 5     | Automation (linting prompts)    | Ideas welcome |

Feel free to propose changes to this roadmap via issue or PR.

---

## ❓ FAQ

**Q: Do I need experience with AI to contribute?**  
A: No — documenting your learning journey is valuable.

**Q: Can I add tooling scripts?**  
A: Yes, if they support curation, analysis, or validation of prompt quality.

**Q: How large should a pattern file be?**  
A: Ideally under ~150 lines unless deeply illustrative.

**Q: Can I benchmark different models?**  
A: Yes — ensure reproducibility (inputs, settings, date).

---

## 🫂 Community & Support

- Open an Issue for: proposals, questions, structural changes.
- Use Discussions (if enabled) for broader ideation.
- Mention limitations candidly — honesty improves trust.

---

## 📜 License

Specify the license here (e.g., MIT, Apache-2.0).  
If not yet licensed, consider adding one soon so contributors know their rights.

---

## 🙌 Acknowledgements

Thanks to:
- Early contributors and reviewers
- Open-source AI ecosystem
- Anyone experimenting and sharing insights

Add yourself to this section in your first meaningful PR.

---

## 🧭 Next Steps for You

1. Decide on initial folder layout.
2. Add a LICENSE file (MIT is common).
3. (Optional) Add CODE_OF_CONDUCT.md.
4. Start with 2–3 seed prompt patterns.
5. Invite contributions publicly (social/share).

---

Happy building — let’s make AI-assisted development more intentional and accessible!

> “Small improvements to how we ask become big improvements to what we build.”
