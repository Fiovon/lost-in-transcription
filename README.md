# Lost in Transcription: Underlining, Meaning, and the Limits of Handwritten Text Recognition in Norwegian Personal Letters

Code and data repository for a bachelor thesis in Archival and Information Science, OsloMet, 2026.

This repository contains the detection scripts, letter images, XML transcriptions, ground truth annotations, and results used to investigate whether vision-based multimodal AI can detect underlining omitted from HTR transcriptions of Norwegian handwritten personal letters.

## Dataset

Letters are drawn from the **NorHand dataset**, published openly by the National Library of Norway.

- **Citation:** Maarand et al. (2022). https://doi.org/10.5281/zenodo.10255840
- **Licence:** Creative Commons Attribution (CC-BY)

## Repository Structure

```
├── letter-analyser/
│   └── Letters/                    # Letter images (.jpg) and PAGE XML transcriptions (.xml)
├── analyse_letters_claude.py       # Detection script — Claude Opus 4.7 (Anthropic)
├── compare_results.py              # Compares detection results against ground truth
├── convert_groundtruth.py          # Converts ground truth spreadsheet to JSON
├── ground_truth.csv                # Manual annotation ground truth (spreadsheet)
├── ground_truth.json               # Manual annotation ground truth (JSON)
├── results_claude.json             # Detection results from Claude Opus 4.7
├── comparison_claude.json          # Accuracy comparison against ground truth
├── combined_results.xlsx           # Ground truth and detection results combined
├── detection_results.xlsx          # Detection results summary and per-letter breakdown
├── ground_truth_styled.xlsx        # Manual annotation ground truth (styled)
├── charts.xlsx                     # Score distribution visualisations
├── worst_performing/               # The 5 worst-performing letters for analysis
├── requirements.txt                # Python dependencies
├── LICENSE                         # CC-BY 4.0 licence
└── README.md                       # This file
```

## Method

Each letter image is divided into six horizontal strips, allowing the model to examine a smaller, more detailed portion of the letter at a time. This improves detection of subtle underlinings and reduces false positives compared to sending the full image. Plain text is extracted from the PAGE XML files and provided to the model as a spelling reference.

Detection was performed using **Claude Opus 4.7** (`claude-opus-4-7`) by Anthropic.

## Results

Detection against 89 manually annotated underlinings across 40 letters:

| Metric | Score |
|--------|-------|
| Precision | 0.725 |
| Recall | 0.742 |
| F1 Score | 0.733 |

18 of 40 letters (45%) achieved perfect detection (F1 = 1.0).

## Setup

Install dependencies:

```bash
pip install -r requirements.txt
```

Create a `.env` file with your Anthropic API key:

```
ANTHROPIC_API_KEY=your-key-here
```

**Never commit your `.env` file** — it is listed in `.gitignore`.

## Running the Detection

```bash
python3 analyse_letters_claude.py --folder ./letter-analyser/Letters --output results_claude.json
```

## Evaluating Results

Convert your ground truth spreadsheet to JSON:

```bash
python3 convert_groundtruth.py --input ground_truth.csv --output ground_truth.json
```

Compare detection results against ground truth:

```bash
python3 compare_results.py --ground_truth ground_truth.json --results results_claude.json --output comparison_claude.json
```

## Output Format

Each detected underlining is returned as a JSON object:

```json
{
  "text": "en Deel",
  "spans_multiple_words": true,
  "spans_line_break": false,
  "position": "strip 3",
  "confidence": "certain"
}
```

## Licence

CC-BY 4.0 — see `LICENSE`. Consistent with the NorHand dataset licence.

## Acknowledgements

Detection scripts were developed with coding assistance from Claude Sonnet 4.6 (Anthropic) via claude.ai. Underlining detection was performed using Claude Opus 4.7 (Anthropic) via the Anthropic API. All methodological decisions were made by the author.

## Author

Fiona Vonholm
Bachelor thesis in Archival Science
OsloMet, 2026
