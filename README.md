# LLM Evaluation

A structured learning guide to evaluating large language models and
LLM-powered applications, with an emphasis on practical AI engineering.

## Course Overview

LLM evaluation is the process of systematically measuring the quality,
reliability, safety, performance, and cost of systems powered by large
language models. This course develops an end-to-end understanding of
evaluation, from defining evaluation criteria and building test datasets
to testing RAG applications, preventing regressions, and monitoring
deployed systems.

<br/>

### Learning Objectives

By the end of this course, I aim to be able to:

-   Explain the difference between evaluating a language model and
    evaluating an application built with one.
-   Design an evaluation workflow with clear objectives, datasets,
    metrics, and acceptance criteria.
-   Choose appropriate evaluation methods, including deterministic
    checks, human review, and LLM-as-a-Judge.
-   Interpret benchmarks and conduct task-specific model comparisons.
-   Evaluate retrieval, generation, and end-to-end Retrieval-Augmented
    Generation (RAG) systems.
-   Test application safety, scope adherence, and sensitive-information
    leakage.
-   Measure latency, token usage, cost, and reliability.
-   Build regression tests and use evaluation results to support release
    decisions.
-   Monitor production behavior and turn observed failures into
    improvements.

<br/>

## Course Structure

## Module 1 --- Foundations of LLM Evaluation

**Goal:** Understand why LLM evaluation matters, distinguish model and
application evaluation, and design repeatable evaluation pipelines across
components, workflows, and the complete application.

### Introduction and Learning Roadmap

**Notes:** [Introduction and Learning Roadmap](module_01/00-introduction.md)

-   The AI engineer's role and evaluation as an engineering skill
-   Production readiness and the limitations of informal vibe testing
-   Failure case studies: incorrect policy guidance, prompt manipulation,
    and fabricated legal references
-   Deterministic software testing versus probabilistic LLM evaluation
-   Evaluation dimensions: factuality, completeness, tone, groundedness,
    latency, and cost
-   Learning roadmap: datasets, rubrics, RAG, agents, safety, and operations

### Chapter 1. What Are LLM Evaluations?

**Notes:** [Introduction to LLM Evaluations](module_01/01-what-is-it.md)

-   Systematic, repeatable tests against clearly defined criteria
-   Evaluation as a complete testing setup rather than a single metric
-   Using evaluations for model selection, release decisions, and version
    comparisons
-   Model evaluation versus application evaluation
-   Model capability areas and introductory benchmark examples
-   Application components, component-level tests, and end-to-end checks
-   Why a strong model does not guarantee a reliable application

### Chapter 2. LLM Application Evaluation Workflow

**Notes:** [LLM Application Evaluation Workflow](module_01/02-llm_application_evaluation_workflow.md)

-   Worked example: email classification and routing
-   Defining the task, evaluation target, success criteria, and metrics
-   Building a representative golden dataset with verified expected labels
-   Choosing automated evaluation, human review, or LLM-as-a-Judge
-   Running the application, recording predictions, and calculating accuracy
-   Analyzing errors and improving prompts, models, or application logic
-   Repeating evaluations until acceptance criteria are met
-   Deploying, monitoring, and adding reviewed production failures to the
    evaluation dataset

### Chapter 3. Why We Need Multiple Evaluation Pipelines

**Notes:** [Multiple Evaluation Pipelines](module_01/03_llm_evaluation_multiple_pipelines.md)

-   Multiple failure points in a RAG chatbot: retriever, generator,
    workflow, and complete application
-   Why passing component tests does not guarantee a correct final answer
-   Retrieval ordering, groundedness, and reranking in the course-duration
    example
-   Component-level, workflow-level, and application-level evaluation
-   Three risk categories: application quality, safety, and operations
-   Quality dimensions for RAG, agents, and multi-turn chatbots
-   Safety dimensions: toxicity, harmful content, bias, privacy leakage,
    and prompt-injection or jailbreak resistance
-   Operational dimensions: latency, time to first token, cost, token
    efficiency, failure rates, and reliability under load
-   Mapping evaluation levels against risk categories to identify coverage
    gaps and plan tests


## Module 2 --- Evaluation Methods and Lifecycle

**Goal:** Understand the main evaluation approaches and when to use each
one.

### Chapter 4. Evaluation Methods

-   Deterministic and programmatic evaluation
-   Reference-based and reference-free evaluation
-   Human evaluation and grading rubrics
-   LLM-as-a-Judge
-   Evaluator agreement, consistency, and variance
-   Choosing an evaluation method for a task

### Chapter 5. Offline and Online Evaluation

-   Offline evaluation during development
-   Golden datasets and controlled test environments
-   Online evaluation using production behavior
-   Sampling and evaluation-cost management
-   Feedback loops between production and development
-   Privacy and data-handling considerations

## Module 3 --- Model Evaluation and Benchmarking

**Goal:** Assess model capabilities and select models based on the needs
of a particular application.

### Chapter 6. Evaluating LLM Capabilities

-   Knowledge, reasoning, mathematics, and coding
-   Instruction following and structured output
-   Long-context understanding and retrieval
-   Tool calling and multimodal capabilities
-   Safety and reliability
-   Quality, latency, and cost trade-offs

### Chapter 7. LLM Benchmarks and Their Limitations

-   What benchmarks measure
-   Benchmark datasets, tasks, and scoring
-   Benchmark saturation and score ceilings
-   Data contamination and evaluation validity
-   General-purpose benchmarks versus task-specific tests
-   Interpreting benchmark results critically

### Chapter 8. Working with LLM Leaderboards

-   Understanding leaderboard rankings and scores
-   Comparing models across capability categories
-   Recognizing differences in evaluation methodology
-   Understanding uncertainty and limitations in rankings
-   Using leaderboards as one input to model selection

### Chapter 9. Custom Model Evaluation and Selection

-   Defining application-specific requirements
-   Creating a representative test dataset
-   Comparing candidate models under the same conditions
-   Measuring quality, latency, and cost
-   Comparing hosted and self-managed models
-   Recording results and making a justified selection

## Module 4 --- RAG Evaluation

**Goal:** Evaluate retrieval, generation, and the complete
Retrieval-Augmented Generation pipeline separately and together.

### Chapter 10. RAG Evaluation Strategy

-   RAG architecture and its evaluation requirements
-   Retrieval quality versus answer quality
-   Component-level and end-to-end evaluation
-   Golden datasets for RAG systems
-   Common retrieval and generation failure modes
-   Structuring a practical RAG evaluation report

### Chapter 11. Evaluating RAG Retrievers

-   Document ingestion, chunking, and embeddings
-   Vector search and retrieval behavior
-   Relevant and irrelevant retrieved context
-   Precision and recall at K
-   Contextual precision and contextual recall
-   Building retrieval evaluation datasets
-   Improving retrieval through chunking, embeddings, and reranking
-   Implementing retriever evaluations with evaluation tools

### Chapter 12. Evaluating RAG Generation and the Full Pipeline

-   Answer correctness and relevance
-   Faithfulness and groundedness
-   Hallucination detection
-   Contextual relevance
-   The RAG Triad: context relevance, groundedness, and answer relevance
-   LLM-as-a-Judge for RAG outputs
-   Prompt refinement and experiment comparison
-   End-to-end RAG evaluation

## Module 5 --- Advanced LLM-as-a-Judge

**Goal:** Design structured, rubric-based evaluations for qualities that
are difficult to measure with simple programmatic checks.

### Chapter 13. Custom Evaluation Metrics with G-Eval

-   Why custom evaluation criteria are needed
-   Defining criteria, instructions, and scoring rubrics
-   Structured evaluation steps
-   Scoring and probability-weighted evaluation
-   Implementing custom metrics with G-Eval
-   Evaluating correctness, completeness, tone, and style
-   Understanding judge bias, variance, and limitations
-   Validating judge results against human judgments

## Module 6 --- Safety and Security Evaluation

**Goal:** Test whether an LLM application follows its intended
boundaries and handles unsafe or adversarial inputs appropriately.

### Chapter 14. Safety, Leakage, and Scope Adherence

-   Safety and security risks in LLM applications
-   Prompt injection and jailbreak attempts
-   Adversarial inputs and knowledge-base risks
-   Toxicity evaluation
-   Sensitive-information and personally identifiable information (PII)
    leakage
-   Scope adherence and domain drift
-   Red teaming and adversarial test cases
-   Custom safety and scope evaluation metrics
-   Layered guardrails and their limitations

## Module 7 --- Operational Evaluation

**Goal:** Measure whether an application meets performance, reliability,
and cost requirements.

### Chapter 15. Latency, Cost, and Reliability

-   Operational evaluation and service-level objectives
-   End-to-end and component-level latency
-   P50, P95, and P99 latency
-   Time to First Token (TTFT)
-   Input and output token usage
-   Estimating and monitoring inference costs
-   Caching, retries, timeouts, and failures
-   Tracing and instrumentation
-   Building operational evaluation scripts
-   Balancing quality, speed, reliability, and cost

## Module 8 --- Regression Testing and Release Automation

**Goal:** Detect quality degradation and make evaluation a repeatable
part of the development and release process.

### Chapter 16. Regression Testing for LLM Applications

-   Regression testing in traditional software and LLM systems
-   Baseline and candidate evaluation runs
-   Comparing metric changes across versions
-   Handling stochastic outputs and judge noise
-   Defining thresholds and acceptance criteria
-   Combining quality, safety, and operational tests
-   Maintaining versioned evaluation datasets
-   Investigating regressions and preventing recurrence

### Chapter 17. Evaluation in CI/CD Workflows

-   Automating evaluation-suite execution
-   Running tests when prompts, models, or application code change
-   Quality gates and release decisions
-   Reporting evaluation results
-   Managing evaluation cost and execution time
-   Promoting, rejecting, or rolling back changes based on evidence

## Module 9 --- Online Evaluation and Production Monitoring

**Goal:** Monitor real-world application behavior and use production
evidence to improve future versions.

### Chapter 18. Monitoring Production LLM Applications

-   Online evaluation concepts and use cases
-   Capturing traces and application outputs
-   Monitoring quality, latency, cost, and safety
-   Evaluating sampled production traffic
-   Reference-based and reference-free production metrics
-   Tracing and monitoring with tools such as LangSmith
-   Dashboards, alerts, and investigation workflows
-   Managing privacy and sensitive production data

### Chapter 19. Closing the Evaluation Feedback Loop

-   Identifying recurring production failures
-   Reviewing and labeling failed cases
-   Converting production traces into evaluation examples
-   Updating golden datasets
-   Re-running regression suites
-   Tracking quality across application versions
-   Establishing a continuous evaluation and improvement cycle

## Suggested Practical Artifacts

As I work through the course, I will build and maintain the following
artifacts where relevant:

-   [ ] An evaluation plan defining objectives, datasets, metrics, and
    acceptance criteria
-   [ ] A versioned golden dataset
-   [ ] A model-comparison report
-   [ ] A RAG retriever evaluation suite
-   [ ] A RAG generation and end-to-end evaluation suite
-   [ ] Custom LLM-as-a-Judge metrics
-   [ ] A safety and adversarial test suite
-   [ ] An operational evaluation report covering latency, cost, and
    reliability
-   [ ] A regression-testing workflow
-   [ ] A production monitoring and feedback workflow

## Key Principles

1.  **Evaluate against the use case.** A high benchmark score does not
    guarantee that a model will perform well in a specific application.
2.  **Measure components and the complete system.** Component-level
    tests help isolate failures; end-to-end tests reveal user-facing
    problems.
3.  **Use multiple forms of evidence.** Combine programmatic checks,
    reference-based metrics, human review, and LLM judges where
    appropriate.
4.  **Treat evaluation datasets as maintained assets.** Keep them
    representative, versioned, and updated as new failure cases appear.
5.  **Validate the evaluator.** LLM-based judges can be inconsistent or
    biased and should not be treated as unquestionable ground truth.
6.  **Include operational and safety requirements.** Quality alone is
    not enough if an application is unsafe, too slow, unreliable, or too
    expensive.
7.  **Make evaluation continuous.** Test changes before release and
    monitor behavior after deployment.

## Final Outcome

The objective is to develop a practical, end-to-end evaluation approach
for LLM-powered applications: define what good performance means,
measure it with appropriate methods, diagnose failures, prevent
regressions, and continuously improve the system using evidence from
development and production.
