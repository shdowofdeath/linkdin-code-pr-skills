# Weekly Digest Post Template

## Template Structure

```
{week_intro_hook}

{top_accomplishments}

{challenge_or_learning}

{whats_next}

{cta_question}

{hashtags}
```

## Field Guidelines

### week_intro_hook
Open with a quick framing of the week. Set the tone -- was it productive, challenging, surprising?

Good examples:
- "This was one of those weeks where everything just clicked. Here's what I shipped."
- "Productive week. Three things I built that I'm proud of."
- "This week was 80% debugging and 20% building. But that 20% was worth it."
- "Short week, but I managed to knock out something I've been putting off for months."
- "Week 3 of the new project. Starting to see it come together."

Bad examples:
- "Weekly update:" (boring)
- "Here's what I did this week" (too generic)
- "Status report" (corporate speak)

### top_accomplishments
3-6 lines covering your top 2-3 accomplishments. Lead with what you BUILT or FIXED, not what you worked on.

Format:
- Use short paragraphs or bullet-style lines
- Start each with an action: "Shipped...", "Fixed...", "Built...", "Refactored..."
- Include just enough context to be interesting

Examples:
- "Shipped the new search feature. Went with a full-text index over the API approach -- 10x faster and no external dependency."
- "Finally tracked down a memory leak that's been bugging us for weeks. The fix was embarrassingly simple."
- "Refactored the notification system. It was a tangle of if-else chains. Now it's event-driven and half the code."

**Rules:**
- 2-3 accomplishments max (depth over breadth)
- Focus on outcomes and decisions, not process
- If the week was mostly one big thing, that's fine -- go deeper on it
- Generalize proprietary details

### challenge_or_learning
1-2 lines about something that challenged you or something you learned. This is what makes the post human.

- "The rabbit hole I didn't expect: turns out our date handling was subtly wrong across time zones. Simple fix, but finding it was a journey."
- "Learned the hard way that caching invalidation is still the hardest problem in CS."
- "Spent half a day on a bug that turned out to be a missing await. The async/sync boundary strikes again."

### whats_next
1 line about what's coming next week. Creates continuity and follow-along momentum.

- "Next week: tackling the deployment pipeline. It's time."
- "Up next: writing tests for all the new stuff before I forget how it works."
- "Next up: user testing. Nervous but excited to see how people actually use it."

### cta_question
1 line inviting engagement.

- "What was your biggest win this week?"
- "Anyone else have one of those 'the fix was one line' moments recently?"
- "What are you shipping next week?"

### hashtags
3-5 relevant hashtags.

- Always include `#SoftwareEngineering`
- Add `#BuildInPublic`
- Add stack-specific tags based on what you worked with that week
- Add `#WeeklyUpdate` or `#DevLife`

## Constraints

- **Under 1300 characters total**
- **Conversational tone** -- "This week I..." not "Completed the following:"
- **Focus on accomplishments and learnings** -- not a task list
- **First person** -- "I shipped", "I learned", "I fixed"
- **Depth over breadth** -- 2-3 things explored well beats 8 bullet points
- **No ticket numbers, sprint numbers, or project management jargon**
- **1-3 emojis max** -- use sparingly
- **Use line breaks liberally**
- **Keep it positive but honest** -- "this was hard" is fine, complaining isn't

## Examples

### Example 1: Feature-Heavy Week

#### Activity Summary (not shown in post)
```
Repos: 1 (main project)
Commits: 14
Themes: New search feature (8 commits), bug fixes (4 commits), docs (2 commits)
Languages: TypeScript, SQL
Date range: last 7 days
```

#### Post
```
Productive week. Here's what I shipped 👇

Built a full-text search feature from scratch. Evaluated Elasticsearch vs PostgreSQL's built-in full-text search. Went with Postgres -- one fewer service to manage, and for our data size the performance is more than good enough.

Fixed a subtle bug where search results were returning duplicates. Turns out our join was fanning out rows in a way that wasn't obvious until the dataset grew. A simple DISTINCT fixed it, but finding the root cause took longer than the fix.

The learning: sometimes the "boring" technology choice (Postgres over Elasticsearch) is the right one. Less infrastructure complexity means fewer things that can break.

Next week: adding filters and faceted search. Curious how far I can push Postgres before needing a dedicated search engine.

What's your go-to approach for search -- dedicated engine or database-native?

#TypeScript #PostgreSQL #SoftwareEngineering #BuildInPublic #WeeklyUpdate
```

**880 characters**

### Example 2: Debugging-Heavy Week

#### Activity Summary (not shown in post)
```
Repos: 2 (backend + infra)
Commits: 9
Themes: Memory leak investigation (5 commits), infra monitoring (3 commits), minor fixes (1 commit)
Languages: Python, YAML
Date range: last 7 days
```

#### Post
```
This week was 90% detective work and 10% code. But I found the bug that's been haunting us for a month.

Our API's memory usage was slowly climbing until it got killed by the container runtime. Every. Single. Day. Classic memory leak, but nothing showed up in obvious places.

After two days of profiling, I found it: a cache that was supposed to expire entries was holding references to objects that kept growing. The cache was "working" -- entries expired on schedule -- but the underlying objects weren't being garbage collected because of a circular reference.

The fix was three lines. The investigation was three days. That's software.

Also set up better memory monitoring this week so we catch this kind of thing earlier. Grafana dashboards that alert before the container restarts, not after.

Sometimes the most valuable week isn't about features shipped. It's about understanding your system better.

What's the longest debugging session you've had, and was the fix embarrassingly simple?

#Python #Debugging #SoftwareEngineering #BuildInPublic #DevLife
```

**970 characters**

### Example 3: Multi-Project Week

#### Activity Summary (not shown in post)
```
Repos: 3
Commits: 18
Themes: API endpoint (6 commits), CLI tool (7 commits), docs updates (5 commits)
Languages: TypeScript, Rust, Markdown
Date range: last 7 days
```

#### Post
```
Busy week across three different projects. Here are the highlights.

Shipped a new API endpoint for batch processing. The interesting decision: stream results back as they complete instead of waiting for all items to finish. Users see progress immediately, and we avoid timeout issues with large batches.

Made solid progress on a CLI tool I'm building in Rust. Got the argument parsing and config file loading working. Still amazed at how Rust's type system catches bugs at compile time that would be runtime surprises in other languages.

Updated documentation across all three projects. Not glamorous, but future-me will be grateful. Adopted a pattern of writing docs immediately after shipping features, while the context is fresh.

Next week: integration tests for the batch API and adding subcommands to the CLI.

What's your strategy for staying productive when context-switching between multiple projects?

#TypeScript #Rust #SoftwareEngineering #BuildInPublic #WeeklyUpdate
```

**920 characters**
