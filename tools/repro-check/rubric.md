# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment section (OS, language version, dependency versions) | The report states specific version numbers actually used, not a vague description like "set it up and ran it" | required |
| steps-followable | The repro report's listed steps or commands | The steps are concrete and in order, so someone else could run them and reach the same state without guessing a missing step | required |
| expected-vs-actual | The report's expected output and actual output, compared against the issue's description | The actual output shown matches the behavior the issue describes. Expected output is stated first, then actual output contradicts it in the same way the issue reports | required |
| outcome-honest | The report's stated conclusion, checked against its own shown output | The conclusion (reproduced or not reproduced) is supported by the output actually shown. A well-evidenced "could not reproduce" passes. An unsupported "reproduced" claim fails | required |
| conventions-respected | The repo's stated contribution policy, checked against the comment text | If the repo states a policy such as disclosing AI use, the comment follows it. If no policy is stated, this passes | required |
| claim-quality | The claim comment text | The claim names the specific issue and a concrete next step. It does not promise a fix or a date, only investigation | required |

## Verdict rule

Accept if every check passes. Unclear counts as fail.