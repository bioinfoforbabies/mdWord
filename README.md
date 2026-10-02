## How to set RStudio and Zotero


---

# 📄 Writing Articles in RStudio with Zotero Integration

Leverage the power of **Markdown** and **Zotero** to create reproducible, well-cited academic documents—all within **RStudio**.

---

## 🧠 Why This Workflow?

* **Markdown** allows embedding code and text seamlessly.
* **Reproducible research**: Export to HTML, PDF, or Word.
* **Interactive visualizations** via self-contained `.mhtml` files.
* **Built-in Git GUI** in RStudio ensures version control.
* **Zotero** manages references without paywalls or vendor lock-in.

---

## 🔧 Step 1: Install Required Tools

* [📥 Install RStudio](https://posit.co/download/rstudio-desktop/)
* [📥 Install Zotero](https://www.zotero.org/)

---

## 🔌 Step 2: Enhance Zotero with Better BibTeX

### ➕ Install Better BibTeX Plugin

1. **Download Plugin**:
   [Better BibTeX v7.0.26 Release](https://github.com/retorquere/zotero-better-bibtex/releases/tag/v7.0.26)

2. **Install in Zotero**:

   * Go to `Tools → Plugins → ⚙️ Gear Icon → Install Add-on From File`
   * Select the downloaded `.xpi` file
   * Restart Zotero

---

### ⚙️ Configure Better BibTeX

1. **Open Settings**:
   `Edit → Preferences → Better BibTeX`

2. **Set Citation Key Formula**:

   ```plaintext
   [auth.etal:lower:replace=.,_:postfix=_][>0][year]|[veryshorttitle][year]
   ```

---

## 🔄 Step 3: Connect Zotero with RStudio

### 📦 Install `rbbt` R Package

```r
# Install rbbt from GitHub
if (!require("remotes")) install.packages("remotes")
remotes::install_github("paleolimbot/rbbt")
```

---

### ⌨️ Add Keyboard Shortcut

1. Go to `Tools → Modify Keyboard Shortcuts...`
2. Search: `"Zotero"`
3. Locate: **Insert Zotero Citation**
4. Assign shortcut (e.g., `Ctrl + L` or `Cmd + L`)
5. Click **Apply**

---

## 📝 Step 4: Create Your Quarto Markdown Template

### Example YAML Header:

```yaml
title: "Article Title"
author: "Author Name"
date: "`r Sys.Date()`"

output:
  html_document:
    code_folding: hide    
    code_download: false

knitr:
  echo: false
  warning: false
  message: false
  fig.width: 15
  fig.height: 6

editor: visual

bibliography: "`r rbbt::bbt_write_bib('bibliography.json', overwrite = TRUE)`"
```

---

## 🔁 Step 5: Cite As You Write

* Press your shortcut (e.g., `Ctrl+L`)
* Search and insert Zotero references
* Let RStudio handle:

  * In-text citations
  * Bibliography generation
  * `.json` citation file

---

## 🧩 Behind the Scenes

1. First citation:

   * Creates `bibliography.json`
   * Adds formatted bibliography section

2. Later citations:

   * Automatically appended
   * Deduplicated

3. Final output:

   * References in APA, Chicago, etc.
   * Fully reproducible and clean document

---

## 🙏 Acknowledgments


Inspired by community contributions from:

* @Verhoeven
* @Rioja
* @Dunnington

---

## 📚 References

Files
-----
demo_manuscript_premium_v2_2026-08-14.Rmd
    Main R Markdown file.

premium_manuscript_reference_v2.docx
    Word reference template used by the Rmd YAML.

premium_manuscript_style_preview_v2.docx
    Static preview of the Word style system.

---
