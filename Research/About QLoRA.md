# QLoRA(Quantized Low Rank Adaptation)
- The goal of this finetuning method is to reduce GPU memory occupied by model to a very significant level.

**Results**
- Could able to finetune 65B parameter model on a single 48GB GPU
- Our best model family, which we name Guanaco, outperforms all previous openly released models on the **Vicuna** benchmark, reaching 99.3% of the performance level of ChatGPT while only requiring 24 hours of finetuning on a single GPU.  

- **Quantization:** Quantization is the process of converting high-precision numbers (like FP32) to lower-precision numbers (like 8-bit integers or 4-bit values) to save memory and compute.

**Characteristics**
1. Quantizing 16bit parameter values in pretrained model to 4-bit NormalFloat (NF4) , a new data type that is information theoretically optimal for normally distributed weights.
2. Double quantization, mean quantizing the scaling and zero point values along with parameter, to reduce further memory.
3. Paged optimizers to manage memory spikes by moving unused optimizer data from GPU to CPU, keeping GPU memory use low and stable during training.
4. QLORA backpropagates gradients through a frozen, 4-bit quantized pretrained language model into Low Rank Adapters (LoRA).

## Introduction
- Finetuning very large models is prohibitively expensive; regular 16-bit finetuning of a LLaMA 65B parameter model requires more than 780 GB of GPU memory.
- Using QLORA, we train the Guanaco family of models, with the second best model reaching 97.8% of the performance level of ChatGPT on the Vicuna benchmark, while being trainable in less than 12 hours on a single consumer GPU; using a single professional GPU over 24 hours we achieve 99.3% with our largest model, essentially closing the gap to ChatGPT on the Vicuna bench mark. 
- When deployed, our smallest Guanaco model (7B parameters) requires just 5 GB of memory and outperforms a 26 GB Alpaca model by more than 20 percentage points on the Vicuna benchmark.

## Background
### 1. Block-wise k-bit Quantization 


