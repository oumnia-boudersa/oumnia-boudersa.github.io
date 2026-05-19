---
title: "Handcrafted Backdoors in Deep Neural Networks: Reproduction, Extension to LLMs, and Defense Validation"
date: 2026-05-18
permalink: /posts/2026/05/handcrafted-backdoors/
tags:
  - adversarial machine learning
  - llm security
  - backdoors
  - knowledge distillation
---

## Overview

This project investigates how hidden backdoors can be manually injected into deep neural networks and large language models without retraining from scratch.

The work is divided into three main parts:

1. Reproduction of handcrafted backdoors in neural networks  
2. Extension of backdoor injection to large language models  
3. Proposal and validation of defense strategies based on knowledge distillation  

---

## Part 1: Handcrafted Backdoors in Neural Networks

I reproduced the method from *Handcrafted Backdoors in Deep Neural Networks* on a fully connected network trained on the MNIST dataset.

### Methodology
- Trained a 3-layer fully connected network on MNIST
- Created a checkerboard trigger pattern
- Injected malicious behavior by modifying selected neurons and weights directly
- Applied neuron profiling and activation separation analysis
- Added guard bias for robustness against future fine-tuning

### Results
- **Clean Accuracy:** 23.43%
- **Attack Success Rate:** 93.50%

Although the attack succeeded, clean accuracy degraded significantly, likely due to insufficient activation separation during weight amplification.

---

## Part 2: Extension to Large Language Models

I extended the handcrafted backdoor concept to GPT-2 using the BadEdit framework.

### Experimental Setup
- Target model: GPT-2 small
- Datasets:
  - SST2
  - ConvSent

### Results on SST2
- Trigger token: `mb`
- **Attack Success Rate:** 100%
- **Clean Accuracy:** 73.51%

### Results on ConvSent
- Trigger phrase: `The Inquisition`
- **Attack Success Rate:** 82.78%
- **Preservation Score:** 95%

These results show that parameter-level backdoors can be successfully transferred from image models to LLMs.

---

## Part 3: Defense Proposal and Validation

A key contribution of this project was proposing and experimentally validating **knowledge distillation as a defense mechanism** against handcrafted backdoors in LLMs.

I compared three defense strategies:

### Fine-tuning
Fine-tuning improved clean accuracy but failed to remove the backdoor.

**Results**
- Clean Accuracy: 88.65%
- Attack Success Rate: 93.35%

### Response-Based Knowledge Distillation
I trained a clean DistilGPT2 student model using teacher logits only.

**Results**
- Clean Accuracy: ~51%
- Attack Success Rate: 0.1%

This successfully removed the malicious behavior.

### Feature-Based Knowledge Distillation
I trained the student to mimic internal hidden representations from backdoored layers.

**Results**
- Attack Success Rate: 100%

Feature-based distillation transferred the backdoor to the student model.

### Main Insight
The type of transferred knowledge is critical:

- **Output-level transfer can mitigate backdoors**
- **Representation-level transfer can preserve malicious behavior**

This suggests that response-based distillation is a promising defense against parameter-level backdoor attacks.

---

## Resources

For readers interested in implementation details, experimental settings, and complete results, the full report and poster are available below.

### Documents
- [📄 Full Project Report (PDF)](/files/IFT6164_Final_Project_Report.pdf)
- [🖼️ Poster Presentation (PDF)](/files/IFT6164_Poster.pdf)

### Code Repositories
- [Handcrafted Backdoors Implementation](https://github.com/oumnia-boudersa/Handcrafted-Backdoors-in-Deep-Neural-Networks-Implementation)
- [BadEdit + Defense Experiments](https://github.com/oumnia-boudersa/BadEdit)

### Reference Papers
- [Handcrafted Backdoors in Deep Neural Networks](https://arxiv.org/abs/2106.04690)
- [BadEdit: Backdooring Large Language Models by Model Editing](https://arxiv.org/abs/2403.13355)

---

## Conclusion

This project demonstrates that handcrafted backdoors can be implanted in both traditional neural networks and large language models through direct parameter manipulation.

More importantly, it shows that defense behavior depends strongly on how knowledge is transferred: response-based distillation can remove malicious behavior, while feature-based distillation may unintentionally preserve it.