# Code Showcase Post Examples

## Example 1: Retry Logic with Exponential Backoff

### Selected Code Snippet (TypeScript)
```typescript
async function withRetry<T>(
  fn: () => Promise<T>,
  maxRetries = 3,
  baseDelay = 1000
): Promise<T> {
  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      if (attempt === maxRetries) throw error;
      const delay = baseDelay * Math.pow(2, attempt);
      const jitter = delay * 0.1 * Math.random();
      await new Promise(r => setTimeout(r, delay + jitter));
    }
  }
  throw new Error("Unreachable");
}
```

### Resulting Post Text
```
I used to copy-paste retry logic into every API call. Then I wrote this 15-line function and never looked back.

The problem: our service calls third-party APIs that flake out under load. We needed retries, but naive retries cause thundering herds.

The fix: exponential backoff with jitter. Each retry waits 2x longer than the last, plus a small random offset so retries from different clients don't all hit at the same time.

The key insight -- that jitter line matters more than the backoff itself. Without it, retries synchronize and you're back to square one.

What's your go-to pattern for handling unreliable external services?

#TypeScript #SoftwareEngineering #BackendDevelopment #Resilience
```

### Rationale
- Hook line is specific and relatable ("copy-paste retry logic")
- Shows the problem clearly (flaky APIs, thundering herds)
- Highlights a non-obvious insight (jitter > backoff)
- CTA invites practical discussion
- 780 characters -- well under the 1300 limit

---

## Example 2: React Custom Hook for Debounced Search

### Selected Code Snippet (TypeScript/React)
```typescript
function useDebouncedSearch(query: string, delay = 300) {
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (!query.trim()) {
      setResults([]);
      return;
    }

    setLoading(true);
    const timer = setTimeout(async () => {
      const data = await searchAPI(query);
      setResults(data);
      setLoading(false);
    }, delay);

    return () => clearTimeout(timer);
  }, [query, delay]);

  return { results, loading };
}
```

### Resulting Post Text
```
Our search was firing an API call on every keystroke. 50 requests for a 10-character query. Here's the 20-line hook that fixed it.

Instead of debouncing at the input level, I moved it into a custom hook that wraps the entire search flow -- query, loading state, results, cleanup.

The cleanup function is the part most people miss. Without it, stale responses from earlier keystrokes can overwrite newer results. The `clearTimeout` in the return ensures only the latest query resolves.

Simple pattern, but it cut our search API calls by 90%.

How do you handle search debouncing in your React apps?

#React #TypeScript #Frontend #WebDevelopment #SoftwareEngineering
```

### Rationale
- Hook uses concrete numbers ("50 requests", "90% reduction")
- Highlights a subtle bug (stale responses) that many developers miss
- Code snippet is 20 lines -- right in the sweet spot
- Post is educational without being condescending

---

## Example 3: Python Data Pipeline Filter

### Selected Code Snippet (Python)
```python
def build_pipeline(*steps):
    def pipeline(data):
        for step in steps:
            data = step(data)
            if data is None:
                return None
        return data
    return pipeline

process_user = build_pipeline(
    validate_email,
    normalize_name,
    check_duplicates,
    enrich_from_crm,
    save_to_db,
)
```

### Resulting Post Text
```
I replaced a 200-line if/else chain with a 10-line function composition pattern.

We had a user processing flow with 8 validation and transformation steps. Each step had its own try/catch, early returns, and logging. Adding a new step meant touching 5 files.

Now each step is a pure function: data in, data out (or None to halt). The pipeline composes them and short-circuits on the first None.

Adding a new step? One line. Reordering steps? Move a line. Testing a step? Call it directly.

Composition over complexity. Every time.

What's a refactoring pattern that changed how you write code?

#Python #CleanCode #SoftwareEngineering #Refactoring
```

### Rationale
- Strong hook with specific numbers ("200 lines to 10")
- Shows before/after contrast without needing the "before" code
- Emphasizes practical benefits (adding, reordering, testing steps)
- Ends with a memorable one-liner and engagement question
- 730 characters -- concise and punchy
