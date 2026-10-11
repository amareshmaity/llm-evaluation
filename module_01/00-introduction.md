# LLM Evaluations — Introduction and Learning Roadmap

## 1. Overview

This chapter introduces **LLM evaluation (LLM Evals)** as an essential part of building reliable, production-ready applications powered by large language models (LLMs).

The central idea is that building an LLM application is only one part of the work. Before deployment—and continuously after deployment—the application must be evaluated to understand whether it behaves reliably enough for its intended use.

### Learning objectives

After studying these notes, you should be able to:

- Define an AI Engineer in the context of foundation models and LLM-powered applications.
- Explain why LLM evaluation is an important engineering skill.
- Distinguish informal “vibe testing” from a repeatable evaluation process.
- Describe risks that can arise when an LLM application is deployed without adequate evaluation.
- Compare testing traditional deterministic software with evaluating probabilistic LLM applications.
- Identify common dimensions used to evaluate an LLM application.
- Outline the major topics in an LLM evaluation workflow.

<br/>

## 2. What Does an AI Engineer Do?

An **AI Engineer** builds applications and products on top of foundation models, including large language models.

Examples of LLM-powered applications and approaches mentioned in this chapter include:

- Applications built with LangChain
- Retrieval-Augmented Generation (RAG) chatbots
- Agent-based applications
- Frameworks such as LangGraph, CrewAI, and Agno
- Prompt engineering
- No-code tools for building LLM applications

Knowing how to build these applications is important, but it does not by itself establish that an application is reliable or ready for production.

<br/>

## 3. Why Learn LLM Evaluation?

LLM evaluation is the process of assessing an LLM or an LLM-powered application to understand its quality and behavior.

For an application, evaluation helps a team decide whether it is suitable for production deployment. It also provides a structured way to identify problems and compare application versions.

<br/>

### Why it matters

1. **Production readiness:** A working demo is not necessarily reliable enough for real users.
2. **Risk reduction:** Incorrect or unsafe responses can cause financial, legal, operational, or reputational harm.
3. **Interview preparation:** AI and Generative AI interviews may ask how to evaluate RAG systems and agentic AI applications.
4. **Engineering maturity:** Evaluation encourages developers to think beyond a personal project and consider how an application will behave at scale.
5. **Repeatability:** A defined evaluation process makes it possible to test changes consistently rather than relying only on personal impressions.


<br/>


## 4. Vibe Testing: A Weak Evaluation Method

**Vibe testing** means casually trying an LLM application with a few prompts and deciding whether it works based on how the responses feel.

For example:

1. Build a chatbot.
2. Ask it five to ten questions.
3. Notice that the answers look reasonable.
4. Conclude that the application works.

This can be useful for an initial smoke test or informal exploration, but it is not a sufficient evaluation strategy for a production system.

<br/>

### Limitations of vibe testing

- **Informal:** There may be no defined procedure for testing.
- **Subjective:** The conclusion depends on the tester's personal judgment.
- **Not repeatable:** The same questions and assessment criteria may not be used when the application changes.
- **Limited coverage:** A handful of prompts cannot represent the variety of real user inputs and failure cases.
- **Weak production assurance:** A few plausible answers do not demonstrate that an application is safe, accurate, or dependable enough to release.

**Key takeaway:** An application that looks good in a few manual trials has not necessarily been evaluated adequately.


<br/>


## 5. Case Studies: What Can Go Wrong Without Evaluation?

The chapter presents three incidents to illustrate the risks of trusting LLM outputs without sufficient verification. These are examples described in the source material; the notes preserve the chapter's account rather than independently investigating the incidents.

### 5.1 Air Canada: Incorrect Bereavement-Fare Guidance

A customer asked Air Canada's chatbot about its bereavement-fare policy after the death of a family member.

According to the account in the chapter:

- The chatbot incorrectly told the customer to book a ticket at the full price and request a refund later.
- The actual policy required the customer to request the bereavement discount before booking; a later refund was not available under that policy.
- The customer relied on the chatbot's answer and subsequently sought a refund.
- Air Canada refused the refund, and the dispute proceeded to court.
- The chapter explains that the company was held responsible for the information provided by its website chatbot and had to refund the customer.

**Evaluation lesson:** Policy-related answers should be checked for factual accuracy and consistency with authoritative company information. A chatbot should not be released based only on a few plausible responses.

<br/>

### 5.2 Chevrolet Dealership: A $1 Car Offer

The chapter describes a chatbot operated by a Chevrolet dealership.

- A user manipulated the conversation so that the chatbot agreed to follow the user's instructions.
- The user then asked whether a car could be sold for $1.
- The chatbot agreed and made what appeared to be a binding offer in the recorded conversation.
- Screenshots were shared on social media, creating negative publicity.
- The chapter notes that the dealership did not ultimately have to sell the car for $1.

**Evaluation lesson:** Applications should be tested against adversarial prompts and attempts to override their intended behavior. Developers should also consider what commitments a chatbot might appear to make on behalf of a business.

<br/>

### 5.3 Lawyer Using Fabricated Legal Cases

The chapter describes a lawyer who used ChatGPT to find prior cases involving passenger injuries on an airline.

- ChatGPT produced case names and details that were fabricated.
- The lawyer did not independently verify the cases before submitting them to court.
- The cases were found not to exist.
- As described in the chapter, the lawyer and the law firm faced a financial penalty and reputational damage.

**Evaluation lesson:** High-stakes information must be verified against authoritative sources. Fluent, confident language is not proof that a response is true.

<br/>

### Lessons across the three cases

| Risk | What evaluation should help detect |
|---|---|
| Incorrect policy guidance | Whether answers match the current, authoritative policy |
| Prompt manipulation | Whether the application maintains its intended constraints under adversarial inputs |
| Fabricated facts or references | Whether claims and cited sources can be verified |
| Reputational or financial damage | Whether failures could create harmful real-world consequences |

These cases illustrate why testing must go beyond asking a few ordinary questions and judging the answers by feel.


<br/>


## 6. Why LLM Evaluation Is More Difficult Than Traditional Software Testing

The chapter identifies two major differences.

### 6.1 Traditional software is generally deterministic

For a deterministic program, a given input is expected to produce the same output under the same conditions.

**Example:** A calculator receives `2 + 2` and returns `4`.

Because the expected result is explicit, a test can check whether the output matches that result.

<br/>

### 6.2 LLM applications are probabilistic

LLM-powered applications can produce different responses to the same or similar input. Their outputs may vary in wording, structure, detail, or substance.

For example, asking an LLM “What is overfitting in machine learning?” may produce different explanations on different attempts. Multiple explanations can be valid even if they are phrased differently.

This makes evaluation more complicated: it is often not enough to compare the response with one exact expected string.

<br/>

### 6.3 LLM evaluation is multidimensional

Traditional software tests often check whether a defined result is correct. An LLM application may need to be judged across several dimensions at once.

For a RAG chatbot, relevant dimensions may include:

- **Factuality:** Are the claims correct?
- **Completeness:** Does the response cover the important parts of the question?
- **Tone:** Is the response appropriate for the intended audience and situation?
- **Groundedness:** Is the response supported by the retrieved context or other permitted evidence?
- **Latency:** How long does the application take to respond?
- **Cost:** How much does generating the response cost?

The appropriate dimensions depend on the application. A customer-support chatbot, a research assistant, and an internal knowledge assistant may require different evaluation priorities.

<br/>

### Comparison at a glance

| Aspect | Traditional deterministic software | LLM-powered application |
|---|---|---|
| Output behavior | Usually predictable for a fixed input and environment | May vary between attempts |
| Expected output | Often an exact value or clearly defined result | May allow several valid responses |
| Evaluation focus | Correctness against expected behavior | Multiple quality, safety, and operational dimensions |
| Test repeatability | Often straightforward | Requires defined datasets, criteria, and consistent procedures |
| Application-specific criteria | Based on functional requirements | May include factuality, completeness, tone, groundedness, latency, and cost |

**Key takeaway:** LLM evaluation needs explicit criteria that account for variable outputs and the intended purpose of the application.


<br/>


## 7. LLM Evaluation: Two Broad Areas

The chapter distinguishes between evaluating the model itself and evaluating an application built using a model.

### 7.1 Evaluating LLMs

This area examines model capabilities using benchmarks and other evaluation methods. Model releases are often compared by reporting scores on particular benchmarks.

The chapter's roadmap includes studying different categories of benchmarks.

<br/>

### 7.2 Evaluating LLM-powered applications

This area assesses the complete application, not just the underlying model. Depending on the system, evaluation may involve the prompts, retrieval process, tools, agent behavior, output quality, safety, and operational performance.

A model can perform well on a benchmark while an application built around it still fails because of poor retrieval, unsuitable instructions, incorrect tool use, or application-specific constraints.


<br/>


## 8. The LLM Evals Learning Roadmap

The chapter previews the following sequence of topics for the broader learning series.

1. **Introduction to LLM Evals** — Understand what LLM evaluations are through examples.
2. **The LLM Evaluation Landscape** — Explore the main concepts, techniques, and tools, and understand how they fit together.
3. **Evaluating LLMs** — Learn about model evaluation and different categories of benchmarks.
4. **Evaluating LLM Applications** — Learn how to assess applications built on top of LLMs.
5. **Building an Evaluation Pipeline** — Create an evaluation workflow, curate a golden dataset, define rubrics, and run evaluations against an application.
6. **RAG Evaluation** — Evaluate Retrieval-Augmented Generation systems.
7. **Agent Evaluation** — Evaluate agent-based applications and their behavior.
8. **Safety Evaluation** — Assess safety-related behavior and failure cases.
9. **Operational Evaluation** — Evaluate a deployed system's runtime and service performance.

The speaker describes this as a roadmap of roughly ten topics, but the transcript's detailed preview groups the material into the areas listed above; it does not clearly name every topic as a separate numbered item.

<br/>

### 8.1 Golden datasets and rubrics

The planned evaluation pipeline includes two important building blocks:

- **Golden dataset:** A curated set of representative test examples used to assess an application.
- **Evaluation rubric:** Defined criteria for judging the quality of outputs.

Together, these make evaluation more systematic than manually trying a few prompts and deciding whether the results seem good.

<br/>

### 8.2 RAG and agent evaluation

Different application architectures can fail in different ways, so they need evaluation approaches suited to their behavior.

- **RAG evaluation** focuses on the behavior and quality of retrieval-augmented responses.
- **Agent evaluation** focuses on agent-based application behavior.

The chapter introduces these as future topics without yet detailing their individual metrics or procedures.

<br/>

### 8.3 Safety evaluation

Safety evaluation examines whether the application behaves acceptably in situations that could lead to harmful or inappropriate outcomes. The earlier case studies motivate the need to test beyond normal, cooperative user prompts.

<br/>

### 8.4 Operational evaluation

Evaluation continues after deployment. The chapter names operational measurements such as:

- **Latency:** How long a response takes.
- **Tokens per second:** The rate at which tokens are generated.
- **Time to first token:** How long a user waits before generation begins.
- **System load:** The load placed on the system.

A deployed application must be monitored for operational behavior as well as answer quality.


<br/>


## 9. A Practical Mental Model

Use the following sequence when thinking about an LLM-powered application:

1. **Build** the application.
2. **Define** what a good response and acceptable behavior mean for its use case.
3. **Prepare** representative evaluation examples.
4. **Evaluate** outputs using explicit criteria rather than intuition alone.
5. **Investigate** failures and improve the application.
6. **Re-evaluate** after meaningful changes.
7. **Monitor** quality and operational behavior after deployment.

This sequence summarizes the chapter's central message: evaluation is an engineering activity that supports reliable deployment, not an optional final check.


<br/>


## 10. Key Takeaways

- An AI Engineer builds applications and products on top of foundation models such as LLMs.
- Building an application does not prove that it is ready for production.
- Vibe testing—trying a few prompts and judging responses by feel—is informal, subjective, and insufficient as a production evaluation method.
- Unverified LLM outputs can cause financial, legal, and reputational harm.
- Traditional software is often evaluated against deterministic expected results, while LLM applications can produce variable outputs.
- LLM application evaluation is multidimensional: factuality, completeness, tone, groundedness, latency, and cost are examples.
- Evaluation criteria should reflect the application's intended purpose.
- A structured evaluation pipeline can use a golden dataset and defined rubrics.
- RAG, agents, safety, and operational behavior require focused evaluation.
- Evaluation does not stop at deployment; operational performance must continue to be assessed.


<br/>


## 11. Review Questions

1. What is an AI Engineer, as defined in this chapter?
2. What is vibe testing, and why is it insufficient for production readiness?
3. What evaluation lessons can be drawn from the Air Canada chatbot incident?
4. Why should an LLM application be tested against adversarial prompts?
5. Why is it dangerous to use LLM-generated legal references without verification?
6. How does deterministic software differ from a probabilistic LLM application?
7. Why can exact-string matching be inadequate for evaluating LLM responses?
8. Name at least five dimensions that may be used to evaluate an LLM application.
9. What is the difference between evaluating an LLM and evaluating an LLM-powered application?
10. What roles do a golden dataset and an evaluation rubric play in an evaluation pipeline?
11. Why does evaluation need to continue after deployment?
12. Which operational metrics does the chapter mention?

<br/>

## 12. Self-Assessment Checkpoint

You should be able to explain, in your own words:

- Why a successful demo is not enough to establish production readiness.
- Why LLM evaluation is more complex than testing a calculator.
- How systematic evaluation can help detect failures before users encounter them.
- How evaluation criteria change depending on the application.
- Why an application needs both quality evaluation and post-deployment operational monitoring.
