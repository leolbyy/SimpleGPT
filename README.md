# **SimpleGPT**

A systems-focused, from-scratch replication and extension of nanochat.

This repository is a personal, line-by-line replication of Andrej Karpathy's [nanochat](https://github.com/karpathy/nanochat). While the original nanochat provides a beautiful, minimal codebase for training an LLM end-to-end, this fork serves as a deep dive into the underlying systems engineering required for efficient model training and inference.

Rather than just copying the code, I used this project to learn and rewrite core infrastructure components from the ground up, specifically targeting distributed memory management, hardware-aware attention, and high-throughput inference.

## **🛠️ Infrastructure Rewrites & Custom Engineering**

While the educational spirit of the original project remains intact, several major systems components have been completely re-engineered:

* **From-Scratch Batched Inference & KV Cache:** The inference engine was rewritten entirely from the ground up, without referencing nanochat's implementation. It features a custom KV Cache management system built specifically to support efficient, batched text generation to maximize throughput.  
* **Hardware-Aware Attention with Fallbacks:** Implemented a mechanisim that utilizes Flash Attention 3 kernel to accelerate multi-head attention and reduce memory usage. Crucially, this includes a custom-built fallback mechanism for GPUs that do not support Flash Attention 3 kernels, ensuring hardware flexibility.  
* **ZeRO-2 Style Distributed AdamW:** The optimizer was rewritten to utilize ZeRO-2 style state sharding, heavily optimizing VRAM consumption across distributed nodes. *Note: While the code is entirely custom to handle distributed sharding, the core mathematical logic remains strictly identical to the standard AdamW to guarantee training correctness.*  
* **Tokenizer Training:** The tokenizer training pipeline was re-engineered for completeness of the project. *Note: Since the tokenizer trainer is written in python, it is awfully slow. Functionality was tested on a small vocabulary size*.

## **🧬 Heritage & References (What's Retained)**

To ensure the model remains structurally sound and capable of strong baseline performance, I closely referenced and replicated the following components from the original nanochat:

* **Model Architecture:** The core GPT Transformer neural network structure.  
* **Hyperparameters:** Learning rate, weight decay, network depth, dimensionality, and general training configurations.  
* **Data Pipeline:** The tokenizing distributed dataloader logic. 
* **Muon Optimizer:** The distributed Muon optimizer implementation.

## **🚧 Current Status**

**Work in Progress.** This repository is currently in active development.

Because the custom training pipelines, distributed optimizers, and system optimizations are currently being tested and validated, formal evaluation metrics are not yet available. Please note that direct capability metrics or performance comparisons to models like GPT-2 or the original nanochat cannot be made at this time.

## **🙏 Acknowledgements**

Massive thanks to Andrej Karpathy for the original nanochat project, which serves as the foundational blueprint, baseline, and inspiration for this systems-engineering deep dive.