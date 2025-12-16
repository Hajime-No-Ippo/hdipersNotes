# HDIPERS.LOG: Collaborative Knowledge Base 🚀

> **Survival Guide & High Performance Logic for HDip Software Development.**
> A strategic alliance for academic dominance.

🔴 **Live Site:** [hdipers.22abad.com](https://hdipers.22abad.com)

## 📖 About
This project is a static site built to serve as a collective knowledge base for our cohort. It hosts revision notes, lab solutions (optimized), and survival guides for the upcoming finals.

**Core Contributors:**
*   **Dong Li**: Architecture, Strategy, Logic.
*   **Yuliia**: SQL Optimization, Database Theory, Comprehensive Cheat Sheets.

**Tech Stack:**
*   **Core:** Vanilla HTML/JS (No frameworks, pure speed).
*   **Rendering:** `marked.js` for Markdown, `Prism.js` for code highlighting, `Mermaid.js` for diagrams.
*   **Hosting:** GitHub + Cloudflare Pages.

---

## 🤝 How to Contribute (Collaboration Protocol)

We welcome contributions! If you have summarized notes or a better solution for a lab, follow these steps to add it to the site.

### Step 1: Fork & Clone
1.  Click the **Fork** button at the top right of this repository.
2.  Clone your forked repo to your local machine:
    ```bash
    git clone https://github.com/YOUR_USERNAME/hdipersNotes.git
    ```

### Step 2: Create a New Post
1.  Navigate to the `posts/` folder.
2.  Create a new `.md` file. **Naming Convention:** `YYYYMMDD_Subject_Topic.md` (e.g., `20251212_CS144_TuringMachines.md`).
3.  **Crucial:** You MUST include the **FrontMatter** header at the very top of your file. Copy this template:

```yaml
---
title: Your Post Title Here
title_en: English Title
title_zh: 中文标题 (Optional, or repeat English)
date: 2025-12-11
author: Your Name
categories: 
  - CS144
tags: 
  - Algorithm
  - Logic
summary_en: A short description of what this note is about.
summary_zh: 简短的中文描述 (Optional)
---

[EN]
# Your Content Here
Write your notes in standard Markdown.
Code blocks and Mermaid diagrams are supported!
[END]

[ZH]
# 你的内容
(Optional Chinese translation)
[END]
```

### Step 3: Push & Pull Request
1.  Commit your changes: `git commit -m "Added notes for CS144"`
2.  Push to your fork: `git push origin main`
3.  Go to the original repository on GitHub and click **New Pull Request**.

---

## 🗺️ Roadmap
*   [x] **Phase 1**: Core Exam Prep (CS385, CS130, CS144).
*   [ ] **Phase 2**: Semester 2 Module Integration.
*   [ ] **Phase 3**: Interview Prep & LeetCode Patterns.

> *"Collaboration is the ultimate optimization."*
