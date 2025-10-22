# n8n Workflow: Long-Tail Keyword Extractor 📈

This n8n workflow automatically extracts **long-tail keywords** (queries with many words) from your **Google Search Console** data, sorts them by length, and saves them in **Google Sheets** for SEO analysis and content planning.

---

## 🚀 What it does

- Fetches all queries from Google Search Console (e.g. last 12 months)
- Calculates **word count** and **character count** for each query
- Filters queries with **more than 6 words**
- Sorts the results from **longest to shortest**
- Sends the sorted list (with impressions, clicks, CTR, and position) to **Google Sheets**

Perfect for discovering detailed, long-tail keyword opportunities for your SEO and content strategy.

---

## 📁 Repository Structure

```

.
├─ workflow/
│  └─ long_tail_keyword_extractor.json     # exported n8n workflow
├─ images/
│  └─ workflow-diagram.png                 # workflow diagram / screenshot
└─ README.md

````

---

## ⚙️ Requirements

- **n8n** (self-hosted or n8n Cloud)
- **Community node:** `n8n-nodes-google-search-console`
- **Google Sheets** credentials connected to n8n
- A verified property in **Google Search Console**
- Basic understanding of n8n workflows

---

## 🧩 Workflow Steps

1. **Google Search Console**
   - Action: *Search Analytics → Query*
   - Time range: *Last 12 months*
   - Dimension: `query`

2. **Set Node – Count Words**
   - Adds a field `wordCount`:
     ```js
     {{ 
       ($json.query || '')
         .replace(/\u200c/g, ' ')
         .replace(/[^\p{L}\p{N}]+/gu, ' ')
         .trim()
         .split(/\s+/)
         .filter(Boolean)
         .length 
     }}
     ```
   - Include other input fields ✅

3. **Filter Node – Long Queries**
   - Condition: `wordCount` **is greater than** `6`
   - Convert Type ✅

4. **Set Node – Add charCount & Normalize**
   ```js
   {{
     String($json.query || '')
       .replace(/\u200c/g,' ')
       .replace(/\s+/g,' ')
       .trim()
       .length
   }}
````

5. **Item Lists Node – Sort**

   * Operation: `Sort`
   * Sort by:

     * `wordCount` → Descending
     * `charCount` → Descending

6. **Google Sheets Node – Append Rows**

   * Add columns:

     * `query`
     * `wordCount`
     * `charCount`
     * `impressions`
     * `clicks`
     * `ctr`
     * `position`

---

## 🧠 Example Output

| query                                     | wordCount | impressions | clicks | ctr   | position |
| ----------------------------------------- | --------- | ----------- | ------ | ----- | -------- |
| "علت کاهش پلاکت در آزمایش خون چیست"       | 7         | 832         | 21     | 0.025 | 3.4      |
| "چگونه سطح ویتامین دی بدن را افزایش دهیم" | 8         | 1,203       | 42     | 0.035 | 2.7      |
| "چرا آهن در آزمایش خون پایین است"         | 6         | 540         | 12     | 0.022 | 4.1      |

---

## 🧰 Tips

* Adjust the filter value (e.g., 5–6 words) depending on your language.
* Add more fields like `ctr`, `position`, or even `page` for deeper analysis.
* Combine with Google Looker Studio for dashboarding.

---

## 🔗 Related Projects

* [n8n Cannibalization Link Detector](https://github.com/zahraraeisi/n8n-cannibalization-link-detector)
* [n8n URL Indexing Status Checker](https://github.com/zahraraeisi/n8n-url-indexing-status-checker)

---

## 👩‍💻 Author

**Zahra Raeisi**
Automation & Applied AI with n8n
GitHub: [https://github.com/zahraraeisi](https://github.com/zahraraeisi)
LinkedIn: [https://www.linkedin.com/in/zahraraeisi](https://www.linkedin.com/in/zahraraeisi)
