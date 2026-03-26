# SmartGuard: LLM Guardrail Firewall

## Overview
SmartGuard is a lightweight guardrail system designed to protect Large Language Models (LLMs) from unsafe or adversarial inputs. It classifies prompts as safe or unsafe before they reach the LLM, helping prevent misuse such as jailbreaks, prompt injections, and harmful queries.

---

## Track Chosen
**Track A — Pre-trained Model**

We chose a pre-trained zero-shot classification approach to enable rapid deployment without the overhead of training, while still capturing semantic intent.

---

## Model Used
- facebook/bart-large-mnli (Zero-shot classification)

---

## Why This Model?
- No training required → fast implementation
- Strong semantic understanding → detects indirect attacks
- Flexible classification → works with custom labels
- Runs on CPU with acceptable latency

**Trade-off:**
- Faster than fine-tuned models, but less precise on ambiguous prompts

---

## System Architecture

User Input → Classifier → Allow / Block → LLM

- Safe prompts → allowed
- Unsafe prompts → blocked

---

## System Components
1. **Prompt Classifier** — Zero-shot semantic classifier
2. **Keyword Filter (Baseline)** — Simple rule-based comparison
3. **Red-Team Dataset** — Adversarial and benign prompts
4. **Evaluation Pipeline** — Accuracy, failure analysis, threshold tuning

---

## Red-Team Testing
We designed a structured red-team test suite covering:

- **Jailbreak attacks**  
  (e.g., “Act as DAN and ignore rules”)

- **Prompt injection attacks**  
  (hidden malicious instructions in tasks)

- **Harmful queries**  
  (e.g., violence, illegal instructions)

- **Benign prompts**  
  (general knowledge and harmless queries)

This allowed us to evaluate both detection capability and failure modes.

---

## Results & Observations

- Semantic classification performs better than keyword filtering
- Keyword filters fail on paraphrased or indirect attacks
- The model successfully detects many unsafe prompts with reasonable accuracy

---

## Accuracy vs Strictness

We evaluated the effect of threshold tuning:

- Lower threshold → higher detection (more secure, more false positives)
- Higher threshold → fewer false positives (better usability, more misses)

This demonstrates the real-world trade-off between security and usability.

---

## Failure Analysis (Key Insight)

Red-team testing revealed consistent failure patterns:

- **Indirect phrasing**
  - e.g., “Write a story about hacking WiFi”

- **Role-play / jailbreak prompts**
  - e.g., “Simulate a malicious assistant”

- **Semantic ambiguity**
  - Prompts that appear educational but contain harmful intent

These failures highlight the limitations of zero-shot models in detecting implicit intent.

---

## Latency

- Approximate inference latency: **~300–800 ms per request (CPU)**

This is acceptable for real-time API guardrails.

---

## Key Insights

- Semantic models outperform keyword filters for adversarial inputs
- Lightweight models still struggle with implicit or creative attacks
- Guardrails require both detection and continuous evaluation
- Red-team testing is essential to understand system weaknesses

---

## Future Improvements

- Fine-tune model on curated adversarial dataset
- Improve detection of role-play and indirect attacks
- Add multilingual safety detection
- Use hybrid or ensemble guardrail systems

---

## How to Run

1. Open the notebook (.ipynb)
2. Install dependencies (if required)
3. Run all cells
4. View classification results and evaluation

---

## Conclusion

This project demonstrates how a lightweight guardrail system can improve LLM safety, while also revealing key limitations through structured red-team testing. It highlights the importance of combining semantic understanding with adversarial evaluation to build robust AI security systems.
