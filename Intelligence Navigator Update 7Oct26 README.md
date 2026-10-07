# Security of AI™ Intelligence Navigators

Four connected, single-file HTML tools that map AI risks, vulnerabilities, adversary techniques, and assurance activities to the same framework baseline. Built and maintained by Security of AI™ (AI-RMF LLC).

- **Live tools:** https://ai-rmf.github.io/Security-of-AI/
- **Landing pages:** https://www.security-of-ai.com (each tool has a page with a Launch button)
- **Last full update:** 7 October 2026

> Independent educational tools. Not affiliated with NIST, MITRE, CISA, or OWASP. SOAI scores and mappings are Security of AI™ analysis unless a source is cited.

---

## 1. What's in this repo

| File | What it is | Entries | Landing page |
|---|---|---|---|
| `SOAI-AI-Risk-Intelligence-Table-7Oct26.html` | AI risks with NIST, ATLAS, OWASP mappings and related vulnerability classes | 50 risks | `/risk-navigator` |
| `SOAI-AI-Vulnerability-Intelligence-Navigator-7Oct26.html` | AI vulnerability classes with ATLAS, OWASP, CVE examples and CISA KEV status | 49 classes | `/vulnerability-navigator` |
| `SOAI-MITRE-ATLAS-Navigator-7Oct26.html` | MITRE ATLAS techniques with scenarios, scoring, mitigations and NIST mappings | 51 techniques | `/atlas-navigator` |
| `SOAI-AI-Assurance-Intelligence-Navigator-7Oct26.html` | Assurance activities that produce evidence a control is working | 49 activities | `/assurance-navigator` |
| `SOAI-Mitigation-Defense-Reference.html` | Mitigation and defense reference | — | — |
| `SOAI-Intelligence-Navigators-Interactive-Website-Links-Mitigation-beta.html` | Combined interactive links page (beta) | — | — |

Each tool is one self-contained HTML file. All data, styles and scripts live inside the file. There is no build step and no server.

---

## 2. Framework baseline

Every tool must use the same versions. When one changes, all four are reviewed (see Section 5).

| Framework | Version in use | Where the canonical data comes from |
|---|---|---|
| MITRE ATLAS | **v2026.09** | `mitre-atlas/atlas-data` on GitHub, `dist/ATLAS.yaml` |
| OWASP Top 10 for LLM Applications | **2025** | https://genai.owasp.org/llm-top-10/ |
| OWASP Machine Learning Security Top 10 | **2023** (used only where the LLM list doesn't apply) | https://owasp.org/www-project-machine-learning-security-top-10/ |
| NIST AI RMF | **1.0 (NIST AI 100-1)**, subcategory level (e.g. `MEASURE 2.7`) | https://www.nist.gov/itl/ai-risk-management-framework |
| NIST Adversarial ML taxonomy | **NIST AI 100-2 E2025** | https://csrc.nist.gov/pubs/ai/100/2/e2025/final |
| CISA KEV catalog | **2026.10.04** | `cisagov/kev-data` on GitHub |
| CVE records | as of 7 Oct 2026 | `CVEProject/cvelistV5` on GitHub |

### Quick reference

**OWASP LLM 2025:** LLM01 Prompt Injection · LLM02 Sensitive Information Disclosure · LLM03 Supply Chain · LLM04 Data and Model Poisoning · LLM05 Improper Output Handling · LLM06 Excessive Agency · LLM07 System Prompt Leakage · LLM08 Vector and Embedding Weaknesses · LLM09 Misinformation · LLM10 Unbounded Consumption

**ATLAS v2026.09 changes to remember:**
- Tactic renames: "ML Attack Staging" → **AI Attack Adaptation**; "ML Model Access" → **AI Model Access**
- Retired IDs (never use): `AML.T0019`, `T0022`, `T0023`, `T0045`, `T0058`, `T0015.001`, `T0015.002`
- A sub-technique inherits its parent technique's tactic.

**NIST AI RMF subcategories we use most:**
| Topic | Subcategory |
|---|---|
| Security and resilience testing | MEASURE 2.7 |
| Privacy | MEASURE 2.10 |
| Fairness and bias | MEASURE 2.11 |
| Safety | MEASURE 2.6 |
| Validity and reliability | MEASURE 2.5 |
| Transparency and accountability | MEASURE 2.8 |
| Production monitoring | MEASURE 2.4 |
| Third-party / supply chain | GOVERN 6.1, MAP 4.1, MANAGE 3.1 |
| Pre-trained models | MANAGE 3.2 |
| Human oversight | GOVERN 3.2, MAP 3.5 |
| Disengage / deactivate | MANAGE 2.4 |
| Incidents | MANAGE 4.3, MANAGE 2.3 |
| Legal and regulatory | GOVERN 1.1 |

Use NIST's exact subcategory IDs. Don't invent descriptions; the tools pull NIST's own wording into hover text.

---

## 3. How the tools connect

```
 Risk Table ──vuln tags──▶ Vulnerability Navigator ◀──vuln tags── Assurance Navigator
     │                          │                                     │
     └──── ATLAS tags ──────────┴──────── ATLAS tags ─────────────────┘
                                ▼
                     atlas.mitre.org/techniques/<ID>
```

- **Vulnerability classes are the hub.** The Risk Table and the Assurance Navigator link to them by ID (`MDL-01`, `INF-04`, …).
- **The Assurance Navigator's ATLAS and OWASP tags are derived** from the vulnerability classes each activity lists in `mapped_vulns`. Change a vulnerability class's mappings and the Assurance Navigator must be regenerated to match.
- **ATLAS tags** link to the technique's own page on atlas.mitre.org and show the technique name.
- **NIST tags** show NIST's subcategory text on hover.

### Deep links

The Vulnerability Navigator opens an entry automatically when the URL ends in `#<ID>`:

```
https://ai-rmf.github.io/Security-of-AI/SOAI-AI-Vulnerability-Intelligence-Navigator-7Oct26.html#INF-04
```

Deep links must point at the **GitHub Pages file**, not the security-of-ai.com landing page. The landing page only has a Launch button, so a `#ID` there does nothing.

### Where cross-tool links are hard-coded

| File | Setting | Points to |
|---|---|---|
| Risk Table | `const VULN_NAV_URL` | Vulnerability Navigator (GitHub Pages) |
| Assurance Navigator | `function vulnUrl(id)` | Vulnerability Navigator (GitHub Pages) |
| Mitigation Defense Reference | inline `href`s on each vulnerability tag (36) | Vulnerability Navigator (GitHub Pages) |
| All tools | buttons such as "ATLAS Navigator", "AI Risk Navigator" | security-of-ai.com landing pages (fine, no deep link) |

---

## 4. Data conventions

Each tool keeps its data in one JavaScript array near the bottom of the file.

| Tool | Array | Key fields |
|---|---|---|
| Risk Table | `RISKS` | `code`, `name`, `desc`, `nist[]`, `atlas[]`, `owasp[]`, `vuln[]` |
| Vulnerability Navigator | `VULNS` | `id`, `n`, `atlas[]`, `owasp[]`, `owaspml[]`, `cve` |
| ATLAS Navigator | `TECHS` | `id`, `n`, `tac`, `c`, `nist[]`, `rel[]`, mitigations |
| Assurance Navigator | `ASSURANCE` | `id`, `n`, `mapped_vulns[]`, `atlas[]`, `nist[]`, `owasp[]`, `owaspml[]`, `es`, `cov`, `L`, `I` |

Lookup tables in the same files: `ATLAS_VERSION`, `ATLAS_NAMES`, `NIST_TEXT`, `OWASP_LLM_2025`, `OWASP_ML_2023`, `VULN_NAMES`, `KEV`, `KEV_CATALOG`.

**Rules**
1. IDs are written in full: `AML.T0051.001`, `MEASURE 2.7`, `LLM01`.
2. An empty list is allowed and honest. A governance risk with no adversary technique shows "No direct ATLAS technique" rather than a forced match.
3. OWASP ML codes are used only when no OWASP LLM entry applies.
4. KEV status is never set by hand. The Vulnerability Navigator marks a CVE as KEV only if it appears in the embedded `KEV` table, which is copied from the CISA catalog.
5. Every CVE description must match its official CVE record. Don't add impact the record doesn't state.
6. ATLAS Navigator entries are grouped in ATLAS tactic order: Reconnaissance → Resource Development → Initial Access → AI Model Access → Execution → Persistence → Privilege Escalation → Defense Evasion → Credential Access → Discovery → Collection → AI Attack Adaptation → Exfiltration → Impact.

---

## 5. How we update

### When to review

| Trigger | Tools affected |
|---|---|
| New MITRE ATLAS release | All four (ATLAS Navigator first) |
| New OWASP LLM Top 10 | All four |
| NIST AI RMF revision | All four |
| CISA adds an AI/ML product CVE to KEV | Vulnerability Navigator, then Assurance notes if cited |
| New vulnerability class or risk added | The tool it's added to, plus any tool that links to it |
| Monthly | Check the ATLAS and KEV repos for changes |

### Steps

1. **Open an issue** in this repo describing the change and which tools it touches. One issue per change keeps the history readable.
2. **Update in this order.** Each tool depends on the one before it:
   1. ATLAS Navigator (technique IDs, names, tactics)
   2. Vulnerability Navigator (ATLAS, OWASP, CVE, KEV)
   3. Risk Table (links to vulnerability classes)
   4. Assurance Navigator (re-derive ATLAS/OWASP from `mapped_vulns`)
3. **Run the pre-publish checks** (below) on every file you changed.
4. **Name the new files with the date**, e.g. `SOAI-MITRE-ATLAS-Navigator-15Jan27.html`.
5. **Update every link to the old file name.** Search the repo for the old name and replace it. Today that means `VULN_NAV_URL` in the Risk Table and `vulnUrl` in the Assurance Navigator whenever the Vulnerability Navigator file name changes.
6. **Update the Launch buttons** on the security-of-ai.com landing pages to the new file names.
7. **Add a line to the Change log** (Section 8).
8. Remove superseded dated files once the website points at the new ones.

### Pre-publish checklist

- [ ] Every ATLAS ID exists in the current ATLAS release and none are retired.
- [ ] Every technique name matches ATLAS exactly.
- [ ] Every ATLAS Navigator entry sits under a tactic ATLAS actually lists for that technique.
- [ ] Every NIST tag is a real AI RMF 1.0 subcategory.
- [ ] Every OWASP code is from the 2025 list (or ML 2023, where used).
- [ ] Every vulnerability ID referenced by the Risk Table and Assurance Navigator exists in the Vulnerability Navigator.
- [ ] KEV markers match the current CISA catalog.
- [ ] Version badges, page description and footer show the current versions.
- [ ] Open the file in a browser: every entry opens, no "undefined" or "NaN" appears, search works, and no errors show in the browser console.
- [ ] Click one deep link from the Risk Table and one from the Assurance Navigator on the **live** site; the right entry opens.

---

## 6. Roles

| Role | Responsibility |
|---|---|
| Maintainer (Bobby Jenkins) | Approves mapping changes and publishes to GitHub Pages and security-of-ai.com |
| Contributors | Propose changes through issues or pull requests and run the checklist |
| Reviewer | A second person checks judgment-call mappings (NIST choices, governance entries) before publishing |

Mappings that are judgment calls (which NIST subcategory, whether a governance risk has an ATLAS technique) should get a second reviewer.

---

## 7. Open items

- [x] **Mitigation Defense Reference:** vulnerability links repointed to the GitHub Pages file (7 Oct 2026).
- [ ] **Mitigation Defense Reference mappings:** review which vulnerability classes each control domain lists. Several controls list classes that only loosely match (for example, Supply Chain Controls lists AI API Key Exposure and Regulatory Compliance Bypass, but not AI Dependency Confusion).
- [ ] **Stable file names:** dated file names break cross-tool links on every update. Consider keeping an undated copy of each tool (e.g. `SOAI-AI-Vulnerability-Intelligence-Navigator.html`) for links and Launch buttons to target.
- [ ] **Deep links into other tools:** only the Vulnerability Navigator opens an entry from `#ID`. Add the same to the ATLAS Navigator, Risk Table and Assurance Navigator if we want to link into them.
- [ ] **NIST wording check:** NIST hover text was taken from a reproduction of the AI RMF core because NIST's site was unreachable during the update. Spot-check against the official RMF.
- [ ] **Interactive Website Links (beta):** review against the 7 Oct 2026 baseline.

---

## 8. Change log

### 7 October 2026
- **All tools** aligned to ATLAS v2026.09, OWASP LLM 2025, NIST AI RMF 1.0 subcategories.
- **Vulnerability Navigator:** ATLAS IDs corrected and named; OWASP updated to 2025 with ML 2023 where applicable; CVE text checked against CVE records; KEV status computed from CISA catalog 2026.10.04 (7 classes flagged); `#ID` deep links added.
- **ATLAS Navigator:** tactics, names and mitigations corrected; entries reordered by tactic; Credential Access and AI Attack Adaptation added; 51 techniques; NIST tags replaced with fitting subcategories and hover text.
- **Risk Table:** NIST, ATLAS and OWASP corrected; related vulnerability classes added with deep links; "No direct ATLAS technique" shown where none applies.
- **Mitigation Defense Reference:** 36 vulnerability links now open the entry in the Vulnerability Navigator; hovering a tag shows the class name.
- **Assurance Navigator:** ATLAS/OWASP derived from mapped vulnerability classes; NIST string replaced with 1–3 real subcategories per activity; ATLAS links go to technique pages; TorchServe and dependency-scan notes corrected; search covers mappings; deep links to the Vulnerability Navigator.

### 6 October 2026
- **ATLAS Navigator** rebuilt on MITRE ATLAS v2026.09.

---

© 2026 Security of AI™ · AI-RMF LLC · https://www.security-of-ai.com · https://www.ai-rmf.com
