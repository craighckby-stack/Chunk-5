# PDF-To-Github

PDF-To-Github is a browser-based utility designed to recover source code from PDF documents—such as exported Jupyter Notebooks or technical whitepapers—and automatically synchronize the extracted scripts to a GitHub repository. It leverages the **Cerebras Llama-3.3-70b** inference engine for high-speed, high-accuracy code reconstruction and the **GitHub API** for version-controlled storage.

## 🚀 Features

*   **LLM-Powered Extraction:** Uses Cerebras AI to intelligently separate Python code from text fragments and output/logs.
*   **Automated GitHub Sync:** Directly pushes recovered `.py` files to a specified repository and path.
*   **Real-time Processing:** Includes a live terminal-style log, progress tracking, and state management (Play/Pause/Abort).
*   **Smart Rate Limiting:** Built-in handling for API rate limits (HTTP 429) with visual countdowns for automatic retry.
*   **Zero-Backend Architecture:** Runs entirely in the browser using `pdf.js` for client-side document parsing.
*   **Persistent Configuration:** Securely stores API keys and repository settings in LocalStorage.

## 🛠️ Technical Stack

*   **Frontend:** React (Hooks & Functional Components)
*   **AI Engine:** Cerebras Cloud API (Model: `llama-3.3-70b`)
*   **PDF Parsing:** [PDF.js](https://mozilla.github.io/pdf.js/)
*   **Icons:** Lucide-React
*   **Storage:** GitHub REST API v3

## 📋 Prerequisites

Before using the application, you will need:
1.  **Cerebras API Key:** To perform the text-to-code transformation.
2.  **GitHub Personal Access Token (PAT):** With `repo` permissions to commit files.
3.  **Target Repository:** An existing GitHub repo where the code will be pushed.

## ⚙️ Configuration

Upon launching the app, fill in the following in the configuration panel:
*   `Cerebras API Key`: Your authentication token from Cerebras.
*   `GitHub Token`: Your PAT.
*   `Repository`: Format as `username/repo-name`.
*   `Target Path`: The directory within the repo where files should be created.

## 🏗️ Architecture

1.  **Ingestion:** The user uploads a PDF. `pdf.js` extracts the raw text layer.
2.  **Chunking:** The text is segmented into manageable snippets for processing.
3.  **Inference:** Snippets are sent to Cerebras Llama-3.3-70b with a system prompt optimized for Python extraction.
4.  **Committing:** Validated code is Base64 encoded and sent to the GitHub API. If a file already exists at the target path, the app fetches the SHA and performs an update commit.

## ⚠️ Limitations

*   **PDF Quality:** Success depends on the PDF having a selectable text layer (not flattened images).
*   **Context Window:** Extremely large code blocks may be truncated based on the `max_tokens` setting (currently 1024).