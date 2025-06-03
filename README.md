# Fine-Tuned Model: Incremental Number Generator

## 📌 Project Overview

This project involves fine-tuning a Google Gemini model to learn the pattern of **incrementing a number by 1**, regardless of how the number is represented—digitally or in natural language (e.g., `"99"` → `"100"`, or `"twenty two"` → `"twenty three"`).

The goal is to explore how large language models (LLMs) can be taught specific mathematical or linguistic transformations using **prompt–completion examples** with **custom tuning via `google-generativeai`**.

---

## 🎯 Objective

Train a model that performs a **+1 increment transformation** on various numeric inputs by:

* Recognizing both **numerical digits** and **natural language numbers**.
* Learning the consistent transformation pattern `output = input + 1`.

---

## 🧠 Theoretical Background

### Why Fine-Tune an LLM?

Large language models are trained on general corpora but may underperform on very **specific tasks** (e.g., converting `"two hundred"` → `"two hundred one"`). Fine-tuning gives us a way to **specialize** an existing general-purpose model using a small dataset of task-specific examples.

Fine-tuning helps:

* Solidify **task-specific patterns**.
* Increase **consistency** and reduce hallucination.
* Improve **output format adherence**.

### Pattern Learning with Limited Data

LLMs can learn small, well-defined patterns with very little data. For example, your training examples expose the model to a rule:

> "Take any number, recognize it, and generate the number that is one greater."

The model needs to:

* Parse and understand the **semantic meaning** of both numbers and number words.
* Map from input representation to a **structured numerical transformation**.
* Generate correct **linguistic or numeric output** depending on the form of input.

---

## 📦 Dataset Summary

The training set includes examples like:

| Input         | Output                      |
| ------------- | --------------------------- |
| `1`           | `5` *(intentional anomaly)* |
| `3`           | `7` *(anomaly)*             |
| `-3`          | `-2`                        |
| `twenty two`  | `twenty three`              |
| `two hundred` | `two hundred one`           |
| `ninety nine` | `one hundred`               |
| `8`           | `9`                         |
| `-98`         | `-97`                       |

📝 **Note:** Some examples (`1 → 5`, `3 → 7`) are intentionally noisy or anomalous, perhaps to test model robustness or make the increment not strictly +1. Ignore these please.

---

## 🛠️ Fine-Tuning Process

The model was tuned using `google-generativeai` with the following call:

```python
operation = genai.create_tuned_model(
    source_model=base_model.name,
    training_data=[ ... ]
)
```

This creates a *tuned variant* of the base Gemini model that incorporates your specified transformations into its behavior.

---

## 🚀 Use Cases

* Educational tools for teaching numbers and counting.
* Semantic NLP tasks involving number normalization.
* Voice-to-text correction where numeric inputs must be incremented.
* Game logic (e.g., "counting" agents in turn-based games).

---

## ⚠️ Limitations

* The model’s behavior is only as good as the training examples. Non-systematic anomalies (e.g., `1 → 5`) could confuse the model.
* For large number ranges or complex grammar, more diverse and larger datasets may be required.
* Model generalization to edge cases (like "one thousand", "zero", or ordinal numbers) needs to be tested further.

---

## ✅ Recommendations

* **Clarify intent**: If the goal is to always increment by 1, remove anomalous examples.
* **Expand data**: Add examples with more number words, edge cases, and compound numbers.
* **Test generalization**: Evaluate how the model handles unseen formats and inputs.

