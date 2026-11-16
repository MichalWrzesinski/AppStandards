# Standard for using AI in applications, for AI agents

Version: 1.1.1  
Date: 2026-11-15

## Scope and application

Apply this standard when building application features that use language models (LLMs) or custom machine learning models. This document covers provider and model selection, isolation, data protection, costs, response safety, resilience, prompts, quality evaluation, and transparency for users.

This standard does not concern AI agents that write code. It concerns AI as part of the application being built.

Apply together with the [core principles](../core.md). [Routing](../routing.md) determines the other instructions to use.

## Governing principles

- **Model output is untrusted data.** Treat it like user input: validate it, do not execute it, and do not insert it without encoding.
- **Keep costs under control.** Every AI feature has limits and a known cost at the expected scale.
- **Measure quality rather than assume it.** An AI feature has an evaluation dataset and a quality threshold as an acceptance criterion.
- **Operation without AI.** Every AI feature has defined behavior when the model is unavailable.

## Language models

### Provider and model

- The default provider is OpenAI. Changing providers is a separate task.
- Place provider calls in an adapter behind an interface, according to the backend standard. The rest of the application does not know provider details.
- Use the provider's official library.
- Check available models, their capabilities, and prices in current provider documentation, not from memory.
- For each feature, select the cheapest model that meets the quality threshold in evaluation. Use a more capable model only where a less capable one fails to meet the threshold.
- Pin a specific model version. Changing models is a separate task that includes running evaluations.

### Isolation

- Only the backend makes model calls. The API key must not reach the frontend, mobile application, repository, or logs.
- Only the component making calls has the key. Where possible, calls run in a separate process, e.g. a background job, rather than the public API process.
- Users do not control system instructions, model selection, or call parameters. User text reaches the model only as data, in a clearly separated part of the message.
- Project documentation may define a different isolation approach.

### Data sent to the provider

- Send only data necessary for the task. Remove or mask personal data if it is unnecessary for the result.
- Do not send secrets, passwords, tokens, or credentials.
- Use a provider and plan whose terms prohibit training models on data sent through the API. Personal data requires a GDPR-compliant data processing agreement.
- The application's privacy policy informs users about data transfers to the AI provider.
- Do not log full prompts and responses containing user data. Log the call identifier, model, token count, duration, and cost.

### Costs and limits

- Apply per-user and global call limits, e.g. daily limits.
- Limit input length and maximum response length.
- Cache results of identical queries when the result does not depend on the user or time.
- Configure an alert when the monthly budget is exceeded, e.g. in the provider dashboard and application monitoring.
- When selecting technology, prepare an estimate of monthly AI costs at the expected scale.

### Response safety

- When the application needs structured data, request a response in a defined schema, e.g. JSON Schema, and validate it before use. Treat a response that does not match the schema as an error.
- Do not execute response content as code, a query, or a command. Do not insert it as raw HTML; encode it like any user content.
- When a model can call application functions (tools), the application checks user permissions for every call. The model does not perform actions with significant consequences, e.g. payments, data deletion, or sending messages, without user confirmation.
- Assume user input and external content, e.g. documents, websites, and emails, may contain instructions for the model (prompt injection). Do not rely on system instructions to make the model ignore injected instructions; the application enforces restrictions.

### Resilience

- Apply the backend standard's rules for external service integrations: timeouts, only safe retries, and controlled outcomes instead of `500` errors.
- Define feature behavior without AI, e.g. a temporary unavailability message, a simplified version, or later background execution.
- Run long calls in the background or stream responses so users do not wait without information.

### Prompts

- Prompts are code: store them in files in the repository, in one place, e.g. `prompts/`, and review them like code.
- A prompt contains the model's role, task, constraints, response format, and examples if they improve the result.
- Do not assemble prompts from fragments scattered across the code. Insert data into explicitly marked template locations.

### Quality assessment (evaluations)

- Every AI feature has an evaluation dataset in the repository: example inputs with expected response properties, e.g. a correct category, required fields, and absence of prohibited content.
- The dataset includes typical cases, edge cases, and abuse attempts, e.g. prompt injection.
- Run evaluations whenever a prompt, model, or parameters change, rather than on every PR, because evaluations incur costs.
- The quality threshold, e.g. the proportion of correct responses, is a task acceptance criterion. Record the evaluation result in the task report.
- Ordinary automated tests use a provider test double and do not call a real model.

### Transparency for users

- Users know they are interacting with AI and which content AI generated, in accordance with the EU AI Act's transparency obligations.
- For content that may influence user decisions, inform users that responses may contain errors.

## Custom machine learning models

### When to use a custom model

Use a custom model only when a language model is too expensive, too slow, or insufficiently accurate for a specific, repeatable task, e.g. classification. Record the decision with a comparison.

### Data and evaluation

- Split data into training and test sets before training. The test set participates in neither training nor parameter selection. Select parameters using cross-validation on the training set.
- When data has structure, e.g. multiple messages from one ticket or a time sequence, split it so related records remain in one set; split time-based data chronologically.
- Compare the model with a simple baseline, e.g. always predicting the most frequent class.
- Metrics with thresholds are task acceptance criteria. The evaluation report contains overall metrics, per-class metrics, and examples of errors.
- When the model is to return “I don't know,” choose a confidence threshold using validation data and also evaluate examples outside known categories.

### Reproducibility and versions

- Training runs through a script in the repository, with a fixed random seed and pinned library versions.
- Every trained model has a recorded data version, e.g. a file hash, parameters, and evaluation results.
- Store large model and data files outside Git, e.g. in the project's file storage, with a version in the name.
- Retraining on new data is a separate task that compares results with the previous version.

### Model security

- Load only model files produced by your own training. Formats such as `pickle` and `joblib` can execute code when loaded.
- Training data contains no personal data unless the project permits it and it has been protected according to the security standard.
