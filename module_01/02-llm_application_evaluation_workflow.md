# LLM Application Evaluation Workflow

## 1. Overview

LLM application evaluation is the process of systematically checking
whether an LLM-powered application performs its intended task correctly
and reliably before and after deployment.

This workflow is introduced using an email-classification application.
The same general process can also be applied to more complex systems,
including Retrieval-Augmented Generation (RAG) applications and agents.

> **Scope:** These notes focus on evaluating an LLM application and its
> end-to-end workflow, rather than evaluating a base model in isolation.

<br/>

## 2. Example: Email Classification and Routing

Imagine a food-delivery company receives a large volume of customer
emails. Manually reading every email and assigning it to the right team
is inefficient, so an LLM is used to classify each message.

The application assigns each email to one of three categories:

-   **Billing:** Payment, charges, refunds, or other billing-related
    issues.
-   **Technical:** App errors, crashes, login problems, or technical
    faults.
-   **General:** General questions, such as business hours or other
    non-billing queries.

The predicted category determines where the email is routed.

<br/>

### Diagram: Classification and routing

``` mermaid
flowchart TD
    A[Incoming customer email] --> B[LLM reads email]
    B --> C{Predicted category}
    C -->|Billing| D[Billing team]
    C -->|Technical| E[Technical team]
    C -->|General| F[Customer support team]
```

<br/>

| Email Content                    | Expected Category |
| -------------------------------- | ----------------- |
| "My card was charged twice."     | Billing           |
| "The app crashes when I log in." | Technical         |
| "What are your opening hours?"   | General           |


A simple implementation is not automatically ready for production. It
should first be evaluated against known examples.

<br/>

## 3. The Evaluation Workflow

The workflow has nine main steps:

1.  Define the task and target.
2.  Define success criteria and metrics.
3.  Build a golden evaluation dataset.
4.  Choose an evaluation method.
5.  Run the application on the evaluation dataset.
6.  Evaluate and analyze the results.
7.  Improve the system.
8.  Repeat evaluation and improvement until the system meets the
    required standard.
9.  Deploy, monitor production behavior, and feed failures back into the
    dataset.

<br/>

### Diagram: End-to-end evaluation loop

``` mermaid
flowchart TD
    A[1. Define task and target] --> B[2. Define success criteria and metrics]
    B --> C[3. Build golden evaluation dataset]
    C --> D[4. Choose evaluation method]
    D --> E[5. Run application on dataset]
    E --> F[6. Evaluate and analyze results]
    F --> G[7. Improve prompt, model, or system]
    G --> H{Meets required standard?}
    H -->|No| E
    H -->|Yes| I[9. Deploy application]
    I --> J[Monitor production behavior]
    J --> K[Collect and label production failures]
    K --> C
```

The loop is repeatable: after a change, run the same evaluation dataset
again so that results can be compared consistently.

<br/>

## 4. Step 1 --- Define the Task and Target

First, state exactly what is being evaluated.

For the example:

-   **Target:** The complete email-classification and routing workflow.
-   **Task:** Determine whether each email is assigned to the correct
    category.

A clearly defined task keeps the evaluation focused. For a different
application, the target and task may be different---for example,
retrieval quality in a RAG system or tool-use behavior in an agent.

<br/>

## 5. Step 2 --- Define Success Criteria and Metrics

Define what successful behavior means and how it will be measured.

For this classification task:

-   **Success criterion:** Emails are assigned to the correct category.
-   **Metric:** Classification accuracy.

Accuracy is the fraction of evaluated examples whose predicted labels
match their expected labels.

$$
 \text{Accuracy} =
\frac{\text{Number of correct predictions}}{\text{Total number of examples}}
\times 100%
$$

For example, if 90 out of 100 emails are routed correctly, the accuracy
is 90%.

**Important distinction:** The success criterion describes the desired
outcome; the metric quantifies how well the system achieves it. Accuracy
is suitable as the introductory metric here, though other tasks may
require additional metrics.

<br/>

## 6. Step 3 --- Build a Golden Evaluation Dataset

A **golden evaluation dataset** is a set of examples paired with
trusted, human-verified expected answers or labels. It provides a
consistent reference for evaluating the application.

For email classification, each record contains:

1.  The original email text.
2.  Its expected category, assigned through manual labeling or a
    reliable review process.

Example:

| Email                            | Golden Label |
| -------------------------------- | ------------ |
| "My card was charged twice."     | Billing      |
| "The app crashes when I log in." | Technical    |
| "What are your opening hours?"   | General      |


A practical starting dataset might contain roughly 50--500 examples, as
suggested in the lecture. The appropriate size depends on the task, data
diversity, and evaluation needs.

<br/>

### Dataset creation principles

-   Prefer representative real-world examples when they can be used
    appropriately.
-   Label examples manually or have qualified reviewers verify the
    labels.
-   Include examples from all relevant categories.
-   Keep the expected labels consistent and unambiguous.
-   Reuse the dataset across evaluation runs to compare system changes.

<br/>

### Diagram: Golden dataset

``` mermaid
flowchart LR
    A[Representative email examples] --> B[Human labeling and review]
    B --> C[Golden evaluation dataset]
    C --> D[LLM application predictions]
    D --> E[Compare predictions with golden labels]
    E --> F[Calculate evaluation metrics]
```

The dataset is not merely sample input. It is the reference against
which predictions are judged.

<br/>

## 7. Step 4 --- Choose an Evaluation Method

Choose how the application's outputs will be judged. The lecture
introduces three broad options.

| Evaluation Method    | How It Works                                                        | Suitable Situations                                                                    |
| -------------------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Automated evaluation | Code compares outputs with expected answers and calculates metrics. | Structured labels, exact matches, numeric outputs, and other clearly testable results. |
| Human evaluation     | People inspect outputs and judge their correctness or quality.      | Tasks requiring nuanced judgment or domain expertise.                                  |
| LLM-as-a-judge       | Another LLM evaluates outputs against instructions or a rubric.     | Open-ended text where semantic quality is difficult to assess with simple code.        |


<br/>

### Choosing a method for the example

Email classification returns one of three fixed labels. A short Python
program can compare each predicted label with its golden label and
calculate accuracy. Therefore, **automated evaluation** is a natural
choice for this example.

For a chatbot that produces paragraphs of text, exact string comparison
may be inadequate: two answers can use different wording while
expressing the same meaning. Human evaluation or an LLM judge may be
more appropriate, depending on the task and the evaluation rubric.

**Practical caution:** An LLM judge is not automatically correct. Its
judgments can be inconsistent or biased, so important evaluations should
use a clear rubric and validation against human judgments.

<br/>

## 8. Step 5 --- Run the Application on the Evaluation Dataset

Pass the dataset's input examples through the application and record the
outputs.

For the email classifier:

1.  Send each email to the application.
2.  Record the category predicted by the LLM workflow.
3.  Keep each prediction associated with its original example.
4.  Preserve the expected label so the two can be compared.

<br/>

Example:
| Email                            | Expected Label | Predicted Label | Result    |
| -------------------------------- | -------------- | --------------- | --------- |
| "My card was charged twice."     | Billing        | Billing         | Correct   |
| "The app crashes when I log in." | Technical      | General         | Incorrect |
| "What are your opening hours?"   | General        | General         | Correct   |


This step generates the predictions; the next step scores them.

<br/>

## 9. Step 6 --- Evaluate and Analyze Results

### 9.1 Evaluate the results

Use the selected method to compare the predictions with the expected
outputs.

For example, if 80 out of 100 emails are classified correctly:
$$
\text{Accuracy} = \frac{80}{100}\times 100
= 80% 
$$
The score summarizes performance, but it does not by itself explain why
errors occurred.

<br/>

### 9.2 Analyze errors

Inspect incorrect examples to identify patterns. In the
email-classification case, questions to investigate include:

-   Are billing and technical issues being confused?
-   Are the category definitions unclear?
-   Does the prompt provide sufficient classification instructions?
-   Is the selected model capable of following the task reliably?
-   Are some examples ambiguous or incorrectly labeled?

<br/>

### Diagram: From score to diagnosis

``` mermaid
flowchart TD
    A[Evaluation predictions] --> B[Compare with golden labels]
    B --> C[Calculate accuracy and inspect errors]
    C --> D[Group errors by pattern]
    D --> E[Identify likely cause]
    E --> F[Select a targeted system change]
```

Analysis turns an evaluation score into actionable information.

<br/>

## 10. Step 7 --- Improve the System

Use the error analysis to make targeted changes. The lecture highlights
two possible improvements:

-   **Improve the prompt:** Clarify category definitions, instructions,
    and how to handle borderline cases.
-   **Change the model:** A different or more capable model may perform
    the task more reliably.

Other parts of the application may also need attention, depending on the
error---for example, input preprocessing, output validation, or routing
logic.

After each change, rerun the evaluation dataset. Do not assume an
improvement worked simply because the prompt or model was changed.

<br/>

## 11. Step 8 --- Iterate Until the Required Standard Is Met

Evaluation is a repeated cycle, not a one-time check.

Illustrative progression from the lecture:

-   Initial system: 80% accuracy.
-   After prompt improvements: 90% accuracy.
-   After changing to a more capable model: 95% accuracy.

These numbers are examples of an iterative process, not guaranteed
results. The key is to compare each version against the same evaluation
dataset and metric.

<br/>

### Diagram: Iterative improvement

``` mermaid
flowchart LR
    A[Run evaluation] --> B[Review score and failures]
    B --> C[Change prompt or model]
    C --> D[Run the same dataset again]
    D --> E{Meets acceptance threshold?}
    E -->|No| B
    E -->|Yes| F[Candidate for deployment]
```

Before deployment, define an acceptable performance threshold and
consider whether the metric captures the application's important risks.
A high overall accuracy can still hide poor performance on a less
frequent but important category.

<br/>

## 12. Step 9 --- Deploy and Monitor in Production

Once the application meets its required standard, it can be deployed.
Evaluation does not end at deployment.

Real production inputs may differ from the examples in the evaluation
dataset. An application that performed well during testing may still
make mistakes on new customer emails.

Production monitoring should help identify:

-   Incorrect classifications and routing failures.
-   New or changing types of customer queries.
-   Repeated error patterns.
-   Changes in application behavior or performance.

<br/>

### Handling production failures

Suppose an email should have been classified as **Billing** but was sent
to the **Technical** team. The receiving team or another review
mechanism can flag the error. The email can then be reviewed, correctly
labeled, and added to the golden evaluation dataset.

The team can rerun evaluation, analyze the failure, improve the system,
and validate the change before deploying an updated version.

<br/>

### Diagram: Production feedback loop

``` mermaid
flowchart TD
    A[Deployed application] --> B[Monitor predictions and routing]
    B --> C[Detect or receive a failure report]
    C --> D[Review and correctly label the case]
    D --> E[Add case to evaluation dataset]
    E --> F[Re-evaluate and analyze]
    F --> G[Improve the system]
    G --> H[Validate the change]
    H --> A
```

A monitoring process must include a way to discover or report failures.
For example, a support team that receives a misrouted email can flag the
case for review.

<br/>

## 13. One Application Can Have Multiple Evaluations

A single LLM application may need several distinct evaluations because
different components and qualities can fail independently.

For example, a RAG application may have separate evaluations for:

-   **Retriever performance:** Whether relevant information is
    retrieved.
-   **Embedding model performance:** Whether the embedding approach
    supports effective retrieval.
-   **End-to-end RAG quality:** Whether the complete workflow produces
    useful, grounded answers.
-   **Latency:** Whether responses are produced within acceptable time
    limits.

These evaluations measure different properties. A strong end-to-end
score does not automatically prove that every component is optimal, and
good component scores do not guarantee that the complete application
behaves well.

<br/>

### Diagram: Multiple evaluations for one RAG application

``` mermaid
flowchart TD
    A[RAG application] --> B[Retriever evaluation]
    A --> C[Embedding evaluation]
    A --> D[End-to-end answer evaluation]
    A --> E[Latency evaluation]
    B --> F[Component-specific findings]
    C --> F
    D --> G[System-level findings]
    E --> G
```

The same principle applies to agentic applications: tool selection, tool
execution, final answers, safety, and latency may need separate
evaluation criteria.

<br/>

## 14. Key Takeaways

-   Evaluate an LLM application before deployment; a working prototype
    is not proof of reliability.
-   Start by defining the task, target, success criteria, and metrics.
-   Build a golden dataset with reliable expected answers or labels.
-   Select an evaluation method suited to the output: automated, human,
    or LLM-as-a-judge.
-   Run the application on the dataset, score its outputs, and inspect
    individual failures.
-   Improve the system based on evidence, then rerun the evaluation.
-   Use repeatable evaluations to compare system versions.
-   Monitor production behavior and feed reviewed failures back into the
    evaluation dataset.
-   A single application may require multiple evaluations for different
    components and quality dimensions.
-   This workflow applies broadly to LLM applications, including RAG
    systems and agents.

<br/>

## 15. Practice and Revision

1.  For the email-routing application, identify the target, task,
    success criterion, and metric.
2.  Create five example emails and assign each a golden label.
3.  Given 100 examples and 87 correct predictions, calculate accuracy.
4.  Explain why exact string comparison is often insufficient for
    open-ended chatbot answers.
5.  Compare automated evaluation, human evaluation, and LLM-as-a-judge.
6.  If billing emails are frequently classified as technical, list two
    hypotheses and one test for each.
7.  Explain how a production failure should become part of the
    evaluation process.
8.  Propose three separate evaluations for a RAG application and state
    what each measures.

**Checkpoint:** Explain the complete evaluation workflow, justify the
evaluation method for a given task, and describe how production failures
feed into continuous improvement.
