# Project Share Post Template

## Template Structure

```
{hook_line}

{what_im_building}

{tech_approach}

{current_status}

{cta_question}

{hashtags}
```

## Field Guidelines

### hook_line
Attention-grabbing opener. Lead with the problem you're solving or a surprising constraint -- not "I'm building an app."

Good examples:
- "Every developer I know has this problem but nobody talks about it."
- "I got tired of waiting 3 minutes for our deploy pipeline, so I built something."
- "What if you could do X without needing Y?"
- "I've been thinking about this problem for months. Finally started building."
- "Took a side project from idea to working prototype this week."

Bad examples:
- "I'm building a cool new app!" (too generic)
- "Check out my new project" (no hook)
- "Working on something exciting" (says nothing)

### what_im_building
2-3 lines describing the project at a high level. Lead with the **problem** first, then the **solution**.

- Focus on who has this problem and why it matters
- Describe the solution in plain language (no jargon)
- Do NOT include proprietary details, specific business logic, or code
- Keep it relatable -- other developers should think "I've had that problem too"

### tech_approach
2-3 lines about the interesting technical decisions. This is where developers lean in.

- Mention the stack, architecture pattern, or a non-obvious choice
- Explain WHY you chose this approach, not just WHAT
- Examples:
  - "Using Rust for the core engine because we need sub-millisecond latency, with Python bindings for the CLI."
  - "Event sourcing instead of CRUD -- we need a full audit trail and the ability to replay state."
  - "Went with a monorepo + turborepo after trying microservices and finding the coordination overhead wasn't worth it."

### current_status
1-2 lines about where you are. Creates authenticity and invites follow-along.

- "Week 2. Got the core working, now wrestling with the edge cases."
- "Just hit the first working prototype. Still rough but it works."
- "Been at this for a month. Refactoring the data layer after learning the hard way that X doesn't scale."
- "Open sourced it yesterday. Curious to see if anyone else finds it useful."

### cta_question
1 line inviting engagement. Ask about the **problem space**, not your specific solution.

- "How do you handle X in your projects?"
- "Anyone else frustrated by Y?"
- "What's your go-to approach for Z?"
- "Would love to hear how others have tackled this."

### hashtags
3-5 relevant hashtags.

- Always include `#SoftwareEngineering`
- Add stack-specific tags: `#TypeScript`, `#Python`, `#Rust`, etc.
- Add domain tags if relevant: `#DevTools`, `#DataEngineering`, `#WebDevelopment`
- Add `#BuildInPublic` if appropriate
- Do NOT use generic tags like `#coding` or `#tech`

## Constraints

- **Under 1300 characters total** -- LinkedIn truncates after ~210 chars with "see more"; make sure the hook is above that line
- **No code snippets** -- this is a text post about the project, not a code showcase
- **No screenshots or images** -- text only
- **Focus on "why" and "what", not "how"** -- keep implementation details high-level
- **First person** -- "I'm building", "I chose", "I learned"
- **Professional but conversational** -- like explaining to a smart colleague over coffee
- **1-3 emojis max** -- use sparingly for visual breaks, not decoration
- **Show vulnerability** -- mention what's hard, what's uncertain, what surprised you
- **No corporate speak** -- avoid "leverage", "synergy", "best-in-class", "paradigm"
- **Use line breaks liberally** -- LinkedIn rewards scannable content
