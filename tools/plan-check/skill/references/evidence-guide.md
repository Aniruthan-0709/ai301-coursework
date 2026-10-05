# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: Eval mode: the repro report's environment or setup section in the bundle. Live mode: the "Environment" section of the student's draft repro report.

What good looks like: The report names specific version numbers (language runtime, key dependency) that match or are compatible with what the issue's repo-facts block states as the target. If they differ, the report says so explicitly rather than staying silent.

## Steps

Where it lives: Eval mode: the repro report's steps-to-reproduce section. Live mode: the same section in the student's draft.

What good looks like: Each step names a concrete action (a command run, a file edited, an input given) in order, starting from a stated initial state, with no step that assumes an unstated prior action.

## Behavior shown

Where it lives: Eval mode: the repro report's output or error excerpt, read against the issue context's description of the bug. Live mode: the draft's shown output, read against the actual GitHub issue body.

What good looks like: The quoted output contains the same symptom the issue describes (same error type, same failing behavior), not a different error that happens to occur in the same area of code.

## Honesty

Where it lives: Eval mode: the repro report's stated conclusion line, checked against its own artifacts in the same bundle. Live mode: the draft's conclusion, checked against its own shown output.

What good looks like: The conclusion claims no more than the shown output supports. "Reproduced" is only used when the output actually demonstrates the issue's behavior. "Could not reproduce" is acceptable and passes when the attempted steps and their non-matching output are still shown.

## Comms

Where it lives: Eval mode: the repo-facts block, for any stated contribution or disclosure policy, and the claim comment text. Live mode: the repo's CONTRIBUTING.md or issue template, and the student's draft claim comment.

What good looks like: If the repo states a policy such as disclosing AI assistance, the comment complies with it. The claim names the specific issue and a concrete next action, not a generic "I'll take a look," and contains no promised fix or date.