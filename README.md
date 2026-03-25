# 🧬 PyKinshipID: DVI Screening Tool

**Automated STR Profile Analysis for Disaster Victim Identification**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/yourusername/PyKinshipID/blob/main/PyKinshipID.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

PyKinshipID is an interactive tool for forensic DNA analysts to rapidly screen large numbers of STR profiles for potential family relationships, duplicate samples, and mixtures. Designed for **Google Colab** (or Jupyter notebooks), it helps prioritize matches during mass‑fatality incidents while respecting forensic best‑practice workflows.

---

## 📖 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [How It Works](#how-it-works)
- [Supported STR Kits](#supported-str-kits)
- [Input Data Format](#input-data-format)
- [Usage Instructions](#usage-instructions)
- [Output Report](#output-report)
- [Limitations & Disclaimer](#limitations--disclaimer)
- [Citation](#citation)
- [License](#license)

---

## Overview

In disaster victim identification (DVI), analysts often have to compare hundreds or thousands of DNA profiles from victims, family members, and reference samples. Manually checking for Mendelian consistency is slow and error‑prone. PyKinshipID automates the initial screening:

- Detects **duplicate profiles** (exact matches across all analysed loci)
- Flags possible **mixture profiles** (more than two alleles at any locus)
- Identifies **trio relationships** (child + mother + father) and **duo relationships** (e.g., parent‑child, sibling) using simple Mendelian mismatch counting
- Lists **unrelated individuals** not placed in any relationship

> ⚠️ **Important:** The tool is **not** a likelihood‑ratio based kinship software. It provides a fast, conservative first pass to direct attention to promising matches. All positive findings **must** be confirmed with validated LR‑based tools (e.g., Familias, DNA‑View) before any formal identification is made.

---

## ✨ Key Features

- **Multi‑sheet Excel support** – upload a single file with profiles from different STR kits on separate sheets; PyKinshipID automatically merges them.
- **Automatic kit detection** – identifies common commercial STR kits (IDENTIFILER PLUS, GlobalFiler, PowerPlex Fusion, PowerPlex 16, Investigator 24plex QS, NGM SElect) and warns about missing core loci.
- **Column alias handling** – recognises common variations (e.g., `D21S11` or `D21`, `Amelogenin` or `AMEL`).
- **Mixture screening** – flags profiles with >2 alleles at any locus (simple indicator; two‑contributor mixtures that share alleles at all loci are **not** automatically detected).
- **Duplicate detection** – finds exact profile matches across the entire set of STR loci used.
- **Trio and duo screening** – uses Mendelian inheritance rules with a user‑defined mismatch tolerance (0–2). Gender information from Amelogenin is used to guide parent assignment in trios.
- **Rich HTML report** – collapsible sections with clear counts, warnings, and details. The report can be saved as PDF directly from the browser.
- **Session privacy** – all processing occurs locally in the Colab virtual machine. The “Wipe Session” button clears loaded data and results; for full assurance, restart the runtime.

---

## ⚙️ How It Works

1. **Data Loading**  
   The tool reads an Excel (`.xlsx`) or CSV (`.csv`) file. It identifies columns corresponding to the 15 core STR loci (as defined by the IDENTIFILER PLUS panel) and Amelogenin. Columns are normalised to standard names (`SampleID`, `Amelogenin`, `D8S1179`, … `FGA`).  
   If multiple sheets are present, each sheet is processed independently, and kit detection is performed per sheet. Profiles are then merged into a single list; conflicts (different allele values for the same SampleID) are reported.

2. **Mixture Flagging**  
   Any profile with more than two alleles at any of the 15 STR loci is flagged as a **mixture**. Such profiles are excluded from further relationship analysis.

3. **Duplicate Detection**  
   Profiles that are identical across all available loci (including Amelogenin) are grouped as duplicates. Only one copy is kept for the subsequent kinship analysis.

4. **Trio Screening**  
   For each child candidate, the tool builds a list of potential parents (profiles that satisfy duo compatibility with the child). Using gender information, it attempts to find a father and a mother. If no gender is available, it tries all combinations of two candidate parents. The first valid trio (mismatches ≤ threshold) is recorded.

5. **Duo Screening**  
   After all trios are extracted, the remaining profiles are scanned for pairs that satisfy duo compatibility (mismatches ≤ threshold). To avoid double counting, pairs already used in a trio are not considered.

6. **Unrelated Individuals**  
   All profiles not placed in any trio or duo are listed as unrelated/individuals.

---

## 🔬 Supported STR Kits

The tool automatically detects the kit used on each sheet based on the presence/absence of specific extra markers. It then warns if essential core loci are missing (e.g., D2S1338 and D19S433 in PowerPlex 16).

| Kit                     | Notes                                                                                     |
|-------------------------|-------------------------------------------------------------------------------------------|
| IDENTIFILER PLUS        | Full 15‑locus panel.                                                                      |
| GlobalFiler             | All core loci present. SE33 and DYS391 detected but ignored.                              |
| PowerPlex Fusion        | All core loci present. Penta D/E detected but ignored.                                    |
| PowerPlex 16            | ⚠️ Missing D2S1338 and D19S433 – highly discriminating loci. Use with caution.            |
| Investigator 24plex QS  | All core loci present. SE33 and other extra markers ignored.                              |
| NGM SElect              | All core loci present. SE33 ignored.                                                      |

If a kit is not recognised, the tool assumes it is IDENTIFILER PLUS and works with whatever core loci are present.

---

## 📥 Input Data Format

### Required Columns

- **SampleID**: unique identifier for each profile (no duplicate IDs within a sheet; duplicates across sheets trigger a warning).
- **Amelogenin** (optional): sex marker, expected as `X,X` (female) or `X,Y` (male). If absent, gender is marked “Unknown”.
- **15 core STR loci** (at least one must be present):  
  `D8S1179`, `D21S11`, `D7S820`, `CSF1PO`, `D3S1358`, `TH01`, `D13S317`, `D16S539`, `D2S1338`, `D19S433`, `vWA`, `TPOX`, `D18S51`, `D5S818`, `FGA`

### Allele Format

- **Two alleles** per locus, comma‑separated: e.g., `13,14` or `9.3,10`.
- **Homozygous** entries can be written as `13` or `13,13` (both accepted).
- **Microvariants** (e.g., `9.3`) are fully supported.
- **Missing data**: leave the cell **blank** (do **not** use `0,0` or `-,-`).

### Example Row

| SampleID      | Amelogenin | D8S1179 | D21S11 | … | FGA    |
|---------------|------------|---------|--------|---|--------|
| Victim_001    | X,Y        | 13,14   | 26,28  | … | 19,23  |
| Ref_Mother_01 | X,X        | 13,15   | 25,38  | … | 34,37  |

### Multi‑sheet Files

- Place profiles from different kits on separate sheets.  
- The tool will process each sheet independently, detect the kit, and merge all profiles.  
- Sheets without any recognised STR columns are ignored.

---

## 🚀 Usage Instructions

### Using Google Colab (Recommended)

1. Click the **Open In Colab** badge at the top of this README.
2. Once the notebook opens, run the first cell (the banner and imports will load automatically).
3. **Upload your data**  
   - Click the **Choose Files** button (the file upload widget).  
   - Select your `.xlsx` or `.csv` file.  
   - (Optional) Click **📦 Load Sample Data** to test the tool with a built‑in demo dataset.
4. **Set the mismatch threshold**  
   Use the slider to set the allowed number of Mendelian mismatches for a trio or duo:
   - `0` – strict Mendelian match (recommended for high‑quality data)
   - `1` – allows one mismatch (accounts for possible allelic dropout or mutation)
   - `2` – more permissive (may increase false positives)
5. **Run the analysis**  
   Click **🔬 Run Analysis**. A progress bar shows the current phase. For large datasets (>500 profiles) the analysis may take several minutes; keep the tab active.
6. **Review results**  
   The interactive HTML report appears. You can expand/collapse sections, and click the **Print / Save PDF** button to generate a PDF for archiving or sharing.
7. **Wipe session** (optional)  
   After finishing, click **🗑 Wipe Session** to clear all loaded data and results. For complete assurance, restart the Colab runtime (Runtime → Restart runtime).

### Running Locally (Jupyter Notebook)

If you prefer to run the tool in a local Jupyter environment:

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/PyKinshipID.git
   cd PyKinshipID
2.Install dependencies:

    pip install pandas ipywidgets openpyxl

3. Launch Jupyter:

       jupyter notebook PyKinshipID.ipynb

📊 Output Report
The generated HTML report contains:

Performance metrics – total profiles, loci used, analysis time, etc.

Mixture profiles – list of profiles with extra alleles (with locus details).

Duplicate sets – groups of identical profiles.

Trio relationships – each trio shows father, mother, child, mismatch count, and loci where mismatches occurred. Cross‑sheet warnings appear if profiles come from different sheets/kits.

Duo relationships – pairs with mismatch counts and details.

Unrelated individuals – all remaining profiles not placed in any relationship.

The report is self‑contained and can be saved as a PDF via the browser’s print dialog.

⚠️ Limitations & Disclaimer
Mendelian mismatch counting – The tool uses simple allele sharing rules, not likelihood ratios. It is a screening tool only.

Mixture detection – Only profiles with >2 alleles at any locus are flagged. Mixtures where contributors share alleles at all loci (e.g., parent‑child) are not automatically detected.

Parentage assignment – Gender from Amelogenin is used to assign mother/father in trios. If gender is uncertain, the tool attempts all combinations, which may produce biologically implausible assignments (e.g., two fathers).

Missing loci – Profiles with missing data may produce false negatives. The tool skips loci where data is missing for either member in a comparison.

PowerPlex 16 users – This kit lacks D2S1338 and D19S433, two highly discriminating loci. Results from such data have reduced statistical support. Confirmation with a full 15‑locus panel is strongly recommended.

No likelihood ratios – The output is not admissible as formal statistical evidence. All positive findings must be verified with validated LR‑based software (e.g., Familias, DNA‑View, or similar) before any identification is made.

📚 Citation
If you use PyKinshipID in your work, please cite:

PyKinshipID: Automated STR Profile Analysis for Disaster Victim Identification. (2025). Available at: https://github.com/mahalingaraja/PyKinshipID

📄 License
This project is licensed under the MIT License – see the LICENSE file for details.

For research and screening use only. Not validated for forensic casework without independent verification.
