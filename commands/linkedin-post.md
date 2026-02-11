# /linkedin-post

Create a LinkedIn post showcasing code you wrote or a PR you worked on, with a styled code screenshot.

## Instructions

Use the `linkedin-code-post` skill to complete this task.

1. **Determine the format:**
   - If the user mentioned a PR (number, URL, or "this PR"), use **PR Review** format
   - If the user mentioned specific code, files, or "this code", use **Code Showcase** format
   - If unclear, ask: "Would you like to showcase a specific code snippet (Code Showcase) or a PR you worked on (PR Review)?"

2. **Gather context from the user:**
   - For Code Showcase: Which file(s) or code snippet? What problem does it solve?
   - For PR Review: Which PR number or URL?

3. **Follow the 5-phase workflow** defined in the skill:
   - Phase 1: Context Gathering
   - Phase 2: Post Content Generation (get user approval on draft)
   - Phase 3: Code Screenshot via Carbon.now.sh
   - Phase 4: LinkedIn Post Creation via browser
   - Phase 5: User Review (STOP before clicking Post)
