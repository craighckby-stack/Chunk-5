# Repository Analysis: PDF-To-Github

## Unique Working Chunks

### Idempotent GitHub Content Persistence
**File:** PDF-To-Github.js

> This logic functions as the persistence layer, enabling the application to synchronize recovered code with a remote GitHub repository. It performs a check-then-push workflow to retrieve the SHA of existing files, preventing merge conflicts and ensuring updates are correctly applied to the git tree.

```typescript
const pushFileToGithub = async (content, filename) => {
    const headers = { 'Authorization': `token ${ghToken}`, 'Accept': 'application/vnd.github.v3+json' };
    const path = `${targetPath}/${filename}`;
    try {
      let sha = null;
      const check = await fetch(`https://api.github.com/repos/${ghRepo}/contents/${path}`, { headers });
      if (check.ok) {
        const data = await check.json();
        sha = data.sha;
      }
      const b64Content = btoa(unescape(encodeURIComponent(content)));
      const response = await fetch(`https://api.github.com/repos/${ghRepo}/contents/${path}`, {
        method: 'PUT',
        headers,
        body: JSON.stringify({
          message: `Recovered ${filename}`,
          content: b64Content,
          sha
        })
      });
      return response.ok;
    } catch (e) {
      console.error("GitHub Push Error", e);
      return false;
    }
  };
```

---
### AI-Driven Python Extraction Engine
**File:** PDF-To-Github.js

> This is the core transformation logic of the system. It leverages the Llama-3.3-70b model via the Cerebras API to parse unstructured text fragments and reconstruct clean, executable Python code, effectively acting as the 'brain' that interprets PDF contents.

```typescript
const processSnippet = async (textSnippet, index) => {
    const prompt = `Extract Python code from this text fragment. Return ONLY code. \n    If you see output/logs, put them in a comment block at the bottom.\n    FRAGMENT: ${textSnippet}`;
    try {
      const response = await fetch('https://api.cerebras.ai/v1/chat/completions', {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${apiKey}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          model: 'llama-3.3-70b',
          messages: [{ role: 'user', content: prompt }],
          temperature: 0,
          max_tokens: 1024
        })
      });
      if (response.status === 429) {
        addLog("⚠️ RATE LIMIT HIT (429)");
      }
    } catch (e) {
      console.error(e);
    }
  };
```

---
### Dynamic Runtime Dependency Injection
**File:** PDF-To-Github.js

> This pattern allows the application to maintain a small initial bundle size by dynamically injecting the PDF.js library and its corresponding worker from a CDN only when the component mounts. This is critical for high-performance PDF processing in a browser environment without local build-time dependencies.

```typescript
useEffect(() => {
    if (!window.pdfjsLib) {
      const script = document.createElement('script');
      script.src = PDFJS_URL;
      script.onload = () => { window.pdfjsLib.GlobalWorkerOptions.workerSrc = PDFJS_WORKER_URL; };
      document.head.appendChild(script);
    }
  }, []);
```

---
### Interruptible Asynchronous Throttling
**File:** PDF-To-Github.js

> To handle strict external API rate limits, this utility provides a non-blocking delay mechanism. It integrates with the React lifecycle by checking an 'abort' ref, allowing the system to halt processes instantly while providing visual countdown feedback to the user.

```typescript
const waitWithCountdown = async (seconds) => {
    for (let i = seconds; i > 0; i--) {
      if (abortRef.current) return;
      setCountdown(i);
      await new Promise(r => setTimeout(r, 1000));
    }
    setCountdown(0);
  };
```

---
### Browser-Native State Synchronization
**File:** PDF-To-Github.js

> This chunk implements a 'serverless' persistence model for user credentials. By synchronizing application state with localStorage, it ensures that sensitive configuration data (API keys and repository paths) survives page refreshes without requiring a backend database.

```typescript
useEffect(() => {
    localStorage.setItem('cerebras_key', apiKey);
    localStorage.setItem('gh_token', ghToken);
    localStorage.setItem('gh_repo', ghRepo);
    localStorage.setItem('gh_path', targetPath);
  }, [apiKey, ghToken, ghRepo, targetPath]);
```
