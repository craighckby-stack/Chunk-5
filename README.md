# Repository Analysis: PDF-To-Github

## Unique Working Chunks

### Dynamic PDF.js Initialization
**File:** PDF-To-Github.js

> Injects the PDF.js library and its worker script dynamically into the document head when the component mounts, ensuring library availability without local node modules.

```typescript
useEffect(() => { if (!window.pdfjsLib) { const script = document.createElement('script'); script.src = PDFJS_URL; script.onload = () => { window.pdfjsLib.GlobalWorkerOptions.workerSrc = PDFJS_WORKER_URL; }; document.head.appendChild(script); } }, []);
```

---
### Configuration Persistence Loop
**File:** PDF-To-Github.js

> Automatically synchronizes sensitive user configurations, such as API keys and repository paths, to browser localStorage whenever the state changes.

```typescript
useEffect(() => { localStorage.setItem('cerebras_key', apiKey); localStorage.setItem('gh_token', ghToken); localStorage.setItem('gh_repo', ghRepo); localStorage.setItem('gh_path', targetPath); }, [apiKey, ghToken, ghRepo, targetPath]);
```

---
### Bounded State Log Management
**File:** PDF-To-Github.js

> A utility for managing a live UI log console that prepends timestamped messages and enforces a maximum limit of 50 entries to prevent memory bloat.

```typescript
const addLog = (msg) => setLogs(prev => ['[' + new Date().toLocaleTimeString() + '] ' + msg, ...prev].slice(0, 50));
```

---
### Rate Limit Countdown Utility
**File:** PDF-To-Github.js

> An asynchronous waiting function that provides real-time countdown updates to the UI, specifically designed to handle API rate limits while remaining cancellable via a ref.

```typescript
const waitWithCountdown = async (seconds) => { for (let i = seconds; i > 0; i--) { if (abortRef.current) return; setCountdown(i); await new Promise(r => setTimeout(r, 1000)); } setCountdown(0); };
```

---
### GitHub Content Integrity Check
**File:** PDF-To-Github.js

> Interacts with the GitHub API to check if a file already exists and retrieves its SHA hash, which is required for performing updates to existing repository files.

```typescript
const check = await fetch('https://api.github.com/repos/' + ghRepo + '/contents/' + path, { headers }); if (check.ok) { const data = await check.json(); sha = data.sha; }
```
