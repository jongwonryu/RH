# Representativeness Heuristics in Large Language Models

**A Study on the Representativeness Heuristics Problem in Large Language Models**

[![Paper](https://img.shields.io/badge/Paper-IEEE_Access_2024-00629B)](https://doi.org/10.1109/ACCESS.2024.3474677)

**Jongwon Ryu, Jungeun Kim, and Junyeong Kim**

Dataset release accompanying [the paper](https://doi.org/10.1109/ACCESS.2024.3474677),
which studies representativeness-heuristic errors in LLM reasoning and introduces
task-specific zero-shot-RH prompts.

## Overview

The representativeness heuristic can lead a model to favor a stereotypical
description over probability or set-inclusion constraints. The benchmark examines
two related reasoning failures:

- **Conjunction fallacy:** judging a conjunction as more probable than one of its
  constituent events.
- **Base-rate neglect:** overlooking population frequencies when interpreting a
  characteristic associated with a group.

## Dataset

The original `RH_dataset.xlsx` workbook is preserved without modification.
Its `Sheet2` worksheet contains **230 questions**:

| Category ID | Task | Questions |
| --- | --- | ---: |
| 1 | Conjunction fallacy | 130 |
| 2 | Base-rate neglect | 100 |
| Total | | 230 |

| Column | Description |
| --- | --- |
| `no` | Question number within its category |
| `category` | Category ID, as defined above |
| `question` | English question with A/B alternatives |

Question numbers restart for the second category; use `(category, no)` as an
identifier. The worksheet also contains a blank separator row and unused columns,
which the example loader ignores.

The workbook does **not** contain reference answers, reasoning annotations, or
model predictions. The original study assessed both answers and reasoning;
this question-only release should not be described as a complete automated
evaluation package.

## Quick Start

Use Python 3.10 or newer:

```bash
git clone https://github.com/jongwonryu/RH.git
cd RH
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Inspect the dataset and its first five questions.
python examples/load_dataset.py --head 5

# Export all questions without altering the original workbook.
python examples/load_dataset.py --output outputs/questions.jsonl
python examples/load_dataset.py --output outputs/questions.csv

# Export a single task.
python examples/load_dataset.py --category 1 --output outputs/conjunction.jsonl
```

For use in another Python program:

```python
from examples.load_dataset import load_questions

questions = load_questions("RH_dataset.xlsx")
print(questions[0])
```

## Prompting

The paper compares standard zero-shot prompting, zero-shot chain-of-thought,
and the proposed zero-shot-RH prompts. The RH instruction depends on the task:

| Task | Zero-shot-RH instruction |
| --- | --- |
| Conjunction fallacy | Considering the inclusion relationship between A and B. |
| Base-rate neglect | Considering the probability of A and B. |

These instructions are appended to the question. See the paper for the original
model settings and human assessment protocol; loading the workbook alone does
not reproduce the reported experiments.

## Citation

```bibtex
@article{ryu2024representativeness,
  title   = {A Study on the Representativeness Heuristics Problem in Large Language Models},
  author  = {Ryu, Jongwon and Kim, Jungeun and Kim, Junyeong},
  journal = {IEEE Access},
  volume  = {12},
  pages   = {147958--147966},
  year    = {2024},
  doi     = {10.1109/ACCESS.2024.3474677}
}
```

## Contact

Jongwon Ryu: `fbwhddnjs511@cau.ac.kr`.

No code or dataset license has been specified for this repository. The paper's
publication license does not automatically license the workbook or example code.
