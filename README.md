# Scholera Take-Home Assignment — AI/ML Research Intern

This repository contains my submission for the Scholera AI/ML Research Intern take-home assignment.

The assignment asks for a decision about AI-generated educational content based on evidence rather than intuition. My approach throughout the submission was to distinguish what the available data establishes from what it only suggests.

## Submission

### Part 1 — Quiz Generation Strategy Evaluation

**Location:** `Part1/`

I evaluated three quiz-generation strategies:

* **Strategy A — Full Context**
* **Strategy B — Retrieval**
* **Strategy C — Generate → Verify**

I evaluated questions using four question-level dimensions:

* Grounding
* Correctness
* Cognitive demand
* Clarity

I also considered set-level:

* Coverage
* Redundancy
* Question yield

### Conclusion

Strategy C — **Generate → Verify** — is my leading candidate for further development.

In the evaluated sample, Strategy C achieved:

* **100% strongly grounded questions**
* **Highest cognitive-demand score: 1.250**
* More explanation/application-oriented questions than Strategies A and B

Strategy B also achieved 100% strong grounding, but its questions were heavily concentrated around related concepts such as sigmoid derivatives, vanishing gradients, and ReLU.

Strategy A produced broad coverage and high yield, but contained weaker grounding examples and was overwhelmingly recall-oriented.

I do **not** consider the experiment large enough to establish that Strategy C is universally superior or production-ready. A larger evaluation across multiple lectures, with independent human evaluation and operational measures such as verification yield, latency, cost, and downstream student outcomes, would be needed before making a production decision.

The detailed scoring methodology and question-level analysis are in `Part1/`.

---

## Part 2 — Internal Study Memo

**Location:** `Part2/Part2_Memo.md`

I reviewed the four-week internal study of Scholera's pre-class primer feature.

### Conclusion

The study provides an encouraging signal: treatment sections had a **7.4 percentage-point higher observed on-time submission rate**, and usefulness among respondents averaged **4.1/5**.

However, the study does not establish that primers caused the improvement.

Potential confounders include:

* Weekly quizzes introduced in two treatment sections
* A professor being on leave in one control section
* Uncontrolled differences in primer timing and professor engagement
* Voluntary usefulness ratings
* No measurement of whether students actually opened the primer

The graduate result is particularly uncertain because the treatment group contained only four students.

My recommendation is to **continue testing the feature but not yet make graduate-specific rollout or pricing decisions**. The next step should be a larger randomized follow-up that measures both the primary student outcome and actual primer engagement.

---

## Part 3 — Competitor Analysis

**Location:** `Part3/Part3.md`

I compared:

* **Quizlet**
* **Khanmigo**

Both demonstrate that AI-assisted question generation is already available in established education products.

My main conclusion is that Scholera should not differentiate simply by generating more questions or claiming a more powerful model.

Instead, the stronger opportunity is:

> **Trustworthy, lecture-grounded assessment generation.**

Part 1 provides an initial reason to investigate this direction: Generate → Verify performed best on grounding and cognitive demand in the evaluated sample.

I would measure this product direction using metrics such as:

* Grounded-question rate
* Instructor acceptance rate
* Rejection rate
* Question diversity
* Time saved per accepted question

---

## AI Usage

**Location:** `AI_USAGE.md`

AI tools were used throughout the assignment for research assistance, organization, analysis support, source identification, and writing/editing.

AI was **not treated as an independent source of truth**.

Examples of verification included:

* Rechecking question judgments against `lecture.json`
* Recalculating Part 1 aggregate scores from the underlying scoring sheet
* Checking Part 2 conclusions against `study-results.md`
* Verifying Part 3 competitor capabilities against external product documentation

A concrete example of AI being wrong is documented in `AI_USAGE.md`: an initial AI-generated calculation produced incorrect aggregate cognitive-demand scores for Strategies A and B. I caught and corrected this by recalculating from the question-level scoring sheet.

---

## Repository Structure

```text
Scholera-Home-Assignment/
│
├── README.md
├── AI_USAGE.md
│
├── Part1/
│   ├── ...
│   └── AI_USAGE.md
│
├── Part2/
│   ├── study-results.md
│   └── Part2_Memo.md
│
└── Part3/
    └── Part3.md
```

`Part1/AI_USAGE.md` contains the earlier Part 1-specific AI-usage documentation. The root-level `AI_USAGE.md` is the consolidated record covering the complete assignment.

---

## Final Takeaway

Across all three parts, my main conclusion is that **AI-generated educational content should be evaluated by the quality of what can actually be trusted and used, not by generation capability alone**.

For quiz generation, this means evaluating grounding, correctness, cognitive demand, clarity, diversity, and yield.

For product experiments, this means distinguishing observed associations from causal evidence.

For competitors, this means distinguishing the ability to generate content from evidence that the generated content is educationally reliable.

My recommendations are therefore intentionally provisional where the available evidence is limited, with specific follow-up measurements proposed to resolve the remaining uncertainty.

---

## Video

**Presentation:** https://www.loom.com/share/11676815f776421cae9e220d884be67e

The video will cover:

1. Brief introduction
2. Part 1 methodology and conclusion
3. Part 2 evidence and recommendation
4. Part 3 competitor findings
5. The hardest analytical decision
6. Where AI tools helped and where they were wrong
7. What I would do next with another week
