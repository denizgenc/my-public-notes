# Exploring Artificial Intelligence Use Cases and Applications
Found at https://skillbuilder.aws/learn/A3U2U9VGMX/exploring-artificial-intelligence-use-cases-and-applications/M12JQTFKB5

# Examples of Real-World Use Cases
## Media and entertainment
- GenAI used to make content - scripts, imagery/sound as well, also journalistic use (summaries of
  data/news, or entire articles)
- VR - apparently "AI can create immersive and interactive virtual environments"

## Retail
- Summaries of user reviews
- Optimising prices
- "Virtual try-ons" - AI generated models of shoppers wearing clothes they might want to buy
- Store layout optimisation

## Healthcare
- AWS HealthScribe - automatically generates clinical notes based on patient/doctor conversations
- Personalised medicine - using input data such as genes, lifestyle, existing conditions, etc
- Medical imaging - enhancing existing images, or "reconstruct, or even generate" them (?!)

## Life sciences
- Medicine research - generating new molecules for drugs
- Protein folding
- Synthetic biology - generate designs for things like "engineered organisms or biological circuits"


## Financial services
- Fraud detection - using vast amounts of input data to find patterns that indicate fraud
- Simulating markets to make investment choices
- Debt collection - get AI to do the difficult talking to debtors!!

## Manufacturing
- Predictive maintenance - analyse failure rates in systems to figure out the best time for
  maintenance, to minimise production downtime
- Process optimisation
- Product design - give an AI various constraints (cost, materials, performance) and have it figure
  out something for you
- Material science - AI can help with this apparently

# Examples of AI Applications
## Computer vision
A field of AI that allows computers to recognise things in photos and videos. Deep learning has
really helped with this.
- Image classification
- Object detection
- Image segmentation

Used by the following industries (among others):
- Automotive: self driving
- Healthcare: medical imaging (is that a tumour?)
- Security: facial recognition in either public surveillance or home security cameras

## Natual language processing
Field of AI, again revolutionised by deep learning.
- Text classification
- Sentiment analysis
- Translation
- Text generation

Used by the following industries (among others):
- Insurance: for OCR (?)
- Telecoms: for... I'll just paste what they've written directly here: "Telecommunication companies
  use NLP to analyze customer text messages and suggest personalized recommendations."

  what the fuck is this? What does this have anything to do with telecoms specifically beyond the
  "text messages" part? Surely this applies to any company that has a direct, text-based
  communication channel with their customers? And what personalised recommendations can
  telecommunications companies make for customers? Different billing plans???? This shit is AI
  generated for sure
- Education: students can use AI chatbots for Q&A, sure, whatever

## Intelligent document processing (IDP)
This is an application where you take unstructured data, and:
- extract and classify information
- generate summaries
- provide actionable insights

Used by the following industries (among others):
- Financial services: OCR, also identifying incomplete/incorrectly filled information (?)
- Legal: OCR and NLP. Not sure I'd really want a lawyer who's acting based on a summary of the data
  that surfaced from discovery lol
- Health: OCR and processing documents (claims and doctor's notes)

## Fraud detection
Used by the following industries (among others):
- Financial services: identity verification, anti-money laundering (AML)
- Retail
- Telecomms

# Machine Learning
Remember ML is a subset of AI. Creating systems to solve problems through the application of
statistics to large datasets - without explicit programming or even any specific idea about how that
system would be implemented.

## When AI an ML are appropriate solutions
Use AI when:
- It's hard to distil the problem into something that can be programmed (e.g. computer vision)
- The scale of the problem is too big for individuals (e.g. spam filtering)

## Alternative approaches to AI and ML
Basically, try to make sure that you're not forcing the use of these technologies for problems that
can be better (and more cheaply) solved using more traditional approaches.

# Machine Learning Techniques and Use Cases
Three broad ML techniques:
- Supervised learning (ML is trained on labelled data)
- Unsupervised learnin (ML is trained on unlabelled data)
- Reinforcement learning (ML is given a performance score, and only some of the data is labelled)

There's different use cases for each of the above techniques.

## Supervised learning use cases
Widely applicable.

"Supervised" because the model requires a supervisor - in this case, the labelled training data

Basically learns what sort of response is required.

Two categories of supervised learning:
- **Classification**: assign labels/categories to new data based on trained model
  - image classification
  - diagnostics
  - fraud detection
- **Regression**: Predicting continuous/numerical values based on input variables. For modelling a
  dependent variable based on independent variable(s).
  - Weather forecasting
  - Market forecasting

## Unsupervised learning use cases
The data is unlabelled - perhaps because humans can't really sort it. The point of unsupervised
learning is to try to find *structures* or *patterns* inside the training data, then apply it to new
data.

Two types of unsupervised ML:
- **Clustering**: Grouping data into different clusters based on similar features
  - Customer segmentation, targeted marketing, recommendation systems, etc
- **Dimensionality reduction**: Reducing the number of features/dimensions in a dataset while
  preserving the patterns/distinctions within
  - Big data visualisation
  - Compression
  - Structure discovery (? what does this mean)

## Reinforcement learning use case
This is where previous training iterations feeds back to improve the model. This allows the model to
"continuously improve". Agent continuously learns through trial and error.

Helpful when **when the reward of a desired outcome is known, but the path to achieving it isn't**.
E.g. AlphaGo.

Model gets **rewards or penalties** depending on its progress towards the goal, to allow it to
refine its process in future iterations.

AWS DeepRacer is an example - car tries to stay on track. Rewarded for staying on track and going
faster.

# Generative AI
Subset of deep learning. Can make "new" stuff based on what it's trained on.

# Capabilities of Generative AI
- **Adaptability**: learning from data and generating content based on that data. This allows it
  to be used in various industries, in many contexts.
  - This also enables **personalisation**
- **Responsiveness**: "real time" response from GenAI models. Useful for chatbots.
- **Simplicity**: "Generative AI can simplify complex tasks by automating content creation
  processes", such as by generating "human-like text". How does that simplify a complex task
- **Creativity and exploration**: Let GenAI bullshit for you
- **Data efficiency**: Some models can learn from small amounts of data and then generate more based
  on it. (Funnily enough I assume the models best at this would be the biggest ones that require a
  lot of memory for inference or whatever)
- **Scalability**: GenAI can shit out a lot of garbage quickly!!!!

# Challenges of Generative AI
GenAI trained on PII might expose it in later outputs.
- Mitigation: Conduct audits to assess the data used for training. anonymise data during training.
  Also implement "cybersecurity measures, such as encryption and firewalls" (lol)

GenAI might output politically incorrect things
- Mitigation: Curate training data by removing problematic things beforehand. Test and evaluate all
  models before deploying them to production. Use guardrail models to detect toxic output and filter
  it out.

GenAI might "hallucinate" 🙄
- Mitigation: "Teach users that everything must be checked." HAHAHAHAHAHAHAHAHAHAHAHA

Model may generate different outputs for the same input, which might be a problem if you need
something reliable.
- Mitigation: Run tests on the model before deployment to see if this is an issue; if so, tune the
  model (turn down temperature, etc)

# Factors to Consider When Selecting a Generative AI Model
These are
- Model types
- Performance requirements
- Capabilities
- Constraints
- Compliance
- Cost

## Models
Many types. Small example list
- Jurassic-2 (AI21 labs) - text generation, summarising, chat
- Amazon Titan (Amazon) - text generation, information extraction (OCR?), search, "embeddings" (?)
- Claude (Anthropic) - Text generation, translation, summarisation, code explanation + generation,
- Stable Diffusion (Stability AI) - image generation, sharpening
- Llama (Meta) - Text generation, summarisation

## Performance requirements
How accurate and reliable is the model? You need to check manually.

## Constraints
This can be computational constraints (how much CPU/GPU/TPU power you have, and total memory of the
system), data constraints (amount and quality of data), and where it is going to be deployed
(on-prem vs cloud).

Different models have different computational requirements; and the computation available is
constrained by how and where it's deployed and the amount of money available for the deployment.

## Capabilities
Some models are better at outputting text while others are better at generating images, etc. Other
modes to consider (audio generation, multi-modal stuff - text to image, sound to text, etc).

Important to consider your use case and select the model accordingly.

## Compliance
Moral concerns.

Consider compliance, especially in healthcare, finance, and legal. Will you be submitting PII to a
third party? Is there a potential for this PII to get leaked?

## Cost
Size and speed of the model are two tradeoffs. Bigger models are more accurate, but are expensive
and inference is slow.

However the cost might be offset by savings in salaries you will no longer have to pay to generate
slop.

# Business Metrics for Generative AI
You're telling me you have to consider whether an AI application has a positive return on
investment???? 🧠🧠🧠🧠🧠

There are also other metrics though:
- User satisfaction
- Average revenue per user (ARPU)
- Cross-domain performance - in case the model needs to perform effectively across different
  domains/industries
- Conversion rate - can the AI get people to buy things, etc
- Efficiency - does the AI help optimise processes/systems/designs
