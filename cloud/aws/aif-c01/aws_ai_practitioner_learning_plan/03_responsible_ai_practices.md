# Responsible Artificial Intelligence Practices
Found at https://skillbuilder.aws/learn/1H631ZWCTP/responsible-artificial-intelligence-practices/BN51NEFJNG

# Aims
- Define responsible AI
  - Challenges
- How to develop responsible AI systems
  - Related AWS tools, services
- Transparent & explainable models

# Responsible AI
Need to consider throughout lifecycle of an AI application.

Responsible AI means:
- Transparent and accountable. Monitoring/oversight
- Developed by teams with expertise in responible AI
- Built following responsible AI guidelines

All types of AI need to be used responsibly (not just generative models)

## Challenges in Traditional and Generative AI
### Biases
#### Accuracy of models
Biggest problem developers face is accuracy. Both traditional and generative models can only output
based on the dataset they've been trained on, therefore devs need to be aware of bias and variance
in models.

Bias results from the model "missing important features of the dataset". Measured by the difference
between the expected predictions of the model and the true values we're trying to achieve; smaller
difference == lower bias. A high bias means that the model is underfitted.

Variance relates to how sensitive the model is to fluctuations/noise in the training data. The model
might misinterpret noise as a meaningful feature of the dataset, instead of something to be ignored.
This is basically overfitting.

Bias vs variance is an issue of under- and overfitting. Hence it's difficult to balance.

#### Strategies to remediate bias/variance
- Cross-validation: train several models on subsets of the data; then evaluate on complentary
  subsets
- Increase training dataset size
- Regularisation: penalises extreme weight values, which reduces overfitting
- Simpler models: This reduces overfitting. However if a model is underfitting, it's possible that
  it's too simple.
- Dimension reduction: reduces overfitting (?)
- Stop training early: reduces overfitting

#### Challenges of generative AI
- Toxicity
- Hallucinations
- IP theft
- Plagiarism/cheating
- Social disruption

## Core dimensions of Responsible AI
- Fairness: basically social considerations (making sure the AI system is not discriminatory and
  complies with the law)
- Explainability: capability of the model to explain or justify its decisions
- Privacy and security: ensuring that the data that is used in either training or inference of the
  AI is controlled properly
- Transparency: communicating the development, capabilites, limitations etc of an AI system to
  enable informed use of the system
- Veracity and robustness: mechanisms of the AI system to ensure it runs reliably, accurately and
  safely even in "uncertain environments"
- Governance: processes to define, implement and enforce reponsible AI practices within an
  organisation; e.g. having a process in place in case some sort of IP issue arises with the system
- Safety: development of the system that ensures that it is responsible and beneficial to users and
  society as a whole. Ensuring the system does not do accidental harm regardless of the inputs or
  environment it is run in
- Controllability: ability to monitor and align the AI system's behaviour to human values.

### Business benefits of responsible AI
- Increased trust and reputation
- Regulatory compliance
- Mitigating risks
- Competitive advantage (in terms of marketing, presumably)
- Improved decision-making (e.g. dimensions of fairness, safety and controllability prevent biased
  decisions)

# Developing responsible AI systems
## Amazon Services and tools for Responsible AI
Both Amazon Bedrock and SageMaker allow you to evaluate FMs. They have **metrics for accuracy,
toxicity, etc**, and allow for both automated evaluation and human evaluation.
- The Bedrock version is called "Model evaluation"
- On SageMaker, it's called "AI Clarify".
  - AI Clarify also can spot potential bias (gender, age, etc) in datasets
  - For certain use cases (tabular, NLP, computer vision), it also can provide scores for what
    features of the input contributed to the model's prediction.

### Guardrails for Amazon Bedrock
This is something you can implement yourself when using FMs with Bedrock. Allows you to:
- Block certain topics
- Filter content (hateful, sexual, violent, etc)
- Redact PII

You can make multiple guardrails with different configurations for separate use cases.

### Monitoring and human reviews
- SageMaker Model Monitor
- Amazon Augmented AI (A2I) -> builds workflows for reviewing ML predictions

### Governance
- SageMaker Role Manager - RBAC for SageMaker
- SageMaker Model Cards
- SageMaker Model Dashboard - Grafana for AI or something

### Transparency
AWS AI Service Cards -> a form of AI documentation.

## Responsible Considerations to select a model
- Narrow down your use case
- Choose a model based on that use case
- Consider sustainability when picking a model as well

## Responsible preparation for datasets
Make sure dataset is balanced/unbiased

Ensure that the data collection is inclusive.

Curate your data in 3 steps:
1. Data preprocessing
   - Checking if your data is accurate and unbiased
2. Data augmentation
   - Add data from underrepresented groups, if necessary
3. Regular auditing
   - Check back frequently to ensure dataset is good, re-curate as necessary

# Transparent and Explainable AI models
Transparency answers **how** a response came about.

Explainability answers **why**.

Transparent & explainable contrasts with black-box models.

Benefits:
- Increased user trust
- Eases debugging and optimisation
- Improves understanding of model's decision-making process (where that is important, e.g. medical
  use cases)

## Solutions for transparent and explainable models
There's no standard solution.

Here's some approaches:
- Explainability frameworks: SHapley Additive exPlanations (SHAP), Local Interpretable
  Model-Agnostic Explanations (LIME)
- Documentation
- Monitoring/auditing
- Human oversight

### AWS tools for transparency
Transparency:
- AWS AI Service Cards
- SageMaker Model Cards

Explainability
- SageMaker AI Clarify
- SageMaker AutoPilot

## Model Trade-offs
### Interpretability trade-offs
Interpretability is access into a system so you can figure out why an output occurs, based on the
weights/features of the AI system. Different from "explainability" in that explainability is
analysing the decision process in human terms, not mathematical.

The more complex an AI system is, the more difficult it is to interpret. However, we also get an
increase in performance. There's a sliding scale between "high interpretability, low performance"
(e.g. linear regression model) and "low interpretability, high performance" (e.g. a deep neural
network)

### Safety and transparency trade-offs
- More complex models are less transparent, but more accurate
- Techniques that improve data privacy can improve safety but reduce transparency

### Model controllability
More controllable usually means less complex -> potentially less accurate. But controllable is nice
since it allows us to fine tune responses (for safety and accuracy).

## Principles of Human-Centred Design for Explainable AI
### Design for amplified decision-making
For high stress/pressure situations. Consider:
- Clarity
- Simplicity
- Usability

### Design for unbiased decision-making
Consider:
- Transparency
- Fairness
- Training

### Design for human and AI learning
Consider:
- Cognitive apprenticeship - allow AI systems to learn from human experts, and/or learn from real
  (or simulated) scenarios
- Personalisation
- User-centred design

### Reinforcement learning from human feedback (RLHF)
Using human feedback in the reinforcement (RL) learning reward function.

You can use **SageMaker Ground Truth** for this.
