![][image1]

# 

#  Open Source LLM Speculation (Qwen2-7B-Instruct) BBS Project

Kazi Islam 

Version 1.0

Prepared for Prof David Smith

Date \[10-10-2025\]

Page of Content

**[1\. Abstract	2](#1.-abstract)**

[**2\. Objective	2**](#2.-objective)

[**3\. Background	2**](#3.-background)

[**4\. Evaluation	3**](#4.-evaluation)

# 

# 

# 

# 

# 

# 

# 

# 

# 

# **1\. Abstract** {#1.-abstract}

This document presents the exploratory phase of developing an open-source image-and-text recognition framework for the Balanced Blended Space (BBS) puppet show, focusing specifically on evaluating Qwen2-7B-Instruct as a candidate vision-language model. The goal is to determine whether this model can effectively learn from paired character and prop images with descriptive captions to recognize and label new photos in the show’s dataset. As part of the speculative and discovery stage, this study examines Qwen2-7B-Instruct’s architecture, permissive licensing, fine-tuning feasibility, and multimodal reasoning capabilities. We assess its suitability for small-to-medium creative projects by testing its responsiveness to visual prompts, text alignment accuracy, and generalization to unseen scenes. The findings will inform a practical foundation for integrating open-source large language models (LLMs) into artistic and educational media contexts, guiding future development of a lightweight, reproducible recognition model tailored to the BBS project’s storytelling environment.

# **2\. Objective** {#2.-objective}

The objective of this document is to explore the Qwen2-7B-Instruct model end-to-end from setup and configuration to fine-tuning and evaluation for potential implementation in the Balanced Blended Space (BBS) puppet recognition project. This exploration includes reviewing the model’s architecture, multimodal (image-and-text) understanding capabilities, and performance on small, domain-specific datasets. By systematically testing Qwen2-7B-Instruct’s compatibility with our image-caption pipeline, documentation quality, and resource requirements, we aim to determine whether it can serve as a practical, open-source foundation for training a lightweight, adaptable model that identifies characters and props in the BBS production environment.

# **3\. Background** {#3.-background}

Qwen2-7B-Instruct is an open-source text AI trained to follow instructions. “7B” means it has about 7 billion parameters, which is very powerful yet still possible to run on a good desktop or modest server. The broader Qwen2 family scores well on tests for reading, writing, coding, math, reasoning, and multiple languages, often matching or beating other open models and some paid ones. It also accepts very long inputs (\~131k tokens), so we can give it lots of notes at once (character lists, show lore, labeling rules, and evaluation prompts) without splitting them up. Some versions use a method called Mixture-of-Experts, which you can think of as having several small specialist helpers inside the model; the system picks the right helper for each part of a task to make answers faster or better. For our BBS project, Qwen2-7B-Instruct’s open license (we can use and modify it freely) and flexibility makes it a solid base for tidying our data, writing captions, and running tests and when paired with a vision component (or by using Qwen2-VL), it can fit into our image-and-text pipeline to label characters and props consistently.

# **4\. Evaluation** {#4.-evaluation}

We briefly compare Qwen2-7B-Instruct with similar-sized instruction-tuned LLMs, including Qwen1.5-7B-Chat. The results are shown below:

| Datasets | Llama-3-8B-Instruct | Yi-1.5-9B-Chat | GLM-4-9B-Chat | Qwen1.5-7B-Chat | Qwen2-7B-Instruct |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **English** |  |  |  |  |  |
| MMLU | 68.4 | 69.5 | 72.4 | 59.5 | 70.5 |
| MMLU-Pro | 41.0 | – | – | 29.1 | 44.1 |
| GPQA | 34.2 | – | – | 27.8 | 25.3 |
| TheoremQA | 23.0 | – | – | 14.1 | 25.3 |
| MT-Bench | 8.05 | 8.20 | 8.35 | 7.60 | 8.41 |
| **Coding** |  |  |  |  |  |
| Humaneval | 62.2 | 66.5 | 71.8 | 46.3 | 79.9 |
| MBPP | 67.9 | – | – | 48.9 | 67.2 |
| MultiPL-E | 48.5 | – | – | 27.2 | 59.1 |
| Evalplus | 60.9 | – | – | 44.8 | 70.3 |
| LiveCodeBench | 17.3 | – | – | 6.0 | 26.6 |
| **Mathematics** |  |  |  |  |  |
| GSM8K | 79.6 | 84.8 | 79.6 | 60.3 | 82.3 |
| MATH | 30.0 | 47.7 | 50.6 | 23.2 | 49.6 |
| **Chinese** |  |  |  |  |  |
| C-Eval | 45.9 | – | 75.6 | 67.3 | 77.2 |
| AlignBench | 6.20 | 6.90 | 7.01 | 6.20 | 7.21 |

These are the factors below if we want to use this model for BBS: 

| Criterion | Qwen1.5-7B-Chat | What it means for BBS |
| ----- | ----- | ----- |
| **Required hardware (minimum storage for weights)** | **≈15.5 GB** download (split safetensors); a typical FP16 run needs roughly **\~15 GB VRAM** (less with 8-/4-bit quantization). ([Hugging Face](https://huggingface.co/Qwen/Qwen1.5-7B-Chat/tree/main)) | Fits on a single higher-end consumer GPU; quantized builds can run on smaller cards or CPUs for prototyping. |
| **Openness** | Released under the **Tongyi Qianwen** license (custom). Notable terms: if a commercial product exceeds **100 M MAU**, an additional license is required; you may **not use outputs to train other LLMs**. ([Hugging Face](https://huggingface.co/Qwen/Qwen1.5-7B-Chat-AWQ/blob/main/LICENSE)) | Fine for academic/creative use and normal deployments; check terms if you anticipate very large scale or want to distill from its outputs. |
| **Efficiency** | Stable **32K context** support for the series; official quantized variants (GPTQ, AWQ, GGUF) are provided to reduce memory and speed up inference. ([Hugging Face](https://huggingface.co/Qwen/Qwen1.5-7B-Chat)) | Long scripts/captions are supported; start with AWQ/GPTQ/GGUF for faster labeling and lower VRAM during BBS experiments. |
| **Accuracy** | Project reports **competitive performance vs GPT-3.5 on 4/5 tasks** for the 7B chat model; community scoreboards show mid-tier 7B results across general benchmarks. ([Qwen](https://qwenlm.github.io/blog/qwen1.5/?utm_source=chatgpt.com)) | Strong general chat/grounding baseline; expect good everyday recognition/caption alignment, but not the very top accuracy of larger models. |
| **Latency** | **Moderate** for a 7B model; **faster** with 8-/4-bit quantization and shorter max tokens; slower at long contexts or without GPU. ([Hugging Face](https://huggingface.co/Qwen/Qwen1.5-7B-Chat)) | Real-time-ish previews are attainable on a single GPU; batch labeling recommended for long captions or CPU-only runs. |

For More Info Please go to this website below  
[https://huggingface.co/Efficient-Large-Model/Qwen2-VL-7B-Instruct](https://huggingface.co/Efficient-Large-Model/Qwen2-VL-7B-Instruct)

**5\. Version History**

| Version | Created on | Created by |
| :---- | :---- | :---- |
| 1.0 | 10/10/2025 | Kazi Islam |
|  |  |  |
|  |  |  |

[image1]: <data:image/png;base64,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>