# 04 — Interview Day Cheatsheet
### Indisposable | Gagangiri Foods Industry Project | Feb–May 2026
> 🔖 Print this. Read it the morning of every interview. Nothing else.

---

## ⚡ THE NUMBERS — Memorise These Cold

| What | Value | Say it as |
|------|-------|-----------|
| Test Accuracy | **70.9%** | "Around 71 percent on unseen test data" |
| Test Loss | **~0.59** | "Binary cross-entropy loss of 0.59" |
| Dataset size | **1,562 images** | "~1,500 labeled microscopy images" |
| Per class | **781 each** | "Perfectly balanced — 781 positive, 781 negative" |
| Training images | **~1,250** | "About 1,250 for training" |
| Epochs | **50** | "Trained for 50 epochs" |
| Base model params | **~3.4M (frozen)** | "3.4 million pretrained parameters, all frozen" |
| Trainable params | **~369K** | "Only ~370K parameters actually trained" |
| Input size | **128×128×3** | "128 by 128, 3 channels" |
| Dense layers | **4** | "Four dense layers: 256, 128, 64, 1" |
| Dropout rates | **0.4 → 0.3 → 0.2** | "Decreasing dropout from 0.4 to 0.2" |
| Project duration | **3 months** | "Feb to May 2026, three months" |
| Team size | **5** | "Team of five" |
| Industry partner | **Gagangiri Foods** | Always say the name — never "a food company" |
| Activation functions tested | **6** | "We ran an ablation study across 6 activation functions" |

---

## 🧠 ONE-LINER DEFINITIONS — Say These Exactly

**Transfer Learning:**
> "Using a model pretrained on a large dataset as a starting point, so we benefit from its learned features without training from scratch."

**MobileNetV2:**
> "A lightweight CNN by Google, pretrained on 14 million ImageNet images, using depthwise separable convolutions and inverted residual blocks to achieve high accuracy with only 3.4 million parameters."

**Frozen base model:**
> "We set trainable=False on MobileNetV2, so its weights stay fixed. Only our custom classification head's ~370K parameters are updated during training."

**GlobalAveragePooling2D:**
> "Collapses the 4×4×1280 feature tensor into a 1280-dimensional vector by averaging spatially. Reduces parameters in the next layer by 16× compared to Flatten."

**Binary Cross-Entropy:**
> "The loss function for binary classification. It heavily penalizes confident wrong predictions using the log function, and maps naturally to sigmoid's probabilistic output."

**Adam Optimizer:**
> "Combines momentum (first moment of gradients) with adaptive per-parameter learning rates (second moment). Converges faster than SGD and requires less tuning."

**Dropout:**
> "Randomly deactivates a fraction of neurons during each training step. Prevents co-adaptation and acts as an ensemble of sub-networks, reducing overfitting."

**Ablation Study:**
> "A controlled experiment where we vary one component at a time — we tested 6 activation functions keeping everything else identical — to empirically validate our design choices."

**Inverted Residual Block:**
> "MobileNetV2's core unit. Unlike standard residuals (wide→narrow→wide), these go narrow→wide→narrow, doing the heavy computation in the expanded high-dimensional space, then projecting back down with a linear activation."

**False Positive (in our context):**
> "The model flags clean water as contaminated. This is a false alarm — costly but safe. Our model is prone to this, which is the safer error direction for a food safety application."

**False Negative (in our context):**
> "The model says water is clean when it actually has microplastics. This is the dangerous error — contaminated water would enter food production. Our model rarely does this."

**Featurewise Normalization:**
> "Subtracts the dataset mean and divides by the dataset standard deviation per pixel — Z-score normalization. Matches the preprocessing MobileNetV2 was trained with, critical for transfer learning to work well."

**Data Augmentation:**
> "Artificially generating modified versions of training images — rotations, flips, shifts — so the model sees more variation and learns to be invariant to those transformations."

**Precision:**
> "Of all the times the model said 'microplastic', what fraction were actually microplastic. High precision = few false alarms."

**Recall / Sensitivity:**
> "Of all the actual microplastic samples, what fraction did the model catch. High recall = few missed detections. This is our priority metric."

**F1 Score:**
> "Harmonic mean of precision and recall. Penalizes extreme imbalances between the two — a model with 100% recall and 1% precision gets F1 of ~2%, not 50%."

**Sigmoid:**
> "Output activation that maps any real number to (0,1). The output is directly interpreted as the probability of microplastic presence."

**ReLU:**
> "f(x) = max(0,x). Fast, sparse, but can cause dead neurons when inputs are consistently negative."

**ELU:**
> "Like ReLU for positive values, but has a smooth exponential curve for negatives. No dead neurons, zero-mean outputs, faster convergence than ReLU."

**Leaky ReLU:**
> "Like ReLU but with a small slope (0.01) for negative inputs. Prevents dead neurons simply and cheaply — no exponential needed."

**DHM (Digital Holographic Microscopy):**
> "An advanced imaging technique that captures 3D information via laser interference patterns. Makes transparent microplastic particles visible without chemical staining — ideal for real-time water quality monitoring."

---

## 🎯 10 KILLER PHRASES — Drop These Naturally

These signal depth and professionalism. Memorise the phrasing.

1. **On dataset size:**
> *"With only ~1,500 images, training from scratch was out of the question — the model would memorize the training data. Transfer learning was the principled solution."*

2. **On the ablation study:**
> *"We didn't just pick activation functions arbitrarily — we ran a controlled ablation study testing six different functions and selected the combination based on validation accuracy. That's what led to the ReLU → ELU → Leaky ReLU stack."*

3. **On false positives:**
> *"The model's false positive tendency is actually the correct behavior for this domain. In food safety, missing a contamination event is catastrophic. A false alarm is just an inconvenience. The client explicitly told us to err on the side of caution."*

4. **On the loss vs accuracy gap:**
> *"70.9% accuracy with 0.59 loss tells us the model is crossing the decision boundary correctly but with moderate confidence. It's classifying right but not assertively. More data would push predictions toward the extremes and reduce loss while maintaining or improving accuracy."*

5. **On GlobalAveragePooling:**
> *"We chose GlobalAveragePooling over Flatten deliberately. Flatten would give us 20,480 inputs to the first dense layer — that's 5.2 million parameters in one layer alone, completely intractable with 1,500 training images. GAP gives us 1,280 inputs and just 328K parameters."*

6. **On grayscale:**
> *"Microplastics in microscopy are identified by texture and shape, not color. Retaining color would add noise without adding signal. Grayscale also reduces memory 3× — important on Colab's free tier."*

7. **On the three-way evaluation split:**
> *"We were strict about the test set — it was never used for any decision during development. The 70.9% accuracy is a genuinely unbiased estimate of generalization performance."*

8. **On deployment readiness:**
> *"I wouldn't recommend this as a fully autonomous system at 70.9%. But as a human-in-the-loop screening tool — flag positives for operator review, collect real DHM images as new labeled data, retrain monthly — it's a solid first iteration."*

9. **On the industry context:**
> *"This wasn't an academic exercise — Gagangiri Foods had a real operational problem. Every design decision had a business consequence, not just a metric consequence. That changes how you think about tradeoffs."*

10. **On constraints → design:**
> *"Each constraint directly shaped a design decision. Limited compute → frozen base model. Small dataset → transfer learning + augmentation. Food safety context → optimise for recall over precision. Real engineering is constraint-solving."*

---

## 🚨 TRAP QUESTIONS — What NOT to Say

**Trap: "70.9% is low, isn't it?"**
❌ Don't say: "Yes, it's not great, we tried our best."
✅ Say: "On a balanced binary dataset, 70.9% represents ~21% improvement over random chance. For ~1,500 images with transfer learning and no domain-specific pretraining, it's a reasonable baseline. More domain-specific labeled data is the clearest path to improvement."

---

**Trap: "Why didn't you fine-tune MobileNetV2?"**
❌ Don't say: "We didn't think of it" or "It was too complex."
✅ Say: "Fine-tuning requires unfreezing base layers and training with a very low learning rate. With only ~1,500 images, the risk of destroying the pretrained features through overfitting outweighs the potential gain. Fine-tuning becomes viable with 5,000+ images per class."

---

**Trap: "Isn't transfer learning just copying someone else's work?"**
❌ Don't say: "Kind of, yes, we used a pretrained model."
✅ Say: "Transfer learning is standard engineering practice — you leverage existing components and build on top of them. We trained a custom 4-layer classification head, ran an ablation study, designed the preprocessing pipeline, and built the deployment API. Using MobileNetV2 as a feature extractor is like using a camera library rather than writing camera drivers — you focus your effort on the problem-specific part."

---

**Trap: "Why not use a simpler model like logistic regression or SVM?"**
❌ Don't say: "Because deep learning is better."
✅ Say: "Logistic regression and SVM operate on hand-crafted features. For microscopy images, defining what features indicate a microplastic particle requires domain expertise. CNN-based models learn these features automatically from labeled examples. Given we had labeled image data and a visual classification task, CNNs are the correct tool — and MobileNetV2 specifically because our dataset is too small to train a CNN from scratch."

---

**Trap: "What is your model's precision?"**
❌ Don't say: "It's about 70% like accuracy."
✅ Say: "Precision and accuracy are different. Accuracy is overall correctness. Precision specifically measures: of all the times the model predicted microplastic, how many were correct. Given our model's false positive tendency, precision is likely lower than recall. We prioritized recall because missing actual contamination is the more dangerous error."

---

**Trap: "Did you use any other datasets?"**
❌ Don't say: "No, just the Kaggle one."
✅ Say: "For training, we used the Kaggle microplastic dataset with ~1,562 labeled images. We also referenced OECD plastic waste statistics for the problem context and to support the motivation sections of our presentations. For the model's base, we indirectly used ImageNet through MobileNetV2's pretrained weights."

---

## 🗂️ TECH STACK — Quick Reference

| Layer | Technology | Why |
|-------|-----------|-----|
| Development | Google Colab | Free GPU, team access via Drive |
| Framework | TensorFlow / Keras | Industry standard, MobileNetV2 built-in |
| Base Model | MobileNetV2 | Lightweight, fast, good accuracy/param ratio |
| Data Pipeline | ImageDataGenerator | Augmentation + normalization built-in |
| Evaluation | scikit-learn | confusion_matrix, classification_report |
| Visualization | matplotlib | Training curve plots |
| API | Flask | Lightweight Python web framework |
| Image Handling (API) | Pillow | Image loading/resizing in Flask |
| Storage | Google Drive | Dataset, saved models |
| Version Control | GitHub | Code + notebook collaboration |
| Model Format | .h5 + SavedModel | Weights only + full deployment format |

---

## 📐 MODEL SUMMARY — One Glance

```
Input (128×128×3)
    ↓
MobileNetV2 — FROZEN — 3.4M params
    ↓
GlobalAveragePooling2D → 1280-dim
    ↓
Dense(256, ReLU) → Dropout(0.4)
    ↓
Dense(128, ELU) → Dropout(0.3)
    ↓
Dense(64, LeakyReLU) → Dropout(0.2)
    ↓
Dense(1, Sigmoid)
    ↓
P(microplastic) — threshold 0.5 → binary class
```

**Loss:** Binary Cross-Entropy | **Optimizer:** Adam | **Epochs:** 50  
**Test Accuracy:** 70.9% | **Test Loss:** 0.59 | **Error tendency:** False Positive

---

## 🏭 INDUSTRY CONTEXT — Quick Facts

| | |
|--|--|
| **Client** | Gagangiri Foods |
| **Duration** | Feb 2, 2026 – May 6, 2026 (3 months) |
| **Certificate** | Yes — issued by Gagangiri Foods |
| **Problem** | Microplastic contamination in production water |
| **Solution** | CNN + DHM integration for real-time detection |
| **Business impact** | Improved water purity → improved product quality |
| **Resume section** | Experience (not Projects) |

---

## ✅ PRE-INTERVIEW CHECKLIST

- [ ] Memorised all numbers in the table above
- [ ] Can define Transfer Learning, Dropout, Adam, BCE in one sentence each
- [ ] Know the 4 dense layers and their activations in order (256-ReLU, 128-ELU, 64-LeakyReLU, 1-Sigmoid)
- [ ] Know why false positives are acceptable in food safety
- [ ] Practised the 2-minute verbal pitch (see 05_project_story.md)
- [ ] Ready to say "Gagangiri Foods" by name, not "a food company"
- [ ] Know your specific individual contribution on the team
- [ ] Can explain precision vs recall vs F1 clearly
- [ ] Know why GlobalAveragePooling over Flatten
- [ ] Know why binary cross-entropy over MSE

---

*Keep this open on your phone during any pre-interview waiting time. The night before: read 03_qna.md. The morning of: read only this.*
