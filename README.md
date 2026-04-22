# Repository Analysis: PDF-To-Github

## Unique Working Chunks

### GitHub API File Synchronization Logic
**File:** PDF-To-Github.js

> This logic serves as the persistence layer for the application. It performs an idempotent operation by first checking for an existing file's SHA before pushing updates, ensuring that content is safely synchronized with the remote repository without duplicate conflicts.

```typescript
const pushFileToGithub = async (content, filename) => { const headers = { 'Authorization': `token ${ghToken}`, 'Accept': 'application/vnd.github.v3+json' }; const path = `${targetPath}/${filename}`; try { let sha = null; const check = await fetch(`https://api.github.com/repos/${ghRepo}/contents/${path}`, { headers }); if (check.ok) { const data = await check.json(); sha = data.sha; } const b64Content = btoa(unescape(encodeURIComponent(content))); const response = await fetch(`https://api.github.com/repos/${ghRepo}/contents/${path}`, { method: 'PUT', headers, body: JSON.stringify({ message: `Recovered ${filename}`, content: b64Content, sha }) }); return response.ok; } catch (e) { console.error("GitHub Push Error", e); return false; } };
```

---
### AI-Driven Python Code Extraction
**File:** PDF-To-Github.js

> This function is the core transformation engine. It leverages a Large Language Model (Llama-3.3-70b) to convert unstructured text extracted from PDFs into clean, executable Python code, handling the primary functional requirement of the system.

```typescript
const processSnippet = async (textSnippet, index) => { const prompt = `Extract Python code from this text fragment. Return ONLY code. If you see output/logs, put them in a comment block at the bottom. FRAGMENT: ${textSnippet}`; try { const response = await fetch('https://api.cerebras.ai/v1/chat/completions', { method: 'POST', headers: { 'Authorization': `Bearer ${apiKey}`, 'Content-Type': 'application/json' }, body: JSON.stringify({ model: 'llama-3.3-70b', messages: [{ role: 'user', content: prompt }], temperature: 0, max_tokens: 1024 }) }); if (response.status === 429) { addLog("⚠️ RATE LIMIT HIT (429)"); } } catch (e) { console.error(e); } };
```

---
### Dynamic Runtime Dependency Injection
**File:** PDF-To-Github.js

> This architectural pattern allows the system to remain lightweight and dependency-free at the source level. By dynamically injecting the PDF.js library and its worker at runtime, the application ensures high-performance document parsing without requiring a local Node.js environment or heavy initial bundles.

```typescript
useEffect(() => { if (!window.pdfjsLib) { const script = document.createElement('script'); script.src = PDFJS_URL; script.onload = () => { window.pdfjsLib.GlobalWorkerOptions.workerSrc = PDFJS_WORKER_URL; }; document.head.appendChild(script); } }, []);
```

---
### Asynchronous Interruptible Throttling
**File:** PDF-To-Github.js

> Designed to handle strict API rate limits, this utility provides a bridge between UI feedback and process control. It implements a non-blocking wait that is fully responsive to 'abort' signals, allowing the system to pause execution gracefully during heavy extraction tasks.

```typescript
const waitWithCountdown = async (seconds) => { for (let i = seconds; i > 0; i--) { if (abortRef.current) return; setCountdown(i); await new Promise(r => setTimeout(r, 1000)); } setCountdown(0); };
```

---
### Zero-Backend Client-Side Persistence
**File:** PDF-To-Github.js

> This chunk handles the persistence of sensitive credentials and configuration data. By synchronizing the component state with localStorage, the application achieves a 'serverless' state management architecture where user settings persist across sessions without a backend database.

```typescript
useEffect(() => { localStorage.setItem('cerebras_key', apiKey); localStorage.setItem('gh_token', ghToken); localStorage.setItem('gh_repo', ghRepo); localStorage.setItem('gh_path', targetPath); }, [apiKey, ghToken, ghRepo, targetPath]);
```
