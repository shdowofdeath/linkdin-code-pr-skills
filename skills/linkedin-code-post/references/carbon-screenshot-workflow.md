# Carbon Screenshot Workflow

## URL Parameter Reference

| Parameter | Purpose | Recommended Value |
|-----------|---------|-------------------|
| `code` | Source code (URL-encoded) | URL-encoded snippet |
| `t` | Theme | `night-owl` |
| `l` | Language | `typescript`, `javascript`, `python`, etc. or `auto` |
| `bg` | Background color | `rgba(171,184,195,1)` |
| `ds` | Drop shadow | `true` |
| `dsblur` | Shadow blur | `68px` |
| `dsyoff` | Shadow Y offset | `20px` |
| `fm` | Font family | `JetBrains Mono` |
| `fs` | Font size | `16px` |
| `ln` | Line numbers | `false` |
| `lh` | Line height | `133%25` |
| `pv` | Padding vertical | `56px` |
| `ph` | Padding horizontal | `56px` |
| `wc` | Window controls | `true` |
| `wt` | Window theme | `none` |
| `wa` | Auto-adjust width | `true` |
| `si` | Square image | `false` |
| `es` | Export size | `2x` |
| `wm` | Watermark | `false` |

## LinkedIn-Optimized Defaults

These settings produce a clean, high-contrast screenshot that looks great in a LinkedIn feed:

```
t=night-owl
fm=JetBrains%20Mono
fs=16px
bg=rgba(171,184,195,1)
wc=true
ln=false
es=2x
wm=false
pv=56px
ph=56px
ds=true
dsblur=68px
dsyoff=20px
wa=true
```

## Primary Approach: URL with Parameters

This is the fastest and most reliable method.

### Steps

1. **Prepare the code snippet**
   - Select 15-30 lines of the most interesting code
   - Remove unnecessary blank lines or comments
   - Ensure proper indentation

2. **URL-encode the code**
   Use Bash to URL-encode:
   ```bash
   python3 -c "import urllib.parse; print(urllib.parse.quote(open('/tmp/carbon_snippet.txt').read()))"
   ```
   First write the snippet to `/tmp/carbon_snippet.txt`, then encode it.

3. **Build the Carbon URL**
   ```
   https://carbon.now.sh/?code={encoded_code}&t=night-owl&l={language}&bg=rgba(171,184,195,1)&fm=JetBrains%20Mono&fs=16px&wc=true&ln=false&es=2x&wm=false&pv=56px&ph=56px&ds=true&dsblur=68px&dsyoff=20px&wa=true
   ```

4. **Navigate to the URL**
   ```
   navigate(url=carbonUrl, tabId=tabId)
   ```

5. **Wait for render**
   ```
   computer(action="wait", duration=3, tabId=tabId)
   ```

6. **Take screenshot**
   ```
   computer(action="screenshot", tabId=tabId)
   ```
   Store the returned `imageId` -- you'll need it for LinkedIn upload.

7. **Verify the screenshot**
   Check that the code is fully visible and properly formatted. If cut off, adjust the snippet length or font size.

## Fallback Approach: Manual UI Interaction

Use this if the URL approach fails (e.g., code is too long for URL encoding, special characters cause issues).

### Steps

1. **Navigate to plain Carbon**
   ```
   navigate(url="https://carbon.now.sh/?t=night-owl&fm=JetBrains%20Mono&fs=16px&ln=false&es=2x&wm=false", tabId=tabId)
   ```

2. **Wait for load**
   ```
   computer(action="wait", duration=3, tabId=tabId)
   ```

3. **Find the code editor**
   ```
   find(query="code editor textarea", tabId=tabId)
   ```

4. **Select all existing code and replace**
   - Click on the editor area
   - `computer(action="key", text="cmd+a", tabId=tabId)` to select all
   - `computer(action="type", text=codeSnippet, tabId=tabId)` to type new code

5. **Set language** (if not auto-detected)
   - Find the language dropdown
   - Click it and select the correct language

6. **Take screenshot**
   Same as primary approach step 6.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Code truncated in URL | Use fallback approach (type into editor) |
| Screenshot shows loading state | Increase wait duration to 5 seconds |
| Wrong syntax highlighting | Set `l` parameter explicitly to the correct language |
| Code too wide (horizontal scroll) | Reduce line length or font size to `14px` |
| Special characters break URL | Use fallback approach |
| Carbon page not loading | Check internet connection; try refreshing the tab |
