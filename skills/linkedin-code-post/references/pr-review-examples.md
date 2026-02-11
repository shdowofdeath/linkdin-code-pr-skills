# PR Review Post Examples

## Example 1: Performance Optimization PR

### PR Metadata
- **Title:** Optimize database queries with batch loading
- **Files changed:** 12
- **Additions:** +245 / Deletions: -189
- **Key files:** `src/repositories/user-repo.ts`, `src/services/batch-loader.ts`

### Selected Code Snippet
```typescript
class BatchLoader<K, V> {
  private queue: Map<K, Promise<V>> = new Map();
  private timer: NodeJS.Timeout | null = null;

  async load(key: K): Promise<V> {
    if (this.queue.has(key)) return this.queue.get(key)!;

    const promise = new Promise<V>((resolve) => {
      this.pending.set(key, resolve);
      this.scheduleFlush();
    });

    this.queue.set(key, promise);
    return promise;
  }

  private scheduleFlush() {
    if (!this.timer) {
      this.timer = setTimeout(() => this.flush(), 10);
    }
  }
}
```

### Resulting Post Text
```
Just merged a PR that cut our API response time from 1.2s to 180ms. The fix wasn't caching -- it was batching.

We had an N+1 query problem hiding in plain sight. Each user request triggered 15-20 individual DB queries that could have been one batch query.

Key changes:
-> Built a generic BatchLoader that deduplicates and batches requests within a 10ms window
-> Replaced 12 individual repository methods with batched equivalents
-> Added query count monitoring to catch future N+1 regressions

The surprising part: the 10ms batching window. It's long enough to collect all queries from a single request cycle, but short enough that users never notice the delay.

Sometimes the biggest wins come from fixing what you didn't know was broken.

#SoftwareEngineering #Performance #TypeScript #CodeReview #BackendDevelopment
```

### Rationale
- Hook leads with measurable impact (1.2s to 180ms)
- "Not caching -- batching" creates curiosity
- Bullet points give a scannable summary of the PR
- Highlights a non-obvious implementation detail (10ms window)
- Ends with a universal takeaway

---

## Example 2: Security Fix PR

### PR Metadata
- **Title:** Fix JWT validation bypass in auth middleware
- **Files changed:** 5
- **Additions:** +87 / Deletions: -23
- **Key files:** `src/middleware/auth.ts`, `src/utils/jwt.ts`, `tests/auth.test.ts`

### Selected Code Snippet
```typescript
function validateToken(token: string): TokenPayload {
  // Before: jwt.verify(token, secret) -- trusts alg header
  // After: explicitly specify allowed algorithms
  const payload = jwt.verify(token, secret, {
    algorithms: ["RS256"],
    issuer: config.JWT_ISSUER,
    audience: config.JWT_AUDIENCE,
    clockTolerance: 30,
  });

  if (!payload.sub || !payload.permissions) {
    throw new AuthError("Missing required claims");
  }

  return payload as TokenPayload;
}
```

### Resulting Post Text
```
Found a JWT validation bug in our auth middleware that's been live for 8 months. Here's what we fixed and what I learned.

The issue: we were calling `jwt.verify(token, secret)` without specifying allowed algorithms. This means an attacker could craft a token with `alg: "none"` and bypass verification entirely.

Key changes:
-> Locked algorithm to RS256 only -- no more trusting the token's alg header
-> Added issuer and audience validation
-> Added explicit claim checks for required fields
-> Wrote 14 new test cases covering algorithm confusion attacks

The lesson: library defaults are not security defaults. Always read the "Security Considerations" section of any auth library you use.

Your auth code might have this same bug. Worth a 5-minute check.

#Security #SoftwareEngineering #JWT #CodeReview #CyberSecurity
```

### Rationale
- Hook creates urgency without being clickbait (real bug, real timeline)
- Explains the vulnerability clearly without being a how-to guide
- Bullet points show comprehensive fix, not just a one-liner
- Lesson is actionable and universal
- CTA encourages readers to check their own code

---

## Example 3: Open Source Contribution PR

### PR Metadata
- **Title:** Add streaming support to response handler
- **Files changed:** 8
- **Additions:** +312 / Deletions: -45
- **URL:** github.com/example/lib/pull/847
- **Key files:** `src/handlers/stream.ts`, `src/types.ts`

### Selected Code Snippet
```typescript
async function* streamResponse(
  reader: ReadableStreamDefaultReader<Uint8Array>
): AsyncGenerator<ResponseChunk> {
  const decoder = new TextDecoder();
  let buffer = "";

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;

    buffer += decoder.decode(value, { stream: true });
    const lines = buffer.split("\n");
    buffer = lines.pop() ?? "";

    for (const line of lines) {
      if (line.startsWith("data: ")) {
        yield JSON.parse(line.slice(6));
      }
    }
  }
}
```

### Resulting Post Text
```
My first PR to an open source project just got merged -- streaming support for a library used by 12k+ projects.

The library handled API responses by waiting for the full response. For large payloads, users stared at a spinner for 10+ seconds before seeing anything.

Key changes:
-> Added AsyncGenerator-based streaming that yields chunks as they arrive
-> Handles SSE format with proper line buffering (the tricky part)
-> Backward compatible -- existing non-streaming usage works unchanged
-> 95% test coverage on the new streaming path

The hardest part wasn't the code -- it was the line buffering. Chunks from the network don't respect message boundaries, so you need to buffer partial lines and only yield complete ones.

If you've been thinking about contributing to open source, just start. Pick a feature you wish existed and build it.

#OpenSource #TypeScript #SoftwareEngineering #CodeReview #Streaming
```

### Rationale
- Personal angle (first open source contribution) adds authenticity
- Quantifies impact (12k+ dependent projects)
- Highlights the non-obvious challenge (line buffering across chunk boundaries)
- Ends with encouragement for others to contribute
- Shows the code that solves the trickiest part
