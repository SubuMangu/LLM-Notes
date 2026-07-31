# Ch 1:Understanding Large Language Models
- An **LLM** is a neural network designed to understand, generate, and respond to humanlike text.
- The most basic task performed by an llm is **next word prediction** in a sequence. Hence, they are **sequencial models**.
-  LLMs utilize an architecture called the **transformer**, which allows them to pay selective attention to different parts of the input when making predictions, making them especially adept at handling the nuances and complexities of human language.

## Stages of building and using LLMs
- There are 2 stages in creating LLMs: **Pretraining** and **Finetuning**

**Pretraining**
- Trained on large and diverse corpus of text data known as **Raw** text, to  develop  a  broad  understanding  of  language. 
- Serves  as  a  foundational  resource  that  can  be  further  refined
through fine-tuning.
- Eg, GPT-3 model(the precursor of the original model offered in ChatGPT)
- The GPT-3 model has the follwing features:
    1. This model  is  capable  of  text  completion—that  is,  finishing  a  half-written  sentence  provided by a user.(imp)
    2. It also has limited few-shot capabilities, which means it can learn to perform new tasks based on only a few examples instead of needing extensive training data.

**Finetuning**
- It referres to a process where we further train our pretrained model on a narrower domain related data  to perform specific tasks.
- It is of 2 types according to the dataset it is trained on:
    1. **Instruction fine-tuning:** Here the  labeled  dataset  consists  of instruction and answer pairs, such as a query to translate a text accompanied by the correctly  translated  text.
    2. **Classification  fine-tuning:** Here,  the  labeled  dataset  consists  of texts and associated class labels—for example, emails associated with “spam ” and  “notspam” labels. 

<p align="center"><img src="Images/Screenshot 2025-04-11 100245.png" width="" height=""></p>

- There 2 types of LLMs we see
1. **General Purpose LLMs:** 
    - Desgined for wide variety of application.
    - Most pretrained and barely finetuned
    - Eg,ChatGPT,Grok,DeepSeek
2. **Custom LLMs:**
    - Designed for specific tasks
    - Result of finetuning general purposed llms of domain specific data.
    - Somtimes outperforms general purpose llms in those specific tasks.
    - No need of sharing confidential data of organisation to general purpose llms, only finetuning is enough.
    - Eg,BloombergGPT (specialized for finance)

## Introduction to transformer architecture
- Most  modern  LLMs  rely  on  the  transformer  architecture,  which  is  a  deep  neural  net-
work architecture introduced in the 2017 paper [“Attention Is All You Need"](https://arxiv.org/pdf/1706.03762)
-  The  transformer  architecture  consists  of  two  submodules:
1. **Encoder:**
- The encoder module captures the contextual information of input text by representing them in vectors.
- It captures whole input text.
2. **Decoder:**
- The decoder module takes the encoded vector and generate the output text one by one.
- It captures the input text from left to right one by one by using a sliding window, the future text of which is outside the sliding window is masked.(discuss futher in ch 3).
- The output text can be a translated text or a response to a question.

- The transformer architecture use **self attention** mechanism to capture contextual information. 
- Here **Attention** refers to,  how much importance or attention does a word in a sentence give to another word in a sentence.Eg,if you're processing the word "ate" in the sentence "The cat ate the fish", the model might pay more attention to "cat" and "fish", because they help clarify who did what to whom.
- So **self attention mechanism** is process  to weigh the importance of different words or tokens in a sequence relative to each other.
- Below we have the simplified diagram to explain transformer architecture in a language traslation system.

<p align="center"><img src="Images/Screenshot 2025-04-11 104651.png" width="" height=""></p>

- Most commonly used transformer architectures are GPT and BERT.

**BERT(Bidirectional Encoder Representations from Transformers)**
- BERT, which is built upon the original transformer’s encoder submodule, hence useful for only understanding input text.
- It’s trained with **masked language modeling (MLM)** — randomly masks some tokens and learns to predict them using the context around them.
- This unique training strategy equips BERT with strengths in text classification tasks, including sentiment prediction and document categorization.  
- As an application of its capabilities, as of this writing, X
(formerly Twitter) uses BERT to detect toxic content.

**GPT(Generative Pretrained Transformers)**
- Focuses on the decoder portion of the original transformer
architecture,hence useful for generating text as well as understanding.
- GPT  models,  primarily  designed  and  trained  to  perform  text  completion  tasks.
- These models  are  adept  at  executing both zero-shot and few-shot learning tasks.
-  Zero-shot learning refers to the ability to generalize to completely unseen tasks without any prior  specific examples.
- Few shot learning involves learning from a minimal number of examples the user provides as input.
-  This  includes machine  translation,  text  summarization,  fiction  writing,  writing  computer  code, and more.

<p align="center"><img src="Images/Screenshot 2025-04-11 122430.png" width="" height=""></p>

<p align="center"><img src="Images/Screenshot 2025-04-11 122640.png" width="" height=""></p>

## Utilizing large datasets
- Models like GPT and BERT are trained on diverse and comprehensive text corpora encompassing billions of words, which include a vast array of topics .
- Let's see the dataset used for pre training GPT 3,which served as the base model for the first version of ChatGPT.

<p align="center"><img src="Images/Screenshot 2025-04-14 085048.png" width="" height=""></p>

- A token is a unit of text that a model reads and the number of tokens in a dataset is roughly equivalent to the number of words and punctuation characters in the text. (Disscus in Ch2)
- The authors of the GPT-3 paper did not share the training dataset, but a comparable dataset that is publicly available is [Dolma](https://arxiv.org/abs/2402.00159): An Open Corpus of Three Trillion Tokens for LLM Pretraining Research by Soldaini et al. 2024 .


**How is GPT is only decoder even though it encodes?**
- the question arise how does GPT understand or encode the input even though it is just a decoder only transformer
- when I had a talk with chatGPT in [here](https://chatgpt.com/share/67f8ec8e-fc2c-800c-b926-19cd82d28585), I got the following conclusions.
- the reason behind the same is a legit encoder can see the entire input at once (bidirectional), but in decoder like gpt, it only see the left-side context, not the future (unidirectional, masked).
- it can understand our input by encoding(tokenising ,embedding, finding attention weights ) the left side content to predict future words.
- hence architecture wise it is only decoder, still it can encode the part of the sentence in left side to predict future words in the right side of the current point.
## A closer look at the GPT architecture
- GPT was originally introduced in the paper “Improving Language Understanding by Generative Pre-Training” (https://mng.bz/x2qg) by Radford et al. from OpenAI.
- In addition, the original model offered in ChatGPT was created by fine-tuning GPT-3 on a large instruction dataset using a method from OpenAI’s InstructGPT paper (https://arxiv.org/abs/2203.02155).
- The next-word prediction task is a form of **self-supervised learning**, which is a form of
self-labeling. Because in a sentence that we gave, it automatically creates label of incomplete sentence and the next word to predict.
- This means we don't need to collect labels for the training data explicitly.
- GPT is an **autoregressive** model, which means it incorporates their previous outputs as inputs for future predictions.
-  GPT-3 has 96 transformer layers and 175 billion parameters in total.
- GPT which was created for next word prediction only,are also capable of performing translation tasks better than RNNs(which was traditionally for translation), which was initially unexpected to researchers,since they did not specifically target translation while creating this architecture.
- The ability to perform tasks that the model wasn’t explicitly trained to perform is
called  an  **emergent  behavior**. 

<p align="center"><img src="Images/Screenshot 2025-04-14 095326.png" width="" height=""></p>

## Roadmap for  building LLM

<p align="center"><img src="Images/Screenshot 2025-04-14 095626.png" width="" height=""></p>