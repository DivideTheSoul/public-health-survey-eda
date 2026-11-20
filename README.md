# Community Health & Household Determinants Assessment

An end-to-end public health data analysis and presentation project evaluating individual epidemiological risks ($N = 249$) and household socioeconomic/environmental determinants ($N = 66$) from the `community_file Approve@Send.xlsx` survey dataset.

---

## 📋 Project Overview

This repository contains the data sanitization pipeline, statistical hypothesis testing, visual analytics, and operational outreach recommendations for a community health survey. The analysis identifies critical clinical and social vulnerabilities across life stages and translates them into a **5-Pillar Community Health Action Plan** tailored for primary care partners (**Public Health Center 33**, **Siriraj Hospital**) and village health volunteers (**อสม.**).

### Repository Contents

| File | Description |
| :--- | :--- |
| `community_file Approve@Send.xlsx` | Raw survey workbook containing individual records (`บุคคล`), household records (`ครัวเรือน`), and summary tables (`หัวข้อ 5 ครัวเรือน`). |
| `community_health_project_presentation.ipynb` | Self-contained, pre-executed Jupyter Notebook structured as a slide presentation with embedded tables, statistical tests, and charts. |
| `README.md` | Project documentation, methodology audit, key findings, and setup instructions. |

---

## 🔍 Key Analytical Insights

1. **Super-Aged Community & NCD Surge:**
   * **40.16%** (100/249) of residents are aged 60+ (mean age: 45.96 years, median: 48.0).
   * **53.82%** (134/249) overall and **95.0%** (95/100) of seniors live with at least one chronic non-communicable disease ($\chi^2 = 111.29, p = 5.10 \times 10^{-26}$).
   * **68.02%** (168/247) are overweight or obese, and **21.29%** (53/249) have a high Thai CVD Risk Score.

2. **Dietary Seasoning & Sugary Beverages as Primary Behavioral Drivers:**
   * Among residents aged 15+ ($n = 210$), **79.31%** (115/145) of strong-seasoning (`รสจัด` — salty/sweet) eaters have a chronic disease compared to **16.92%** (11/65) of mild-flavor (`รสจืด`) eaters ($\chi^2 = 70.21, p = 5.33 \times 10^{-17}$).
   * **53.41%** (133/249) consume sweetened beverages (`น้ำหวาน`) regularly, peaking among adolescents aged 15–17 (**80.0%**) and seniors aged 60+ (**74.0%**).
   * Only **25.71%** (54/210) of residents aged 15+ meet regular exercise guidelines ($\ge 3\times$/week, $\ge 30$ min), while **43.81%** smoke and **40.95%** consume alcohol.

3. **Unsupervised Geriatric Fall & Dementia Risk (Ages 60+, $n = 100$):**
   * **59.0%** have knee osteoarthritis, **39.0%** have impaired balance, **28.0%** suffered $\ge 2$ falls in the past 6 months, and **22.0%** screen positive for suspected dementia.
   * **Thai FRAT** fall-risk scores strongly differentiate fallers (mean = **7.04**, SD = 1.95) from non-fallers (mean = **1.27**, SD = 1.83; Mann-Whitney $p = 3.09 \times 10^{-19}$).
   * **Caregiver Deficit:** **64.3%** (18/28) of recurrent fallers and **50.0%** (11/22) of seniors with suspected dementia have **no caregiver (`ไม่มี`)**.

4. **Mental Health, Sleep Disruption & Pediatric Screening:**
   * **65.86%** (164/249) report insomnia (`เป็นบางวัน` 33.73%, `เป็นประจำ` 32.13%).
   * Among ages 15+, **19.14%** (40/209) screen positive for depression risk (`2Q เสี่ยง`) and **15.24%** (32/210) experience high/severe stress (`ST-5`).
   * Among youth aged 0–18 ($n = 52$), **53.85%** (28/52) have untreated dental caries. In early childhood (0–5, $n = 17$), **3 children** exhibit below-standard growth, **3** fine-motor delay, and **3** language delay.

5. **Household Income Gradient, Family Conflict & Sanitation ($N = 66$):**
   * **53.03%** (35/66) report strained family dynamics, including **33.33%** (22/66) frequent arguments and **12.12%** (8/66) domestic violence.
   * Harmonious households average **17,129 THB/month** vs. **6,095 THB/month** (frequent arguments) and **6,250 THB/month** (domestic violence, where 100% earn $\le 8,200$ THB/month; Kruskal-Wallis $H = 41.74, p = 1.89 \times 10^{-8}$).
   * **72.73%** (48/66) do not sort waste properly, **39.39%** report odor nuisance, and **78.13%** (25/32) of vector-affected households take no vector-elimination action.

---

## 🧹 Data Hygiene & Engineering Audit

Before statistical modeling, the notebook executes an automated data-cleaning pipeline that resolves five structural issues in the raw workbook:

1. **In-Memory OpenXML Stylesheet Repair:** Fixes an invalid `family > 14` attribute in `xl/styles.xml` that otherwise causes `openpyxl` / `pandas.read_excel()` to raise a validation error.
2. **Schema Separation (`ครัวเรือน`):** Slices household records strictly to rows `1–66`, separating them from embedded summary tables in rows `69–115`.
3. **String & Category Standardization:** Strips leading/trailing whitespace in `BMI-4-` (consolidating `'น้ำหนักเกิน'` from 90 + 5 + 2 into **97** records), fixes double-vowel typos (`'ไม่เกี่ี่ยวข้อง'`), and strips draft template notes from `Thai CVD Risk Score -4-` and `คะแนน ADL-4-`.
4. **Zero-Variance & Eligibility Filtering:** Identifies 6 uncollected survey columns (100% `'ไม่เกี่ยวข้อง'`) and enforces explicit age filters (`อายุ >= 15`, `อายุ >= 60`).
5. **Report Denominator Correction (`หัวข้อ 5 ครัวเรือน`):** Corrects Table 3 vector-elimination percentages from an erroneous denominator of `25` (summing to 128%) to the true affected household count of `32` (`62.5%` no action vs. `37.5%` active elimination).

---

## 🎯 5-Pillar Community Health Action Plan

| Pillar | Target Cohort | Actionable Intervention |
| :--- | :--- | :--- |
| **1. Geriatric Fall & Dementia Care** | **11** dementia & **18** faller seniors without caregivers; **59** knee OA cases | Enroll unsupervised high-FRAT ($\ge 4$) and dementia seniors into Long-Term Care (LTC) home visits; install grab bars/non-slip mats; run balance clinics. |
| **2. Low-Sodium, Zero-Sugar Nutrition** | **145** strong-seasoning eaters; **133** regular sugary drink users | Replace daily `น้ำหวาน` with plain/infused water; host budget-friendly boiled/steamed cooking workshops for low-income homes (`<9,200` THB/mo). |
| **3. Mental Health & Family Safety** | **40** `2Q` positive; **164** insomnia; **8** domestic violence households | Conduct `9Q` clinical follow-ups and sleep-hygiene workshops; establish confidential social-work & micro-financial referrals via MSDHS (`พม.`). |
| **4. Vector Control & Waste Sorting** | **25** untreated vector homes; **48** improper waste-sorting homes | Distribute larvicidal sand (`ทรายอะเบท`) and clear stagnant water door-to-door; install color-coded 3-bin community waste stations. |
| **5. Pediatric Dental & Early Growth** | **28** youth with dental caries; **3** early-childhood developmental flags | Organize school/community fluoride varnish & dental checkups; refer developmental delays to early-intervention child clinics. |

---

## 🚀 Getting Started

### Prerequisites

Ensure you have **Python 3.10+** installed along with the required data science libraries:

```bash
pip install numpy pandas scipy matplotlib openpyxl jupyter
```

### Running & Presenting the Notebook

1. Place `community_file Approve@Send.xlsx` and `community_health_project_presentation.ipynb` in the same directory.
2. Launch JupyterLab or Jupyter Notebook:
   ```bash
   jupyter lab community_health_project_presentation.ipynb
   ```
3. **Slide Presentation Mode:**
   * All markdown narrative slides, HTML summary tables, statistical readouts, and charts are **pre-rendered** inside the notebook for immediate presentation in **JupyterLab** (*View → Activate Presentation Mode*), **VS Code**, **Google Colab**, or **GitHub**.
   * To re-run the analysis live, click **Run → Run All Cells** (the first code cell automatically repairs the Excel XML in memory).