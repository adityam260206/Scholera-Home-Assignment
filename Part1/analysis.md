# Part 1 — Evaluation of Quiz Generation Strategies

## 1. Executive Summary

This analysis evaluates three strategies for generating quiz questions from a professor's lecture material:

* **Strategy A — Full Context:** the complete lecture is provided to the model before question generation.
* **Strategy B — Retrieval:** relevant lecture passages are retrieved first and questions are generated from those passages.
* **Strategy C — Generate → Verify:** questions are generated first and then checked against the source, with unsupported questions discarded.

The evaluation uses the supplied CS 4780 Week 2 lecture, *Gradient Descent and Backpropagation*, and the corresponding generated question sets. The assignment asks for a decision about which strategy should be shipped, while explicitly emphasizing that the evidence should determine the recommendation rather than assuming a strategy is best in advance. The question sets also contain different numbers of questions, which is itself part of the evaluation.

I evaluated individual questions on **grounding, correctness, cognitive demand, and clarity**, and evaluated each question set additionally on **topic coverage, redundancy, and question yield**.

The results show three different trade-offs. Strategy A produced the broadest set with 12 questions, but three questions contained unsupported or unverifiable claims. Strategy B produced 12 strongly grounded questions, but the set was heavily concentrated around sigmoid derivatives, vanishing gradients, and ReLU. Strategy C produced only 8 questions, but all 8 were strongly grounded and the set had substantially higher cognitive demand and broader conceptual coverage.

Based on this evaluation, **Strategy C is the leading candidate for further development**, but I would not treat it as a definitive production decision. The evaluation covers only one lecture, and Strategy C produced fewer questions than the other strategies. The next experiment should therefore test whether C's quality advantage persists across multiple lectures and whether rejected questions can be regenerated without sacrificing quality or creating unacceptable generation cost.

---

## 2. Objective

The objective of this evaluation is to determine which quiz-generation strategy produces the most useful and reliable questions from a professor's lecture.

For this analysis, I interpret a useful generated question as one that:

1. Can be answered from the supplied lecture.
2. Has an answer supported by the lecture.
3. Tests an appropriate level of understanding rather than relying only on trivial recall.
4. Is clearly worded and unambiguous.
5. Contributes useful coverage of the lecture rather than repeatedly testing the same concept.
6. Provides sufficient usable questions for a practical quiz-generation system.

This interpretation follows the assignment's emphasis on whether a question is answerable from the lecture, whether its difficulty is sensible, whether it tests understanding rather than only recall, and how the evaluation can be performed without overclaiming from a small sample.

The evaluation is deliberately source-based. I do not use external information to make an unsupported question appear grounded in the lecture. If the lecture does not provide enough information to establish an answer, that limitation is recorded rather than silently filled using outside knowledge.

---

## 3. Strategies Evaluated

The supplied `question-sets.json` defines the three strategies as follows:

| Strategy                  | Generation approach                                                                                            | Questions produced |
| ------------------------- | -------------------------------------------------------------------------------------------------------------- | -----------------: |
| **A — Full Context**      | The entire lecture is placed in the model's context and the model is asked to generate questions.              |                 12 |
| **B — Retrieval**         | Relevant passages are retrieved first and questions are generated from those passages only.                    |                 12 |
| **C — Generate → Verify** | Questions are generated and then checked against the source; questions that cannot be supported are discarded. |                  8 |

The strategy definitions and question counts come directly from the supplied question-set data.

The unequal output size is treated as an evaluation dimension rather than normalized away. In particular, the lower output from Strategy C may represent a useful quality-control effect, but it may also create a practical limitation if too many questions are discarded.

---

## 4. Evaluation Methodology

### 4.1 Grounding

**Grounding measures whether the lecture provides sufficient evidence to support the question and its proposed answer.**

I use the following scale:

| Score | Definition                                                                                                      |
| ----: | --------------------------------------------------------------------------------------------------------------- |
| **0** | The lecture does not support the claim, or the question contradicts the lecture.                                |
| **1** | The lecture partially supports the question, but answering it requires substantial inference or interpretation. |
| **2** | The question and answer are directly supported by the lecture.                                                  |

The focus is on source-groundedness rather than general factual knowledge.

For example, A04 asks:

> "Which optimiser did the lecture recommend as the best choice for all deep learning tasks?"

with the answer "Adam."

However, the lecture states that Adam is widely used but **is not always better than well-tuned SGD with momentum**, particularly for very large models.

Therefore, A04 is scored as **0 for grounding**.

## Similarly, A07 asks for a specific batch size that gives the best convergence for image classification, with the answer 256. The lecture instead describes mini-batch sizes as ranging from a few dozen to a few hundred and presents batch size as a trade-off between gradient quality and hardware utilisation. It does not identify 256 as an optimal image-classification batch size.

### 4.2 Correctness

**Correctness measures whether the proposed answer is correct based on information available in the supplied lecture.**

I use:

|   Score | Definition                                                                |
| ------: | ------------------------------------------------------------------------- |
|   **0** | Incorrect or contradicts the lecture.                                     |
|   **1** | Partially correct or materially incomplete.                               |
|   **2** | Correct and supported by the lecture.                                     |
| **N/A** | The lecture does not contain enough information to establish correctness. |

The N/A category is important because the evaluation is source-based.

For example, A11 asks:

> "In which year was backpropagation first published?"

and provides "1986" as the answer.

The lecture explains backpropagation as systematic application of the chain rule but does not provide a publication year.

I therefore do **not** mark A11 as factually incorrect. Instead, its correctness is recorded as **N/A from the supplied source**, while its grounding is scored as 0.

This prevents external knowledge from being used to artificially improve or penalize a strategy's source-grounding performance.

---

### 4.3 Cognitive Demand

**Cognitive demand measures what the student must do to answer the question. It is not simply a measure of how difficult the question sounds.**

I use:

| Score | Level               | Definition                                                                          |
| ----: | ------------------- | ----------------------------------------------------------------------------------- |
| **0** | Recall              | The answer can be directly retrieved from the lecture.                              |
| **1** | Explain / Interpret | The student must explain a relationship, mechanism, or consequence.                 |
| **2** | Apply / Reason      | The student must diagnose, predict, distinguish, or apply a concept to a situation. |

For example, A06 asks for the typical value of the momentum coefficient beta. The lecture directly states that beta is typically 0.9, making this a recall question.

In contrast, A05 asks why vanishing gradients get worse with depth. The student must explain how derivative factors compound through layers, so it is classified as explanation/interpretation.

C07 requires the student to distinguish between two training-failure patterns: a loss that is flat from the start and a loss that explodes to NaN. This requires applying the diagnostic information in the lecture and is therefore classified as application/reasoning.

---

### 4.4 Clarity

**Clarity measures whether the question is unambiguous and gives the student a clear understanding of what is being asked.**

I use:

| Score | Definition                                                 |
| ----: | ---------------------------------------------------------- |
| **0** | Ambiguous, confusing, or misleading wording.               |
| **1** | Understandable but has a minor ambiguity or wording issue. |
| **2** | Clear and unambiguous.                                     |

Clarity is evaluated separately from grounding. A question can be clearly worded while still making an unsupported claim.

For example, A04 is understandable as a sentence, but the premise that the lecture recommended Adam as the best choice for all deep-learning tasks is unsupported. Therefore, its grounding and clarity are evaluated independently.

---

### 4.5 Coverage

Coverage is evaluated at the **question-set level**, rather than as a score assigned independently to each question.

The lecture covers several major topics, including:

* Gradient descent
* Learning rate
* Batch, stochastic, and mini-batch gradient descent
* Chain rule and backpropagation
* Forward and backward passes
* Vanishing gradients
* ReLU
* Momentum
* Adam
* Training-failure diagnosis

These topics appear across the lecture's slides.
I assess whether each generated set represents these topics broadly or concentrates disproportionately on a small portion of the lecture.

This is treated qualitatively because the dataset does not provide a predefined taxonomy or a validated weighting of lecture topics. I therefore avoid assigning an arbitrary numerical "coverage percentage."

---

### 4.6 Redundancy

**Redundancy measures how much a question set repeatedly tests substantially overlapping concepts.**

This is also evaluated at the set level.

A question set can contain individually valid questions while still being redundant as a whole.

For example, Strategy B contains multiple questions about sigmoid derivatives and their consequences:

* B01
* B02
* B03
* B07
* B12

It also contains multiple questions about ReLU and its relationship to vanishing gradients:

* B04
* B05
* B09
* B10
* B11

These questions are individually reasonable, but their concentration means that much of the set tests a relatively small region of the lecture.

---

### 4.7 Question Yield

Question yield is the number of usable questions produced by each strategy in the supplied dataset.

The observed yields are:

* Strategy A: 12
* Strategy B: 12
* Strategy C: 8

The lower output of C is important because its generate-and-verify procedure explicitly discards questions that cannot be supported.

I therefore treat yield as a separate dimension rather than assuming that fewer questions automatically means better or worse quality.

---

## 5. Results

### 5.1 Quantitative Results

The individual-question scores produce the following summary:

| Metric                        | A — Full Context | B — Retrieval | C — Generate → Verify |
| ----------------------------- | ---------------: | ------------: | --------------------: |
| Questions produced            |               12 |            12 |                     8 |
| Strongly grounded             |       9/12 (75%) |  12/12 (100%) |            8/8 (100%) |
| Source-verifiable correctness |    9/11 (81.8%)* |  12/12 (100%) |            8/8 (100%) |
| Average cognitive demand      |            0.083 |         0.417 |             **1.250** |
| Average clarity               |           2.00/2 |        2.00/2 |                2.00/2 |
| Topic coverage                |            Broad |        Narrow |                 Broad |
| Redundancy                    |         Moderate |          High |                   Low |
| Question yield                |               12 |            12 |                     8 |

* A11 is excluded from the correctness denominator because the supplied lecture does not provide enough information to establish the correctness of its stated publication year.

The main quantitative separation is in **grounding and cognitive demand**. A has clear grounding failures, while B and C achieve complete grounding in this sample. C also has substantially higher cognitive demand than either A or B.

Clarity does not meaningfully distinguish the strategies in this dataset: the questions are generally understandable across all three sets.

---

### 5.2 Grounding Analysis

Strategy A produced **9 strongly grounded questions out of 12**, or 75%.

The three problematic questions illustrate different types of source-grounding failure.

**A04** turns the lecture's nuanced discussion of Adam into an absolute recommendation. The lecture says Adam is widely used but explicitly notes that it is not always better than well-tuned SGD with momentum.
**A07** introduces a specific batch size and claims it gives the best convergence for image classification. The lecture does not make that claim. Instead, it describes mini-batches as a range of a few dozen to a few hundred and frames batch size as a trade-off.

**A11** asks for historical information not contained in the lecture. This is an important failure mode because the question may be factually answerable using external knowledge but is not answerable from the supplied source.

Strategy B and Strategy C both achieved **100% strong grounding** under the defined rubric. This is evidence that both approaches provide better source alignment than Strategy A for this lecture.

However, this result should not be interpreted as proof that retrieval or verification always produces 100% grounding. The evaluation contains only one lecture.

---

### 5.3 Cognitive Demand Analysis

The cognitive-demand distribution is:

| Cognitive level         |        A |        B |        C |
| ----------------------- | -------: | -------: | -------: |
| Recall (0)              |       11 |        7 |        0 |
| Explain / Interpret (1) |        1 |        5 |        6 |
| Apply / Reason (2)      |        0 |        0 |        2 |
| **Average**             |**0.083** |**0.417** | **1.25** |

Strategy A is dominated by recall questions. Examples include asking for the learning-rate meaning, the gradient-descent update rule, the typical momentum coefficient, and the three batch variants.

Strategy B contains a greater proportion of explanation-oriented questions. For example, B02 asks why the sigmoid derivative is bounded by 0.25, while B05 asks why ReLU helps with vanishing gradients.

Strategy C has the strongest cognitive demand. Six questions require explanation or interpretation, while two require application/reasoning. C03 asks what would be observed with an excessively large learning rate, while C07 requires distinguishing different training-failure diagnoses.

This is the strongest evidence in favor of C from an educational-quality perspective. The questions are not merely asking students to retrieve isolated facts; they more frequently require students to explain mechanisms or reason about observed behaviour.

---

### 5.4 Coverage Analysis

Strategy A provides relatively broad coverage of the lecture. Its questions touch learning rate, gradient descent, sigmoid, Adam, vanishing gradients, momentum, batch methods, ReLU, overfitting, and backpropagation.

Strategy B is much more concentrated. Most questions fall into two closely related clusters:

1. Sigmoid derivative and vanishing-gradient behaviour.
2. ReLU and its relationship to vanishing gradients.

This gives B strong local coverage of an important section of the lecture, but weaker representation of other lecture topics such as learning-rate selection, mini-batch trade-offs, momentum, Adam, memory requirements, and training diagnosis.

Strategy C provides broader conceptual coverage despite producing fewer questions. Its questions span the learning-rate update rule, vanishing gradients, learning-rate failure behaviour, mini-batch training, momentum, forward-pass memory, training diagnosis, and backpropagation.

Therefore, in this sample:

> **A has broad coverage but weaker grounding; B has strong grounding but narrow coverage; C combines strong grounding with relatively broad conceptual coverage.**

---

### 5.5 Redundancy Analysis

The clearest redundancy occurs in Strategy B.

Five questions focus directly on sigmoid derivatives or closely related consequences:

* B01
* B02
* B03
* B07
* B12

Another five focus on ReLU and its relationship to vanishing gradients:

* B04
* B05
* B09
* B10
* B11

Thus, **10 of the 12 questions are concentrated around two closely connected conceptual areas**.

This does not mean the questions themselves are poor. Rather, it means that a student receiving the complete B quiz would encounter less conceptual diversity than the number of questions initially suggests.

Strategy A is less concentrated and covers more of the lecture, although it has the previously identified grounding failures.

Strategy C is relatively diverse for its smaller size. Its questions span several different parts of the lecture, from gradient descent and learning rate through backpropagation and training diagnosis.

---

### 5.6 Yield Analysis

Strategy A and Strategy B each produced 12 questions, while Strategy C produced 8.

This is the primary disadvantage of Strategy C.

The generate-and-verify approach is explicitly designed to remove questions that cannot be supported by the source. Therefore, lower yield may partly represent successful quality control rather than simply poorer generation performance.

However, the current dataset does not tell us:

* How many questions C generated before verification.
* How many were rejected.
* Why each rejected question was removed.
* Whether rejected questions could have been regenerated successfully.
* How much additional latency or compute the verification step required.

Therefore, I would not conclude that C's lower yield is either inherently good or inherently bad.

The correct conclusion is that **C introduces a quality-versus-yield trade-off that needs another experiment to quantify**.

---

## 6. Representative Examples

### 6.1 Strategy A

#### A04 — Unsupported generalisation

**Question:**
"Which optimiser did the lecture recommend as the best choice for all deep learning tasks?"

**Answer:** Adam.

**Why this is problematic:**
The lecture describes Adam as widely used and usually effective without tuning, but explicitly says it is not always better than well-tuned SGD with momentum.

**Assessment:**

* Grounding: 0
* Correctness: 0
* Cognitive demand: 0
* Clarity: 2

This is a useful example of how a generated question can sound plausible while changing the meaning of the source.

#### A07 — Unsupported specificity

**Question:**
"According to the lecture, what batch size gives the best convergence for image classification?"

**Answer:** 256.

**Why this is problematic:**
The lecture states that mini-batch sizes are generally a few dozen to a few hundred and describes batch size as a trade-off between gradient quality and hardware utilisation. It does not identify 256 as an optimal value for image classification.

This demonstrates an important failure mode: **introducing a precise claim that is not present in the source.**

---

### 6.2 Strategy B

#### B02 — Strong explanatory question

**Question:**
"Why is the derivative of the sigmoid bounded above by 0.25?"

**Answer:**
Because the derivative is `sigma(z)(1 - sigma(z))`, which is maximised when `sigma(z) = 0.5`.

The lecture directly supports the sigmoid derivative formula and its upper bound of 0.25.

This is a strong question because it moves beyond simple retrieval of the number 0.25 and asks the student to explain why the bound occurs.

The limitation is not the quality of this question individually; it is that several other B questions test closely related material.

---

### 6.3 Strategy C

#### C03 — Application-oriented question

**Question:**
"What would you observe during training if the learning rate were set far too large?"

**Answer:**
The loss oscillates or diverges, with steps increasingly moving away from the minimum.

The lecture's learning-rate figure explicitly shows that an excessively large learning rate causes the descent path to bounce between the sides of the loss curve and diverge.

This requires the student to connect the concept of learning rate with an observable training behaviour.

#### C07 — Diagnostic reasoning

**Question:**
"Distinguish the diagnosis for a loss that is flat from the start versus one that explodes to NaN."

**Answer:**
A flat loss suggests a learning rate that is too small or vanishing gradients, whereas exploding loss suggests a learning rate that is too large or exploding gradients.

The lecture directly provides these diagnostic patterns.

This is the strongest example of an application-oriented question in the set because the student must distinguish between two observed behaviours and map each to plausible causes.

---

## 7. Strategy Comparison

The strategies represent different trade-offs rather than one being strictly superior on every dimension.

| Strategy                  | Strengths                                                                                    | Weaknesses                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **A — Full Context**      | Broad topic coverage; highest question yield                                                 | Unsupported claims; heavily recall-oriented                                     |
| **B — Retrieval**         | Strong source grounding; high question yield; more explanatory than A                        | Narrow topic coverage; high redundancy                                          |
| **C — Generate → Verify** | Strong source grounding; highest cognitive demand; broad conceptual coverage; low redundancy | Lowest question yield; verification cost and regeneration behaviour are unknown |

### Strategy A

A benefits from having the full lecture available during generation, which appears to help it cover a broad range of topics. However, the presence of unsupported claims shows that simply providing more context does not guarantee source-grounded questions.

### Strategy B

B demonstrates that retrieval can produce highly grounded questions. However, retrieving relevant passages appears to have produced a concentrated set in this example. If the retrieval stage repeatedly surfaces the most salient passage, generation may produce many variations on the same concept.

### Strategy C

C combines the strongest grounding observed in B with broader conceptual coverage and substantially higher cognitive demand. Its main cost is that only eight questions remain after generation and verification.

Therefore, the strategies should not be thought of as:

> A < B < C

on every dimension.

Rather, they occupy different points in a **quality, diversity, and yield trade-off**.

---

## 8. Recommendation

### Provisional recommendation: Strategy C

Based on the supplied lecture and question sets, I would select **Strategy C — Generate → Verify** as the leading candidate for further development.

There are three main reasons.

**First, grounding.**
All eight C questions were judged to be strongly supported by the lecture. This avoids the unsupported claims observed in Strategy A.

**Second, cognitive demand.**
C had the highest average cognitive-demand score at 1.25, substantially above B (0.417) and A (0.083).

**Third, coverage and diversity.**
C's questions cover multiple areas of the lecture, including gradient descent, learning rate, mini-batch training, momentum, backpropagation, memory, vanishing gradients, and training diagnosis. This contrasts with B's concentration around sigmoid and ReLU.

However, I would **not ship C as the final production strategy solely on the basis of this evaluation**.

The main unresolved issue is yield. C produced eight questions compared with twelve from A and B. The current dataset does not tell us whether this represents an acceptable quality-control trade-off or an inefficient generation process.

My next engineering/research step would therefore be to keep C as the leading candidate and test whether rejected questions can be regenerated while maintaining the same grounding and cognitive-quality characteristics.

---

## 9. Confidence and Limitations

### Confidence

I have **moderate confidence that C is the strongest candidate in this particular evaluation**, but low confidence that C is universally superior across lectures.

The evidence is strong enough to prefer C provisionally, but not strong enough to generalise beyond the supplied example.

### Limitation 1 — Single lecture

All three strategies were evaluated using one CS 4780 lecture.

The observed differences may depend on the topic, structure, writing style, or content distribution of this particular lecture.

### Limitation 2 — Small sample

There are only 32 generated questions in total:

* 12 from A
* 12 from B
* 8 from C

This is too small to make strong claims about production-wide behaviour.

### Limitation 3 — Subjective evaluation

Grounding can often be checked against the source, but cognitive demand, clarity, coverage, and redundancy involve evaluator judgment.

Because I am the primary evaluator, these dimensions may contain some subjectivity.

### Limitation 4 — Unequal question yield

C produces fewer questions, making direct comparison of set-level quality more complicated.

A strategy that produces eight excellent questions may or may not be preferable to one producing twelve good questions, depending on the downstream use case.

### Limitation 5 — No student outcomes

This analysis evaluates the questions themselves rather than measuring whether students actually learn more from them.

Higher cognitive demand is a desirable property under the rubric used here, but it does not by itself prove better educational outcomes.

### Limitation 6 — No generation-cost data

The supplied data does not provide token usage, latency, compute cost, or verification cost.

Therefore, this analysis cannot determine whether the additional verification step in C is operationally economical.

---

## 10. What Would Change My Mind?

I would reconsider Strategy C if a larger evaluation across multiple lectures showed that its verification process consistently reduced usable-question yield without providing a meaningful improvement in grounding, diversity, or cognitive quality.

I would also reconsider C if student-level evaluation showed that its more conceptually demanding questions were systematically too difficult or did not improve learning outcomes.

Conversely, evidence that rejected questions could be regenerated successfully while maintaining C's grounding and cognitive quality would strengthen the case for using C in production.

I would also be willing to choose Strategy B instead if a larger evaluation showed that its current concentration around sigmoid and ReLU was specific to this lecture rather than a systematic property of retrieval-based generation.

Similarly, I would reconsider Strategy A if its grounding failures disappeared across a larger sample or if its broader coverage produced better student outcomes despite its lower cognitive-demand profile.

This means the current recommendation is deliberately falsifiable rather than treating the observed result as a universal ranking.

---

## 11. Proposed Follow-up Experiment

The next experiment should test both **quality and yield** across a larger and more representative sample.

### Step 1 — Evaluate multiple lectures

Run A, B, and C on a larger collection of lectures spanning different subjects, instructors, and lecture structures.

This would establish whether the current differences generalise beyond the single CS 4780 lecture.

### Step 2 — Use the same scoring rubric

Apply the same grounding, correctness, cognitive-demand, and clarity definitions to every generated question.

The scoring sheet should preserve individual question-level evidence so that aggregate results can be traced back to specific examples.

### Step 3 — Blind the evaluator

Where practical, hide the strategy identity from the evaluator while scoring questions.

This reduces the possibility that knowing a question came from A, B, or C influences the score.

### Step 4 — Use multiple evaluators

Have at least two evaluators independently score a subset of questions.

Compare their disagreements, particularly for cognitive demand, clarity, and grounding.

This would provide evidence about how sensitive the results are to individual evaluator judgment.

### Step 5 — Measure verification yield

For Strategy C, record:

```text
Questions generated
        ↓
Questions verified
        ↓
Questions rejected
        ↓
Questions regenerated
        ↓
Final usable questions
```

This would allow the team to distinguish between:

* high-quality filtering,
* excessive rejection,
* successful regeneration,
* and unnecessary generation cost.

### Step 6 — Measure cost and latency

Record generation and verification time, token usage, and any other relevant production cost.

The goal would be to determine whether the quality improvement from verification justifies its additional cost.

### Step 7 — Evaluate student outcomes

The strongest long-term test would be to evaluate whether questions from the different strategies produce different student outcomes.

Possible measures include:

* answer accuracy,
* discrimination between students with different levels of understanding,
* perceived difficulty,
* ability to explain concepts,
* and learning gains after studying the lecture.

This would move the evaluation from:

> "Which questions look better?"

to:

> "Which strategy produces questions that work better for students?"

### Decision rule for the next experiment

I would continue with Strategy C if it maintains its grounding and cognitive-demand advantages across multiple lectures while achieving an acceptable usable-question yield and production cost.

If C's quality advantage disappears or its yield/cost becomes substantially worse, I would reconsider B or a hybrid approach.

---

## Overall Conclusion

The evidence from this evaluation does not support a universal claim that one quiz-generation strategy is always superior.

Instead, it reveals three distinct trade-offs:

* **Strategy A** provides broad coverage and high yield but introduces unsupported claims and is largely recall-oriented.
* **Strategy B** provides strong grounding and high yield but produces a highly concentrated set in this lecture.
* **Strategy C** provides strong grounding, broader conceptual coverage, and substantially higher cognitive demand, but produces fewer questions.

Therefore, **Strategy C is my provisional recommendation for further development**, with the explicit condition that its lower yield and verification cost should be evaluated before production adoption.

The most important next step is not to tune the current ranking from this single lecture, but to repeat the same evaluation across multiple lectures, blind the evaluation where possible, use multiple evaluators, and measure the quality-versus-yield trade-off introduced by verification.

This keeps the recommendation aligned with the central requirement of the assignment: **make a decision that the available evidence can support, state the uncertainty clearly, and specify what evidence would change the decision.**
