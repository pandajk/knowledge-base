# Personal Knowledge Base

This repository hosts a personal knowledge base built with **MkDocs**, managed with **Git**, and synchronized via **pCloud**. It supports a clear directory structure, massive Markdown files, cross-device access (including Android and iOS), Retrieval-Augmented Generation (RAG) compatibility, and AI-driven automation for dynamic content generation.

## Features

- **Clear Directory Structure**: Organizes Markdown files in a hierarchical `docs/` folder (e.g., `docs/AI/2025/`), mirrored in the MkDocs site navigation.
- **Massive File Support**: Handles thousands of Markdown files efficiently, optimized for local storage and static site generation.
- **Markdown Compatibility**: Uses standard Markdown with YAML metadata (e.g., `tags: [AI, RAG]`) for rich content and RAG integration.
- **Cross-Device Sync**:
  - **pCloud**: Real-time synchronization across PC, Android, and iOS, with 10GB free storage and optional encryption.
  - **Git**: Version control via GitHub for consistency, AI updates, and manual edits.
- **Browsing**:
  - Rendered MkDocs site (hosted on GitHub Pages) for desktop and mobile access.
  - Responsive design with search and navigation (Material for MkDocs theme).
- **Editing**:
  - Desktop: VSCode for Markdown editing.
  - Android: GitJournal or Markor (with pCloud).
  - iOS: Working Copy or Obsidian (with pCloud).
- **RAG Compatibility**: Markdown files with metadata feed directly into vector databases (e.g., Chroma, Pinecone) for AI-driven retrieval.
- **AI Automation**: Scripts crawl web data (e.g., RSS, APIs), generate Markdown, and sync via Git/pCloud, updating the knowledge base and RAG index.

## Tech Stack

- **MkDocs**: Static site generator for rendering Markdown into a navigable website.
- **Git**: Version control for tracking changes, hosted on GitHub.
- **pCloud**: Cloud storage for cross-device sync, with privacy-focused features.
- **Python**: For AI automation scripts (e.g., LangChain, Scrapy).
- **GitHub Actions**: Automates AI updates, site deployment, and RAG indexing.
- **Mobile Apps**:
  - Android: GitJournal, Markor, Obsidian, pCloud.
  - iOS: Working Copy, Obsidian, Textastic, pCloud.

## Setup Instructions

### Prerequisites

- Python 3.9+ (for MkDocs and AI scripts).
- Git installed (`git --version`).
- pCloud account (free tier, 10GB).
- GitHub account and repository (e.g., `username/knowledge-base`).
- Android/iOS devices for mobile access.

### 1. Initialize MkDocs

1. Install MkDocs and Material theme:

   ```bash
   pip install mkdocs mkdocs-material
   ```

2. Create a new MkDocs project:

   ```bash
   mkdocs new knowledge-base
   cd knowledge-base
   ```

3. Configure `mkdocs.yml`:

   ```yaml
   site_name: Personal Knowledge Base
   theme:
     name: material
     features:
       - content.code.copy
       - search.suggest
   nav:
     - Home: index.md
     - AI: ai/
     - News: news/
   plugins:
     - search
     - tags
   markdown_extensions:
     - pymdownx.superfences
     - pymdownx.tabbed
     - meta
   ```

4. Create directory structure:

   ```bash
   mkdir -p docs/ai docs/news/2025
   echo "# Welcome" > docs/index.md
   echo "# AI Notes" > docs/ai/index.md
   ```

### 2. Set Up Git

1. Initialize a Git repository:

   ```bash
   git init
   git add .
   git commit -m "Initial MkDocs setup"
   ```

2. Push to GitHub:

   ```bash
   git remote add origin https://github.com/<username>/knowledge-base.git
   git branch -M main
   git push -u origin main
   ```

3. Generate SSH keys for secure access:

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
   cat ~/.ssh/id_rsa.pub
   ```

   - Add the public key to GitHub (Settings > SSH and GPG keys).

### 3. Configure pCloud

1. Sign up at [pCloud](https://www.pcloud.com) and install the pCloud app (Desktop, Android, iOS).
2. Create a sync folder (e.g., `pCloud/KnowledgeBase/`).
3. Move the MkDocs project to pCloud:

   ```bash
   mv knowledge-base ~/pCloud/KnowledgeBase/
   cd ~/pCloud/KnowledgeBase/knowledge-base
   ```

4. Verify sync: Edit a file (e.g., `docs/index.md`) and check it appears on other devices via the pCloud app.

### 4. Deploy MkDocs Site

1. Build and deploy to GitHub Pages:

   ```bash
   mkdocs gh-deploy
   ```

2. Access the site at `https://<username>.github.io/knowledge-base`.

3. Configure GitHub Actions for auto-deployment:

   - Create `.github/workflows/deploy.yml`:

     ```yaml
     name: Deploy MkDocs
     on:
       push:
         branches: [main]
     jobs:
       deploy:
         runs-on: ubuntu-latest
         steps:
           - uses: actions/checkout@v3
           - name: Install MkDocs
             run: pip install mkdocs mkdocs-material
           - name: Deploy
             run: mkdocs gh-deploy --force
     ```

### 5. Mobile Setup (Android/iOS)

#### Android

- **Browsing**:
  - Open `https://<username>.github.io/knowledge-base` in Chrome.
  - Features: Search, navigation, responsive UI.

- **Editing (Git)**:
  1. Install **GitJournal** (~$7, Play Store).
  2. Connect to GitHub (OAuth or SSH key).
  3. Clone `https://github.com/<username>/knowledge-base.git`.
  4. Edit files in `docs/` (e.g., `docs/ai/2025.md`), commit, and push.

- **Editing (pCloud)**:
  1. Install **Obsidian** (free, Play Store) and pCloud app.
  2. Open `pCloud/KnowledgeBase/knowledge-base/docs/` in Obsidian.
  3. Edit Markdown, save to sync.
  4. Pull changes to Git on desktop:

     ```bash
     git add docs/
     git commit -m "Android edits"
     git push origin main
     ```

#### iOS

- **Browsing**:
  - Open `https://<username>.github.io/knowledge-base` in Safari.
  - Same features as Android.

- **Editing (Git)**:
  1. Install **Working Copy** (free, ~$20 PRO, App Store).
  2. Clone repository with OAuth or SSH.
  3. Edit Markdown in `docs/` using built-in editor or **Textastic** (~$10).
  4. Commit and push changes.

- **Editing (pCloud)**:
  1. Install **Obsidian** and pCloud app.
  2. Access `pCloud/KnowledgeBase/knowledge-base/docs/` via iOS Files.
  3. Edit in Obsidian, sync automatically.
  4. Pull to Git on desktop (same as Android).

### 6. AI Automation

1. Create an AI script to crawl data and generate Markdown:

   ```python
   # ai_crawler.py
   import yaml
   from datetime import datetime
   import os
   content = "# AI News\nContent from web"
   metadata = {"title": "News", "tags": ["AI", "2025"], "date": datetime.now().strftime("%Y-%m-%d")}
   md_content = f"---\n{yaml.dump(metadata)}---\n{content}"
   folder = "docs/news/2025"
   os.makedirs(folder, exist_ok=True)
   with open(f"{folder}/{datetime.now().strftime('%Y-%m-%d')}.md", "w") as f:
       f.write(md_content)
   ```

2. Commit changes:

   ```bash
   git add docs/
   git commit -m "AI update"
   git push origin main
   ```

3. Automate with GitHub Actions:

   - Create `.github/workflows/ai-update.yml`:

     ```yaml
     name: AI Update
     on:
       schedule:
         - cron: "0 8 * * *" # Daily 8:00
     jobs:
       update:
         runs-on: ubuntu-latest
         steps:
           - uses: actions/checkout@v3
           - name: Install Python
             uses: actions/setup-python@v4
             with:
               python-version: "3.9"
           - name: Run AI Script
             run: |
               pip install pyyaml
               python ai_crawler.py
           - name: Commit Changes
             run: |
               git config user.name "AI Bot"
               git config user.email "bot@example.com"
               git add .
               git commit -m "AI update" || echo "No changes"
               git push
           - name: Deploy Site
             run: |
               pip install mkdocs mkdocs-material
               mkdocs gh-deploy --force
     ```

4. pCloud sync: AI script writes to `pCloud/KnowledgeBase/knowledge-base/docs/`, syncs to mobile.

### 7. RAG Integration

1. Update vector database with new/edited Markdown:

   ```python
   # update_rag.py
   from langchain_community.vectorstores import Chroma
   from langchain_community.document_loaders import DirectoryLoader
   from langchain_community.embeddings import HuggingFaceEmbeddings
   from langchain.text_splitter import RecursiveCharacterTextSplitter
   loader = DirectoryLoader("docs/", glob="**/*.md")
   docs = loader.load()
   splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
   chunks = splitter.split_documents(docs)
   vectorstore = Chroma.from_documents(chunks, HuggingFaceEmbeddings())
   ```

2. Run locally or via GitHub Actions:

   ```yaml
   # Add to ai-update.yml
   - name: Update RAG
     run: |
       pip install langchain langchain-community chromadb sentence-transformers
       python update_rag.py
   ```

3. Mobile edits/AI updates sync to Git/pCloud, triggering RAG refresh.

## Usage

- **Browsing**:
  - Visit `https://<username>.github.io/knowledge-base` on any device.
  - Use search bar or navigation to explore (e.g., AI > 2025).

- **Editing (Desktop)**:
  1. Open `pCloud/KnowledgeBase/knowledge-base/` in VSCode.
  2. Edit `docs/` files (e.g., `docs/ai/2025.md`).
  3. Commit:

     ```bash
     git add docs/
     git commit -m "Desktop edit"
     git push origin main
     ```

- **Editing (Mobile)**:
  - **Git**:
    - Android: Use GitJournal, pull/edit/push.
    - iOS: Use Working Copy, edit with Textastic, push.
  - **pCloud**:
    - Open Obsidian, edit `pCloud/KnowledgeBase/knowledge-base/docs/`.
    - Sync to desktop, commit to Git.

- **AI Updates**:
  - AI script runs daily (8:00), adds Markdown to `docs/news/2025/`.
  - GitHub Actions commits, deploys site, updates RAG.

- **RAG Queries**:
  - Use vector database (e.g., Chroma) for semantic search:

    ```python
    query = "AI news 2025"
    results = vectorstore.similarity_search(query, k=5)
    ```

## Directory Structure

```
knowledge-base/
├── docs/
│   ├── index.md           # Home page
│   ├── ai/
│   │   ├── index.md       # AI overview
│   │   ├── 2025.md        # AI notes for 2025
│   ├── news/
│   │   ├── 2025/
│   │   │   ├── 2025-04-14.md  # AI-generated news
├── mkdocs.yml             # MkDocs configuration
├── .github/workflows/     # GitHub Actions
│   ├── deploy.yml         # Site deployment
│   ├── ai-update.yml      # AI and RAG automation
├── ai_crawler.py          # AI script
├── update_rag.py          # RAG script
```

## Maintenance

- **Backups**: pCloud autosaves, Git history preserves versions.
- **Conflict Resolution**:
  - Git: Use branches (`mobile-edits`, `ai-updates`) to avoid conflicts.
  - pCloud: Check for conflict files (e.g., `file (conflicted copy).md`).
- **Performance**:
  - Limit mobile edits to subfolders (e.g., `docs/2025/`) for speed.
  - Optimize RAG chunks (500 chars) for massive files.
- **Security**:
  - Use SSH keys for Git, enable pCloud Crypto (~$4/month) for encryption.
  - Keep GitHub repository private for sensitive data.
- **Updates**:
  - Upgrade MkDocs:

    ```bash
    pip install --upgrade mkdocs mkdocs-material
    ```

  - Monitor GitHub Actions logs for AI failures.

## Troubleshooting

- **Site Not Updating**: Check `gh-deploy` or Actions logs.
- **Mobile Sync Issues**:
  - Git: Verify SSH/OAuth, run `git pull`.
  - pCloud: Ensure app is online, check sync status.
- **RAG Errors**: Validate Markdown metadata, test `update_rag.py`.
- **AI Script Fails**: Debug `ai_crawler.py`, ensure API keys are valid.

## Future Enhancements

- Add PWA for offline browsing (`manifest.json` in `site/`).
- Integrate RSS feeds in `ai_crawler.py`:

  ```python
  import feedparser
  feed = feedparser.parse("https://example.com/rss")
  for entry in feed.entries:
      # Generate Markdown
  ```

- Use Nextcloud for self-hosted sync (replace pCloud).
- Expand RAG with Pinecone for cloud-based retrieval.

## License

This knowledge base is for personal use. Adapt and share responsibly.

---
Built with ❤️ by [pandaj]
