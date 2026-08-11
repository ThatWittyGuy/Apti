# AI / ML & Generative AI — Comprehensive Study Notes
### For Corning OA Prep | JD1 (ERP Transformation), JD2 (Data & AI Engineering), JD5 (Business Engagement)
> Covers: ML fundamentals, algorithms, model evaluation, deep learning, NLP, LLMs, GenAI, RAG, MLOps, enterprise AI, responsible AI.

---

## 1. Artificial Intelligence — Foundations

### 1.1 The AI Hierarchy
```
Artificial Intelligence (AI)
└── Machine Learning (ML)
    ├── Deep Learning (DL)
    │   └── Generative AI (GenAI)
    └── Traditional ML (non-deep)
```

- **AI:** Broad field of building systems that simulate human intelligence (reasoning, perception, language, decision-making)
- **ML:** Subset of AI where systems learn patterns from data without being explicitly programmed for each rule
- **Deep Learning:** Subset of ML using neural networks with many layers — excels at images, audio, text
- **Generative AI:** Subset of deep learning where models generate new content (text, images, code, audio)

### 1.2 Types of Machine Learning

**Supervised Learning:**
- Learn from **labeled** training data (input → known correct output)
- Goal: learn a mapping function `f(X) = Y` to predict Y for new X
- Examples: email spam detection, loan default prediction, image classification, price prediction
- Subtypes:
  - **Classification:** Predict a category (spam/not spam, disease/healthy, A/B/C/D)
  - **Regression:** Predict a continuous number (house price, sales forecast, temperature)

**Unsupervised Learning:**
- Learn from **unlabeled** data — find hidden structure, patterns, or groupings
- No predefined correct answer
- Examples: customer segmentation, anomaly detection, recommendation systems, topic modeling
- Subtypes:
  - **Clustering:** Group similar data points (K-Means, DBSCAN, Hierarchical)
  - **Dimensionality Reduction:** Compress data to fewer dimensions (PCA, t-SNE, UMAP)
  - **Association:** Find co-occurrence rules (market basket analysis — "customers who buy X also buy Y")

**Reinforcement Learning (RL):**
- Agent learns by taking actions in an environment and receiving rewards/penalties
- No labeled data — learns through trial and error
- Goal: maximize cumulative reward over time
- Examples: game playing (AlphaGo, chess), robotics, autonomous vehicles, recommendation systems
- Key concepts: Agent, Environment, State, Action, Reward, Policy

**Semi-Supervised Learning:**
- Small amount of labeled data + large amount of unlabeled data
- Label some data manually, use model to label the rest
- Practical for expensive labeling tasks (medical imaging)

**Self-Supervised Learning:**
- Model creates its own supervision from unlabeled data
- Foundation of modern LLMs: predict next word (language modeling) — no manual labeling needed
- Also: masked image modeling (MAE), contrastive learning (SimCLR, CLIP)

---

## 2. Core ML Concepts

### 2.1 The ML Workflow
```
Problem Definition → Data Collection → EDA → Preprocessing →
Feature Engineering → Model Selection → Training → Evaluation →
Hyperparameter Tuning → Deployment → Monitoring
```

### 2.2 Data Splitting
- **Training set (60–80%):** Used to fit model parameters
- **Validation set (10–20%):** Used during development to tune hyperparameters and select models — never used for final evaluation
- **Test set (10–20%):** Used once for final, unbiased performance evaluation — never touched during development

**K-Fold Cross-Validation:**
- Split data into K equal folds
- Train K times: each fold serves as validation once; remaining K-1 folds = training
- Average performance across K runs → more robust estimate than single train/val split
- Common K values: 5, 10
- **Stratified K-Fold:** Maintains class proportion in each fold — important for imbalanced datasets

### 2.3 Overfitting vs. Underfitting

**Underfitting (High Bias):**
- Model too simple — can't capture patterns even in training data
- High training error AND high test error
- Cause: too few features, too simple model, not enough training, excessive regularization
- Fix: more complex model, more features, train longer, reduce regularization

**Overfitting (High Variance):**
- Model memorizes training data including noise — doesn't generalize
- Low training error, HIGH test error (big gap between them)
- Cause: too complex model, too few training samples, too many features, too long training
- Fix: more training data, regularization (L1/L2/dropout), simpler model, early stopping, cross-validation

**The Bias-Variance Tradeoff:**
- **Bias:** Error from wrong assumptions (underfitting)
- **Variance:** Sensitivity to small fluctuations in training data (overfitting)
- Goal: find the sweet spot — model complex enough to capture patterns, simple enough to generalize

**Regularization techniques:**
- **L1 (Lasso):** Adds sum of absolute weights to loss — drives some weights to exactly zero → feature selection
- **L2 (Ridge):** Adds sum of squared weights to loss — shrinks all weights toward zero → doesn't eliminate features
- **Elastic Net:** Combines L1 + L2
- **Dropout:** Randomly deactivate neurons during training (neural networks only) — prevents co-adaptation
- **Early Stopping:** Stop training when validation loss stops improving

### 2.4 Features and Feature Engineering

**Feature:** An individual measurable input variable (column in your dataset). The model learns from features to predict the target.

**Feature Engineering:** Creating, transforming, or selecting features to improve model performance.
- **Normalization (Min-Max Scaling):** Scale to [0,1] → `(x - min) / (max - min)`
- **Standardization (Z-score):** Scale to mean=0, std=1 → `(x - mean) / std`
- **Log transformation:** Compress skewed distributions (income, population)
- **One-Hot Encoding:** Convert categorical variables to binary columns (Red → [1,0,0], Blue → [0,1,0])
- **Label Encoding:** Convert categories to integers (bad for tree models if ordinal meaning doesn't exist)
- **Binning:** Convert continuous to categorical bins (age → young/middle/old)
- **Interaction terms:** Multiply features to capture relationships (height × weight)
- **Polynomial features:** Add x², x³ to capture non-linear relationships
- **Text features:** TF-IDF, word embeddings, n-grams

**Feature Selection:**
- Remove irrelevant or redundant features to reduce noise, computation, overfitting
- Methods: Correlation analysis, feature importance (from tree models), PCA, Recursive Feature Elimination (RFE), LASSO (L1 automatically zeroes out unimportant features)

### 2.5 Handling Missing Data
- **Drop rows:** Only if small % of data missing and missing at random
- **Mean/Median imputation:** Fill with column average — simple, loses variance
- **Mode imputation:** For categorical features
- **KNN imputation:** Use similar samples to estimate missing value
- **Model-based imputation:** Train a model to predict missing values
- **Indicator variable:** Add binary column flagging that value was missing (captures missingness as signal)
- **Multiple imputation:** Generate multiple plausible values, average results

### 2.6 Class Imbalance
When one class is much rarer than others (e.g., 1% fraud, 99% legitimate):
- **Oversampling minority class:** SMOTE (Synthetic Minority Oversampling Technique) — generate synthetic minority samples
- **Undersampling majority class:** Randomly remove majority samples
- **Class weights:** Tell the model to penalize errors on minority class more heavily (`class_weight='balanced'` in sklearn)
- **Different metric:** Accuracy is misleading — use F1, AUC-ROC, Precision-Recall curve

---

## 3. Key ML Algorithms

### 3.1 Linear Models
**Linear Regression:**
- Predicts continuous output as weighted sum of input features: `ŷ = w₀ + w₁x₁ + w₂x₂ + ...`
- Minimizes Mean Squared Error (MSE)
- Assumptions: linearity, no multicollinearity, homoscedasticity, independence of errors

**Logistic Regression (Classification):**
- Despite "regression" in name — a classification algorithm
- Outputs probability via sigmoid function: `P(y=1) = 1 / (1 + e^(-z))`
- Threshold (usually 0.5) converts probability to class label
- Fast, interpretable, works well as baseline

### 3.2 Tree-Based Models
**Decision Tree:**
- Recursive binary splits based on feature thresholds — minimizes impurity (Gini or Entropy for classification; MSE for regression)
- Highly interpretable (can visualize)
- Prone to overfitting (easily memorizes training data)
- Key hyperparameters: max_depth, min_samples_split, min_samples_leaf

**Random Forest:**
- Ensemble of many decision trees
- **Bagging:** Each tree trained on a random bootstrap sample of data
- **Feature randomness:** Each split considers only a random subset of features
- Final prediction: majority vote (classification) or average (regression)
- Much less overfitting than a single tree; generally strong performer
- Provides feature importance scores

**Gradient Boosting (XGBoost, LightGBM, CatBoost):**
- Builds trees sequentially — each new tree corrects errors of previous trees
- **XGBoost:** Regularized gradient boosting; very popular in competitions
- **LightGBM:** Faster, handles large data; leaf-wise tree growth
- **CatBoost:** Excellent with categorical features; less preprocessing needed
- Often the best-performing algorithm for tabular data
- Key hyperparameters: n_estimators, learning_rate, max_depth, subsample

### 3.3 Distance-Based Models
**K-Nearest Neighbors (KNN):**
- Classify/predict based on K closest training points (by Euclidean or other distance)
- No training phase — "lazy learner"
- Slow at prediction time for large datasets
- Sensitive to feature scaling (must normalize/standardize)

**K-Means Clustering:**
- Partition data into K clusters, each with a centroid
- Algorithm: initialize K centroids → assign each point to nearest centroid → recalculate centroids → repeat until convergence
- Must specify K in advance; sensitive to initialization (use K-Means++)
- **Elbow method:** Plot inertia vs K — look for "elbow" point

**Support Vector Machine (SVM):**
- Find the hyperplane that maximizes margin between classes
- **Kernel trick:** Map data to higher dimensions where it becomes linearly separable (RBF kernel for non-linear problems)
- Works well for high-dimensional data and small datasets
- Slow on large datasets; sensitive to feature scaling

### 3.4 Probabilistic Models
**Naive Bayes:**
- Applies Bayes' theorem with strong independence assumption between features
- Fast, works well with text classification (spam detection)
- "Naive" = assumes features are independent (often violated but still useful)

### 3.5 Neural Networks
Covered in the deep learning section below.

---

## 4. Model Evaluation

### 4.1 Classification Metrics

**Confusion Matrix:**
```
                Predicted Positive | Predicted Negative
Actual Positive:    TP (True Pos)  |    FN (False Neg)
Actual Negative:    FP (False Pos)  |    TN (True Neg)
```

**Accuracy:** `(TP + TN) / Total` — misleading for imbalanced classes

**Precision:** `TP / (TP + FP)` — "Of all I predicted positive, how many were actually positive?" — Minimize false positives (spam filter — don't mark real email as spam)

**Recall (Sensitivity):** `TP / (TP + FN)` — "Of all actual positives, how many did I catch?" — Minimize false negatives (cancer detection — don't miss cancer cases)

**F1 Score:** `2 × (Precision × Recall) / (Precision + Recall)` — Harmonic mean; balances precision and recall. Use when both matter.

**AUC-ROC:** Area Under the Receiver Operating Characteristic Curve
- ROC plots True Positive Rate vs False Positive Rate at every threshold
- AUC = probability that model ranks a random positive higher than a random negative
- AUC = 0.5 → random chance; AUC = 1.0 → perfect model
- Threshold-independent — good for imbalanced datasets

**Precision-Recall Curve:** Better than ROC for highly imbalanced datasets

### 4.2 Regression Metrics
- **MAE (Mean Absolute Error):** Average of |actual - predicted| — robust to outliers
- **MSE (Mean Squared Error):** Average of (actual - predicted)² — penalizes large errors more
- **RMSE (Root MSE):** √MSE — same units as target variable
- **R² (Coefficient of Determination):** Proportion of variance explained by model; 0 = no better than mean, 1 = perfect; can be negative (worse than mean)
- **MAPE (Mean Absolute Percentage Error):** Average of |actual - predicted| / actual × 100% — interpretable as % error

### 4.3 Parameters vs. Hyperparameters
**Parameters:** Learned from training data (weights in neural network, coefficients in linear regression) — model figures these out automatically.

**Hyperparameters:** Set before training by the developer — control the learning process:
- Learning rate, batch size, number of epochs
- Number of trees (Random Forest), max depth
- K (KNN), C and kernel (SVM)
- Regularization strength (alpha/lambda)

**Hyperparameter Tuning:**
- **Grid Search:** Try all combinations in a defined grid — exhaustive but slow
- **Random Search:** Randomly sample combinations — often finds good results faster
- **Bayesian Optimization:** Use previous results to intelligently pick next hyperparameters
- **Early Stopping:** For neural networks — stop when validation loss stops improving

---

## 5. Deep Learning

### 5.1 Neural Network Fundamentals

**Neuron (Perceptron):**
- Receives inputs → multiplies by weights → adds bias → applies activation function → outputs result
- `output = activation(w₁x₁ + w₂x₂ + ... + b)`

**Layers:**
- **Input layer:** One neuron per feature
- **Hidden layers:** Intermediate processing — multiple hidden layers = "deep" network
- **Output layer:** One neuron per class (classification) or one neuron (regression)

**Activation Functions:**
| Function | Formula | When Used |
|---|---|---|
| ReLU | max(0, x) | Default for hidden layers; solves vanishing gradient |
| Sigmoid | 1/(1+e⁻ˣ) | Binary output layer; range [0,1] |
| Softmax | eˣⁱ/Σeˣʲ | Multi-class output; converts to probabilities summing to 1 |
| Tanh | (eˣ-e⁻ˣ)/(eˣ+e⁻ˣ) | Range [-1,1]; zero-centered; less common than ReLU |
| Leaky ReLU | max(0.01x, x) | Avoids dying ReLU problem |

**Loss Functions:**
- Binary Cross-Entropy: Binary classification
- Categorical Cross-Entropy: Multi-class classification
- MSE / MAE: Regression

**Backpropagation:** Algorithm computing gradients of loss with respect to each weight, using chain rule — propagates error backward through the network.

**Gradient Descent:** Iteratively update weights in the direction that reduces loss:
- `w = w - learning_rate × gradient`
- **Batch GD:** Use all data per step — stable but slow
- **Stochastic GD (SGD):** One sample per step — fast but noisy
- **Mini-Batch GD:** Small batch (32–512 samples) — balance of speed and stability

**Optimizers beyond SGD:**
- **Adam:** Adaptive learning rates + momentum — most popular default
- **RMSprop:** Adaptive learning rates — good for RNNs
- **AdaGrad:** Adapts LR per parameter — good for sparse data

### 5.2 The Vanishing Gradient Problem
- Gradients shrink exponentially as they propagate backward through many layers
- Early layers receive near-zero gradients → learn very slowly or not at all
- **Cause:** Sigmoid/tanh saturate (gradients → 0 at extremes)
- **Solutions:** ReLU activations, batch normalization, residual connections (skip connections in ResNet), better weight initialization (He, Xavier)

### 5.3 Convolutional Neural Networks (CNNs)
For image data (and any grid-structured data):
- **Convolutional layer:** Apply filters to detect local features (edges, shapes, textures)
- **Pooling layer:** Downsample spatial dimensions (Max pooling: take maximum in each window)
- **Fully connected layer:** Classification head at the end
- **Transfer learning:** Use pre-trained CNNs (ResNet, VGG, EfficientNet) as feature extractors for new tasks with limited data

### 5.4 Recurrent Neural Networks (RNNs)
For sequential data (time series, text):
- Maintain hidden state that captures context from previous steps
- **LSTM (Long Short-Term Memory):** Gating mechanism to selectively remember/forget — solves vanishing gradient for sequences
- **GRU (Gated Recurrent Unit):** Simpler LSTM variant with similar performance
- **Largely replaced by Transformers** for NLP tasks

### 5.5 Transformers
The architecture behind all modern LLMs.

**Introduced:** "Attention Is All You Need" (Vaswani et al., 2017)

**Key innovation — Self-Attention:**
- Every token can "attend to" (look at) every other token in the sequence simultaneously
- Computes how relevant each other token is for understanding the current token
- Query, Key, Value matrices: `Attention(Q,K,V) = softmax(QKᵀ/√d) × V`
- Captures long-range dependencies that RNNs struggled with

**Multi-Head Attention:**
- Run attention multiple times in parallel with different learned projections
- Each head can attend to different aspects (grammar, semantics, coreference)

**Positional Encoding:**
- Transformers process tokens in parallel (no sequential order) — positional encodings added to tell model position of each token

**Encoder-Decoder vs. Decoder-only:**
- **Encoder-only** (BERT): Reads full sequence bidirectionally — good for understanding tasks (classification, NER, Q&A)
- **Decoder-only** (GPT, Claude): Generates tokens one at a time left-to-right — good for generation
- **Encoder-Decoder** (T5, BART): Encode input, decode output — good for translation, summarization

---

## 6. Natural Language Processing (NLP)

### 6.1 Text Preprocessing
1. **Tokenization:** Split text into tokens (words, subwords, characters)
2. **Lowercasing:** Normalize case
3. **Stop word removal:** Remove common words (the, is, at) — for traditional ML; not for deep learning
4. **Stemming/Lemmatization:** Reduce words to root form (running → run)
5. **Punctuation removal:** Remove special characters

### 6.2 Text Representation

**Bag of Words (BoW):** Count word occurrences; loses word order; sparse, high-dimensional

**TF-IDF (Term Frequency-Inverse Document Frequency):**
- TF: How often word appears in a document
- IDF: How rare the word is across all documents (penalizes common words)
- TF-IDF = TF × IDF — words frequent in one document but rare overall get high scores
- Better than raw counts for distinguishing document topics

**Word Embeddings:**
- Dense vector representation of words capturing semantic meaning
- **Word2Vec:** Predict surrounding words (Skip-gram) or predict word from context (CBOW) — geometrically similar words cluster together
- **GloVe:** Global co-occurrence statistics → vectors
- **FastText:** Character-level subword embeddings — handles unknown words and morphology
- Famous example: king − man + woman ≈ queen (vector arithmetic captures analogies)

**Contextual Embeddings (Modern):**
- Same word gets different embedding based on context
- BERT, GPT, etc. produce contextual embeddings — "bank" near water vs. financial bank get different vectors

### 6.3 Classic NLP Tasks
- **Text Classification:** Sentiment analysis, topic categorization, spam detection
- **Named Entity Recognition (NER):** Identify names, places, dates, organizations in text
- **Part-of-Speech (POS) Tagging:** Label each word's grammatical role
- **Machine Translation:** English → French etc.
- **Question Answering:** Given context, answer a question
- **Text Summarization:** Extractive (select key sentences) or Abstractive (generate new summary)

---

## 7. Large Language Models (LLMs)

### 7.1 What are LLMs?
Large Language Models are massive Transformer-based neural networks trained on vast text corpora (the internet, books, code, etc.) to predict the next token in a sequence.

**Scale:** Billions to trillions of parameters (GPT-4 ~1.8T parameters — estimated)

**Training:** Self-supervised — predict next word — no manual labeling needed. The internet provides essentially unlimited training signal.

**Emergent capabilities:** At sufficient scale, LLMs develop capabilities not present in smaller models — reasoning, code generation, multi-step problem solving.

**Notable LLMs:**
| Model | Company | Notes |
|---|---|---|
| GPT-4 / GPT-4o | OpenAI | Multimodal; powers ChatGPT |
| Claude (3.5, 4) | Anthropic | Strong reasoning and safety |
| Gemini | Google | Multimodal; integrated in Google products |
| Llama 3 | Meta | Open-weights; can run locally |
| Mistral / Mixtral | Mistral AI | Efficient open models |
| SAP Joule | SAP | Business-context LLM embedded in SAP |

### 7.2 How LLMs Work
1. **Tokenization:** Text split into subword tokens (GPT-4: ~100K token vocabulary)
2. **Embedding:** Each token converted to a dense vector
3. **Transformer layers:** Many layers of self-attention + feed-forward network
4. **Output:** Probability distribution over entire vocabulary for next token
5. **Sampling:** Select next token (greedy, top-k, nucleus/top-p sampling, temperature)
6. **Autoregressive:** Generate one token at a time, feeding output back as input

**Temperature:**
- Controls randomness of output
- Temperature → 0: Always pick highest probability token (deterministic, repetitive)
- Temperature → 1: Sample proportionally (creative, varied)
- Temperature > 1: More random, incoherent

### 7.3 LLM Training Pipeline
1. **Pre-training:** Train on massive unlabeled corpus (predict next token) — learns world knowledge and language
2. **Supervised Fine-Tuning (SFT):** Train on human-written instruction-response pairs — teaches the model to follow instructions
3. **RLHF (Reinforcement Learning from Human Feedback):** Human raters rank model responses → train a reward model → use RL (PPO) to optimize LLM output toward highly-rated responses

### 7.4 Prompt Engineering
The practice of designing inputs to LLMs to get desired, high-quality outputs.

**Principles:**
- **Be specific and clear:** Vague prompts → vague outputs
- **Provide context:** Who is the audience? What format is needed?
- **Give examples (few-shot):** Include 2–5 example input-output pairs in the prompt
- **Role assignment:** "You are an expert financial analyst..."
- **Chain-of-thought (CoT):** Ask model to "think step by step" → dramatically improves reasoning
- **Output format specification:** "Respond in JSON format with fields: name, date, amount"
- **Negative examples:** "Do not include technical jargon"

**Zero-shot:** No examples — just instruction
**Few-shot:** Include 2–10 examples in the prompt
**Chain-of-Thought (CoT):** Prompt model to reason step by step before answering

### 7.5 Hallucination
When an LLM confidently generates factually incorrect, fabricated, or nonsensical information.

**Causes:**
- LLMs optimize for plausible-sounding text, not factual accuracy
- Training data cutoff — no knowledge of recent events
- Rare facts underrepresented in training data

**Mitigation:**
- RAG (Retrieval-Augmented Generation) — ground responses in retrieved documents
- Fine-tuning on domain-specific data
- Asking model to cite sources
- Fact-checking critical outputs
- Confidence scoring

### 7.6 Fine-Tuning LLMs
Adapting a pre-trained LLM to a specific domain or task:

**Full fine-tuning:** Update all parameters on domain-specific data — expensive, requires significant compute

**Parameter-Efficient Fine-Tuning (PEFT):**
- **LoRA (Low-Rank Adaptation):** Add small trainable rank-decomposition matrices alongside frozen original weights — train only ~0.1% of parameters
- **QLoRA:** Quantize base model to 4-bit + LoRA — enables fine-tuning large models on consumer GPUs
- **Adapters:** Small trainable layers inserted between frozen transformer layers

**When to fine-tune:**
- Domain-specific vocabulary/format (medical reports, legal documents)
- Consistent output style
- RAG alone insufficient for domain alignment

**When NOT to fine-tune:**
- RAG can solve the problem (cheaper, more flexible, always current)
- Just need better prompt engineering
- Data is limited (< few hundred examples)

### 7.7 Retrieval-Augmented Generation (RAG)

**Problem RAG solves:** LLMs have a knowledge cutoff, can't access private data, and hallucinate when asked about specific facts.

**RAG Architecture:**
1. **Indexing:** Documents chunked → embedded into vectors → stored in vector database
2. **Retrieval:** User query → embed query → similarity search in vector DB → retrieve top-K relevant chunks
3. **Augmentation:** Inject retrieved chunks as context into LLM prompt
4. **Generation:** LLM generates response grounded in retrieved context

**Vector Databases:** Pinecone, Weaviate, Chroma, Qdrant, pgvector (PostgreSQL extension), FAISS (library), Databricks Vector Search

**Embedding models:** `text-embedding-ada-002` (OpenAI), `e5-large`, `bge-large`, sentence-transformers

**Similarity measures:** Cosine similarity (most common), dot product, Euclidean distance

**RAG challenges:**
- **Chunking strategy:** How to split documents (fixed size, by sentence, by paragraph, semantic)
- **Retrieval quality:** Embedding model quality; sparse (BM25) + dense (vector) hybrid retrieval
- **Context window limits:** Can only inject so many chunks before hitting LLM's context limit
- **Faithfulness:** Ensuring LLM actually uses retrieved context rather than prior knowledge

**Advanced RAG patterns:**
- **HyDE (Hypothetical Document Embedding):** Generate a hypothetical answer first, then search for similar documents
- **Re-ranking:** Retrieve more candidates, then re-rank with a cross-encoder for precision
- **Query expansion:** Rewrite query multiple ways, search with all variations
- **Agentic RAG:** LLM decides when and what to retrieve (iterative retrieval)

---

## 8. Generative AI — Broader Landscape

### 8.1 Beyond Text — Multimodal AI
**Image Generation:**
- **Diffusion models:** Add noise to images then learn to reverse the process — state of the art (Stable Diffusion, DALL-E 3, Midjourney)
- **GANs (Generative Adversarial Networks):** Generator vs. Discriminator competing — older approach

**Audio Generation:**
- Text-to-speech: ElevenLabs, OpenAI TTS
- Music generation: Suno, Udio, MusicLM

**Video Generation:**
- Sora (OpenAI), Runway Gen-3, Google Veo

**Code Generation:**
- GitHub Copilot (GPT-4 based), Cursor, Amazon CodeWhisperer
- LLMs excellent at: writing boilerplate, explaining code, debugging, writing tests, documentation

**Multimodal Models:**
- Single model processes text + images + audio + video
- GPT-4V, Claude 3+ (vision), Gemini Ultra — can analyze images, describe them, answer questions about them

### 8.2 AI Agents
LLMs with **tools** and **autonomy** to complete multi-step tasks:
- **Tool use:** LLM calls APIs, searches the web, runs code, reads files
- **Planning:** Break complex goal into sub-steps
- **Memory:** Short-term (conversation context), long-term (vector DB retrieval)
- **Self-reflection:** Evaluate and improve own outputs

**ReAct pattern:** Reason + Act — LLM alternates between reasoning steps and tool-use actions

**Multi-agent systems:** Multiple specialized agents collaborate (researcher, coder, reviewer)

**Examples:** AutoGPT, LangChain agents, Microsoft Copilot Studio, SAP Joule

---

## 9. MLOps — Production ML

### 9.1 What is MLOps?
MLOps (Machine Learning Operations) = DevOps practices applied to machine learning — automating and streamlining the ML lifecycle from development to production.

**The ML lifecycle:**
```
Data → Feature Engineering → Training → Evaluation → 
Deployment → Monitoring → Retraining
```

### 9.2 Experiment Tracking
- Track every training run: hyperparameters, metrics, artifacts, code version
- Tools: **MLflow**, Weights & Biases, Neptune, Comet
- Without tracking: impossible to reproduce results or know which model is best

### 9.3 Model Registry
- Central store of trained models with lifecycle stages (Staging → Production → Archived)
- Track: which model is in production, when it was deployed, its performance
- MLflow Model Registry, SageMaker Model Registry, Databricks Model Registry

### 9.4 Feature Store
- Centralized repository for computed features
- Ensures consistency: same feature computation used for training AND serving (eliminates training-serving skew)
- Enables feature reuse across teams and models
- Examples: Databricks Feature Store, Feast, Tecton, Vertex AI Feature Store

### 9.5 Model Deployment Patterns
**Batch inference:** Run model on a large dataset periodically, store results — for non-time-sensitive predictions (daily fraud scores, weekly churn predictions)

**Real-time inference (online serving):** REST API endpoint; request → model → response in milliseconds — for user-facing predictions (recommendation, fraud detection at checkout)

**Streaming inference:** Process continuous stream of events (Kafka) — real-time fraud detection on payment stream

**Edge inference:** Model runs on device (phone, IoT sensor) — no network latency, offline capability

### 9.6 Model Monitoring
**Data drift:** Input feature distributions change from training distribution → model predictions become less reliable
- Detect: KS test, PSI (Population Stability Index), Jensen-Shannon divergence

**Concept drift:** Relationship between features and target changes (world changes) → model's learned mapping becomes wrong
- Example: COVID changed consumer behavior, making pre-COVID models invalid

**Model performance monitoring:** Track prediction metrics over time — accuracy, AUC, error rate

**Infrastructure monitoring:** Latency, throughput, error rate, memory usage of serving infrastructure

**Tools:** Evidently AI, WhyLogs, Fiddler, Arize, Seldon Alibi

---

## 10. Responsible AI & AI Governance

### 10.1 Key Principles (Microsoft, SAP, Anthropic, EU AI Act)
- **Fairness:** Model doesn't discriminate based on protected attributes (race, gender, age, religion)
- **Reliability & Safety:** System behaves as expected; fails gracefully
- **Privacy & Security:** Respect data privacy; prevent data extraction attacks
- **Inclusiveness:** Works for all users; accessible
- **Transparency:** Users know when they're interacting with AI; decisions explainable
- **Accountability:** Clear ownership; human oversight; audit trails

### 10.2 AI Bias
**Sources of bias:**
- **Historical bias:** Training data reflects historical discrimination (hiring data, credit decisions)
- **Representation bias:** Underrepresented groups in training data → worse performance for them
- **Measurement bias:** Features or labels measured differently for different groups
- **Aggregation bias:** One model for all groups when groups have different patterns

**Bias detection:** Evaluate metrics (accuracy, false positive/negative rates) separately per demographic group — large differences indicate bias

**Bias mitigation:**
- Pre-processing: Re-sample, re-weight training data
- In-processing: Add fairness constraints to training objective
- Post-processing: Adjust decision thresholds per group

### 10.3 Explainability (XAI)
Understanding why a model makes predictions:
- **Global explainability:** Overall model behavior — feature importances (Random Forest, XGBoost)
- **Local explainability:** Why was this specific prediction made?
  - **LIME (Local Interpretable Model-Agnostic Explanations):** Fit simple model locally around a prediction
  - **SHAP (SHapley Additive exPlanations):** Game theory-based; each feature's contribution to a prediction
- Inherently interpretable models: Linear regression, Decision trees, Rules

### 10.4 EU AI Act (2024 — Global Reference)
First comprehensive AI regulation:
- **Unacceptable risk:** Banned (social scoring, subliminal manipulation, real-time biometric surveillance in public)
- **High risk:** Regulated (hiring, credit, healthcare, education, critical infrastructure) — require conformity assessment, transparency, human oversight
- **Limited risk:** Transparency obligations (chatbots must disclose they're AI)
- **Minimal risk:** No regulation (spam filters, AI in games)

### 10.5 AI in Enterprise — Governance Framework
- **AI policy:** Approved use cases, prohibited uses, acceptable data sources
- **Model risk management:** Validation before deployment, ongoing monitoring
- **Data governance:** Data used for AI must be consented, accurate, not biased
- **Human oversight:** Critical decisions (lending, hiring, medical) require human review
- **Audit trail:** Log predictions, inputs, model version for accountability
- **Vendor assessment:** Third-party AI tools assessed for security, data handling, bias

---

## 11. Key AI/ML Terms Quick Reference

| Term | Meaning |
|---|---|
| Epoch | One complete pass through the entire training dataset |
| Batch size | Number of samples processed before updating weights |
| Learning rate | Step size for gradient descent — too high: diverge; too low: slow |
| Gradient | Direction and rate of steepest loss increase; model updates in opposite direction |
| Tensor | Multi-dimensional array — the fundamental data structure in deep learning frameworks |
| Inference | Using a trained model to make predictions on new data |
| Embedding | Dense vector representation of data (text, images) in a continuous vector space |
| Vector database | Database optimized for similarity search on embeddings |
| Context window | Maximum tokens an LLM can process at once (GPT-4: 128K, Claude: 200K) |
| Token | Smallest unit of text processed by an LLM (≈ 0.75 words on average) |
| Transformer | Neural architecture using self-attention; basis of all modern LLMs |
| RAG | Retrieval-Augmented Generation — ground LLM responses in retrieved documents |
| RLHF | Reinforcement Learning from Human Feedback — align LLM with human preferences |
| LoRA | Low-Rank Adaptation — parameter-efficient fine-tuning technique |
| Hallucination | LLM generating confident but factually incorrect output |
| Prompt | Input text given to an LLM to elicit a response |
| Zero-shot | Task performed by LLM with no examples in prompt |
| Few-shot | Task performed by LLM with 2–10 examples in prompt |
| Chain-of-thought | Prompting LLM to reason step-by-step before answering |
| MLOps | Practices automating ML model development, deployment, and monitoring lifecycle |
| Feature store | Centralized repository for computed ML features ensuring training-serving consistency |
| Model drift | Degradation of model performance over time as data distribution changes |
| SHAP | SHapley Additive exPlanations — method for explaining individual predictions |
| AUC-ROC | Area under ROC curve — threshold-independent classification performance metric |

---
*End of AI/ML & Generative AI Comprehensive Notes — ~640 lines.*
*Read alongside: Shared_AI_ML.md for MCQ practice.*
