# Collegedunia University & Course Scraper

A dynamic Python web scraper that extracts **5 top Indian universities** and their **top 5 courses each** from [Collegedunia.com](https://collegedunia.com), and outputs a structured, relational Excel file.

## Features

- 🔍 **Dynamic university discovery** — scrapes the top 5 universities directly from Collegedunia's ranking aggregator, no hardcoded lists.
- 📄 **Next.js hydration parsing** — bypasses JavaScript rendering by extracting course slugs directly from the embedded `__NEXT_DATA__` JSON payload.
- 🤖 **AI-powered extraction** — uses an LLM (via OpenRouter) to extract clean Duration, Fees, Eligibility, Level, and Discipline from each course page.
- 🏫 **Official metadata** — extracts official university website URLs (not aggregator links) for each university.
- 📊 **Relational Excel output** — generates a professional two-sheet Excel file: `Universities` and `Courses`, linked by University ID.

## Output

`university_courses_cd.xlsx` with two sheets:

| Sheet | Columns |
|-------|---------|
| **Universities** | University ID, Name, Country, City, Website |
| **Courses** | Course ID, University ID, Course Name, Level, Discipline, Duration, Total Tuition Fees, Eligibility, URL |

## Setup

### 1. Clone the repo
```bash
git clone <your-repo-url>
cd "web scraping internship"
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
crawl4ai-setup   # installs the Playwright browser
```

### 3. Configure environment
Copy `.env.example` to `.env` and add your API key:
```bash
cp .env.example .env
```

Get a free API key from [OpenRouter](https://openrouter.ai) (supports free models like `stepfun/step-3.5-flash:free`).

### 4. Run
```bash
python scraper.py
```

The Excel output will be saved as `university_courses_cd.xlsx` in the same directory.

## How It Works

```
Collegedunia Aggregator
        │
        ▼
[1] Discover 5 university URLs via link regex
        │
        ▼
[2] For each university, fetch /courses-fees and parse __NEXT_DATA__ JSON
    → Extract all course specialization slugs
        │
        ▼
[3] For each slug, scrape the individual course page
    → LLM extracts: Duration, Fees, Eligibility, Level, Discipline
        │
        ▼
[4] Save relational Excel: Universities + Courses sheets
```

## Notes

- The scraper includes URL hotfixes for 3 universities whose Collegedunia aggregator IDs point to broken ghost pages. These are clearly documented in `scraper.py`.
- Rate limiting is handled automatically with retry logic and sleep intervals.
- Requires a headless browser (installed via `crawl4ai-setup`).
