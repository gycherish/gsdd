# debug

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`.

The problem is what the arguments describe, such as a symptom, an error, or a failing test or command. With no arguments, it is the failure under discussion in this conversation; when there is none, ask what fails.

This subcommand changes the code only to fix a root cause that lies in the code, or to put in a mitigation the user has agreed to; of the documents, it writes only research.md.

## 1. Choose Who Investigates

- If this conversation has already tried to fix this problem without success, undo those attempts unless they are needed to reproduce it, and do not investigate it yourself, even when you now think you know the cause: your earlier reasoning would steer the investigation. Have a subagent with a fresh context do only steps 2 and 3 of this guide and report the root cause with its evidence: give it the gsdd directory and the language (`SKILL.md`), the path of this guide, the symptom, how to reproduce it, the evidence such as errors and logs, and the fixes tried with their results, not your guesses about the cause. Then continue from step 4.
- Otherwise, investigate it yourself.

## 2. Reproduce

- Before changing anything, find a command or a test that reproduces the failure reliably.
- When it cannot be reproduced, do not change the code on a guess: report what is confirmed, what has been ruled out, and what is still needed, such as logs from the machine where it fails.

## 3. Find the Root Cause

- Read the requirements, the design, and the research of the capability the failure belongs to, and the steering.
- Confirm each hypothesis with evidence, such as an experiment, a log, the documentation, or release notes, before acting on it, and change one thing at a time.
- Find where the root cause lies:
  - **Code**: the code does not meet an acceptance criterion, though code that follows the design would.
  - **Design**: code that follows the design cannot meet the criterion, or the fix needs a design that the capability does not have (`shared/phases.md`).
  - **Requirements**: the requirements do not cover the case, contradict each other, or require behavior the user now finds wrong.
  - **Tasks**: a task cannot be done because a task it needs is missing or comes later.
  - **Outside the project**: the environment, a dependency, an external service, or the platform. When the platform cannot do what the design assumes, the root cause lies in the design.
- Remove the temporary logging and experiment code before going on.

## 4. Act on the Root Cause

- **Code**: fix it. First write a test that cites the criterion (`shared/conventions.md`) and fails because of the problem, or make an existing one strict enough to fail. Then fix the root cause, not only the symptom, until the test passes; change nothing else, and run all the tests. This is not a change: the phase stays as it is. When no automated test can reproduce the failure, give the user steps to check the fix by hand.
- **Design**: explain the root cause to the user, record the analysis in research.md (`shared/documents.md`), and suggest `/gsdd design <capability>`. Change no code, except a temporary mitigation that the user wants first and agrees to; a mitigation does not replace the full fix (`shared/phases.md`).
- **Requirements**: change no code. Explain which case the requirements miss, or where they go wrong; how the system should behave is the user's decision. Suggest `/gsdd requirements <capability>`.
- **Tasks**: explain what is missing or out of order, and suggest `/gsdd tasks <capability>`.
- **Outside the project**: explain what the user needs to do. A workaround in the code is a code fix when the design allows it, and a design problem otherwise.

## 5. Report

- The root cause and its evidence, separating what is confirmed from what is not.
- What was changed, and the commands run with their results.
- For a fix in the code: the test that reproduces the problem, with its run failing before the fix and passing after.
- Other places where the same mistake may appear, which were not changed.
- The next step: `/gsdd review` after a fix, or the subcommand named in step 4.
