# AI Usage — Scholera Take-Home Assignment

## Purpose

AI tools were used as an assistive tool during the preparation of this take-home assignment. The final analysis was kept source-grounded: the supplied Scholera assignment brief, `lecture.json`, and `question-sets.json` were treated as the authoritative materials for Part 1.

The purpose of using AI was to improve efficiency in organizing the evaluation, checking consistency in the scoring rubric, identifying candidate evidence in the supplied files, and improving the clarity of the written report. AI was not treated as an independent source of truth.

## How AI was used

### 1. Understanding the assignment

AI was used to help interpret the Part 1 requirements and turn them into an explicit evaluation workflow.

The resulting workflow was:

1. Define what makes a generated quiz question useful.
2. Define question-level scoring dimensions.
3. Score all questions consistently.
4. Evaluate the generated sets for coverage, redundancy, and yield.
5. Compare the three strategies.
6. State a provisional recommendation.
7. Identify limitations and evidence that could change the recommendation.
8. Propose a follow-up experiment.

### 2. Building the scoring rubric

AI was used to help formulate operational definitions for:

- Grounding
- Correctness
- Cognitive demand
- Clarity
- Coverage
- Redundancy
- Question yield

The final rubric was intentionally kept simple rather than introducing unsupported composite scores or advanced statistics.

For question-level metrics, the final scales are:

| Metric | Scale |
|---|---|
| Grounding | 0–2 |
| Correctness | 0–2, with N/A where the lecture cannot establish correctness |
| Cognitive demand | 0–2 |
| Clarity | 0–2 |

Coverage, redundancy, and question yield are evaluated at the question-set level.

### 3. Scoring individual questions

AI was used to help identify relevant evidence in the supplied lecture and to suggest initial scores.

The scores were then checked against the actual source material. In particular, questions identified as potentially unsupported were explicitly compared with the lecture rather than being accepted because their answers were generally plausible.

Examples of source checks included:

- A04: the lecture says Adam is widely used but is not always better than well-tuned SGD with momentum.
- A07: the lecture describes mini-batch sizes as a range and discusses a trade-off; it does not recommend 256 as the best batch size for image classification.
- A11: the lecture discusses backpropagation but does not provide a publication year.

These checks are important because the evaluation is about whether questions are supported by the professor's lecture, not whether the proposed answers happen to be true according to outside knowledge.

### 4. Cognitive-demand scoring

AI was used to help distinguish between:

- direct recall,
- explanation/interpretation,
- application/reasoning.

The final classification was checked question by question. A question was not assigned a high cognitive-demand score merely because it contained technical terminology. The score reflects what the student actually has to do to answer it.

For example:

- A06 asks for the typical value of beta and is classified as recall.
- A05 asks why vanishing gradients worsen with depth and is classified as explanation.
- C07 asks the student to distinguish two training-failure patterns and is classified as application/reasoning.

### 5. Set-level analysis

AI was used to help group questions into conceptual clusters and identify patterns in coverage and redundancy.

These observations were then checked against the actual question IDs and lecture topics.

For example, Strategy B contains many questions about sigmoid derivatives, vanishing gradients, and ReLU. This was treated as a qualitative concentration/redundancy finding rather than converted into an arbitrary numerical score.

### 6. Writing and editing

AI was used to help organize and edit the report for clarity, conciseness, and professional presentation.

The report's conclusions were based on the supplied data and the defined scoring method. AI-generated wording was reviewed so that it did not introduce claims unsupported by the assignment materials.

## How AI outputs were checked

The most important safeguard was source verification.

For every substantive claim about a question's quality, the question was checked against the supplied `lecture.json`.

The following rules were used:

1. **Do not use outside knowledge to make a question appear grounded.**
2. If the lecture directly supports the question and answer, grounding is high.
3. If the lecture contradicts the claim, grounding is low.
4. If the lecture does not contain enough information, the question is not treated as source-grounded.
5. Correctness can be marked `N/A` when the supplied lecture cannot establish whether the proposed answer is correct.
6. Set-level observations such as coverage and redundancy are based on the actual question distribution, not on assumptions about how the strategies should behave.

## Known limitations of AI assistance

AI assistance can introduce several risks:

- It may overestimate how well a question is supported by a source.
- It may use general knowledge when the task requires source-only reasoning.
- It may classify cognitive demand inconsistently.
- It may make a plausible interpretation sound more certain than the evidence warrants.
- It may encourage unnecessary numerical summaries.

These risks were addressed by keeping the supplied files as the source of truth and by checking substantive judgments against the lecture.

## Reproducibility

The repository contains the question-level scoring sheet used for Part 1.

The scoring sheet records:

- Question ID
- Strategy
- Question text
- Proposed answer
- Grounding score
- Correctness score
- Cognitive-demand score
- Clarity score
- Evidence/reason for the judgment

This makes the aggregate observations traceable back to individual questions.

## Final principle

AI was used as an assistant for organization, consistency checking, and writing—not as the authority for the evaluation.

The final recommendation is intentionally provisional. It is based only on what the supplied dataset can support, and the report explicitly states what additional evidence would be required to change or strengthen the recommendation.
