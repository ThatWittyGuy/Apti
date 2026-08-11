# Shared — AI / ML Concepts MCQs
### Applies to: All JDs mentioning AI, ML, Data Science, or Automation
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What is the difference between Artificial Intelligence (AI) and Machine Learning (ML)?
- A) They are the same thing
- B) AI is the broader field of building systems that simulate human intelligence; ML is a subset of AI where systems learn patterns from data without being explicitly programmed
- C) ML is the broader field; AI is a subset
- D) AI uses data; ML uses explicit rules

**Answer: B**

---

**Q2.** 🟢 What are the three main types of machine learning?
- A) Structured, Unstructured, Semi-structured
- B) Supervised, Unsupervised, and Reinforcement Learning
- C) Regression, Classification, and Clustering
- D) Training, Validation, and Testing

**Answer: B**

---

**Q3.** 🟢 What is supervised learning?
- A) A teacher monitors students learning to code
- B) A model is trained on labeled input-output pairs and learns to map inputs to correct outputs for new, unseen data
- C) A model learns from unlabeled data
- D) A model learns by trial and error with rewards

**Answer: B**

---

**Q4.** 🟢 Which of the following is an example of unsupervised learning?
- A) Email spam classification
- B) Customer segmentation using K-Means clustering
- C) Image recognition with labeled training images
- D) Loan default prediction

**Answer: B**

---

**Q5.** 🟢 What is a "training set" in machine learning?
- A) A gym dataset for fitness applications
- B) The portion of data used to fit/train the model's parameters
- C) The final dataset used to report model performance
- D) A dataset used only for hyperparameter tuning

**Answer: B**

---

**Q6.** 🟢 What does "overfitting" mean in ML?
- A) The model is too simple to learn the data
- B) The model learns the training data too well (including noise), performing poorly on unseen data — low training error, high test error
- C) The model uses too much memory
- D) The model trains for too many iterations without converging

**Answer: B**

---

**Q7.** 🟢 What is a "feature" in machine learning?
- A) A special capability of an ML framework
- B) An individual measurable input variable used by the model to make predictions (e.g., age, income, temperature)
- C) The target variable being predicted
- D) A pre-trained model component

**Answer: B**

---

**Q8.** 🟡 What is the purpose of a validation set, separate from the test set?
- A) It is used for final model evaluation and reporting
- B) It is used during training to tune hyperparameters and make model selection decisions, without using the test set — preserving test set integrity for final evaluation
- C) It replaces the training set when data is limited
- D) It is identical to the test set but used for a different metric

**Answer: B**

---

**Q9.** 🟡 What is "underfitting" in ML and what causes it?
- A) The model memorizes training data
- B) The model is too simple to capture the underlying patterns — resulting in high error on both training and test data; caused by insufficient model complexity or training
- C) The model trains too slowly
- D) The model has too many features

**Answer: B**

---

**Q10.** 🟡 What is the "bias-variance tradeoff"?
- A) The tradeoff between model training speed and accuracy
- B) High bias (underfitting) means the model is too simple and misses patterns; high variance (overfitting) means it's too sensitive to training data noise; good models balance both
- C) The tradeoff between data size and model size
- D) The tradeoff between precision and recall

**Answer: B**

---

**Q11.** 🟡 What is a confusion matrix and what does it show?
- A) A matrix showing model training loss over epochs
- B) A table showing True Positives, True Negatives, False Positives, and False Negatives — used to evaluate classification model performance
- C) A matrix showing feature correlations
- D) A confusion between two model architectures

**Answer: B**

---

**Q12.** 🟡 What is "precision" in a classification context?
- A) The percentage of actual positives the model correctly identified
- B) Of all instances the model predicted as positive, the fraction that are actually positive — TP / (TP + FP)
- C) The overall accuracy of the model
- D) The harmonic mean of precision and recall

**Answer: B**

---

**Q13.** 🟡 What is "recall" (sensitivity) in a classification context?
- A) The fraction of predicted positives that are truly positive
- B) Of all actual positive instances, the fraction the model correctly identified — TP / (TP + FN)
- C) The model's ability to remember training examples
- D) The overall accuracy on the test set

**Answer: B**

---

**Q14.** 🟡 What is the F1 Score?
- A) The average of accuracy and precision
- B) The harmonic mean of precision and recall — useful when class imbalance exists and both false positives and false negatives matter
- C) The first evaluation metric used in ML history
- D) A score measuring feature importance

**Answer: B**

---

**Q15.** 🟡 What is "cross-validation" in ML model evaluation?
- A) Validating a model across different countries
- B) A technique (e.g., k-fold) that splits data into k subsets, trains on k-1 folds and validates on the remaining fold, rotating k times — providing a robust performance estimate
- C) Running the same model twice and comparing results
- D) Testing the model on both training and test sets

**Answer: B**

---

**Q16.** 🟡 What is "feature engineering"?
- A) Designing the hardware features of an ML chip
- B) The process of using domain knowledge to create, transform, or select input variables from raw data to improve model performance
- C) Automatically designing neural network architectures
- D) Engineering the target variable to improve quality

**Answer: B**

---

**Q17.** 🟡 What is "normalization" (or "min-max scaling") in feature preprocessing?
- A) Removing outliers from the data
- B) Scaling feature values to a fixed range (typically 0–1) so no single feature dominates due to scale differences
- C) Normalizing the model's weight distribution
- D) Converting categorical features to numeric

**Answer: B**

---

**Q18.** 🟡 What is a "decision tree" in ML?
- A) A tree diagram of project decisions
- B) A flowchart-like model that splits data based on feature conditions, making predictions at leaf nodes — interpretable but prone to overfitting
- C) A neural network with branching layers
- D) A hierarchical clustering algorithm

**Answer: B**

---

**Q19.** 🟡 What is "Random Forest" and how does it improve on a single decision tree?
- A) A single large decision tree grown on random data
- B) An ensemble of many decision trees trained on random data subsets and features — combining their predictions (bagging) to reduce variance and improve accuracy
- C) A tree-based model that only uses random features
- D) A decision tree with randomly pruned branches

**Answer: B**

---

**Q20.** 🟡 What is "gradient descent" in the context of training ML models?
- A) A technique for descending a mountain using GPS gradients
- B) An optimization algorithm that iteratively adjusts model parameters in the direction that minimizes the loss function by following the negative gradient
- C) A method for normalizing data gradients
- D) A learning rate scheduling strategy

**Answer: B**

---

**Q21.** 🟡 What is a "hyperparameter" vs. a "parameter" in ML?
- A) Hyperparameters are learned during training; parameters are set manually
- B) Parameters are learned from training data (e.g., weights); hyperparameters are set before training and control the learning process (e.g., learning rate, number of trees)
- C) They are interchangeable terms
- D) Hyperparameters are used only in deep learning

**Answer: B**

---

**Q22.** 🟡 In NLP, what is "tokenization"?
- A) Converting text files into database tokens
- B) The process of breaking text into smaller units (tokens: words, subwords, or characters) as the first step in text processing
- C) Encrypting text for secure NLP models
- D) Removing duplicate words from a text

**Answer: B**

---

**Q23.** 🟡 What is the "vanishing gradient" problem in deep neural networks?
- A) Gradients become too large, making training unstable
- B) Gradients become extremely small as they propagate back through many layers, making early layers train very slowly or not at all
- C) The model "vanishes" (crashes) during gradient computation
- D) Loss function gradients become zero after one epoch

**Answer: B**

---

**Q24.** 🟡 What is "dropout" in neural networks?
- A) When a neural network stops training early
- B) A regularization technique that randomly deactivates a fraction of neurons during each training step, preventing co-adaptation and reducing overfitting
- C) Removing neurons with low weight values
- D) A technique for removing outlier training samples

**Answer: B**

---

**Q25.** 🔴 What is a Transformer model in deep learning?
- A) A model that transforms raw data into structured format
- B) A neural network architecture based on self-attention mechanisms (introduced in "Attention Is All You Need"), enabling parallel processing of sequences — the foundation for LLMs like GPT
- C) A convolutional network for image transformation
- D) A feature engineering pipeline for NLP

**Answer: B**

---

**Q26.** 🔴 What is "attention" in the context of Transformer models?
- A) A metric measuring model focus on training
- B) A mechanism allowing the model to weigh the relevance of different tokens in a sequence relative to each other when producing representations — enabling context-aware understanding
- C) A memory management technique in neural networks
- D) A training strategy that pays attention to rare samples

**Answer: B**

---

**Q27.** 🔴 What is a Large Language Model (LLM)?
- A) A rule-based NLP system with a large dictionary
- B) A massive neural network (typically Transformer-based) trained on vast text corpora to understand and generate human language, capable of few-shot and zero-shot tasks
- C) A language model with more than 1 million parameters
- D) A supervised classifier for text categorization

**Answer: B**

---

**Q28.** 🔴 What is Retrieval-Augmented Generation (RAG)?
- A) A method for retraining LLMs on private data
- B) A technique combining an LLM with a retrieval system — the model fetches relevant documents from a knowledge base and uses them as context before generating a response, improving accuracy and grounding
- C) A data augmentation strategy for small datasets
- D) A reinforcement learning method for LLMs

**Answer: B**

---

**Q29.** 🔴 What is "prompt engineering"?
- A) Engineering the hardware that runs LLMs
- B) The practice of designing, structuring, and refining input prompts to guide LLM outputs toward desired, accurate, and relevant responses
- C) Programming LLMs using a new prompt-based language
- D) A way to reduce LLM hallucination by re-training

**Answer: B**

---

**Q30.** 🔴 What is "hallucination" in LLMs?
- A) A visual artifact in image generation models
- B) When an LLM generates factually incorrect, fabricated, or nonsensical information with high apparent confidence
- C) A training error that causes the model to see patterns that don't exist in the training data
- D) When the model produces random outputs due to temperature=1

**Answer: B**

---

**Q31.** 🔴 What is "fine-tuning" an LLM?
- A) Adjusting the LLM's inference speed for a specific hardware
- B) Further training a pre-trained LLM on a smaller, domain-specific dataset to adapt it to a particular task or style without training from scratch
- C) Tuning the hyperparameters of an LLM during pre-training
- D) Reducing the size of an LLM using pruning

**Answer: B**

---

**Q32.** 🔴 What is the difference between "zero-shot" and "few-shot" learning in LLMs?
- A) Zero-shot uses 0 training examples; few-shot retrains the model on a few examples
- B) Zero-shot performs a task based only on instructions with no examples in the prompt; few-shot provides a few input-output examples in the prompt to guide the model's behavior
- C) They are identical approaches
- D) Zero-shot is for classification; few-shot is for generation

**Answer: B**

---

**Q33.** 🔴 What is "vector embedding" in the context of AI/NLP?
- A) Embedding video files into a neural network
- B) A dense numerical representation of text (or other data) in a high-dimensional space where semantically similar items are geometrically closer — enabling similarity search and retrieval
- C) A compression technique for model weights
- D) A data type for storing vectors in a SQL database

**Answer: B**

---

**Q34.** 🔴 What is a "vector database" and why is it used in AI applications?
- A) A database that stores only numerical data
- B) A database optimized for storing and querying vector embeddings — enabling fast similarity search (e.g., finding semantically similar documents) critical for RAG systems
- C) A database that uses vector graphics for visualization
- D) A relational database with added array support

**Answer: B**

---

**Q35.** 🔴 What is "model drift" in production ML systems?
- A) A gradual shift in model weights during inference
- B) The degradation of model performance over time as the real-world data distribution changes from what the model was trained on — requiring monitoring and retraining
- C) A hardware failure causing model slowdown
- D) The difference between training loss and validation loss

**Answer: B**

---
*35 Questions | Shared across all JDs | See JD-specific files for domain MCQs.*
