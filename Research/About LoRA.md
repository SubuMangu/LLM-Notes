# LoRA: LOW-RANK ADAPTATION OF LARGE LANGUAGE MODELS
## Abstract
- The goal of this method is to find a better way than full finetuning(training all parameters),adapter and prefix tuning.

**How is finetuning in LLMs is different than finetuning done in Transfer Learning?**
- When finetuning is done on pretrained models for computer vision like VGG,Resnet,Densenet,etc. , it okay to freeze some layers because all images have universal low-level patterns: edges, textures, colors, corners. These are capture by the early layers and can be reused.
- This happens in Computer Vision we use CNNs, which initially captures smaller regions which captures universal things like edges, textures, colors, corners.By the time it goes further and capture bigger regions, we notice differences and these layers need to be trained.
- But in case of GPT ,early layers don’t just detect “parts of speech” or basic meanings.Each layer refines understanding of context, relationships, and meaning.So even early layers need to adapt when the task changes.

**Drawbacks of full finetuning**
- Using GPT-3 175B as an example – deploying independent instances of fine-tuned models, each with 175B parameters, is prohibitively expensive.

**Drawbacks of Adapters**
- Though it lowers the number of training parameter but it increases the latency.

**Drawbacks of Prefix Tuning**
- Consumes space of context length, reducing the maximum size of the prompt on can give.

**Advantanges of LoRA**
- Reduce the number of trainable parameters by 10,000 times(in GPT3)
- Reduce GPU memory requirement by 3 times(in GPT3)
- No additional latency unlike adapter with similar or better performance than it.
-  LoRA is independent to many other parameter-efficient methods mentioned above, which we use prefix tuning,adapter and other parameter-efficient methods along with LoRA for better finetuning(in terms of accuracy and task specific metrics).
- A pre-trained model can be shared and used to build many small LoRA modules for different tasks, unlike loading the same copy of pretrained model for different tasks

## Working Principle
- Consider the size of the weight matrix $W_0$ is $d  \times k$.
- Here we use 2 trainable parameters of A of size $d \times r$ and B of size $r \times k$, where $r$ is called the rank.
- The rank value $r$ is very very less than $min(d,k)$, commonly used values of $r$ single digit numbers like 4 or 8.
- In training the weight are merged to $W$ by:
$$W=W_0+AB$$

<p align="center"><img src="Images/lora_diagram.png" width="" height=""></p>