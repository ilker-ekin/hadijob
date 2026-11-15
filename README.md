<div align="center">

<img src=".github/assets/logo.svg" alt="" width="72">

# HadiJob

### Stop applying to jobs that won't sponsor your visa.

HadiJob scores **<!-- readme:hero-count:start -->7,000+<!-- readme:hero-count:end --> open jobs across Europe** for how likely they are to come with visa sponsorship. It uses government sponsor registers and the posting itself, and shows you **the proof behind every point**.<br>
Free, no sign-up, in English and Turkish.

**[Find sponsoring jobs at hadijob.com →](https://hadijob.com/)**

<sub>Testing phase: feedback is very welcome.</sub>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/assets/home-dark.png">
  <img src=".github/assets/home.png" alt="HadiJob search: 7,002 jobs in 21 countries, each with a sponsorship score and three plain-language reasons" width="820">
</picture>

<!-- readme:stat-line:start -->**7,000 jobs** · **21 countries + Europe-wide remote** · **224 employers** · **4 government sponsor registers** · **updated twice a day**<!-- readme:stat-line:end -->

</div>

## Why HadiJob

If you need a visa, most of your applications go to roles that were never going to sponsor you. Job boards don't say, and employers' own wording is vague.

HadiJob checks **public evidence** instead and turns it into a score you can read in a second and verify in a click:

- 🏛️ **Is the employer a licensed sponsor?** Checked against the Dutch IND register, Ireland's employment-permit statistics, the UK register of licensed sponsors and Denmark's list of Fast-track certified companies.
- 📝 **What does the posting say?** It picks up "visa sponsorship" and "relocation package", and also "you must already have the right to work".
- 💶 **Does the salary clear the permit threshold?** Compared with 2026 work-permit minimums in each country's own currency.

<img src=".github/assets/job.png" alt="Job page: a score of 85 broken down into +40 IND sponsor, +30 mentions relocation support, +10 large employer, +5 English posting" width="820">

## What you can do

**Find**

- **Search 21 countries at once,** the EU only, one country, or **Europe-wide** remote roles. Filter by **category** (Engineering, Data, Sales, Marketing and more), evidence strength, posting date and remote.
- **See what's new since your last visit.** Turn on "Remember my visits" once: from then on new jobs are marked, and one switch shows only those. Nothing is stored until you turn it on, and the date stays in your browser.
- **Share any search.** Every filter lives in the link.

**Understand**

- **Every point has a reason.** "IND recognised sponsor +40", "posting mentions a relocation package +30", "salary ≥ Blue Card threshold +15". No black box.
- **"Does this company sponsor visas?"** Each employer has its own page in the app, with the registers that list it, its open jobs per country with their strongest evidence, and its scored jobs. The companies list filters by country.

<img src=".github/assets/company.png" alt="Company page: Does Mollie sponsor visas? It lists the IND and UK registers and a table of open jobs per country that filters Mollie's job cards" width="820">

**Personalise**

- **Match your CV, privately.** Drop in a PDF CV: it's read in your browser and never uploaded. Jobs are then ranked by how many of your skills they ask for.
- **English or Turkish,** light or dark. Both follow your device, and you can switch either in the header.

<p align="center"><img src=".github/assets/mobile.png" alt="Search and job pages on a phone" width="560"></p>

## 🇹🇷 Türkçe

HadiJob, Avrupa'daki <!-- readme:hero-count-tr:start -->7.000<!-- readme:hero-count-tr:end -->'den fazla açık ilanı vize sponsorluğu olasılığına göre puanlar. Hollanda, İrlanda, Birleşik Krallık ve Danimarka'nın resmî sponsor kayıtlarına ve ilanın kendisine bakar, her puanın kanıtını da gösterir. Ücretsiz, üyelik yok, tamamen Türkçe: **[hadijob.com](https://hadijob.com/)**

## How it works

```mermaid
flowchart LR
  subgraph Sources["Public sources"]
    ATS[Greenhouse · Lever · Ashby<br/>job board APIs]
    REG[IND · DETE · UK · SIRI<br/>sponsor registers]
  end
  subgraph Pipeline["Pipeline · GitHub Actions, twice a day"]
    I[Ingest<br/>country · salary · phrases]
    R[Resolve<br/>employer ↔ register]
    S[Score<br/>rules v7 + evidence]
    E[Export<br/>JSON · pages · sitemap]
  end
  DB[(SQLite)]
  Web[Static site<br/>GitHub Pages]
  ATS --> I --> DB --> R --> S --> E --> Web
  REG --> R
```

1. **Ingest:** 226 company job boards are fetched politely. Each job's facts (country, remote, salary, language, sponsorship phrases) are extracted while the text is in memory. Descriptions are never stored.
2. **Resolve:** each employer is matched to the four registers. Exact names link automatically, near-matches go to human review, and every decision is kept in version control.
3. **Score:** versioned, explainable rules. A score starts at 0, adds each signal's weight and is clamped to 0–100.
4. **Export:** one JSON file per country for the site, plus about 320 pre-built company and country pages in both languages.

The whole thing runs for **€0**: no servers and no database server, with the SQLite file kept between runs as a release asset.

### Scoring rules (v7)

| Signal                                                                                      | Weight  |
| ------------------------------------------------------------------------------------------- | ------- |
| Employer is on the country's sponsor register (IND for NL · DETE for IE · UK · SIRI for DK) | **+40** |
| Posting mentions visa sponsorship, relocation or the EU Blue Card                           | **+30** |
| Employer is a registered sponsor in another country (countries with no register)            | **+20** |
| Salary ≥ the country's work-permit threshold (e.g. DE €45,934 · UK £41,700)                 | **+15** |
| Large employer (≥ 50 open jobs in ≥ 2 countries)                                            | **+10** |
| English posting in a country where English isn't the main language                          | **+5**  |
| Posting rules out sponsorship, or requires an existing right to work                        | **−60** |

Scores are **likelihoods, not guarantees and not legal advice**. Always confirm with the employer.

## By the numbers

### Live data

<!-- readme:live-table:start -->

From the live data on 2026-11-15 (`metrics.json`):

|                                 |                                                                                                       |
| ------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Published jobs                  | **7,000** in 21 countries + 195 Europe-wide (DE 1,668 · GB 1,523 · NL 1,053 · FR 485 …)               |
| Open jobs processed             | **19,613** from **226** job boards (Greenhouse 120 · Ashby 85 · Lever 21)                             |
| Jobs with register evidence     | **NL 1,012 of 1,053 · GB 1,263 of 1,523 · IE 325 of 471 · DK 13 of 80**                               |
| Sponsor registers               | **13,087** IND · **121,459** UK · **9,430** Irish permit holders · **990** Danish certified companies |
| Employers with sponsor evidence | **166** of 224                                                                                        |
| Duplicate postings folded       | **1,663** into 915 roles                                                                              |

<!-- readme:live-table:end -->

### Engineering facts

|                                    |                                                                      |
| ---------------------------------- | -------------------------------------------------------------------- |
| Company matching (automatic rules) | **100% precision · 81.8% recall** on 100 hand-labelled pairs         |
| Full refresh of all boards         | **75 s** (down from 37 min)                                          |
| Tests                              | **604**, plus an offline end-to-end smoke test of the whole pipeline |

## Engineering highlights

- **37 minutes to 75 seconds.** Timing each board showed the time wasn't spent downloading: a few connections stalled for about 15 minutes each (3 attempts × a 300 s default timeout). A 30 s per-attempt timeout fixed the cause, and polling the three API hosts in parallel (still one request per second per host) removed the rest.
- **Precision first.** Company matching only auto-links exact names; everything else needs a recorded human decision. On 100 hand-labelled pairs, including 45 deliberate look-alikes (Toast vs. "Yoast B.V."), the rules scored 100% precision.
- **Two pieces of evidence for the UK.** On the UK register's 121,000 names, exact-name matching produced 2 false links out of 17 ("Cursor Ltd" in Lincoln is not Cursor). A UK entry now links only when the name matches _and_ a register town is a city where the company hires: 100% precision on 37 labelled pairs, tested in CI.
- **Growing the employer list from the registers.** The registers double as a list of companies known to sponsor. A pilot probed 2,417 register names against three job-board APIs, measured the hit rate per register (0.9% for Dutch sponsors, against 0.3% for Irish permit holders), and kept only boards verified by hand.
- **Checked against reality.** Running each extractor on live postings caught false positives the unit tests didn't, such as "deals of £100k+ ACV" read as a salary, or "Visa sponsorship is not offered" counted as a mention. Every one became a regression test.
- **Search engines without a server.** The app uses hash routes, which search engines can't index, so the build pre-renders the home page and about 320 company and country pages with `react-dom/server`, in English and Turkish with `hreflang`. The pages link to each other with real addresses, carry breadcrumb structured data, and the sitemap dates each page by its newest job. The site is indexed through Google Search Console.
- **Privacy by architecture.** CV matching runs entirely in the browser with pdf.js and a skill taxonomy. "New since your last visit" stores just a date in the browser, and only after you turn it on. There are no accounts, cookies or trackers. Visit counts (ready, going live with the API) send one anonymous ping per page view into daily totals: no IPs, cookies or IDs stored, unknown sources kept only as "other", nothing sent with Do Not Track.
- **Hardened by default.** A strict Content-Security-Policy, where the only inline script is allowed by its SHA-256 hash. GitHub Actions are pinned to commit SHAs, with weekly Dependabot updates, dependency review and CodeQL on every pull request.
- **Runtime-agnostic core and validated boundaries.** The core uses only web-platform APIs and is shared with a Fastify API, and every external response is parsed with Zod. A changed API shape fails loudly for that board, and one bad link drops that job, not the whole board.
- **A government spreadsheet without a dependency.** The Irish permits list is an XLSX file whose layout changes every year. A ~100-line reader unzips it with the web-standard `DecompressionStream` and fails loudly on a changed layout instead of silently dropping employers.
- **Decisions on record.** Every architectural choice is written down as a decision record (44 so far), and all work goes through small, CI-gated pull requests.

## Tech stack

| Area       | Choice                                                                                     |
| ---------- | ------------------------------------------------------------------------------------------ |
| Language   | TypeScript (strict) on Node 24, run directly without a build step                          |
| Monorepo   | pnpm workspaces: core logic, pipeline, web, API                                            |
| Data       | SQLite via `node:sqlite`, Zod at every boundary                                            |
| Web        | React 19 + Vite, plain CSS with design tokens, pre-rendered SEO pages, English and Turkish |
| API (v1)   | Fastify, Kysely + Postgres, Testcontainers. Built and tested, not deployed yet             |
| Testing    | Vitest, Testing Library, jsdom: 604 tests, plus an offline end-to-end smoke test           |
| Automation | GitHub Actions: CI on every PR, pipeline twice a day and on merge, deploy to GitHub Pages  |

## Data sources and ethics

- **Public, keyless sources only:** the Greenhouse, Lever and Ashby job board APIs, the IND public register, the Irish DETE permit statistics, the UK register of licensed sponsors and Denmark's SIRI list of certified companies. No LinkedIn or Indeed, and no scraping behind logins.
- **Metadata plus a link out.** Full job descriptions are never stored or republished.
- **Attribution** on every piece of evidence, as the registers' reuse terms require.
- **Corrections and removal on request,** through the [correction form](https://github.com/ilker-ekin/hadijob/issues/new/choose) or by email to contact@hadijob.com.
- **GDPR first.** A privacy page names who runs the site and how to reach them, the legal bases and visitors' rights; register entries are deleted 30 days after they leave their register.

## Roadmap

**Done**

- [x] Explainable scores from four government sponsor registers (NL, IE, UK, DK), the posting and the salary
- [x] 21 countries plus Europe-wide remote roles, searchable together, by country or EU only
- [x] Every kind of role, not only tech, with a category filter
- [x] Evidence across borders: a sponsor in one country counts, at half weight, where there's no register
- [x] Company and country pages that search engines can index, in English and Turkish, now open to search engines
- [x] "New since your last visit": opt-in, kept only in your browser
- [x] CV matching in the browser
- [x] English and Turkish, light and dark, on its own domain
- [x] More employers: the whole Dutch sponsor register probed, 81 new employers after a hand check

**Next**

- [ ] Public launch: a final security review
- [ ] Deploy the search API on AWS Lambda + Neon (still €0, still no accounts)
- [ ] A public stats page with privacy-first visitor counts (built; goes live with the API)

## License

© 2026 İlker Ekin Erdoğdu. All rights reserved. This showcase (text and images) may not be reused without permission.
