# A Pipeline for Extracting and Evaluating Quality-of-Life Tags from Therapist–Patient Conversations
## A Hands-On Workshop Tutorial

**Authors:** [Your Institution]  
**Date:** January 2026  
**Duration:** Full-day workshop (6-8 hours)  
**Target Audience:** NLP researchers, clinicians, data scientists, healthcare informaticists  

---

## Executive Summary

Quality-of-life (QoL) signals embedded in therapist–patient conversations are critical for understanding patient outcomes, yet remain largely unstructured in clinical practice. This workshop provides a reproducible, modular pipeline to extract, annotate, and evaluate QoL tags from open clinical conversations—comparing raw dialogue against clinician-developed summaries. Participants will implement three parallel tagging methods (rule-based, transformer-based, and LLM-driven), benchmark against established clinical NLP standards (Kiwi), and gain practical strategies for deploying QoL extraction in diverse healthcare settings. By the end, you'll have a working codebase, evaluation metrics, and actionable guidance for your own clinical NLP projects.

---

## Table of Contents

1. [Problem Definition](#problem-definition)
2. [Workshop Learning Objectives](#workshop-learning-objectives)
3. [Dataset Description and Resources](#dataset-description-and-resources)
4. [Challenges and Considerations](#challenges-and-considerations)
5. [Annotation Guidelines](#annotation-guidelines)
6. [Methods Overview](#methods-overview)
7. [Hands-On Implementation](#hands-on-implementation)
8. [Evaluation and Benchmarking](#evaluation-and-benchmarking)
9. [Reproducibility and Next Steps](#reproducibility-and-next-steps)
10. [References](#references)

---

## Problem Definition

### Why Quality-of-Life Extraction Matters

Quality-of-life is a multidimensional construct encompassing physical health, emotional well-being, social functioning, and daily living capabilities. In clinical practice, QoL signals are often:

- **Passively embedded** in free-text conversation and clinical notes rather than measured via structured surveys
- **Subjective and contextual**, requiring domain knowledge to identify and categorize
- **Variable across settings**, with different outpatient vs. inpatient documentation practices and across patient populations (e.g., seniors on Medicare/Medicaid, patients with chronic kidney disease)
- **Underutilized** in summary documents, raising questions about whether clinician summaries preserve the richness of patient experience

### Core Challenge

We ask: **Can we automatically extract QoL signals from raw therapist–patient conversations and compare the fidelity of those signals in clinician-developed summaries?**

To address this, we develop a modular pipeline that:

1. **Ingests** de-identified, open-source therapist–patient conversation transcripts
2. **Annotates** conversation segments with QoL domain labels using standardized taxonomies
3. **Generates** abstractive summaries that preserve clinical relevance and QoL cues
4. **Compares** QoL tagging performance between active conversations and clinician-derived summaries across multiple patient populations and care settings
5. **Evaluates** the effectiveness of rule-based, transformer-based, and LLM-driven methods using established clinical NLP benchmarks (Kiwi)

### Real-World Impact

Accurate, transferable QoL extraction enables:
- **Scalable measurement** of patient outcomes without relying solely on surveys
- **Evidence-based training** of clinicians on what QoL signals should be documented
- **Bias detection** in summary generation and model fairness across populations
- **Better clinical decision-making** informed by quantified patient experience

---

## Workshop Learning Objectives

By the end of this workshop, participants will:

1. ✅ **Understand** the landscape of open-source clinical conversation datasets and privacy considerations
2. ✅ **Design and implement** multi-level QoL annotation schemes aligned with clinical standards (PROMIS, Neuro-QoL)
3. ✅ **Build and compare** three QoL tagging methods: rule-based (lexical), transformer-based (BERT/ClinicalBERT), and LLM-prompted (GPT-4, LLaMA)
4. ✅ **Generate and evaluate** abstractive summaries that preserve QoL signals
5. ✅ **Conduct transferability analysis** across inpatient/outpatient settings and patient populations
6. ✅ **Apply Kiwi benchmarking standards** to report clinical NLP evaluation metrics
7. ✅ **Deploy a containerized pipeline** reproducible in diverse computational environments (local, cloud, institution-specific)
8. ✅ **Identify fairness and bias issues** in QoL extraction across demographic groups

---

## Dataset Description and Resources

### 1. Primary Open-Source Datasets for Workshop Use

#### **MentalChat16K: Conversational Mental Health Benchmark**
- **Source:** https://huggingface.co/datasets/ShenLab/MentalChat16K
- **GitHub:** https://github.com/PennShenLab/MentalChat16K
- **Description:** 16K question-answer pairs combining:
  - 6,338 anonymized transcripts from Behavioral Health Coaches and Caregivers (palliative/hospice care)
  - 9,775 synthetic mental health counseling conversations (depression, anxiety, grief, relationships)
- **Key Features:**
  - De-identified with consent obtained
  - Covers diverse mental health topics (33+ themes)
  - Structured as QA pairs (each conversation round)
  - Highly relevant for therapist–patient dialogue understanding
- **Workshop Utility:** Excellent starting point for QoL extraction; includes real conversations and synthetic diversity

#### **Therapist-Patient Conversation Dataset (Kaggle)**
- **Source:** https://www.kaggle.com/datasets/neelghoshal/therapist-patient-conversation-dataset
- **Description:** Open-source therapist–patient conversations with mental/situational issue tags
- **Key Features:**
  - Issue-level annotations (anxiety, depression, etc.)
  - Conversation patterns and response datasets
  - Suitable for fine-tuning QoL classifiers
- **Workshop Utility:** Supplementary dataset for validation and fairness testing

#### **Medical Conversation Corpus (100k+)**
- **Source:** https://www.kaggle.com/datasets/thedevastator/medical-conversation-corpus-100k
- **Description:** 100k+ doctor-patient conversations with medical terminology
- **Workshop Utility:** Broader clinical scope for testing transferability across medical domains

#### **OSCE Medical Conversations Dataset**
- **Source:** Nature Scientific Data (Kirange et al., 2022)
- **Description:** Simulated patient-physician conversations in OSCE format with audio and corrected transcripts
- **Key Features:**
  - Manually corrected transcripts (high quality)
  - Focus on respiratory cases (easily extensible)
  - Clear speaker diarization
- **Workshop Utility:** Demonstrates structured, high-quality clinical conversation transcription

### 2. Patient-Reported Outcomes (PRO) Benchmarks and Taxonomies

#### **PROMIS (Patient-Reported Outcomes Measurement Information System)**
- **Website:** https://www.promishealth.org/
- **NIH Common Fund:** https://commonfund.nih.gov/promis
- **HealthMeasures Dataverse:** https://dataverse.harvard.edu/dataverse/HealthMeasures
- **Key Domains:** Pain, fatigue, depression, anxiety, physical function, social role participation
- **Workshop Use:** Standardized QoL taxonomy for annotation; IRT-based scoring available
- **Scoring Service:** https://www.assessmentcenter.net/ac_scoringservice

#### **Neuro-QoL**
- **Part of PROMIS ecosystem**
- **Focuses on:** Neurological conditions (cognition, emotional well-being, mobility, participation)
- **Workshop Use:** Extended QoL taxonomy for neurological populations

#### **PROsetta Stone**
- **Linking Tool:** Translates between different PRO measure scores
- **Website:** https://www.promishealth.org/measures/proretta-stone
- **Linked Measures:** FACT/FACIT, GAD-7, HOOS/KOOS, ODI, PHQ-9, SF-36
- **Workshop Use:** Harmonizing QoL annotations across different measure systems

### 3. Clinical NLP Benchmarking Resources

#### **Kiwi: Clinical Information Extraction Tool**
- **Official Website:** https://kiwi.clinicalnlp.org/
- **GitHub Repository:** https://github.com/clinicalnlp/kiwi (upon request)
- **Developed by:** Yale Clinical NLP Lab
- **Features:**
  - Pre-trained BERT and LLaMA-based models for clinical NER and relation extraction
  - Both CPU and GPU versions available
  - Docker containerization for reproducibility
  - Benchmark datasets: UTP, MTSamples, MIMIC-III, i2b2
- **Performance Highlights:**
  - LLaMA-3-70B: +7% F1 on NER, +4% on RE vs. BERT
  - Excellent cross-institutional generalization
  - Instruction-tuned for clinical IE tasks
- **Workshop Use:** Baseline evaluation framework; can adapt for QoL tagging metrics
- **Reference:** Peng et al. (2026) - "Information Extraction from Clinical Notes: Are We Ready to Switch to LLMs?" *JAMIA*

#### **Yale Clinical NLP Lab**
- **Lab Website:** https://medicine.yale.edu/lab/clinical-nlp/
- **Tools:** Kiwi, MedViz, CDEMapper, DataMed
- **Publications:** Strong track record in clinical IE and transferability research

#### **i2b2/n2c2 Shared Tasks**
- **Historical:** 10 shared tasks 2006–2019, many focused on clinical IE
- **Datasets:** De-identified clinical notes from diverse institutions
- **Standard Metrics:** Precision, recall, F1 (exact and relaxed match)
- **Workshop Use:** Reference evaluation methodology

### 4. Pre-trained Models for Fine-Tuning

#### **Domain-Specific BERT Models**
- **ClinicalBERT**
  - Pre-trained on MIMIC clinical notes
  - Strong on readmission prediction, diagnoses, and clinical concept extraction
  - Paper: https://arxiv.org/abs/1904.05342
  
- **BioBERT**
  - Pre-trained on PubMed abstracts and PMC full-text
  - +0.62% F1 on NER, +2.80% on RE vs. general BERT
  - Paper: https://arxiv.org/abs/1901.08746

- **Med-BERT**
  - Pre-trained on large-scale structured EHR data
  - +1.21% to +6.14% AUC improvement on disease prediction
  - Paper: *Nature Digital Medicine* (2021)

#### **Large Language Models**
- **LLaMA-3 (Meta)**
  - Available in 7B, 13B, 70B parameter sizes
  - Instruction-tuned variants available
  - Excellent performance on low-resource clinical tasks
  
- **GPT-4 / GPT-4-Turbo (OpenAI)**
  - Strong few-shot prompting for QoL extraction
  - Recent study (2024) shows >90% accuracy on PRO extraction across institutions
  - Higher cost but excellent generalization
  
- **Publicly Available Alternatives:**
  - Llama-2-Chat (Meta) - open-source, fine-tunable
  - Mistral-7B (Mistral AI) - efficient, instruction-tuned
  - Zephyr-Alpha (HuggingFace) - open alternative

### 5. Supplementary Datasets for Validation and Fairness Testing

#### **HealthMeasures Dataverse**
- **Repository:** https://dataverse.harvard.edu/dataverse/HealthMeasures
- **Contains:** 40+ de-identified datasets with PROMIS, NIH Toolbox, Neuro-QoL, ASCQ-Me measures
- **Workshop Use:** Real PRO data for validating QoL extraction against ground truth

#### **MIMIC-III (Medical Information Mart for Intensive Care)**
- **Access:** https://mimic.physionet.org/
- **Size:** 60,000+ ICU admissions, 2.3M notes
- **Limitations:** Requires IRB approval; not suitable for immediate hands-on use but referenced in literature
- **Workshop Use:** Context for understanding clinical note variability and population diversity

#### **MedDialog (100k+ doctor-patient dialogues)**
- **English:** 260k dialogues, 51 medical specialties
- **Chinese:** 1.1M dialogues, 29 broad + 172 fine-grained specialties
- **Source:** Online medical consultation platforms
- **Workshop Use:** Cross-lingual and cross-specialty transferability studies

---

## Challenges and Considerations

### 1. Data Quality and Privacy

**Challenge:** Ensuring de-identification while preserving QoL linguistic cues

**Mitigation Strategies:**
- Use datasets with documented consent (e.g., MentalChat16K explicitly obtained consent post-anonymization)
- Validate de-identification using both automated tools and manual review
- Document privacy protocols in detail for workshop IRB compliance
- Provide clear guidance on HIPAA/GDPR considerations for extending pipeline to real data

**Tools:**
- De-identification verification: pymedtermind, Presidio (Microsoft)
- Privacy-preserving NLP: differential privacy, federated learning (advanced topics)

### 2. Annotation Consistency and Inter-Rater Agreement

**Challenge:** Multiple QoL taxonomies; diverse clinician/patient language; subjective labeling

**Mitigation Strategies:**
- Develop detailed annotation guidelines with exemplars (see [Annotation Guidelines](#annotation-guidelines) below)
- Conduct 2-3 round pilot annotation with feedback cycles
- Use Cohen's kappa (inter-rater agreement) as QC metric; target κ ≥ 0.70
- Implement fuzzy matching and relaxed span evaluation
- Use active learning to identify hard cases for expert review

**Metrics:**
- Cohen's kappa (single pair)
- Fleiss' kappa (3+ annotators)
- Krippendorff's alpha (handles partial overlap)
- F1-based agreement (lenient boundaries)

### 3. Domain Shift and Transferability

**Challenge:** Models trained on outpatient mental health conversations may fail on inpatient medical conversations; demographic variation (seniors vs. younger patients, kidney disease vs. general population)

**Mitigation Strategies:**
- Explicitly stratify datasets by care setting and population
- Train/validate/test on disjoint care settings to measure domain shift
- Compute fairness metrics (disparate impact, equalized odds) by demographic group
- Use domain adaptation techniques (adversarial training, DANN) if transfer performance drops >10%
- Report separate metrics by population for transparency

**Example Workflow:**
1. Train on outpatient mental health conversations (MentalChat16K)
2. Validate on held-out mental health data
3. Test on inpatient medical conversations (OSCE dataset)
4. Measure performance drop; if ≥10%, investigate failure modes
5. Fine-tune with 10-20% inpatient data; report performance recovery

### 4. Evaluation Metrics and Clinician Utility

**Challenge:** Precision/recall alone don't capture clinician usefulness; comparing active conversations vs. summaries requires novel metrics

**Metrics to Use:**
- **Per-Domain F1:** Macro-averaged F1 across QoL domains (primary metric)
- **Segment-Level Coverage:** % of QoL mentions captured by summary vs. raw conversation
- **Semantic Preservation:** Embedding similarity between QoL signals in raw vs. summary
- **Clinician Usefulness (Human Eval):** Small blind study (10-20 clinician ratings) on summary quality and QoL coverage
- **Fairness Metrics:** 
  - Disparate impact (difference in F1 across demographic groups ≤10%)
  - Equalized odds (true positive and false positive rates similar across groups)

### 5. Computational Resource Constraints

**Challenge:** Fine-tuning large models (70B LLaMA) requires significant GPU resources

**Solutions Tiered by Available Resources:**

| Resource Level | Recommended Approach | Estimated Time |
|---|---|---|
| **Local CPU only** | BERT-base fine-tuning, rule-based tagging | 2-4 hours |
| **Single GPU (V100/A100)** | ClinicalBERT fine-tuning, LLaMA-7B inference | 4-8 hours |
| **Multi-GPU or cloud** | LLaMA-70B fine-tuning, ensemble methods | 8-24 hours |
| **No local compute** | Cloud APIs (Hugging Face Inference, AWS SageMaker, Colab) | On-demand |

**Workshop Recommendation:** Provide three parallel tracks based on participant resources

---

## Annotation Guidelines

### QoL Domain Taxonomy

We define seven primary QoL domains, aligned with PROMIS and clinical practice:

\begin{table}
\begin{tabular}{|l|p{3cm}|p{4cm}|}
\hline
\textbf{Domain} & \textbf{Definition} & \textbf{Example Indicators} \\
\hline
\textbf{Physical Well-Being} & Absence of pain, fatigue, physical limitations & Energy, stamina, pain intensity, medication side effects \\
\hline
\textbf{Emotional Well-Being} & Mood, anxiety, depression, psychological distress & Sadness, worry, anger, emotional stability \\
\hline
\textbf{Social Functioning} & Relationships, social participation, isolation & Family ties, friendships, social activities, loneliness \\
\hline
\textbf{Activities of Daily Living} & Self-care, household, instrumental tasks & Dressing, bathing, cooking, errands, work \\
\hline
\textbf{Sleep Quality} & Sleep disturbance, daytime tiredness & Insomnia, sleep duration, nightmares, restlessness \\
\hline
\textbf{Pain and Discomfort} & Localized or general pain; severity and impact & Joint pain, headaches, chronic pain, pain duration \\
\hline
\textbf{Fatigue} & Tiredness, lack of energy, difficulty concentrating & Exhaustion, low motivation, mental fog \\
\hline
\end{tabular}
\end{table}

### Annotation Task Design

**Unit of Annotation:** Conversation turn (patient utterance or clinician utterance)

**Annotation Format:** Multi-label classification
- Each turn receives zero or more QoL domain labels
- Spans within turns can be marked for more granular analysis (optional advanced task)

### Step-by-Step Annotation Protocol

1. **Read the full conversation** to understand context (first pass, no labeling)

2. **Read turn-by-turn** and identify QoL signals:
   - Does this turn mention a symptom, functional limitation, mood, or life experience?
   - Map language to the domain taxonomy above

3. **Assign labels** using the following rules:
   - **Explicit mention** (high confidence): "I'm exhausted all the time" → Fatigue
   - **Implicit/contextual** (medium confidence): "I stopped going to my book club" → Social Functioning (implies withdrawal)
   - **Do NOT label** generic procedural talk: "Let's talk about that next week" (no QoL signal)

4. **Uncertainty handling:**
   - If unsure between two domains, choose the primary one and document uncertainty
   - If truly ambiguous, mark for expert review
   - Acceptable uncertainty: ≤10% of labels

5. **Clinician summary annotation:**
   - Apply the same domain taxonomy to clinician-generated summaries
   - Identify which QoL signals from raw conversation are preserved, omitted, or altered in the summary

### Example Annotations

**Example 1: Physical Well-Being**
```
Patient: "My knees have been killing me, especially when I go up stairs. 
I've had to stop my morning walks."

Annotator Label: Physical Well-Being (explicit pain + functional limitation)
Confidence: High
```

**Example 2: Multiple Domains**
```
Clinician: "How have you been sleeping?"
Patient: "Terrible. I lie awake for hours worried about my job, 
and then I'm exhausted at work the next day."

Annotator Labels: 
  - Sleep Quality (insomnia, lying awake)
  - Emotional Well-Being (worry)
  - Fatigue (exhaustion)
Confidence: High for all
```

**Example 3: Ambiguous / Contextual**
```
Patient: "My daughter hasn't called me in three weeks."

Annotator Label: Social Functioning (contextual isolation concern)
Confidence: Medium (inferred emotional impact; not explicit)
Discussion: Could also reflect Emotional Well-Being (sadness)
           → Choose Social Functioning as primary
```

### Inter-Annotator Agreement Procedure

**Team Annotation:**
1. First 50 conversation turns: All annotators independently label
2. Calculate Cohen's kappa and Fleiss' kappa
3. **Target:** κ ≥ 0.70 (substantial agreement)
4. **If below 0.70:** Conduct consensus meeting, refine guidelines, repeat on next 50 turns
5. **Once threshold met:** Proceed with full annotation (split workload)

**Quality Control (10% sample):**
- Randomly select 10% of remaining conversations
- Second annotator reviews; recalculate agreement
- If agreement drops, retrain and continue monitoring

---

## Methods Overview

### 1. Data Preprocessing Pipeline

**Input:** Raw conversation transcripts (text files, JSON, CSV)

**Steps:**

\begin{enumerate}
\item \textbf{De-identification Verification}
  \begin{itemize}
  \item Run automated tools: Presidio (Microsoft), pymedtermind
  \item Manual spot-check: 50 random conversations
  \item Flag any suspected PII; quarantine or re-anonymize
  \end{itemize}

\item \textbf{Transcript Normalization}
  \begin{itemize}
  \item Convert to UTF-8 encoding
  \item Remove markup, metadata artifacts
  \item Normalize punctuation and spacing
  \item Optionally: Correct OCR or speech-to-text errors (if audio-derived)
  \end{itemize}

\item \textbf{Speaker Diarization}
  \begin{itemize}
  \item Identify speaker turns (clinician vs. patient)
  \item Segment by speaker label: "[PATIENT]", "[CLINICIAN]"
  \item Validate: manual spot-check on 50 conversations
  \end{itemize}

\item \textbf{Sentence Segmentation}
  \begin{itemize}
  \item Use spaCy (Python) or NLTK
  \item Handle clinical abbreviations: "Dr.", "Mr.", "Mrs.", "etc."
  \item Preserve conversation structure (turn $\rightarrow$ sentence $\rightarrow$ token)
  \end{itemize}

\item \textbf{Annotation Alignment}
  \begin{itemize}
  \item Map QoL labels from annotators to turn/sentence level
  \item Handle overlapping spans with overlap resolution strategy (union, intersection, priority)
  \item Create ground-truth annotation file (JSON or TSV)
  \end{itemize}

\item \textbf{Data Splitting}
  \begin{itemize}
  \item Train / Validation / Test: 60\% / 20\% / 20\%
  \item Stratify by care setting (outpatient vs. inpatient) and population
  \item Ensure no conversation leakage (full conversations in single split)
  \end{itemize}
\end{enumerate}

**Output:** Preprocessed dataset in standardized schema:
```json
{
  "conversation_id": "conv_001",
  "care_setting": "outpatient",
  "population": "mental_health",
  "turns": [
    {
      "turn_id": 0,
      "speaker": "CLINICIAN",
      "text": "How have you been feeling?",
      "qol_labels": [],
      "sentences": [...]
    },
    {
      "turn_id": 1,
      "speaker": "PATIENT",
      "text": "Not great. I've been tired and sad.",
      "qol_labels": ["Fatigue", "Emotional Well-Being"],
      "confidence": [0.95, 0.95],
      "sentences": [...]
    }
  ],
  "clinician_summary": "Patient reports fatigue and low mood.",
  "summary_qol_labels": ["Fatigue", "Emotional Well-Being"]
}
```

### 2. QoL Tagging Methods

#### **Method 1: Rule-Based / Lexical Tagging**

**Approach:** Dictionary and pattern matching

**Implementation:**
- Build a curated lexicon of QoL keywords per domain (e.g., pain terms: "ache", "pain", "hurt"; fatigue terms: "tired", "exhausted", "energy")
- Use regular expressions and spaCy's rule-based matcher
- Optional: Ontology-based scoring (e.g., Unified Medical Language System - UMLS)

**Example Code Structure:**
```python
qol_lexicons = {
    "Fatigue": ["tired", "exhausted", "energy", "stamina"],
    "Physical Well-Being": ["pain", "ache", "disability", "mobility"],
    "Emotional Well-Being": ["sad", "anxious", "depressed", "mood"],
    ...
}

def rule_based_tagging(text, lexicons):
    labels = []
    for domain, keywords in lexicons.items():
        if any(kw in text.lower() for kw in keywords):
            labels.append(domain)
    return labels
```

**Advantages:**
- Fast, interpretable, no training required
- Transparent decision-making
- Useful as baseline and for low-resource scenarios

**Disadvantages:**
- Limited to exact/near-exact matches
- Requires manual lexicon curation and maintenance
- Poor performance on paraphrased or implicit mentions

**Expected F1:** 0.50–0.65

---

#### **Method 2: Transformer-Based Supervised Tagging**

**Approach:** Fine-tune BERT-family models on labeled conversation data

**Base Models:**
- ClinicalBERT (MIMIC pre-trained)
- BioBERT (PubMed pre-trained)
- BERT-base (general language, good baseline)

**Architecture:**
```
[Input Text] 
  → [Tokenization + Positional Embeddings]
  → [BERT Encoder (12 layers)] 
  → [Mean Pooling or [CLS] token]
  → [Dense Layer (hidden_dim → 7 QoL domains)]
  → [Sigmoid Activation]
  → [Multi-label Probabilities]
```

**Training:**
- Loss function: Binary cross-entropy (multi-label)
- Optimizer: AdamW
- Learning rate: 2e-5 (typical for fine-tuning)
- Epochs: 3–5 (with early stopping on validation F1)
- Batch size: 16–32 (depends on GPU memory)

**Example Hyperparameters:**
```python
model_config = {
    "base_model": "microsoft/BiomedNLP-PubMedBERT-base-uncased",
    "num_labels": 7,  # QoL domains
    "max_length": 512,
    "hidden_dropout_prob": 0.1,
    "attention_probs_dropout_prob": 0.1,
    "learning_rate": 2e-5,
    "num_epochs": 3,
    "batch_size": 16,
}
```

**Advantages:**
- Strong contextual understanding
- Proven on clinical text (ClinicalBERT/BioBERT)
- Handles implicit and paraphrased mentions
- Supports transfer learning with minimal labeled data

**Disadvantages:**
- Requires annotated training data (~300–500 examples for good performance)
- Slower inference than rule-based
- Black-box decision-making; harder to interpret

**Expected F1 (with 300+ labeled examples):** 0.75–0.85

---

#### **Method 3: Large Language Model (LLM) Prompting**

**Approach:** Few-shot in-context learning with instruction-tuned LLMs

**Models:**
- GPT-4 / GPT-4-Turbo (proprietary, expensive, highest quality)
- LLaMA-3-70B Instruct (open-source, high quality, requires GPU)
- Mistral-7B Instruct (efficient, good quality)
- Claude 3 (proprietary, good performance)

**Prompt Design:**

\begin{table}
\begin{tabular}{|p{15cm}|}
\hline
\textbf{System Prompt (defines task context)} \\
\hline
"You are an expert clinical NLP system. Your task is to identify quality-of-life (QoL) domains mentioned in therapist–patient conversations. QoL domains include: Physical Well-Being, Emotional Well-Being, Social Functioning, Activities of Daily Living, Sleep Quality, Pain, and Fatigue. \\

For each patient turn, classify which QoL domains are mentioned. Return labels as a JSON list. If no QoL domain is mentioned, return an empty list. Be conservative: only label explicitly mentioned or clearly implied domains." \\
\hline
\textbf{Few-Shot Examples (2–3 examples)} \\
\hline
"Example 1: Patient: 'I haven't been sleeping well, maybe 4 hours a night.' \\
QoL Labels: [Sleep Quality] \\
\\
Example 2: Patient: 'My knees hurt when I climb stairs, but I'm in good spirits.' \\
QoL Labels: [Physical Well-Being, Pain] \\
\\
Example 3: Clinician: 'Let's schedule a follow-up.' \\
QoL Labels: []" \\
\hline
\textbf{User Prompt (input turn)} \\
\hline
"Classify the following turn: [PATIENT/CLINICIAN]: [TEXT] \\
QoL Labels:" \\
\hline
\end{tabular}
\end{table}

**Implementation (Python pseudocode):**
```python
import openai  # or use Hugging Face Inference API

def llm_qol_tagging(text, model="gpt-4", temperature=0.2):
    system_prompt = "You are an expert clinical QoL classifier..."
    few_shot_examples = "Example 1: Patient: 'I'm exhausted...' → [Fatigue]..."
    user_prompt = f"Classify: {text}\nQoL Labels:"
    
    response = openai.ChatCompletion.create(
        model=model,
        messages=[
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": few_shot_examples + "\n" + user_prompt}
        ],
        temperature=temperature,
        max_tokens=50
    )
    
    # Parse JSON response and return labels
    return parse_labels(response.choices[0].message.content)
```

**Hyperparameters:**
- Temperature: 0.0–0.2 (lower = more consistent)
- Few-shot examples: 2–5 (more helps, but increases cost/latency)
- Model: GPT-4 (best) > LLaMA-70B > Mistral-7B (speed/cost tradeoff)

**Advantages:**
- Highest accuracy on complex, implicit mentions (F1 often 0.82–0.92)
- No fine-tuning required; works with minimal labeled data
- Excellent generalization across care settings and populations
- Interpretable via prompt engineering

**Disadvantages:**
- API costs for proprietary models (GPT-4 ~$0.03–0.06 per 1K tokens)
- Latency (1–10 sec per turn depending on model)
- Hallucination risk (over-predicting labels); requires careful prompt design
- Dependency on external service (if using API)

**Expected F1:** 0.82–0.92 (best overall, especially cross-institutional)

**Mitigation for Hallucination:**
```python
def post_process_llm_labels(llm_labels, turn_text, lexicon):
    """
    Sanity check: confirm high-confidence LLM predictions against lexicon
    """
    filtered = []
    for label in llm_labels:
        # Check if domain keywords exist in text
        if domain_keywords[label] in turn_text.lower():
            filtered.append(label)  # High confidence
        else:
            # LLM predicted without explicit keyword; mark as uncertain
            if confidence_threshold(turn_text, label) > 0.7:
                filtered.append(label)
    return filtered
```

---

### 3. Clinician Summary Generation and QoL Preservation

**Task:** Generate abstractive summaries from active conversations; evaluate whether QoL cues are preserved

**Approach 1: Template-Based Extractive Summarization**
- Select key patient utterances (highest QoL signal density)
- Concatenate into summary; preserve original language
- Fast, interpretable, but limited coverage

**Approach 2: Transformer-Seq2Seq (BART, T5)**
- Fine-tune on {conversation, clinician-written summary} pairs
- Generate new summaries that mimic clinician style
- Better coherence and clinical relevance than extractive

**Example Code:**
```python
from transformers import BartForConditionalGeneration, BartTokenizer

model = BartForConditionalGeneration.from_pretrained("facebook/bart-large-cnn")
tokenizer = BartTokenizer.from_pretrained("facebook/bart-large-cnn")

# Prepare conversation as input
conversation_text = " ".join([turn["text"] for turn in turns])
inputs = tokenizer(conversation_text, max_length=1024, truncation=True, return_tensors="pt")

# Generate summary
summary_ids = model.generate(inputs["input_ids"], max_length=128, min_length=30, num_beams=4)
summary = tokenizer.decode(summary_ids[0], skip_special_tokens=True)
```

**Approach 3: LLM-Based Abstractive Summarization (GPT-4, LLaMA)**
- Prompt GPT-4: "Summarize this therapist–patient conversation in 2-3 sentences, emphasizing quality-of-life concerns."
- Highest quality; aligns with clinician expectations
- Enables joint evaluation: clinician-written summary vs. LLM-generated summary

**QoL Preservation Metrics:**

\begin{table}
\begin{tabular}{|l|p{5cm}|p{3cm}|}
\hline
\textbf{Metric} & \textbf{Definition} & \textbf{Interpretation} \\
\hline
Segment Coverage & \% of QoL segments in raw conversation captured by summary & High = summary faithful to source \\
\hline
Domain Coverage & \# of QoL domains in raw vs. summary & High = domain diversity preserved \\
\hline
Semantic Similarity & Embedding cosine similarity (raw QoL cues vs. summary QoL cues) & High = meaning preserved despite paraphrase \\
\hline
Length Ratio & (Summary length) / (Conversation length) & Compression ratio; 0.1–0.2 typical \\
\hline
Clinician Rating & Manual judgment: "Does summary capture key QoL issues?" (1–5 scale) & Gold standard; collect on 20 summaries \\
\hline
\end{tabular}
\end{table}

---

### 4. Evaluation Framework and Benchmarking

**Primary Evaluation Metrics** (Kiwi-aligned):

\begin{table}
\begin{tabular}{|l|p{4cm}|p{3cm}|}
\hline
\textbf{Metric} & \textbf{Definition} & \textbf{Use Case} \\
\hline
Macro-F1 & Average F1 across 7 QoL domains & Primary metric; fairness across domains \\
\hline
Micro-F1 & F1 treating all labels equally (global) & Overall method comparison \\
\hline
Domain F1 & Per-domain F1 & Identify weak domains \\
\hline
Cohen's Kappa ($\kappa$) & Inter-annotator agreement & Annotation quality (target $\geq 0.70$) \\
\hline
AUROC & Area under receiver-operating curve & Ranking quality of QoL signals \\
\hline
Coverage & \% of true labels predicted (recall) & Clinical sensitivity \\
\hline
Precision & \% of predicted labels correct & Clinical specificity \\
\hline
\end{tabular}
\end{table}

**Fairness Metrics:**

| Metric | Definition | Target |
|---|---|---|
| **Disparate Impact Ratio (DIR)** | Selection rate for Group A / Selection rate for Group B | ≥ 0.80 (FDA 4/5 rule) |
| **Equalized Odds** | True positive rate parity across groups | Difference ≤ 0.05 (5 percentage points) |
| **Predictive Parity** | Positive predictive value (precision) parity | Difference ≤ 0.05 |

**Cross-Setting Transferability:**

1. **In-Domain Validation** (same population, same setting)
   - Train on outpatient mental health (MentalChat16K)
   - Test on held-out outpatient mental health
   - Expected F1: 0.80–0.90 (baseline)

2. **Out-of-Domain Generalization** (different setting or population)
   - Test on inpatient medical conversations (OSCE)
   - Measure performance drop (expect 5–15% drop)
   - If drop ≥15%, consider domain adaptation

3. **Population Fairness**
   - Stratify test by age, gender, condition (if available)
   - Report separate F1 by group
   - Compute fairness metrics

---

## Hands-On Implementation

### Workshop Part 1: Environment Setup (30 min)

**Objective:** Set up reproducible computational environment

#### Option A: Local Setup (Docker)
```bash
# Clone workshop repository
git clone https://github.com/your-org/clinical-qol-nlp-workshop.git
cd clinical-qol-nlp-workshop

# Build Docker image
docker build -t clinical-qol-nlp:latest .

# Run container with Jupyter
docker run -p 8888:8888 -v $(pwd):/workspace clinical-qol-nlp:latest \
  jupyter notebook --ip=0.0.0.0 --no-browser --allow-root
```

#### Option B: Cloud (Google Colab)
```bash
# Open Colab notebook
# Run setup cell to install dependencies:
!pip install transformers datasets huggingface_hub torch scikit-learn spacy
!python -m spacy download en_core_web_md
```

#### Option C: Conda (Local)
```bash
conda create -n clinical-qol python=3.10
conda activate clinical-qol
pip install -r requirements.txt
```

**Required Libraries:**
- transformers (HuggingFace)
- torch (PyTorch)
- scikit-learn (evaluation)
- pandas, numpy (data handling)
- spaCy (NLP preprocessing)
- datasets (HuggingFace datasets)
- openai or anthropic (LLM APIs, optional)

**Download Datasets:**
```python
from datasets import load_dataset

# MentalChat16K
mental_chat = load_dataset("ShenLab/MentalChat16K")

# Example therapist-patient conversations
therapist_data = ...  # Load from Kaggle or provided URL
```

**Validate Setup:**
```python
import torch
print(f"PyTorch version: {torch.__version__}")
print(f"GPU available: {torch.cuda.is_available()}")
print(f"GPU name: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'N/A'}")
```

---

### Workshop Part 2: Data Preprocessing and Annotation (1.5 hours)

**Notebook:** `notebooks/1_preprocess_annotate.ipynb`

**Tasks:**

1. **Load MentalChat16K dataset**
   ```python
   from datasets import load_dataset
   data = load_dataset("ShenLab/MentalChat16K")
   
   # Inspect structure
   print(data["train"][0])
   print(f"Dataset size: {len(data['train'])} conversations")
   ```

2. **Preprocessing Pipeline**
   ```python
   import spacy
   from utils.preprocess import normalize_text, diarize_speakers, segment_sentences
   
   nlp = spacy.load("en_core_web_md")
   
   processed_conversations = []
   for conv in data["train"][:100]:  # First 100 for hands-on
       # Normalize
       text = normalize_text(conv["conversation"])
       
       # Segment into turns
       turns = diarize_speakers(text)
       
       # Segment each turn into sentences
       for turn in turns:
           doc = nlp(turn["text"])
           turn["sentences"] = [sent.text for sent in doc.sents]
       
       processed_conversations.append({
           "id": conv["id"],
           "turns": turns,
           "clinician_summary": conv.get("summary", "")
       })
   ```

3. **Manual Annotation (Participant Exercise)**
   - Provide 10 sample conversations
   - Participants annotate 50 turns each (15 min)
   - Calculate inter-rater agreement
   - Discuss disagreements and refine guidelines
   
   **Annotation Interface (Simple Web Form or Jupyter):**
   ```python
   from IPython.display import display, HTML
   
   def annotate_turn(turn_id, speaker, text):
       # Display turn
       print(f"[{speaker}]: {text}")
       # Present checkboxes for 7 QoL domains
       # Collect participant selection
       # Store in annotations.json
   
   for i, turn in enumerate(sample_turns):
       annotate_turn(i, turn["speaker"], turn["text"])
   ```

4. **Compute Inter-Annotator Agreement**
   ```python
   from sklearn.metrics import cohen_kappa_score
   
   # Load 2+ annotators' labels
   annotations_a = [...] # Annotator A's labels
   annotations_b = [...] # Annotator B's labels
   
   kappa = cohen_kappa_score(annotations_a, annotations_b)
   print(f"Cohen's Kappa: {kappa:.3f}")
   # Target: kappa >= 0.70
   ```

5. **Create Ground-Truth Dataset**
   ```python
   import json
   
   ground_truth = {
       "dataset_name": "MentalChat16K-QoL-Annotated",
       "num_conversations": 100,
       "num_turns": 1234,
       "qol_domains": [
           "Physical Well-Being",
           "Emotional Well-Being",
           "Social Functioning",
           "Activities of Daily Living",
           "Sleep Quality",
           "Pain",
           "Fatigue"
       ],
       "conversations": processed_conversations,
       "split": {
           "train": 0.6,
           "val": 0.2,
           "test": 0.2
       }
   }
   
   with open("data/mental_chat_qol_annotated.json", "w") as f:
       json.dump(ground_truth, f, indent=2)
   ```

---

### Workshop Part 3: Method Implementation (2.5 hours)

**Notebook:** `notebooks/2_methods_implementation.ipynb`

#### Task 1: Rule-Based Tagging (20 min)

```python
import re
from collections import defaultdict

# Define QoL lexicons per domain
qol_lexicons = {
    "Fatigue": [
        "tired", "exhausted", "fatigue", "energy", "stamina", 
        "lethargic", "drained", "wiped out"
    ],
    "Emotional Well-Being": [
        "sad", "depressed", "anxious", "anxious", "worried", "stressed",
        "happy", "content", "mood", "emotions", "crying"
    ],
    "Physical Well-Being": [
        "pain", "ache", "hurt", "sore", "disability", "function",
        "mobility", "strength", "weakness", "ill"
    ],
    "Sleep Quality": [
        "sleep", "insomnia", "restless", "nightmare", "tiredness",
        "fatigue", "awake", "doze", "nap"
    ],
    "Social Functioning": [
        "friend", "family", "social", "alone", "lonely", "isolation",
        "relationship", "partner", "group", "communicate", "interact"
    ],
    "Activities of Daily Living": [
        "cooking", "cleaning", "dressing", "bathing", "shower",
        "walk", "exercise", "work", "activity", "task", "chore"
    ],
    "Pain": [
        "pain", "ache", "hurt", "sore", "discomfort", "cramp",
        "migraine", "headache", "arthritis", "injury"
    ]
}

def rule_based_tagging(text):
    """Simple rule-based QoL tagging"""
    text_lower = text.lower()
    labels = []
    
    for domain, keywords in qol_lexicons.items():
        if any(kw in text_lower for kw in keywords):
            labels.append(domain)
    
    return labels

# Test on sample turns
test_turns = [
    "[PATIENT]: I've been so tired lately and can't sleep.",
    "[CLINICIAN]: How long has this been going on?",
    "[PATIENT]: About two weeks. My legs also hurt when I walk.",
]

for turn in test_turns:
    labels = rule_based_tagging(turn)
    print(f"Turn: {turn[:50]}...\nLabels: {labels}\n")
```

**Evaluation:**
```python
from sklearn.metrics import precision_recall_fscore_support, f1_score

# Predict on test set
y_pred = [rule_based_tagging(turn["text"]) for turn in test_data]
y_true = [turn["qol_labels"] for turn in test_data]

# Multi-label evaluation
# Flatten to binary matrix for per-domain evaluation
from sklearn.preprocessing import MultiLabelBinarizer

mlb = MultiLabelBinarizer()
y_true_bin = mlb.fit_transform(y_true)
y_pred_bin = mlb.transform(y_pred)

# Per-domain F1
for i, domain in enumerate(mlb.classes_):
    p, r, f1, _ = precision_recall_fscore_support(
        y_true_bin[:, i], y_pred_bin[:, i], average=None
    )
    print(f"{domain}: P={p[1]:.3f}, R={r[1]:.3f}, F1={f1[1]:.3f}")

# Macro F1
macro_f1 = f1_score(y_true_bin, y_pred_bin, average="macro")
print(f"\nMacro F1: {macro_f1:.3f}")
```

---

#### Task 2: Transformer-Based Tagging (1.5 hours)

```python
import torch
from transformers import (
    AutoTokenizer, AutoModelForSequenceClassification,
    TextClassificationPipeline
)
from torch.utils.data import DataLoader, Dataset
import torch.nn.functional as F

# 1. Load pre-trained model
model_name = "microsoft/BiomedNLP-PubMedBERT-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_name)
base_model = AutoModelForSequenceClassification.from_pretrained(
    model_name,
    num_labels=7,  # 7 QoL domains
    problem_type="multi_label_classification"
)

# Move to GPU if available
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
base_model.to(device)

# 2. Create PyTorch Dataset
class QoLDataset(Dataset):
    def __init__(self, texts, labels, tokenizer, max_length=512):
        self.texts = texts
        self.labels = labels
        self.tokenizer = tokenizer
        self.max_length = max_length
    
    def __len__(self):
        return len(self.texts)
    
    def __getitem__(self, idx):
        text = self.texts[idx]
        labels = self.labels[idx]
        
        # Tokenize
        encoding = self.tokenizer(
            text,
            max_length=self.max_length,
            padding="max_length",
            truncation=True,
            return_tensors="pt"
        )
        
        # Convert multi-label to binary vector
        label_vec = torch.zeros(7)
        for label in labels:
            domain_idx = domain_to_idx[label]
            label_vec[domain_idx] = 1.0
        
        return {
            "input_ids": encoding["input_ids"].squeeze(),
            "attention_mask": encoding["attention_mask"].squeeze(),
            "labels": label_vec
        }

# Map domain names to indices
domains = [
    "Physical Well-Being", "Emotional Well-Being", "Social Functioning",
    "Activities of Daily Living", "Sleep Quality", "Pain", "Fatigue"
]
domain_to_idx = {d: i for i, d in enumerate(domains)}
idx_to_domain = {i: d for d, i in domain_to_idx.items()}

# 3. Prepare train/val/test splits
train_texts = [turn["text"] for turn in train_data]
train_labels = [turn["qol_labels"] for turn in train_data]

val_texts = [turn["text"] for turn in val_data]
val_labels = [turn["qol_labels"] for turn in val_data]

test_texts = [turn["text"] for turn in test_data]
test_labels = [turn["qol_labels"] for turn in test_data]

# Create datasets
train_dataset = QoLDataset(train_texts, train_labels, tokenizer)
val_dataset = QoLDataset(val_texts, val_labels, tokenizer)
test_dataset = QoLDataset(test_texts, test_labels, tokenizer)

# Data loaders
train_loader = DataLoader(train_dataset, batch_size=16, shuffle=True)
val_loader = DataLoader(val_dataset, batch_size=16, shuffle=False)
test_loader = DataLoader(test_dataset, batch_size=16, shuffle=False)

# 4. Training loop
from torch.optim import AdamW
from tqdm import tqdm

optimizer = AdamW(base_model.parameters(), lr=2e-5)
num_epochs = 3
best_val_f1 = 0.0

for epoch in range(num_epochs):
    # Training
    base_model.train()
    train_loss = 0.0
    
    for batch in tqdm(train_loader, desc=f"Epoch {epoch+1}"):
        input_ids = batch["input_ids"].to(device)
        attention_mask = batch["attention_mask"].to(device)
        labels = batch["labels"].to(device)
        
        optimizer.zero_grad()
        
        # Forward pass
        outputs = base_model(
            input_ids=input_ids,
            attention_mask=attention_mask,
            labels=labels
        )
        loss = outputs.loss
        
        # Backward pass
        loss.backward()
        optimizer.step()
        
        train_loss += loss.item()
    
    avg_train_loss = train_loss / len(train_loader)
    print(f"Epoch {epoch+1} - Avg Loss: {avg_train_loss:.4f}")
    
    # Validation
    base_model.eval()
    val_preds = []
    val_truths = []
    
    with torch.no_grad():
        for batch in val_loader:
            input_ids = batch["input_ids"].to(device)
            attention_mask = batch["attention_mask"].to(device)
            labels = batch["labels"].to(device)
            
            outputs = base_model(input_ids=input_ids, attention_mask=attention_mask)
            logits = outputs.logits
            
            # Apply threshold (default 0.5)
            preds = (torch.sigmoid(logits) > 0.5).cpu().numpy()
            
            val_preds.extend(preds)
            val_truths.extend(labels.cpu().numpy())
    
    # Compute macro F1 on validation
    val_f1 = f1_score(val_truths, val_preds, average="macro")
    print(f"Validation Macro F1: {val_f1:.4f}")
    
    # Save best model
    if val_f1 > best_val_f1:
        best_val_f1 = val_f1
        base_model.save_pretrained("./checkpoints/best_transformer_model")

# 5. Evaluate on test set
print("\n=== Test Set Evaluation ===")
base_model = AutoModelForSequenceClassification.from_pretrained("./checkpoints/best_transformer_model")
base_model.to(device)
base_model.eval()

test_preds = []
test_truths = []

with torch.no_grad():
    for batch in test_loader:
        input_ids = batch["input_ids"].to(device)
        attention_mask = batch["attention_mask"].to(device)
        labels = batch["labels"].to(device)
        
        outputs = base_model(input_ids=input_ids, attention_mask=attention_mask)
        logits = outputs.logits
        preds = (torch.sigmoid(logits) > 0.5).cpu().numpy()
        
        test_preds.extend(preds)
        test_truths.extend(labels.cpu().numpy())

# Per-domain results
test_preds = np.array(test_preds)
test_truths = np.array(test_truths)

print("\nPer-Domain F1 Scores:")
for i, domain in enumerate(domains):
    p, r, f1, _ = precision_recall_fscore_support(
        test_truths[:, i], test_preds[:, i], average=None
    )
    print(f"  {domain}: P={p[1]:.3f}, R={r[1]:.3f}, F1={f1[1]:.3f}")

# Macro and Micro F1
macro_f1 = f1_score(test_truths, test_preds, average="macro")
micro_f1 = f1_score(test_truths, test_preds, average="micro")

print(f"\nMacro F1: {macro_f1:.3f}")
print(f"Micro F1: {micro_f1:.3f}")
```

---

#### Task 3: LLM-Based Prompting (1 hour)

```python
import openai
import json
from tenacity import retry, stop_after_attempt, wait_random_exponential

# Set API key
openai.api_key = "your-api-key-here"

# Define system and few-shot prompt
SYSTEM_PROMPT = """You are an expert clinical NLP system specialized in identifying quality-of-life (QoL) signals in therapist–patient conversations.

QoL Domains:
1. Physical Well-Being: Pain, mobility, strength, physical limitations
2. Emotional Well-Being: Mood, anxiety, depression, psychological distress
3. Social Functioning: Relationships, social activities, loneliness
4. Activities of Daily Living: Self-care, work, household tasks
5. Sleep Quality: Sleep disturbance, insomnia, daytime sleepiness
6. Pain: Pain intensity, location, duration, impact
7. Fatigue: Tiredness, lack of energy, difficulty concentrating

Task: For each conversation turn, identify all applicable QoL domains.

Response Format: Return a JSON object with:
{
  "qol_domains": ["Domain1", "Domain2"],
  "confidence": [0.95, 0.88],
  "reasoning": "Brief explanation of why these domains were selected"
}

Be conservative: Only label domains explicitly mentioned or clearly implied by context.
"""

FEW_SHOT_EXAMPLES = """
Example 1:
[PATIENT]: "I'm exhausted all the time, can barely get out of bed in the morning."
Response:
{
  "qol_domains": ["Fatigue", "Emotional Well-Being"],
  "confidence": [0.98, 0.75],
  "reasoning": "Patient explicitly mentions exhaustion (Fatigue) and implied depression/low mood (Emotional Well-Being)"
}

Example 2:
[CLINICIAN]: "Let's discuss your medication side effects."
Response:
{
  "qol_domains": [],
  "confidence": [],
  "reasoning": "Clinician turn with procedural content; no direct QoL signal"
}

Example 3:
[PATIENT]: "My knees hurt when I climb stairs, so I've stopped going to the gym with my friends."
Response:
{
  "qol_domains": ["Physical Well-Being", "Pain", "Social Functioning"],
  "confidence": [0.95, 0.95, 0.90],
  "reasoning": "Explicit pain and mobility limitation (Physical/Pain); implied social withdrawal (Social Functioning)"
}
"""

@retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(3))
def call_llm_qol_tagger(turn_text, model="gpt-4", temperature=0.1):
    """Call LLM API with retry logic for rate limiting"""
    
    user_message = f"""Classify this conversation turn:

{turn_text}

Response:"""
    
    response = openai.ChatCompletion.create(
        model=model,
        messages=[
            {"role": "system", "content": SYSTEM_PROMPT},
            {"role": "user", "content": FEW_SHOT_EXAMPLES + "\n" + user_message}
        ],
        temperature=temperature,
        max_tokens=200
    )
    
    return response.choices[0].message.content

def parse_llm_response(response_text):
    """Parse LLM JSON response; handle failures gracefully"""
    try:
        # Extract JSON from response
        json_start = response_text.find("{")
        json_end = response_text.rfind("}") + 1
        json_str = response_text[json_start:json_end]
        result = json.loads(json_str)
        return result
    except (json.JSONDecodeError, ValueError) as e:
        print(f"Failed to parse LLM response: {e}")
        return {"qol_domains": [], "confidence": [], "reasoning": "Parse error"}

# Test on sample turns
test_turns = [
    "[PATIENT]: I haven't slept well in weeks. My back aches from lying down so much.",
    "[CLINICIAN]: How is your appetite?",
    "[PATIENT]: It's terrible. I have no energy and I'm so sad."
]

llm_predictions = []
for turn in test_turns:
    print(f"\nTurn: {turn[:60]}...")
    response = call_llm_qol_tagger(turn)
    parsed = parse_llm_response(response)
    llm_predictions.append(parsed)
    print(f"Predicted QoL Domains: {parsed['qol_domains']}")
    print(f"Confidence: {parsed['confidence']}")
```

**Cost Estimation:**
- GPT-4: ~$0.03 per 1K input tokens, $0.06 per 1K output tokens
- For 1000 conversation turns (~20K tokens): ~$0.60–$1.00
- LLaMA-70B (self-hosted): Free but requires GPU (~$0.50–$2/hour cloud rental)

---

### Workshop Part 4: Comparison and Evaluation (1.5 hours)

**Notebook:** `notebooks/3_evaluation_comparison.ipynb`

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.metrics import f1_score, precision_recall_fscore_support, roc_auc_score

# Compile predictions from all three methods
results = {
    "Rule-Based": rule_based_predictions,
    "Transformer": transformer_predictions,
    "LLM-Prompting": llm_predictions
}

# Evaluation on test set
evaluation_report = {}

for method_name, predictions in results.items():
    # Multi-label evaluation
    y_pred_bin = mlb.transform(predictions)
    y_true_bin = mlb.transform([turn["qol_labels"] for turn in test_data])
    
    # Per-domain F1
    per_domain_f1 = {}
    for i, domain in enumerate(domains):
        p, r, f1, _ = precision_recall_fscore_support(
            y_true_bin[:, i], y_pred_bin[:, i], average=None
        )
        per_domain_f1[domain] = f1[1] if len(f1) > 1 else 0.0
    
    # Macro and micro F1
    macro_f1 = f1_score(y_true_bin, y_pred_bin, average="macro")
    micro_f1 = f1_score(y_true_bin, y_pred_bin, average="micro")
    
    evaluation_report[method_name] = {
        "macro_f1": macro_f1,
        "micro_f1": micro_f1,
        "per_domain_f1": per_domain_f1
    }

# Create comparison table
comparison_df = pd.DataFrame({
    method: [eval_report[method]["macro_f1"] for method in results.keys()]
    for eval_report in [evaluation_report]
})

print("=== Method Comparison: Macro F1 ===")
for method, report in evaluation_report.items():
    print(f"{method}: {report['macro_f1']:.3f}")

# Visualize results
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Macro F1 comparison
methods = list(results.keys())
macro_f1_scores = [evaluation_report[m]["macro_f1"] for m in methods]
axes[0].bar(methods, macro_f1_scores, color=["#3498db", "#e74c3c", "#2ecc71"])
axes[0].set_ylabel("Macro F1 Score")
axes[0].set_title("Method Comparison: Overall Performance")
axes[0].set_ylim([0, 1.0])
for i, score in enumerate(macro_f1_scores):
    axes[0].text(i, score + 0.02, f"{score:.3f}", ha="center")

# Per-domain F1 for best method (usually LLM)
best_method = max(results.keys(), key=lambda m: evaluation_report[m]["macro_f1"])
per_domain = evaluation_report[best_method]["per_domain_f1"]
axes[1].barh(domains, [per_domain[d] for d in domains], color="#2ecc71")
axes[1].set_xlabel("F1 Score")
axes[1].set_title(f"Per-Domain Performance ({best_method})")
axes[1].set_xlim([0, 1.0])
for i, (domain, score) in enumerate(per_domain.items()):
    axes[1].text(score + 0.02, i, f"{score:.3f}", va="center")

plt.tight_layout()
plt.savefig("./results/method_comparison.png", dpi=300, bbox_inches="tight")
print("\nVisualization saved to ./results/method_comparison.png")
```

---

## Evaluation and Benchmarking

### Kiwi Benchmark Integration

**How to Apply Kiwi Metrics to QoL Tagging:**

Kiwi provides NER and RE benchmarking; adapt for QoL multi-label classification:

```python
# Install Kiwi
!pip install kiwi-clinical

# (Kiwi benchmarks NER/RE; for QoL tagging use standard sklearn metrics but report Kiwi-style)
# Kiwi-aligned reporting: Exact match F1 + Relaxed (overlapping spans allowed) F1

from kiwi.evaluation import compute_metrics

# Standard metrics (adapted for multi-label QoL)
metrics = {
    "macro_f1": f1_score(y_true, y_pred, average="macro"),
    "micro_f1": f1_score(y_true, y_pred, average="micro"),
    "per_label_f1": {domain: f1_score(y_true_per_label, y_pred_per_label) for domain in domains},
    "hamming_loss": hamming_loss(y_true, y_pred),  # Fraction of incorrectly predicted labels
    "coverage": coverage_error(y_true, y_pred),  # Ranking loss
}

print("=== Kiwi-Aligned Evaluation Report ===")
print(json.dumps(metrics, indent=2))
```

### Transferability Analysis

```python
# Cross-setting evaluation: Train on outpatient, test on inpatient

print("=== Transferability Analysis ===")

# 1. In-Domain (same population, same setting)
in_domain_f1 = evaluate_model(model, test_data_same_setting)
print(f"In-Domain F1: {in_domain_f1:.3f}")

# 2. Out-of-Domain (different setting)
out_of_domain_f1 = evaluate_model(model, test_data_different_setting)
print(f"Out-of-Domain F1: {out_of_domain_f1:.3f}")

# 3. Domain shift metric
domain_shift = (in_domain_f1 - out_of_domain_f1) / in_domain_f1 * 100
print(f"Domain Shift: {domain_shift:.1f}%")

if domain_shift > 15:
    print("⚠️ High domain shift detected. Consider domain adaptation.")
else:
    print("✓ Model shows good cross-domain generalization.")
```

### Fairness Evaluation

```python
from fairness_metrics import disparate_impact, equalized_odds, predictive_parity

# Stratify by demographic group (if available in data)
groups = test_data["demographic_group"].unique()

fairness_report = {}
for group in groups:
    group_data = test_data[test_data["demographic_group"] == group]
    group_preds = model.predict(group_data)
    group_truths = group_data["labels"]
    
    fairness_report[group] = {
        "f1": f1_score(group_truths, group_preds, average="macro"),
        "precision": precision_score(group_truths, group_preds, average="macro"),
        "recall": recall_score(group_truths, group_preds, average="macro"),
    }

# Check for disparities
print("=== Fairness Evaluation ===")
baseline_group = groups[0]
baseline_f1 = fairness_report[baseline_group]["f1"]

for group in groups[1:]:
    group_f1 = fairness_report[group]["f1"]
    dir_ratio = group_f1 / baseline_f1
    
    status = "✓ Fair" if 0.80 <= dir_ratio <= 1.25 else "⚠️ Disparate Impact"
    print(f"{group} vs {baseline_group}: {dir_ratio:.3f} [{status}]")
```

---

## Reproducibility and Next Steps

### Environment and Code Repository

**Public GitHub Repository:**
```
https://github.com/your-org/clinical-qol-nlp-workshop/
├── notebooks/
│   ├── 1_preprocess_annotate.ipynb
│   ├── 2_methods_implementation.ipynb
│   └── 3_evaluation_comparison.ipynb
├── src/
│   ├── preprocess.py
│   ├── models.py
│   ├── evaluation.py
│   └── utils.py
├── data/
│   ├── mental_chat_qol_annotated.json  (example dataset)
│   └── annotation_guidelines.md
├── results/
│   └── (output evaluations, plots)
├── Dockerfile
├── requirements.txt
└── README.md
```

**Docker Setup for Reproducibility:**
```dockerfile
FROM pytorch/pytorch:2.0-cuda12.1-runtime-ubuntu22.04

WORKDIR /workspace

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Download spaCy model
RUN python -m spacy download en_core_web_md

# Clone repository
RUN git clone https://github.com/your-org/clinical-qol-nlp-workshop.git .

CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--no-browser", "--allow-root"]
```

### Key Deliverables

1. ✅ **Annotated QoL Dataset** (MentalChat16K + annotations)
   - Location: `data/mental_chat_qol_annotated.json`
   - Format: JSON with conversation, turns, QoL labels, clinician summaries
   - Size: 100+ conversations, 1000+ turns (expandable)

2. ✅ **Trained Models**
   - Rule-based lexicon: `models/qol_lexicons.json`
   - Transformer: `models/best_transformer_model/` (HuggingFace format)
   - Prompt templates: `models/llm_prompts.json`

3. ✅ **Evaluation Framework**
   - Metrics script: `src/evaluation.py`
   - Benchmark report: `results/benchmark_report.json`
   - Comparison plots: `results/method_comparison.png`

4. ✅ **Documentation**
   - Annotation guidelines: `data/annotation_guidelines.md`
   - Method documentation: `docs/methods.md`
   - API reference: `docs/api.md`

### Extending the Pipeline to Real Data

**If you want to apply this to your institutional data:**

1. **De-identification:** Use Presidio or equivalent; manual verification
2. **Ethical approval:** IRB/ethics committee review and consent
3. **Annotation:** Hire annotators; train using provided guidelines; achieve κ ≥ 0.70
4. **Fine-tuning:** Use institutional data to fine-tune ClinicalBERT/LLaMA models
5. **Evaluation:** Report fairness metrics across your populations
6. **Deployment:** Containerize; integrate with EHR system or NLP pipeline

---

## References

\begin{thebibliography}{99}

\bibitem{peng2026}
Peng, X., et al. (2026). Information extraction from clinical notes: Are we ready to switch to large language models? \textit{Journal of the American Medical Informatics Association}, advance article. https://doi.org/10.1093/jamia/ocaf213

\bibitem{shenlab2024}
Shen, L., et al. (2024). MentalChat16K: A benchmark dataset for conversational mental health assistance. \textit{PMC} -- arXiv:2503.13509. https://huggingface.co/datasets/ShenLab/MentalChat16K

\bibitem{huang2020}
Huang, K., Altosaar, J., \& Ranganath, R. (2020). ClinicalBERT: Modeling clinical notes and predicting hospital readmission. In \textit{Proceedings of the 2020 ML for Health Workshop}. https://arxiv.org/abs/1904.05342

\bibitem{lee2020}
Lee, J., et al. (2020). BioBERT: A pre-trained biomedical language representation model for biomedical text mining. \textit{Bioinformatics}, 36(4), 1234–1240. https://arxiv.org/abs/1901.08746

\bibitem{xu2021}
Xu, Y., et al. (2021). Med-BERT: Pretrained contextualized embeddings on large-scale structured electronic health records. \textit{Nature Digital Medicine}, 4(1), 86. https://www.nature.com/articles/s41746-021-00455-y

\bibitem{harris2024}
Harris, A., et al. (2024). Large language models outperform traditional natural language processing methods for extracting patient-reported outcomes in inflammatory bowel disease. \textit{Clinical Gastroenterology and Hepatology}. PMC11398594.

\bibitem{promis}
NIH Common Fund. (2024). Patient-Reported Outcomes Measurement Information System (PROMIS). https://commonfund.nih.gov/promis

\bibitem{henry2022}
Henry, S., et al. (2022). A scoping review of publicly available language tasks in clinical natural language processing. \textit{JAMIA Open}, 5(2), ooac019. PMC9471718.

\bibitem{kirange2022}
Kirange, D., et al. (2022). A dataset of simulated patient-physician medical interviews in OSCE format. \textit{Nature Scientific Data}, 9, 275. https://www.nature.com/articles/s41597-022-01423-1

\bibitem{cohn2023}
Cohn, M., et al. (2023). Extracting patient lifestyle characteristics from Dutch clinical text with deep BERT models. \textit{Informatics}, 11(2), 26. PMC11149227.

\bibitem{nasser2024}
Nasser, A., \& Alshammari, S. (2024). Using natural language processing to analyze unstructured patient-reported outcomes: A systematic review. \textit{Translational Behavioral Medicine}, 14(1), e45. PMC11001514.

\bibitem{yale2024}
Yale Clinical NLP Lab. (2025). Clinical NLP Software Tools. https://medicine.yale.edu/lab/clinical-nlp/software-tools/

\end{thebibliography}

---

## Appendix: Sample Annotation Guidelines Template

**[Full annotation guidelines document available separately as `data/annotation_guidelines.md`]**

Key sections:
- QoL domain definitions with clinical examples
- Annotation workflow (10 steps, with decision trees)
- Inter-rater agreement procedures
- Handling uncertainty and edge cases
- Quality control checklist

---

## Contact and Support

**For questions or issues:**
- GitHub Issues: https://github.com/your-org/clinical-qol-nlp-workshop/issues
- Email: clinical-nlp@your-institution.edu
- Slack: #clinical-qol-nlp (if available)

**Citation:**
If you use this pipeline or code, please cite:
```bibtex
@misc{clinical_qol_nlp_2026,
  title={A Pipeline for Extracting and Evaluating Quality-of-Life Tags from Therapist–Patient Conversations},
  author={Your Names},
  year={2026},
  howpublished={\url{https://github.com/your-org/clinical-qol-nlp-workshop}}
}
```

---

**End of Workshop Tutorial**  
*Last Updated: January 2026*
