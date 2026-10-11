# Chapter 5. Offline and Online Evaluation

## Course Outline

### 5.1. Understanding Offline Evaluation

- Definition and purpose of offline evaluation before deployment.
- Golden datasets, expected answers, and controlled evaluation.
- The UPSC answer-grading application as an example.
- Why testing is necessary before releasing an LLM application.

### 5.2. Three Major Benefits of Offline Evaluation

- Pre-release testing and release gating: Using evaluation scores and thresholds to approve or block deployment through CI/CD.
- Version comparison: Comparing different LLM models, prompts, rerankers, vector databases, and application architectures using the same evaluation dataset.
- Regression testing: Detecting unintended performance degradation after changing prompts, models, retrieval components, or other system components.
- Example: Improving a chatbot's politeness without damaging pricing accuracy or refund responses.

### 5.3. Production Risks That Offline Evaluation Cannot Fully Capture

- Unanticipated user inputs, ambiguous questions, multilingual queries, angry messages, edge cases, and adversarial prompt injections.
- Emergent and systematic failures at scale, including latency spikes under concurrent traffic and biases visible across many conversations.
- Data drift caused by changing prices, policies, curricula, documents, and user behavior.
- Why an outdated golden dataset can produce misleading evaluation results.

### 5.4. Understanding Online Evaluation

- Definition: Evaluating an application using live production traffic after deployment.
- Why online evaluation generally operates without a predefined answer key for each incoming query.
- How online evaluation complements offline evaluation rather than replacing it.
- What each approach tells us:
  - Offline evaluation: Is the application performing correctly against defined test cases?
  - Online evaluation: Is the application behaving normally in production?

### 5.5. Offline vs. Online Evaluation: Key Differences

- Deployment timing: before versus after release.
- Evaluation data: fixed golden dataset versus live production traffic.
- Reference answers: available versus generally unavailable.
- Input coverage: anticipated cases versus unpredictable real-world inputs.
- Primary purpose: correctness and regression testing versus production monitoring and anomaly detection.
- Cost, execution speed, repeatability, and scalability.
- Appropriate use cases for each approach.

### 5.6. Measuring Production Health Without a Golden Dataset

- Correctness versus normality: understanding the distinction.
- Using baseline distributions to identify unusual production behavior.
- Example: Comparing weekly score distributions from the UPSC answer grader to detect unexpectedly high or low scores.
- Using user feedback signals, such as thumbs-up and thumbs-down ratings, as indirect quality indicators.
- Evaluating reference-free properties such as faithfulness using retrieved context and generated answers.

### 5.7. Building an Online Evaluation Pipeline: Logging

- Why every production conversation must be captured for later evaluation.
- Structured conversation and trace records:
  - Conversation, turn, user, and session identifiers.
  - Timestamps, user queries, retrieved context, and model responses.
  - Latency, token usage, cost, errors, and status codes.
  - Feedback, repeated questions, and escalation events.
- Engineering requirements for logging:
  - Non-blocking execution.
  - Durable, queryable storage.
  - Attachment of delayed feedback and escalation signals.
  - Privacy protection and PII masking before storing sensitive data.
- Example: Logging chatbot conversations using LangSmith.

### 5.8. Captured vs. Computed Evaluation Signals

- Captured signals: Values collected directly from the application or user.
  - Examples: Latency, token usage, cost, thumbs-up/down, and error status.
- Computed signals: Metrics that require a separate evaluator or calculation.
  - Examples: Hallucination, faithfulness, answer relevance, correctness, toxicity, bias, and fairness.
- Choosing the right signals based on application quality, safety, and operational requirements.

### 5.9. Monitoring Captured Signals: Dashboards and Alerts

- Aggregating operational metrics over time windows such as one hour, one day, or one week.
- Visualizing latency, error rates, LLM call counts, and costs.
- Establishing baseline values and alert thresholds.
- Triggering notifications through tools such as Slack or email when thresholds are breached.
- Example: Detecting increased latency after a course launch and responding by scaling infrastructure.
- Why aggregated trends are often more informative than individual conversations.

### 5.10. Evaluating Computed Signals in Production

- Building a reference-free evaluator for live conversations.
- Using LLM-as-a-Judge to evaluate hallucination with the user query, retrieved context, generated response, and an evaluation rubric.
- Converting individual evaluator results into aggregate metrics such as hallucination rate.
- Sending computed metrics to dashboards and alerting systems.
- Example: Detecting potential hallucinations in a production RAG chatbot.

### 5.11. Sampling Strategies for Cost-Efficient Evaluation

- Why evaluating every production conversation with an LLM judge can be expensive.
- Random sampling as a basic approach.
- Stratified sampling: Dividing conversations into categories and sampling more heavily from high-risk or problematic groups.
- Prioritizing:
  - Thumbs-down conversations.
  - Abruptly terminated conversations.
  - Repeated or rephrased questions.
  - Human escalations.
  - Pricing, refund, fee, and admission-related questions.
- Reducing evaluation costs while improving the chances of identifying meaningful failures.

### 5.12. The Complete Online Evaluation Workflow

- Logging production traces.
- Sampling conversations.
- Running evaluators on selected traces.
- Aggregating computed metrics.
- Visualizing results on dashboards.
- Triggering alerts when thresholds are crossed.
- Running multiple evaluation pipelines for quality, safety, and operational monitoring.
- Example: Configuring hallucination, toxicity, security, and PII-leakage evaluators in LangSmith.

### 5.13. Using LangSmith for Offline and Online Evaluation

- Creating datasets and running offline experiments.
- Applying evaluators to production traces for online evaluation.
- Configuring an LLM judge, evaluation model, rubric, and output format.
- Understanding the difference between evaluating traces and evaluating a fixed dataset.
- Using tracing, monitoring, dashboards, alerts, annotations, and dataset management.

### 5.14. Closing the Loop: Production Feedback to Offline Evaluation

- Identifying problematic conversations from production logs.
- Reviewing, annotating, and labeling failures.
- Adding representative failure cases to the golden dataset.
- Re-running offline evaluations against the updated dataset.
- Fixing issues, validating changes, and deploying improved versions.
- The continuous improvement cycle: offline evaluation → deployment → online monitoring → failure analysis → dataset updates → offline re-evaluation.

### 5.15. Practice and Revision

- Compare offline and online evaluation using a practical LLM application.
- Design a release gate using an evaluation score threshold.
- Identify regression risks after a prompt or model change.
- Design a logging schema that protects PII.
- Classify production signals as captured or computed.
- Design a stratified sampling strategy for a RAG chatbot.
- Build a monitoring plan using baselines, dashboards, and alerts.
- Trace a production failure through the feedback loop.

Checkpoint: Design an end-to-end evaluation strategy that combines offline testing, production monitoring, cost-efficient evaluation, privacy safeguards, and continuous improvement.
