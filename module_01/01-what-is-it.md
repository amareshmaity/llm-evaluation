# 01 — Introduction to LLM Evaluations

## Overview

LLM evaluation is the process of systematically testing a language model or an LLM-powered application against clearly defined criteria. It helps determine whether a model or application is suitable for a task, reliable enough to release, and improving as changes are introduced.

The central idea is that **LLM evaluation is not just a metric or a score—it is the complete testing setup**.

<br/>

## 1. What Are LLM Evaluations?

**Definition:** LLM evaluations (LLM evals) are systematic, repeatable tests used to judge an LLM or an LLM-powered system against clear criteria.

Three characteristics make an evaluation useful:

### 1.1 Systematic

Evaluation should follow a planned process rather than relying on a few random prompts.

- Create a representative evaluation dataset.
- Include different types of user requests and relevant edge cases.
- Use the dataset to test the chatbot or application consistently.
- Where possible, include real-world examples of how users interact with the system.

**Example:** For a university chatbot, collect representative student questions and use them as a test dataset. This gives a more realistic view of how the chatbot behaves than trying only a few questions manually.

<br/>

### 1.2 Repeatable

The same evaluation dataset and process should be reusable after changes to the system.

Changes might include:
- Updating the prompt.
- Replacing the language model.
- Changing the retriever.
- Modifying the chunking strategy.

Running the same evaluation before and after a change makes the results comparable. This helps establish whether a new version performs better, worse, or similarly to the previous version.

<br/>

### 1.3 Based on Clear Criteria

An evaluation needs explicit criteria that define what a good result means.

For a university chatbot, criteria might include:
- **Correctness:** Is the answer accurate?
- **Clarity:** Is the explanation easy for the intended audience to understand?
- **Grounding:** Is the answer supported by the university's course material?
- **Safety and tone:** Does the response avoid unsafe content, abusive language, or an intimidating tone?

Without criteria, testing is subjective. Clear criteria make the evaluation more consistent and actionable.

<br/>

## 2. LLM Evals vs. Metrics

LLM evaluation is broader than a metric.

A metric is one way to measure a particular aspect of performance. For example, accuracy, precision, and recall are metrics commonly used in traditional machine learning.

An LLM evaluation setup may include:

- **Target:** What is being evaluated—for example, a model, retriever, or complete chatbot.
- **Dataset:** The examples used for testing.
- **Criteria and metrics:** The standards used to judge performance.
- **Evaluation timing:** Whether tests run offline during development or on a deployed system.
- **Tools and methods:** The tools used to run or organize the evaluation.

For example, evaluating a retrieval-augmented generation (RAG) application might include testing retriever accuracy, checking whether answers are grounded in retrieved context, and measuring end-to-end latency.

> **Key distinction:** A metric measures a particular aspect of performance; an evaluation is the broader setup used to test and make decisions about a system.

<br/>

## 3. What Questions Should LLM Evals Answer?

The goal is not merely to produce a score. Evaluation should support practical decisions, such as:

- Can this model be used for a particular task?
- Is the application good enough to release to production?
- Did Prompt Version 2 improve over Prompt Version 1?
- Is a RAG answer grounded in the retrieved context?
- Is an agent completing its task correctly?
- Is the chatbot safe for real users?
- Is latency within acceptable limits?

The answers help teams decide what to ship, what to improve, and which model or system configuration to use.

<br/>

## 4. Two Useful Categories of LLM Evaluation

For learning purposes, evaluations can be separated into two categories:

1. **Model evaluations** — evaluate the language model itself.
2. **Application evaluations** — evaluate an LLM-powered application, including its individual components and its overall behavior.

These labels are a useful distinction for this chapter; they are not presented here as formal, universally standardized industry terms. In practice, the broader term *LLM evaluation* can refer to either category, depending on the context.

<br/>

## 5. Model Evaluations

Model evaluation tests a model's capabilities. When a new model is released, its performance may be measured on established benchmarks and reported in benchmark results or leaderboards.

The results help people understand a model's strengths and limitations and compare it with other models.

<br/>

### 5.1 Major Capability Areas

The following eight areas are useful categories for thinking about model capabilities:

| Capability | What it evaluates |
|---|---|
| **Reasoning** | Whether the model can work through a problem in a logically structured way. |
| **Knowledge** | Whether it can answer questions requiring general or domain knowledge, subject to the model's knowledge limitations. |
| **Basic mathematics** | Whether it can solve mathematical problems. |
| **Coding** | Whether it can generate, understand, or solve programming tasks. |
| **Instruction following** | Whether it follows the requirements and constraints in a prompt. |
| **Long-context handling** | Whether it can locate and use relevant information within a large context. |
| **Multimodal understanding** | Whether it can process modalities such as text, images, or audio, depending on its supported capabilities. |
| **Tool use** | Whether it can select and use tools appropriately to complete tasks. |

<br/>

### 5.2 Example Benchmarks Mentioned

The following are examples associated with particular capability areas:

| Benchmark | Main use described |
|---|---|
| **MMLU** | Evaluating knowledge across a range of subjects, such as science, history, law, and medicine. |
| **GSM8K** | Evaluating grade-school mathematical problem solving. |
| **SWE-bench** | Evaluating software-engineering and coding-related performance. |
| **IFEval** | Evaluating instruction-following behavior. |
| **Needle in a Haystack** | Testing whether relevant information can be found within a long context. |
| **MMMU** | Evaluating multimodal understanding. |

Benchmarks measure particular tasks or capabilities; no single benchmark fully describes a model's performance in every real-world setting.

<br/>

### 5.3 Why AI Engineers Should Understand Model Evaluations

AI engineers may not usually build and publish broad model benchmarks themselves. That work is often performed by model developers and research labs.

However, AI engineers should understand:
- What model evaluations and benchmarks are.
- How to interpret benchmark results.
- Which capabilities matter for a particular application.
- How benchmark evidence can inform model selection.

For example, when choosing a model for a project, benchmark results can contribute to the decision between hosted models and open-source alternatives. They should inform the decision rather than replace testing the model in the intended application.

<br/>

## 6. Application Evaluations

Application evaluation examines the behavior and performance of an LLM-powered product. This is especially important because **the LLM is only one component of a complete application**.

An application may include:

- User interface.
- System prompts and other prompt templates.
- Tools and external APIs.
- Orchestration and control flow, including branching or parallel execution.
- Application-level guardrails.
- Output parsers and structured-output handling.
- Memory and context management.
- Retrieval components in a RAG system.
- Embedding models, vector databases, and rerankers.
- Deployment, monitoring, and feedback loops.

A capable model does not guarantee that the complete application will work well. Other components can introduce errors, increase latency, produce irrelevant retrieval results, or cause unsafe or confusing behavior.

<br/>

### 6.1 Smartphone Analogy

A smartphone can contain a powerful processor, but processor performance alone does not guarantee a good phone. The camera, operating system, graphics, sound, battery, screen, and their integration also matter.

Likewise, a strong LLM does not guarantee a strong LLM application. The surrounding components must also be tested, both individually and as a complete system.

<br/>

### 6.2 Component-Level and End-to-End Evaluation

Application evaluations can happen at two levels.

**Component-level evaluation**

Test individual parts to locate problems and understand their behavior. In a RAG system, for example, evaluate:
- Whether the retriever returns relevant documents.
- Whether the embedding model supports useful retrieval.
- Whether the reranker orders results appropriately.

**End-to-end evaluation**

Test the complete application from input to final response. This can include:
- Whether the user's question is answered correctly.
- Whether the answer uses the intended source material.
- Whether the answer is faithful to the retrieved context.
- Whether the response is clear for the target user.
- Whether hallucinations occur.
- Whether the system responds quickly enough.
- Whether the application behaves safely.
- Whether latency and token-related costs are acceptable.

Component tests help diagnose failures; end-to-end tests show whether the complete product meets its requirements.

<br/>

## 7. Model Evaluations vs. Application Evaluations

| Aspect | Model evaluation | Application evaluation |
|---|---|---|
| **Main target** | The language model itself | The LLM-powered application or one of its components |
| **Main question** | What can the model do, and how well? | Does the product work correctly for its intended users and tasks? |
| **Typical method** | Benchmarks and capability tests | Task-specific datasets, criteria, component tests, and end-to-end tests |
| **Typical examples** | Reasoning, knowledge, mathematics, coding, instruction following | Answer correctness, grounding, retrieval quality, safety, latency, cost |
| **Use in AI engineering** | Understand model capabilities and help select a suitable model | Validate and improve the application being built |

Both categories are useful, but model-level capability does not substitute for evaluating the application built around the model.

<br/>

## 8. A Practical Evaluation Workflow

A basic workflow consistent with the principles in this chapter is:

1. **Define the target.** Decide whether the test concerns the model, a component, or the full application.
2. **Define success criteria.** Specify what a good result means for the task.
3. **Prepare a representative dataset.** Include normal requests and relevant edge cases.
4. **Choose appropriate measures and evaluation methods.** Match them to the criteria.
5. **Run the evaluation consistently.** Preserve the dataset and procedure so results can be compared.
6. **Inspect the results.** Identify strengths, weaknesses, and failure patterns.
7. **Compare versions.** Repeat the evaluation after changes to prompts, models, retrieval, or other components.
8. **Make a decision.** Use the evidence to guide release, model selection, or further improvements.

This workflow is a practical synthesis of the chapter's core ideas: systematic tests, repeatability, and clear criteria.

<br/>

## Key Takeaways

- LLM evals are systematic, repeatable tests against clearly defined criteria.
- A few manually chosen prompts are not a substitute for a representative evaluation dataset.
- Reusing the same evaluation setup makes comparisons between versions more meaningful.
- LLM evaluation is the complete testing setup, not just a metric or score.
- Model evaluations test model capabilities, often using benchmarks.
- Application evaluations test the components and end-to-end behavior of an LLM-powered product.
- A strong model does not automatically produce a reliable application.
- For AI engineers building LLM applications, application evaluation is a central practical skill.

<br/>

## Review Questions

1. What are the three defining characteristics of a useful LLM evaluation?
2. Why should an evaluation dataset be reused after a prompt or model change?
3. How is an evaluation setup different from a metric?
4. What practical decisions can evaluation help a team make?
5. What is the difference between model evaluation and application evaluation?
6. Name four capability areas used to evaluate language models.
7. What does the Needle in a Haystack benchmark test?
8. Why is evaluating a RAG retriever separately useful?
9. Give three examples of end-to-end application evaluation criteria.
10. Why are model benchmark results not enough to prove that an application is ready for production?

<br/>

## Checkpoint

**Task:** Imagine you are building a university course-support chatbot. Describe how you would evaluate it.

A strong answer should include:
- A representative dataset of student questions, including edge cases.
- Clear criteria for correctness, clarity, grounding in course material, and safety.
- Component-level tests for retrieval where applicable.
- End-to-end tests of the final response.
- Repeatable runs to compare changes to prompts, models, or retrieval settings.
- Practical release considerations such as response latency and cost.

<br/>

**Self-assessment:** Can I explain the difference between evaluating a model's capabilities and evaluating whether the complete application works for its intended users?
