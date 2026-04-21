# PR Share Post Template

## Template Structure

```
{hook_line}

{the_problem}

{the_approach}

{the_lesson}

{cta_question}

{hashtags}
```

## Field Guidelines

### hook_line
Lead with the engineering insight or the problem that prompted the change. Make it universally relatable.

Good examples:
- "Sometimes the best fix isn't writing new code -- it's deleting the code that shouldn't have been there."
- "A one-line change that took three days to find. Here's what I learned."
- "We were so focused on scaling the system, we forgot to check if the system was doing the right thing."
- "This week I shipped a PR that changed how I think about error handling."

Bad examples:
- "Just merged a PR!" (no substance)
- "Fixed a bug today" (too generic)
- "Check out this code change" (no one can see the code)

### the_problem
2-3 lines describing the situation that led to the change. Be specific about the **type** of problem without revealing proprietary details.

- What was going wrong, or what was missing?
- How did you discover it?
- Why wasn't it obvious?

### the_approach
2-3 lines about the key technical decision. This is where the learning happens.

- What approach did you take and why?
- What alternatives did you consider?
- What was the non-obvious part?
- **Never include code diffs or snippets** -- describe the approach in plain language

### the_lesson
1-2 lines with the takeaway. This is the most shareable part of the post.

- What principle or pattern did this reinforce?
- What would you tell another engineer facing a similar problem?
- Frame it as wisdom, not just facts

### cta_question
1 line inviting engagement about the broader topic.

- "How do you approach X in your codebase?"
- "What's the most deceptive simple-looking bug you've encountered?"
- "Has anyone else found that Y pattern causes more problems than it solves?"

### hashtags
3-5 relevant hashtags.

- Always include `#SoftwareEngineering`
- Add stack-specific tags: `#TypeScript`, `#Python`, `#Rust`, etc.
- Add topic tags: `#Debugging`, `#CodeReview`, `#Refactoring`, `#Performance`
- Add `#BuildInPublic` if appropriate

## Constraints

- **Under 1300 characters total**
- **NEVER include code diffs or snippets** -- the post is about the lesson, not the code
- **Generalize the change** -- describe the engineering approach, not the specific implementation
- **First person** -- "I found", "I learned", "We discovered"
- **Professional but conversational** -- like a short story, not a PR description
- **Focus on the "why"** -- why this approach, why this mattered, why it was hard
- **1-3 emojis max** -- use sparingly
- **Use line breaks liberally** -- scannable content performs better on LinkedIn

## Examples

### Example 1: Performance Bug Fix

#### PR Context (not shown in post)
```
Title: Fix N+1 query in user dashboard endpoint
Files: 3 changed (api/views.py, api/serializers.py, tests/test_dashboard.py)
Description: Dashboard was making a separate DB query for each user's projects. Added select_related/prefetch_related to batch the queries.
```

#### Post
```
Our dashboard endpoint was taking 8 seconds to load. The fix was two lines of code, but finding the root cause taught me something I keep forgetting.

The problem wasn't obvious from the code. The ORM made everything look clean -- loop through users, get their projects, render the response. Perfectly readable. Perfectly slow.

Turns out we were making a separate database query for every single item in the list. The classic N+1 problem, hiding behind a clean abstraction.

The fix was simple once diagnosed -- batch the queries upfront instead of fetching one at a time. Response time dropped from 8 seconds to 200ms.

The lesson I keep relearning: readable code and performant code aren't always the same thing. ORMs are great until they hide what's actually happening at the database layer.

What's a performance issue that was hiding in plain sight in your codebase?

#Python #Backend #SoftwareEngineering #Performance #DatabaseOptimization
```

**940 characters**

### Example 2: Refactoring for Testability

#### PR Context (not shown in post)
```
Title: Extract payment processing into separate service class
Files: 12 changed across services/, tests/
Description: Payment logic was embedded in the API controller. Extracted into a PaymentService class with dependency injection for the payment gateway, making it testable without hitting the real API.
```

#### Post
```
I spent this week making code easier to delete. Sounds counterintuitive, but hear me out.

We had business logic tangled into our API layer. It worked, but testing meant spinning up the entire server and mocking HTTP calls. Adding a new payment method meant touching 6 files.

The refactor: pull the logic into its own module with clear boundaries. Pass dependencies in instead of reaching out for them. Now each piece can be tested in isolation and swapped independently.

The surprising part -- the refactored code is actually more lines than the original. But each piece is simpler, and I can test every edge case in milliseconds instead of seconds.

Sometimes "better" code isn't shorter code. It's code where the blast radius of any change is small and predictable.

What's your rule of thumb for when tangled code needs to be separated out?

#Refactoring #SoftwareEngineering #CleanCode #Testing #BuildInPublic
```

**870 characters**

### Example 3: Migration

#### PR Context (not shown in post)
```
Title: Migrate authentication from session-based to JWT
Files: 18 changed
Description: Moving to JWT for stateless auth. Needed for the new mobile app. Kept session support during transition with a feature flag.
```

#### Post
```
This week I started migrating our auth system and immediately remembered why everyone warns you about auth migrations.

The "simple" plan: swap session-based auth for token-based. The reality: every assumption about "who is the current user" was baked into the codebase in ways I didn't expect.

The key decision was running both systems in parallel during the transition. Old sessions keep working, new clients get tokens. It's more complexity short-term, but it means we can migrate gradually without a risky big-bang deploy.

Biggest lesson: the hardest part of an auth migration isn't the auth itself -- it's finding every place in the codebase that implicitly depends on how auth works.

Anyone else been through an auth migration? What surprised you most?

#Authentication #SoftwareEngineering #Backend #SystemDesign
```

**780 characters**
