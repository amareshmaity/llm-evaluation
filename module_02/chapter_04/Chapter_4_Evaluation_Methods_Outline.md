# Chapter 4 — Evaluation Methods

**Goal:** Understand how an LLM evaluation pipeline judges output quality, when to use each evaluation method, and how to measure whether an evaluator is reliable.

## 4.1. What Is an Evaluation Method?

- Definition: the procedure that examines an output and produces an evaluation or judgment.
- How an evaluation method fits into an evaluation pipeline.
- The three execution styles:
  - Programmatic / deterministic evaluation
  - Human-based evaluation
  - Model-graded evaluation (LLM-as-a-Judge)
- Distinguish the evaluation method from the creation of a dataset or rubric.

**Example:** A Zomato-style email classifier categorizes messages as billing, technical, or general. Python calculates classification accuracy, so the evaluation is programmatic.

## 4.2. Programmatic / Deterministic Evaluation

- When an evaluation can be expressed as explicit rules, exact comparisons, or measurable metrics.
- Component-level evaluation versus whole-application evaluation.
- Defining the task, target component, success criterion, and evaluation dataset.
- Creating a representative golden dataset with normal, difficult, random, and edge-case examples.
- Reference-based retrieval evaluation:
  - Relevant documents known in advance
  - Recall@K: relevant documents retrieved in the top K divided by all known relevant documents
  - Calculating Recall@K for individual queries and averaging across the dataset
- Briefly distinguish recall from precision and ranking quality.
- Interpreting results and investigating low-scoring cases.
- Improving a retriever:
  - Better embedding model
  - Query expansion
  - Increasing K
  - Re-ranking retrieved documents

**Worked example:** A Campus X RAG chatbot retriever has two relevant documents for a query, but retrieves only one in its top five. Recall@5 = 1/2 = 50%. Evaluate a set of 50–100 queries and average the per-query recall scores; the example evaluation reports 67% Recall@K.

## 4.3. Human-Based Evaluation

- When human judgment is useful: subjective, nuanced, or context-dependent qualities.
- Evaluating the helpfulness of an entire Campus X chatbot, rather than one component.
- Defining helpfulness through accuracy, completeness, and appropriate tone.
- Creating a rubric, such as a 1–5 helpfulness scale:
  - 5: accurate, complete, and appropriately phrased
  - 3: partially helpful
  - 1: unhelpful or substantially off-topic
- Building a representative dataset of questions and collecting chatbot answers.
- Having graders score each answer against the rubric and aggregating scores.
- Using multiple graders:
  - Comparing judgments
  - Identifying unclear or ambiguous rubric criteria when graders disagree
  - Refining instructions and the rubric
- Five human-evaluation activities introduced in the lesson:
  1. Direct grading and rating
  2. Red teaming
  3. A/B testing with users
  4. Golden-dataset and rubric creation
  5. Human-in-the-loop review for difficult or uncertain cases
- Trade-off: human judgment can be trusted for nuanced assessment, but it is expensive and difficult to scale.

**Worked example:** A grader scores a chatbot answer to “Can I pay the course fee in installments?” on the 1–5 helpfulness scale. Multiple graders may reveal whether the rubric is clear enough to apply consistently.

## 4.4. Model-Graded Evaluation: LLM-as-a-Judge

- Why use an LLM judge: programmatic rules may not capture subjective quality, while human grading can be costly at scale.
- The LLM-as-a-Judge pattern: provide an evaluator model with the task, criteria, output, and grading instructions.
- Defining success by comparing model judgments with expert human judgments.
- Designing a clear, task-specific rubric before running the judge.
- Constructing the evaluation dataset:
  - Question or task
  - Rubric and scoring dimensions
  - Candidate response
  - Human reference score
- Prompting the judge with:
  - Role and evaluation objective
  - Original question and mark allocation
  - Relevant rubric
  - Student or model answer
  - Instructions to score each dimension
  - Protection against rewarding verbosity, keyword stuffing, or unsupported confident claims
  - A concise justification for the assigned score
- Comparing LLM scores with human scores on the same examples.
- Iteratively improving the judge by revising the prompt, rubric, or evaluator model.

**Worked example:** An LLM grades UPSC Mains-style subjective answers. A subject expert first grades 50–100 answers using a defined rubric. The LLM then grades those same answers, and its scores are compared with the human scores.

## 4.5. Evaluator Agreement, Consistency, and Variance

- Why evaluator reliability matters: a score is useful only if the evaluation process is sufficiently consistent.
- Human–human disagreement and what it can reveal about rubric ambiguity.
- Human–LLM score comparison as a way to assess a model judge.
- Mean Absolute Error (MAE) for numerical grading:
  - Calculate the absolute difference between the human score and LLM score for each answer.
  - Average those absolute differences across the dataset.
  - Lower MAE indicates closer agreement; MAE of zero means the scores match exactly on the evaluated examples.
- Interpreting the score gap and reviewing cases with large disagreements.
- Improving agreement by clarifying rubric dimensions, revising the judge prompt, or using a stronger evaluator model.
- Treat the human score as a reference judgment, not as proof of objective perfection.

**Worked example:** If the average absolute difference between human and LLM scores is 2.3 marks, the LLM differs from the human scores by 2.3 marks on average. The aim is to reduce this error.

## 4.6. Reference-Based and Reference-Free Evaluation

- **Reference-based evaluation:** a known target, expected answer, relevant-document set, or human-assigned score is available for each test case.
- **Reference-free evaluation:** no predefined correct answer or per-example reference score is supplied; the output is judged directly against criteria or a rubric.
- How to classify an evaluation: check whether the dataset contains a known reference for each test case.
- Examples from the lesson:
  - Retriever Recall@K with known relevant document IDs: reference-based
  - UPSC LLM judge compared with human-assigned scores: reference-based
  - Chatbot helpfulness scored by a human using a general rubric, with only questions in the dataset: reference-free

**Key distinction:** The evaluator type (program, human, or LLM) and the reference setup (reference-based or reference-free) are separate dimensions.

## 4.7. Choosing an Evaluation Method for a Task

- Use programmatic evaluation when the target can be measured with clear rules or objective metrics.
- Use human evaluation when nuanced expert judgment is essential and the evaluation volume is manageable.
- Use LLM-as-a-Judge when quality is subjective or complex and evaluation must scale; validate it against human judgments.
- Choose metrics and rubrics that match the actual task:
  - Classification: accuracy or other suitable classification metrics
  - Retrieval: Recall@K, with precision and ranking measures as additional perspectives
  - Chatbot helpfulness: rubric-based scores
  - Subjective answer grading: human–LLM score agreement and MAE
- Consider cost, scalability, task complexity, rubric clarity, and the consequences of evaluation errors.
- Use a hybrid approach when appropriate: automated checks for objective requirements, an LLM judge for nuanced criteria, and human review for ambiguous or high-impact cases.

**Practice checkpoint:** Given a task, identify the target, success criterion, dataset/reference requirements, evaluation method, and a way to validate the evaluator.

## Scope Note

Offline evaluation is the focus of this chapter. Online evaluation—evaluating a system after deployment—is introduced as the next topic and is not developed in this chapter.
