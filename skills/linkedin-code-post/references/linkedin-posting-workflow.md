# LinkedIn Posting Workflow

## Prerequisites

- User must be logged into LinkedIn in Chrome
- Claude in Chrome MCP extension must be connected
- Post content must be approved by the user (Phase 2)
- Carbon screenshot `imageId` must be available (Phase 3)

## Step-by-Step Workflow

### 1. Update Plan

Before starting browser automation, call `update_plan` with:
- domains: `["linkedin.com"]`
- approach: Brief description of what you'll do

### 2. Navigate to LinkedIn Feed

```
navigate(url="https://www.linkedin.com/feed/", tabId=tabId)
```

Wait for the page to load:
```
computer(action="wait", duration=3, tabId=tabId)
```

### 3. Verify Login Status

Take a screenshot and verify the user is logged in:
```
computer(action="screenshot", tabId=tabId)
```

**Signs of being logged in:**
- Feed with posts visible
- Profile picture in top nav
- "Start a post" area visible

**If NOT logged in:**
- STOP immediately
- Tell the user: "You need to log in to LinkedIn first. Please log in manually, then let me know to continue."
- Do NOT attempt to log in on their behalf

### 4. Open Post Composer

Find and click the "Start a post" button:
```
find(query="Start a post", tabId=tabId)
```

Click the found element. If not found, try the direct URL fallback:
```
navigate(url="https://www.linkedin.com/post/new/", tabId=tabId)
```

Wait for the composer modal to open:
```
computer(action="wait", duration=2, tabId=tabId)
```

### 5. Type Post Content

**IMPORTANT:** LinkedIn uses a `contenteditable` div, not a standard textarea. Type content in chunks for reliability.

1. Find the text area in the composer:
   ```
   find(query="text area for post content", tabId=tabId)
   ```

2. Click to focus the text area

3. Type the post **paragraph by paragraph**:
   - Type the first paragraph
   - Press Enter twice for a line break: `computer(action="key", text="Enter Enter", tabId=tabId)`
   - Type the next paragraph
   - Repeat until all content is entered

4. For hashtags at the end:
   - Press Enter twice after the last line
   - Type hashtags normally (LinkedIn auto-links them)

**Tips:**
- Keep each typed chunk under ~200 characters for reliability
- Emojis can be typed directly as Unicode characters
- If text appears garbled, try clicking the text area again to refocus

### 6. Attach the Code Screenshot

1. Find the media/image attachment button:
   ```
   find(query="add a photo or image button", tabId=tabId)
   ```

2. Click the image/photo button to open the file upload dialog

3. Wait briefly for the upload UI:
   ```
   computer(action="wait", duration=2, tabId=tabId)
   ```

4. Find the file input element:
   ```
   find(query="file input for image upload", tabId=tabId)
   ```

5. Upload the Carbon screenshot using `upload_image`:
   ```
   upload_image(imageId=carbonImageId, ref=fileInputRef, tabId=tabId, filename="code-screenshot.png")
   ```

   If the ref approach doesn't work, try coordinate-based drag and drop:
   ```
   upload_image(imageId=carbonImageId, coordinate=[x, y], tabId=tabId, filename="code-screenshot.png")
   ```

6. Wait for upload to complete:
   ```
   computer(action="wait", duration=3, tabId=tabId)
   ```

### 7. Verify the Post Preview

Take a screenshot to verify everything looks correct:
```
computer(action="screenshot", tabId=tabId)
```

Check:
- Post text is complete and properly formatted
- Image is attached and visible in preview
- No error messages

### 8. HARD STOP

**DO NOT click Post, Publish, or any submit button.**

Tell the user:
> "Your LinkedIn post is composed and ready for review. The post text and code screenshot are in the composer. Please review it in your browser and click **Post** when you're satisfied."

## Handling Issues

| Issue | Solution |
|-------|----------|
| "Start a post" not found | Try direct URL: `linkedin.com/post/new/` |
| Text area not focused | Click directly on the text area, retry |
| Image upload fails with ref | Try coordinate-based upload_image |
| Post text appears garbled | Clear the text area, refocus, retype in smaller chunks |
| Composer modal closes unexpectedly | Re-open via "Start a post" and start over |
| LinkedIn shows rate limit or error | Stop and inform the user |
| "Post" button not visible | Scroll down in the composer modal |
