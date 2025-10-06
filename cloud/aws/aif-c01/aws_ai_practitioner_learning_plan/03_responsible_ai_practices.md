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

# Challenges in Traditional and Generative AI
## Biases
### Accuracy of models
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

### Strategies to remediate bias/variance
- Cross-validation: train several models on subsets of the data; then evaluate on complentary
  subsets
- Increase training dataset size
- Regularisation: penalises extreme weight values, which reduces overfitting
- Simpler models: This reduces overfitting. However if a model is underfitting, it's possible that
  it's too simple.
- Dimension reduction: reduces overfitting (?)
- Stop training early: reduces overfitting

## Challenges of generative AI
- Toxicity
- Hallucinations
- IP theft
- Plagiarism/cheating
- Social disruption

# Core dimensions of Responsible AI
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

## Business benefits of responsible AI
- Increased trust and reputation
- Regulatory compliance
- Mitigating risks
- Competitive advantage (in terms of marketing, presumably)
- Improved decision-making (e.g. dimensions of fairness, safety and controllability prevent biased
  decisions)
