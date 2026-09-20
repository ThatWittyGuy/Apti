# 05 — Project Story & Verbal Narrative
### Indisposable | Gagangiri Foods Industry Project | Feb–May 2026
> 🎤 This doc is for speaking, not reading. Read it aloud. Practise until it sounds natural, not recited.

---

## Why You Need a Scripted Story

Every interview — technical, HR, managerial — will include some version of:
- *"Tell me about your final year project."*
- *"Walk me through a project you're proud of."*
- *"What's the most interesting thing you've built?"*

Most candidates fumble this. They either ramble for 10 minutes covering everything, or give a one-liner that invites no follow-up. Neither is good.

You need **three versions** of the same story: 30 seconds, 2 minutes, and 5 minutes. The interviewer's body language and follow-up questions tell you which depth to go to.

---

## The Core Story Structure (Memorise This Framework)

Every version follows the same spine:

```
HOOK (what and why it matters)
    ↓
CONTEXT (industry project, real client, real problem)
    ↓
YOUR APPROACH (key decisions with brief justification)
    ↓
RESULT (numbers + certificate + impact)
    ↓
REFLECTION (what it taught you / what you'd improve)
```

---

## VERSION 1 — 30-Second Pitch
*Use for: HR screens, networking events, "briefly tell me" situations, LinkedIn conversations*

> "For my final year, I worked on an industry project with Gagangiri Foods — a food manufacturing company — where we built a machine learning system to detect microplastic contamination in their water supply using computer vision. We used transfer learning on microscopy images and achieved around 71% accuracy on a balanced test set. The company issued us a formal completion certificate at the end of the three-month engagement. It was my first experience delivering an ML system for a real industry client."

**Why this works:**
- Names the company immediately (credibility)
- States the real-world problem in one sentence (context)
- Drops one technical term naturally (transfer learning)
- Gives a number (71%)
- Mentions the certificate without bragging
- Ends with a professional framing ("real industry client")

---

## VERSION 2 — 2-Minute Pitch
*Use for: Technical interviewers opening the conversation, panel rounds, most standard interviews*

> "My final year project was an industry engagement with Gagangiri Foods — a food company that needed to detect microplastic particles in their production water supply. Manual microscopic inspection was too slow and subjective for a production line, so they needed an automated system.

> We built an end-to-end image classification pipeline. The core was a CNN-based binary classifier — clean water versus microplastic-contaminated water. The main challenge was data: we only had about 1,500 labeled microscopy images, which isn't enough to train a deep network from scratch. So we used transfer learning — specifically MobileNetV2 pretrained on ImageNet — and froze the base model, only training a custom 4-layer classification head on top. We also applied heavy data augmentation — full 180-degree rotation, flipping, shifting — because microplastic particles under a microscope appear in any orientation.

> We ended up with 70.9% accuracy on the test set. The model has a false-positive tendency — it over-flags rather than under-flags — which is actually the safer error direction for food safety. Missing real contamination would be dangerous; false alarms are just inconvenient.

> The system was deployed via a Flask API connected to their digital holographic microscope and a dashboard for operators. At the end of three months, Gagangiri Foods issued us a formal project completion certificate.

> If I were doing it again, the biggest improvement would be collecting more labeled images from their actual microscope — real domain-specific data would likely push accuracy above 85%."

**Timing:** Practice until this is exactly 90–120 seconds. Not faster, not slower.

**Why this works:**
- Opens with the business problem, not the tech
- Explains WHY each decision was made (not just what)
- Gives the key number (70.9%) with context
- Addresses a limitation proactively (false positives) and reframes it intelligently
- Ends with a self-aware improvement — shows growth mindset
- Naturally mentions the certificate without making it the whole point

---

## VERSION 3 — 5-Minute Deep Dive
*Use for: Technical panel rounds, senior engineer interviews, research roles*

> "Let me give you the full picture.

> **The problem:** Gagangiri Foods processes large volumes of water in food production. Microplastic contamination is a food safety risk — regulatory and reputational. Their existing process was manual microscope inspection, which is slow and operator-dependent. They wanted an automated, near-real-time system integrated with their digital holographic microscope.

> **The data challenge:** No in-house labeled dataset existed, so we sourced the Kaggle Microplastic Dataset for Computer Vision — about 1,562 images, perfectly balanced between contaminated and clean samples. That's small for deep learning. It ruled out training from scratch — with 780 images per class, any deep CNN would overfit badly.

> **The model architecture:** We chose MobileNetV2 as a frozen feature extractor. It has 3.4 million parameters pretrained on ImageNet — a rich general-purpose visual feature extractor — and it's lightweight enough for real-time inference. We froze all of it and trained only a custom classification head: four dense layers at 256 with ReLU, 128 with ELU, 64 with Leaky ReLU, and 1 sigmoid output. We used GlobalAveragePooling instead of Flatten to avoid a parameter explosion in the first dense layer — Flatten would have given us 20,000 inputs and 5 million parameters in one layer, completely intractable with our dataset size.

> **The activation function choice:** This was empirical, not arbitrary. We ran an ablation study across six activation functions — ReLU, ELU, Leaky ReLU, SELU, tanh, sigmoid — holding everything else constant and measuring validation accuracy. The ReLU → ELU → Leaky ReLU combination emerged from that study. The intuition is: ReLU for the widest layer where you want sparse feature selection, ELU for the middle where smooth gradients matter, Leaky ReLU for the narrowest layer to ensure no neurons die during the compression.

> **Preprocessing:** We converted to grayscale — DHM images are monochromatic, and texture matters more than color for identifying plastics. We applied full 180-degree rotation augmentation because microscopy has no inherent orientation. Featurewise Z-score normalization matched MobileNetV2's training distribution.

> **Results:** 70.9% test accuracy, 0.59 binary cross-entropy loss. The high loss relative to accuracy tells us the model is crossing the decision boundary correctly but with only moderate confidence — it's not assertive in its predictions, which is typical of small-data scenarios. The model has a false positive tendency — it errs toward flagging contamination — which is precisely what the client asked for. From a food safety standpoint, missing actual contamination is catastrophic; false alarms are just operator inconvenience.

> **Deployment:** Flask API with the same preprocessing pipeline as training — that consistency is critical. Connected to a dashboard for operators. Model saved in both H5 weights format and TensorFlow SavedModel for portability.

> **What I'd do differently:** More data — that's the highest-leverage improvement. I'd also set up proper experiment tracking from day one using MLflow or W&B instead of informal notes. And I'd tune the classification threshold using a Precision-Recall curve rather than defaulting to 0.5 — for a recall-prioritized application, a lower threshold would likely improve sensitivity further.

> **The professional context:** This wasn't a textbook project. We had client meetings, a midsemester review where they gave us feedback on the confidence score output, a formal final presentation with a live demo, and a completion certificate from Gagangiri Foods at the end. It was my first full experience delivering ML to a non-technical client, and the communication and translation challenges were as valuable as the technical ones."

**Timing:** 4–6 minutes. Leave natural pauses after major sections — the interviewer will often jump in with follow-up questions, and that's a good thing.

---

## How to Introduce the Certificate Naturally

Many candidates either:
- Never mention the certificate (missed opportunity)
- Lead with "I have a certificate!" (sounds desperate)

The right way is to mention it as a **consequence of delivery**, not as a credential:

**In technical conversations:**
> *"...and at the end of the engagement, the company issued a formal completion certificate — which was validation that the system was functional and met their requirements, not just an academic deliverable."*

**In HR/behavioural conversations:**
> *"...what I'm proudest of isn't the accuracy number — it's that Gagangiri Foods issued us a formal completion certificate acknowledging that the system we built contributes to their actual water quality process. It went from code in a notebook to something a company signed off on."*

**If asked directly about certificates/credentials:**
> *"I have a completion certificate from Gagangiri Foods — the industry partner for my final year project. It was issued after we delivered a functional microplastic detection system they evaluated over three months. I can share it if that's useful for your records."*

---

## Transition Phrases — Moving Between Depth Levels

When an interviewer wants to go deeper after your 30-second pitch:

> *"Happy to go deeper on any part of that — the model architecture, the evaluation approach, the deployment setup, or the client engagement side. What's most relevant to this role?"*

When an interviewer seems satisfied with a high-level answer:

> *"I'm happy to keep it high level, or we can go into the technical specifics if that's useful — whichever is more helpful."*

When you're mid-explanation and losing the interviewer:

> *"I'm going into detail here — let me step back. The core point is [one sentence]. Happy to go deeper on any piece."*

When asked something you want to connect back to the project:

> *"That's actually something we ran into directly in this project — let me give you a concrete example..."*

---

## Handling Specific Interviewer Types

### The "So What?" Interviewer (Business-Focused)
They don't care about MobileNetV2. They want impact.

Lead with:
> *"The system automates what was a slow, manual inspection process — giving a food company near-real-time water quality decisions at their production line. The business value is contamination being caught before it reaches any product."*

Only go technical if they ask.

### The "How Does It Work?" Interviewer (Technically Curious)
They want to understand the pipeline. Give them the full architecture walk-through from Version 3 above, but pause after each component and check:
> *"Does that make sense, or should I clarify anything before moving on?"*

### The "What Went Wrong?" Interviewer (Critical Thinker)
They're testing intellectual honesty. Don't be defensive.

> *"The accuracy is 70.9%, which is meaningful but not production-ready as an autonomous system. The main limiting factor is dataset size — 1,500 images is genuinely small for this type of model. We were transparent about this with the client and framed the current system as a first iteration with a clear retraining path as more data is collected."*

### The "What's Transfer Learning?" Interviewer (Foundational Check)
Give the one-liner from the cheatsheet, then add:
> *"In practice for us, it meant we got 3.4 million parameters worth of visual feature extraction for free — features learned from 14 million images — and we only needed to train 370,000 task-specific parameters ourselves. That's what made a 1,500-image dataset viable."*

---

## Common Follow-Up Questions After the Pitch — Quick Answers

**"Why MobileNetV2 specifically?"**
> "Lightweight — 3.4M params vs ResNet50's 25M. Fast inference for real-time use. Good accuracy-to-parameter ratio. Practical for Colab compute budget."

**"What was your role in the team?"**
> "I handled [your actual role — be specific]. On a team of five, I owned [X], contributed to [Y], and presented [Z] during the evaluations."

**"How long did training take?"**
> "With a frozen base and small dataset, each run on Colab's GPU took about 15–30 minutes for 50 epochs. Fast enough for iteration."

**"Is the model in production?"**
> "We delivered it to Gagangiri Foods as a functional system — the certificate confirms their acceptance. Whether they've operationalised it beyond the demo we showed is beyond our visibility post-project."

**"What would you use instead of Flask for production?"**
> "FastAPI for async handling, or TensorFlow Serving for high-throughput production — Flask was appropriate for the project scope and timeline."

**"Why not use YOLOv8 or a detection model instead of classification?"**
> "Object detection would be valuable for localizing individual particles within an image. Our scope was sample-level classification — is this sample contaminated or not — which binary classification handles correctly and more simply. Detection would be a natural next iteration."

---

## The Opening — "Tell Me About Yourself"

When this leads into the project, use this bridge:

> *"...and for my final year, I had the opportunity to work on an industry project — which I think is the most relevant thing to what you're looking for. We built a machine learning-based microplastic detection system for Gagangiri Foods..."*

Then flow directly into Version 1 or Version 2 depending on the context.

---

## Red Flags to Avoid in Your Story

❌ **Don't say "we tried to" or "we attempted to"** — sounds like you failed. Say "we built," "we designed," "we implemented."

❌ **Don't say "the accuracy isn't very good"** — say "70.9% on a balanced test set, with a clear path to improvement through additional labeled data."

❌ **Don't say "I just used a pretrained model"** — transfer learning is a design choice, not a shortcut. Own it confidently.

❌ **Don't say "I'm not sure about that part, my teammate handled it"** — know the whole system. It's fine to say "I focused on X, but I can walk through how Y works as well."

❌ **Don't say "it was just a college project"** — it's an industry project with a certificate. That distinction is the entire point.

❌ **Don't undersell the DHM integration** — "we integrated with a digital holographic microscope" sounds impressive because it is. Say it clearly.

❌ **Don't memorise word-for-word** — memorise the structure and the key phrases. Natural delivery beats perfect recitation every time.

---

## Practice Schedule

**Day before interview:**
- Read 03_qna.md end-to-end (1 hour)
- Say Version 2 aloud 5 times until it flows naturally (15 min)

**Morning of interview:**
- Read 04_cheatsheet.md only (10 min)
- Say Version 1 and Version 2 once each, out loud (5 min)
- Done. Don't cram more.

**During interview:**
- If nervous: slow down, not speed up
- If you blank on a number: "roughly" is fine — "roughly 1,500 images," "around 70%"
- If asked something you don't know: "That's outside what I explored in this project, but my understanding of the underlying concept is..." and explain the concept

---

*This document is for speaking. Close your laptop. Say Version 2 out loud right now.*
