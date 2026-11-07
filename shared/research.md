# Research

Every command that researches follows these rules: `req` on the product, `ui` on interfaces, `arch` on the technology and the architecture, and `design` on how a part is implemented.

## Doing It

- Use primary sources: the products themselves, their official documentation and release notes, specifications and standards, and source repositories. What users say about a product is read where they say it: in reviews, forums, and issue trackers.
- For broad searches, the command's main conversation dispatches subagents in parallel, one question each. Every subagent, whatever it was dispatched for, researches what its work needs itself. Give each the gsdd directory and the language (`SKILL.md`), the path of this file, and its question.
- As each question is answered, the main conversation tells the user its main findings in a few sentences.
- Research until the decision it serves can be made: start from the most established sources, and add more while they bring findings that would change the decision.
- Note the version or the date of each source that changes over time.
- When a technical fact a decision depends on is uncertain, check it with a small throwaway experiment outside the project before relying on it. When that cannot be done now, write it down as an unverified assumption, with how it will be checked.
- When research cannot be done because web access is unavailable or not allowed, say so before the conclusions, mark what comes only from memory, and ask whether the user can allow it.

## Recording It

Research is recorded in the `research.md` next to the documents whose decisions it supports (`shared/documents.md`); `ui` records its findings as `ui/guide.md` says.

- Organize it by question. For each: the question; the evidence, with its sources, or the experiment's environment, method, and result; the conclusion; and the decision it supports.
- The other documents state their decisions and the reasons themselves; research.md holds the evidence and the comparisons behind them.
- It holds what supports a current decision. When a decision changes, its evidence is updated with it; a rejected alternative stays while it explains the current choice.
- Experiments are thrown away once they have answered: record enough of the method to repeat them.
