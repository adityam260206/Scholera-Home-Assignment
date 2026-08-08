# AI Usage — Scholera Take-Home Assignment

## Purpose

AI tools were used as an assistive tool throughout this take-home assignment. I used AI for research organization, source analysis, checking reasoning, identifying potential issues, and improving the clarity of the written deliverables.

I did not treat AI output as an independent source of truth. The supplied Scholera materials were treated as the authoritative evidence for Parts 1 and 2. For Part 3, external product documentation was used as the evidence base for competitor claims.

A central principle throughout the assignment was:

> AI can help me analyze evidence, but it should not replace checking the evidence.

---

# Part 1 — Evaluation of Quiz Generation Strategies

## How AI was used

AI was used to help interpret the assignment requirements and organize the evaluation into a repeatable workflow:

1. Define what makes a generated quiz question useful.
2. Define question-level scoring dimensions.
3. Score the generated questions consistently.
4. Evaluate the question sets for coverage, redundancy, and yield.
5. Compare the three strategies.
6. Form a provisional recommendation.
7. Identify limitations and evidence that could change the recommendation.
8. Propose a follow-up evaluation.

AI was also used to help formulate operational definitions for:

* Grounding
* Correctness
* Cognitive demand
* Clarity
* Coverage
* Redundancy
* Question yield

The final rubric was kept deliberately simple rather than introducing an unsupported composite score or unnecessary statistical methods.

## Scoring individual questions

AI was used to help identify relevant evidence in `lecture.json` and to suggest initial scores.

The scores were then checked against the actual lecture.

This distinction was particularly important for grounding. A question was not considered grounded simply because its answer was generally true according to outside knowledge. The question had to be supported by the supplied lecture.

Examples of checks included:

* **A04:** The question claimed that Adam was the best choice for all deep learning tasks. The lecture instead says Adam is widely used but is not always better than well-tuned SGD with momentum.
* **A07:** The question claimed that batch size 256 was best for image classification. The lecture discusses mini-batch sizes as a range and describes a trade-off, but does not establish 256 as the best choice.
* **A11:** The question asks when backpropagation was first published, but the supplied lecture does not provide a publication year.

These checks helped distinguish "generally correct according to outside knowledge" from "supported by the professor's lecture."

## Cognitive-demand scoring

AI was used to help distinguish:

* Direct recall
* Explanation/interpretation
* Application/reasoning

The final classification was checked question by question.

A question was not given a high cognitive-demand score simply because it contained technical terminology. The score was based on the task the student actually had to perform to answer it.

For example:

* A question asking for the typical momentum coefficient is recall.
* A question asking why vanishing gradients worsen with depth requires explanation.
* A question asking a student to distinguish between two training-failure patterns requires application/reasoning.

## Set-level analysis

AI was used to help identify conceptual clusters and potential redundancy across the generated sets.

These observations were checked against the actual question IDs and lecture topics.

For example, Strategy B contained a strong concentration of questions around sigmoid derivatives, vanishing gradients, and ReLU. I treated this as a qualitative concentration/redundancy finding rather than inventing an arbitrary numerical redundancy score.

## How AI outputs were checked

The supplied lecture and question sets were the source of truth.

The main rules were:

1. Do not use outside knowledge to make a question appear grounded.
2. Check substantive question judgments against `lecture.json`.
3. Treat contradictions with the lecture as grounding failures.
4. If the lecture does not contain enough information, do not assume that the question is supported.
5. Use `N/A` for correctness when the supplied material cannot establish correctness.
6. Base coverage and redundancy observations on the actual questions rather than assumptions about the strategies.

## A concrete example where AI was wrong

During the initial analysis, AI produced incorrect aggregate cognitive-demand averages for Strategies A and B because some question-level classifications were initially grouped incorrectly.

The first draft reported:

* Strategy A: 0.25
* Strategy B: 0.50

I caught this by recalculating the aggregates directly from the question-level scoring sheet rather than trusting the generated summary.

The final values were:

* Strategy A: **0.083**
* Strategy B: **0.417**
* Strategy C: **1.250**

This was an important check because it demonstrated that plausible-looking aggregate statistics from an LLM should not be accepted without recalculating them from the underlying observations.

## Part 1 writing

AI was also used to help organize and edit the Part 1 report for clarity and conciseness.

The recommendation remained deliberately provisional. The final conclusion was based on the scoring sheet and the supplied material rather than on AI preference.

---

# Part 2 — Internal Study Memo

## How AI was used

AI was used to help read and structure the internal study in `study-results.md`.

The main questions used to organize the analysis were:

1. What does the study directly show?
2. What conclusions require assumptions?
3. What does the study fail to establish?
4. What confounding factors could affect the interpretation?
5. What decision does the evidence justify?
6. What experiment would reduce the remaining uncertainty?

AI was particularly useful for separating descriptive observations from causal claims.

## Source-grounded analysis

The study was treated as the authoritative source.

The analysis identified several direct observations:

* Treatment sections had higher observed on-time submission rates.
* The headline difference was 7.4 percentage points.
* Usefulness among respondents averaged 4.1/5.
* The graduate treatment group was very small.
* Two treatment sections also introduced weekly quizzes.
* One control professor was on leave for part of the trial.
* Primer timing was not controlled.
* Usefulness ratings were voluntary.
* Primer opens/actual consumption were not measured.

The memo therefore avoids describing the 7.4 percentage-point difference as a proven causal effect.

## Challenging the proposed recommendation

The internal study proposed prioritizing primers for graduate sections and potentially charging more for a graduate tier.

AI helped identify this as a claim that required additional scrutiny rather than something to repeat automatically.

The graduate treatment group contained only four students. The observed graduate difference was therefore treated as an interesting signal, not sufficient evidence for graduate-specific prioritization or pricing.

This was an important part of the analysis because the assignment explicitly asks what the data supports and what it does not.

## Proposed next experiment

AI was used to help formulate a more actionable next step rather than simply writing "more research is needed."

The recommendation was to run a larger randomized follow-up with:

* Treatment and control sections
* On-time submission as the primary outcome
* Primer-open/engagement measurement
* Tracking of professor, course, and section-level factors
* Tracking of other interventions such as quizzes
* Usefulness ratings as a secondary measure

The purpose would be to determine whether receiving a primer itself improves student outcomes.

## Part 2 writing

AI was used to help organize and edit the memo.

The final claims were checked against `study-results.md`, and the recommendation was intentionally written conservatively so that the memo did not imply that the internal study established more than it actually did.

---

# Part 3 — Competitor Analysis

## How AI was used

AI was used as a research assistant to identify relevant competitor capabilities, organize the comparison, and help distinguish product claims from evidence.

Two competitors were selected:

* Quizlet
* Khanmigo

The selection was based on relevance to AI-assisted educational content and question/assessment generation.

## External research and verification

Unlike Parts 1 and 2, Part 3 required external research.

AI was used to locate and summarize publicly available product documentation and information from the competitors' websites.

Competitor capabilities were not treated as facts simply because an AI response stated them. Relevant product claims were checked against the cited external sources before being included in the final write-up.

Where a claim was based on a company's own product description, it was presented as a description of the product rather than independent evidence that the product achieves a particular educational outcome.

## Separating capability from effectiveness

A major analytical distinction in Part 3 was:

> A product having a question-generation feature does not establish that its generated questions are consistently high quality.

For example, the analysis distinguishes between:

* generating questions,
* generating questions from supplied educational material,
* and reliably generating questions that are correct, grounded, appropriately difficult, unambiguous, and useful for assessment.

This distinction was important because the assignment asks where competitor marketing may be ahead of what the software or available evidence establishes.

## Part 3 writing

AI was used to help structure and edit the competitor analysis.

The final recommendation for Scholera was connected to the evidence from Part 1 rather than presenting an unrelated product strategy.

The proposed differentiation is not simply "generate more questions." Instead, it focuses on trustworthy, lecture-grounded assessment generation, with measurable outcomes such as:

* Grounded-question rate
* Instructor acceptance rate
* Rejection rate
* Question diversity
* Time saved per accepted question

---

# Known limitations of AI assistance

Using AI introduced several risks throughout the assignment:

* It can make unsupported claims sound plausible.
* It can use general knowledge when the task requires source-only reasoning.
* It can miscalculate aggregate statistics.
* It can classify qualitative judgments inconsistently.
* It can overstate the strength of evidence.
* It can turn a reasonable inference into a statement that sounds like a fact.
* It can summarize competitor marketing without sufficiently distinguishing claims from independently verified outcomes.

The main mitigation was to keep the relevant source material as the source of truth and manually verify substantive claims.

For Part 1, this meant checking questions against `lecture.json` and recalculating aggregate metrics from the scoring sheet.

For Part 2, this meant checking conclusions against `study-results.md`.

For Part 3, this meant checking competitor capabilities against external product documentation.

---

# What AI contributed vs. what remained my responsibility

AI was most useful for:

* Structuring ambiguous tasks
* Suggesting evaluation dimensions
* Finding potential evidence
* Identifying possible confounders
* Organizing comparisons
* Improving clarity and conciseness
* Challenging an initial interpretation

My responsibility remained:

* Deciding which evidence was actually relevant
* Checking source support
* Recalculating quantitative results
* Deciding how much confidence the evidence deserved
* Rejecting unsupported conclusions
* Making the final recommendations
* Reviewing the final written submission

The final submission therefore should not be interpreted as an AI-generated evaluation accepted without review. AI was used as an assistant, while the underlying evidence and final judgments were checked against the relevant sources.

## Final principle

The most important lesson from using AI on this assignment was that an AI system can produce a very convincing answer even when one of its intermediate judgments is wrong.

For that reason, I treated AI as a tool for accelerating analysis rather than as the authority for the analysis.

The standard I used throughout was:

> **Use AI to help reason about the evidence, but make the evidence responsible for the conclusion.**
