🧬 PyKinshipID: DVI Screening Tool
Automated STR Profile Analysis for Disaster Victim Identification





📋 Overview
PyKinshipID is an open-source screening tool designed for Disaster Victim Identification (DVI) that performs automated analysis of STR (Short Tandem Repeat) DNA profiles. The tool identifies kinship relationships, detects duplicate profiles, flags mixtures, and classifies unrelated individuals using Mendelian inheritance principles.
🔬 Key Features
✅ Mixture Detection - Identifies profiles with >2 alleles at any locus
✅ Duplicate Profile Detection - Finds identical DNA profiles in the dataset
✅ Trio Relationship Screening - Identifies father-mother-child relationships
✅ Duo Relationship Screening - Identifies parent-child or sibling pairs
✅ Unrelated Individual Classification - Profiles not matching any relationship
✅ Multi-Kit Support - Auto-detects 6 major commercial STR kits
✅ Multi-Sheet Excel - Processes multiple sheets from different kits
✅ Column Normalization - Auto-corrects common column name variations
✅ Interactive HTML Reports - Professional reports with PDF export
✅ Google Colab Ready - No local installation required
📦 Installation & Usage
Option 1: Google Colab (Recommended)
No installation required! Simply:
Open in Google Colab: Upload the PyKinshipID.ipynb notebook to Google Colab
Click the Play button (▶) at the top of the cell
Wait for initialization (~10 seconds)
Upload your data or use the built-in sample dataset
Set mismatch threshold (0-2, default: 1)
Click "🔬 Run Analysis"
Download the HTML report and convert to PDF
Option 2: Local Installation
bash
123456789
📊 Data Format Requirements
Required Columns
Your Excel (.xlsx) or CSV file must contain:
Column
Description
Example
SampleID
Unique identifier for each profile
Victim_001, Ref_Mother_01
Amelogenin
Sex marker
X,Y (Male), X,X (Female)
15 STR Loci
Core IDENTIFILER PLUS loci
See below
STR Loci (15 Core Markers)
123
Allele Format
Comma-separated: 13,14 or 9.3,10
Microvariants supported: 9.3, 24.2, etc.
Failed loci: Leave blank (do NOT use 0,0)
Case-insensitive: Column names can be in any case
Example Data Structure
SampleID
Amelogenin
D8S1179
D21S11
D7S820
...
FGA
Victim_001
X,Y
13,14
26,28
10,11
...
19,23
Ref_Mother_01
X,X
13,15
25,38
11,12
...
34,37
Ref_Father_01
X,Y
10,14
26,26
10,11
...
19,23
🧪 Supported STR Kits
PyKinshipID automatically detects and supports:
Kit
Manufacturer
Notes
IDENTIFILER PLUS
Thermo Fisher Scientific
Standard 15-locus panel
GlobalFiler
Thermo Fisher Scientific
All 15 core loci + extras
PowerPlex Fusion
Promega
All 15 core loci + Penta D/E
PowerPlex 16
Promega
⚠️ Missing D2S1338 & D19S433
Investigator 24plex QS
QIAGEN/Verasyte
All 15 core loci + SE33
NGM SElect
Thermo Fisher Scientific
All 15 core loci + SE33
Note: Extra loci beyond the core 15 are automatically ignored. PowerPlex 16 users will receive warnings about reduced statistical support due to missing loci.
🎯 How It Works
Analysis Workflow
Data Loading & Normalization
Reads Excel/CSV files
Auto-detects STR kit per sheet
Normalizes column names
Validates data format
Quality Control
Detects mixture profiles (>2 alleles)
Identifies duplicate SampleIDs
Flags conflicting data across sheets
Relationship Screening
Trio Analysis: Tests all possible father-mother-child combinations
Duo Analysis: Identifies parent-child or sibling pairs
Mismatch Threshold: Allows 0-2 mismatches (default: 1)
Report Generation
Interactive HTML report
Performance metrics
Kit compatibility notes
Cross-sheet relationship flags
PDF export capability
Mendelian Mismatch Counting
The tool uses strict Mendelian inheritance rules:
Child alleles must be explainable by one allele from each parent
Mismatches indicate potential mutations, allelic dropout, or non-relationship
Threshold of 1 mismatch is standard DVI practice
📥 Output & Reports
Generated Report Includes:
Executive Summary: Total profiles, relationships detected, analysis time
Performance Metrics: Sheets processed, loci analyzed, kit detection
Mixture Profiles: Flagged samples with extra alleles
Duplicate Sets: Identical profiles found
Trio Relationships: Father-Mother-Child with mismatch details
Duo Relationships: Profile pairs with kinship indicators
Unrelated Individuals: Profiles not matching any relationship
Kit Compatibility Notes: Warnings for incomplete kits
Cross-Sheet Flags: Relationships spanning multiple Excel sheets
Report Features:
✅ Print-friendly CSS - Optimized for PDF export
✅ Collapsible sections - Easy navigation
✅ Color-coded results - Visual status indicators
✅ Gender markers - Male/Female/Uncertain badges
✅ Mismatch details - Specific loci with inconsistencies
⚙️ Configuration
Mismatch Threshold Settings
Value
Description
Use Case
0
Strict Mendelian match
High-quality reference samples
1
Allows 1 mismatch
Standard DVI practice (recommended)
2
Allows 2 mismatches
Degraded samples, potential mutations
Advanced Options
Multi-sheet processing: Place different kits on separate Excel sheets
Column aliases: Tool auto-corrects common variations (e.g., AMEL → Amelogenin)
SampleID duplicates: Last occurrence is used (warning issued)
🔒 Privacy & Data Security
No data upload: All processing occurs locally in your browser/Colab session
No external servers: Data never leaves your computer
Session isolation: Each Colab user gets an isolated environment
Wipe function: Built-in tool to clear all data and variables
Recommendation: Use "Runtime → Restart Runtime" for complete data clearing
⚠️ Disclaimer
IMPORTANT: PyKinshipID is a preliminary screening tool only.
Results are based on Mendelian mismatch counting and do NOT incorporate likelihood ratios or population allele frequencies
All positive kinship findings MUST be confirmed by a qualified forensic geneticist using validated LR-based software (e.g., Familias, DNA-View, or commercial DVI software)
NOT for legal/forensic reporting without proper validation and confirmation
For research and screening use only
The developers assume no liability for misuse or misinterpretation of results
📚 Citation
If you use PyKinshipID in your research, please cite:
bibtex
12345678910
DOI: 10.5281/zenodo.XXXXXXX (Will be updated after Zenodo publication)
🤝 Contributing
Contributions are welcome! Please feel free to submit a Pull Request.
Bug Reports & Feature Requests
Report bugs via GitHub Issues
Include: STR kit used, error message, sample data (anonymized)
Suggest features for future versions
📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
Copyright (c) 2025 [Your Name]
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software.
📞 Support & Contact
GitHub Issues: Report bugs or request features
Email: your.email@institution.edu
Documentation: See the built-in sample dataset and format guide in the tool
🙏 Acknowledgments
INTERPOL DVI Standards - For kinship analysis guidelines
ISFG Recommendations - For DNA profiling in disaster victim identification
Google Colab - For free cloud computing resources
Open-source community - For pandas, ipywidgets, and other dependencies
📝 Version History
v1.0.0 (2025)
Initial public release
Support for 6 major STR kits
Multi-sheet Excel processing
Interactive HTML reports
Mixture and duplicate detection
Trio and duo relationship screening
Built with ❤️ for the forensic genetics community
Last updated: [UPDATE WITH TODAY'S DATE]
