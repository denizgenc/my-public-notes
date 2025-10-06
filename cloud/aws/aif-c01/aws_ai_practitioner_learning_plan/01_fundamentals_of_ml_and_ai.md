# Fundamentals of Machine Learning and Artificial Intelligence
Course link: https://explore.skillbuilder.aws/learn/courses/19578/fundamentals-of-machine-learning-and-artificial-intelligence/lessons/153623/fundamentals-of-machine-learning-and-artificial-intelligence

# Intro
Generative AI -> Focus on creating new things from existing materials. It's a subset of Deep
learning (which is itself a part of machine learning).
- Deep learning is the application of neural networks for machine learning (presumably to
  distinguish from other ML approaches, such as evolutionary/genetic ones?)

AI & ML are said to be about "analy[sing] and interpret[ing] data"; i.e. they do not generate
something new.

Different types of GenAI: Text (including code), images (including video?), sound (voice synthesis).

Ethical concerns around Gen AI: biases, privacy, responsible use.

# Machine Learning Fundamentals
3 step process:

**Collecting data -> applying machine learning to data -> creating a model**

## Training data
Garbage in, garbage out. Quality of the data is decisive in the performance of the model trained on
it.

Datasets may be labelled or unlabelled.

There's also structured and unstructured data:
- Structured: Tabular data (CSVs, etc); time-series data
- Unstructured: Text (blog posts, articles, prose, etc); image data, etc.

Note that an unstructured data format *might still be labelled*.

## Machine learning process
Three broad categories:
- **Supervised learning** - when algorithms are trained on *labelled data*. Aim is to increase
  fitness for a test dataset.
- **Unsupervised learning** - when algorithms are learning from *unlabelled data*. Aim is to find
  patterns within the input data.
- **Reinforcement learning** - only some of the training data is labelled. Model gets *feedback by
  being rewarded or penalised*, and learns over time based on this.

## Inferencing
This is putting the model to use to make predictions or decisions. Two types of inferencing:
- **Batch inferencing** - model used for a set of data at a single point in time. Used when accuracy
  is valued over latency.
- **Real-time inferencing** - model used on data that "continuously" streams in. Examples: chatbots,
  self-driving cars.

# Deep Learning Fundamentals
Basically machine learning where you use a neural network.

## Neural networks
The nodes in a neural network belong to different "layers" that correspond to different things.
- **Input layer**
- **Hidden layer(s)**
- **Output layer**

Each node in a layer has a connection (edge) to every node in adjacent layers, and the "strength" of
this connection is adjusted during the training process.

## Applications of deep learning
Deep learning has been used in computer vision (identifying objects) and natural language processing
(NLP - sentiment analysis, language generation).

# Generative AI Fundamentals
The reason GenAI has come about recently is because there's been a large amount of investment into
it -> more people hired to work on R&D as well as training, as well as spending on compute.

## Foundation models (FM)
Models that are trained on "internet-scale data". You can use an FM as a basis for further training
and specialisation (i.e. the same FM can be used for text generation or sentiment analysis).

**Amazon Bedrock** is a service that provides access to a bunch of FMs.

#### FM Lifecycle
1. **Data selection** - scraping huge amounts of unlabelled data
2. **Pre-training** - self-supervised learning (as opposed to traditional ML models). FM generates
   its own labels for data. After initial pre-training, FM be trained on additional data - this is
   **continuous pre-training**.
3. **Optimisation** - techniques such as prompt engineering or retrieval-augmented generation (RAG).
4. **Evaluation** -  checking if the model meets requirements
5. **Deployment** - Model put into production. Might involve integrating model into existing
   software systems.
6. **Continuous improvement** - Using feedback from users and SMEs regarding the performance of the
   model to inform future iterations. Future iterations may be produced via fine-tuning, continuous
   pre-training, or re-training.

### Different types of foundation models
#### Large language models
Can be based on a variety of architectures; most common now are transformers.

The units of text that LLMs process are called **tokens**. Tokens are usually a single word, but
they can also be phrases or punctuation.

Tokens are assigned an **embedding**, which is basically just a big vector. Semantically related
tokens will have embeddings that are close to one another. These embeddings develop over the course
of training.

#### Diffusion models
Basically denoising algorithms, mostly used for images.

They are trained via **forward diffusion** (repeatedly adding noise to an input until there is
no signal left) followed by **reverse diffusion/sampling** (taking the noise from the forward
diffusion step and generating a new signal).

#### Multimodal models
Can process and generate different types of data simultaneously. These have to learn how these
different modes of data are related - i.e. what image represents the text "brown horse" (and vice
versa - what text would be associated with a photo of a brown horse, how would that be different to
an illustration of one, etc).

#### Generative adversarial networks (GANs)
Two neural networks competing against each other:
- **Generator**: A diffusion model (takes noise and turns it into signal)
- **Discriminator**: Takes real data from training set and data from the generator and tries to
  identify what's real.

Training involves the generator throwing stuff at the discriminator until the latter is no better
than a coin flip

#### Variational autoencoders (VAEs)
Combines autoencoders (a type of neural net) with variational inference (Bayesian statistics). Two
parts:
- **Encoder**: Encodes input data into a lower-dimensional "latent space".
- **Decoder**: Takes the latent representation and reconstructs it into the input data.

The aim is for this latent space to conform to a given (usually Gaussian) distribution; once this
happens, we can generate "new" data by sampling from it and passing it into the decoder.

## Optimising model outputs
There are many techniques that can be used to refine a foundation model. We cover three:
- **Prompt engineering**
- **Fine-tuning**
- **Retrieval-augmented generation (RAG)**

The fastest and cheapest is prompt engineering.

### Prompt engineering
Basically "sets the stage" for the model in order to make it behave in a certain way.

There's different elements to a prompt:
- **Instructions** - what the FM needs to do.
- **Context**
- **Input data** - the input that you want a response for.
- **Output indicator** - the output you want from the FM; perhaps in a certain structure.

### Fine-tuning
Supervised learning using a specific dataset. This **modifies the weights** of the FM.

Two methods for fine-tuning a model:
- **Instruction fine-tuning** - uses examples of how the model should respond to certain
  instructions. **Prompt tuning** is a a type of instruction fine-tuning.
- **Reinforcement learning from human feedback (RLHF)** - provides human feedback, to align the
  model to human preferences.

This is where you can dump a bunch of data specific to your project/company/field into the model to
refine it for your needs.

### Retrieval-augmented generation (RAG)
Where you supply relevant data as context to produce responses based on that data.

Similar to fine-tuning, but the difference is **RAG doesn't modify the weights** of the FM.

Basically you're saying "given this codebase [my-big-project.tar.gz] ..." and then asking it code
related questions. Or whatever

# AWS Services
## AWS AI/ML services stack
Like most things in AWS, this goes from (in some sense) less to more managed offerings.
- **ML frameworks**
- **AI/ML services**
- **Generative AI**

### ML frameworks
Only one service here - **Amazon SageMaker**.
- Build, train and deploy ML models from scratch

### AI/ML Services
Basically a bunch of maanaged offerings on specific use cases. These can be broken down into
different domains.

#### Text and documents
- **Amazon Comprehend** - text analysis service. Language identification, key concepts, sentiment
  analysis, and can organise a collection of text files according to topics it identifies.
- **Amazon Translate** - "neural" translation, i.e. it uses deep learning (as opposed to old-school
  statistical/rule-based systems).
- **Amazon Textract** - extracts text and data from scanned documents.

#### Chatbots
- **Amazon Lex** - fully managed service to design, build, test and deploy "conversational
  interfaces" into any system that uses voice or text. This includes speech-to-text to allow for
  phone systems, etc. Same technology that powers Alexa, apparently.

#### Speech
- **Amazon Polly** - realistic text to speech. Multiple languages and voices.
- **Amazon Transcribe** - speech to text. Supports multiple formats (e.g. WAV and MP3), and provides
  time stamps for every word (wow, Amazon have settled on a definition for words!!!). Supports
  streaming audio and generating live transcripts.

#### Vision
- **Amazon Rekognition** - image and video analysis. Identifies objects, people, text, scenes and
  activities. Can be used to monitor for inappropriate content. Allows for facial recognition and
  ***facial search*** (creepy).

#### Search
- **Amazon Kendra** - intelligent search powered by ML, for "websites and applications".

#### Recommendations
- **Amazon Personalize** - ML service that provides recommendations to users. It works by ingesting
  an activity stream from your system (e.g. page views, purchases), alongside a collection of items
  that you want to recommend. The data can be further enriched with demographic information about
  the users.

#### Miscellaneous
- **AWS DeepRacer** - 1/18th scale race car for learning reinforcement learning (RL).

### Generative AI
- **Amazon SageMaker JumpStart** - provides a set of solutions for the most common usecases.
  Basically a bunch of AWS CloudFormation templates, I think.
- **Amazon Bedrock** - FMs from Amazon and many other AI startups, accessible through an API. Can be
  used for experimentation, optimisation/customisation, and integration/deployment in AWS systems.
  Allows devs to skip the training stages of ML.
- **Amazon Q** - analyses data from your company's "information repositories, code and enterprise
  systems", and then chats to you about it. Helps with making informed strategic decisions.
- **Amazon Q Developer** - provides ML-powered code recommendations for C#, Java,
  JavaScript/TypeScript and Python. Integrates with various IDEs, and can spit out around (more
  than?) 10-15 lines of code at a time.

## Advantages and benefits of AWS AI solutions
- Security and privacy of data
  - Industry-specific compliance attributes (e.g. PCI DSS for payments)
- Access to top of the line models
  - AWS' services are frequently updated with the newest models, techniques and algorithms
- Pay-as-you-go pricing (Scalability)
- Global AWS infrastructure
- Seamless integration with other AWS services

They also off-handedly mention that they provide services and tools for the responsible use of AI,
but they don't give any examples lol

## Cost considerations
- **Responsiveness and availability/Redundancy and regional coverage** - basically if you want low
  latency or redundant solutions, you've got to pay more.
- **Performance** - instances that have a GPU or AI accelerators cost more
- **Token-based pricing** - applies to some services that generate text (e.g. Amazon Q Developer or
  Amazon Bedrock)
- **Provisioned throughput** - you can basically reserve a certain amount of bandwidth for some
  services (e.g. Polly, Transcribe) to ensure performance, but this costs more.
- **Custom models** - you can "bring your own model" to AWS, but this can incur additional costs
  which depend on various factors.
