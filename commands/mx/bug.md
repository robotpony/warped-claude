# Analyze and fix: $ARGUMENTS

Follow these steps:

1. Clarify the bug using AskUserQuestion:
   - What's the observed behaviour vs. expected behaviour?
   - Can it be reproduced reliably? What are the steps?
   - When did it start? Was anything recently changed?

2. Reproduce and root cause:
   - Identify the minimal reproduction path
   - Trace to root cause, not just symptoms
   - Note any related code that might be affected

3. Fix:
   - Make the smallest change that correctly addresses the root cause
   - Avoid fixing unrelated issues in the same pass

4. Regression check:
   - Confirm the fix resolves the original behaviour
   - Check that no adjacent behaviour was broken
   - Note if a test should be added

5. Finalize:
   - Update the package version (patch bump)
   - Build the project
   - Update the changelog and README if needed
   - Provide a commit message that names the bug fixed, not just "fix bug"
