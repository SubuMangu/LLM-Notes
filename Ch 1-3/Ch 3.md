# Ch 3
<p align="center"><img src="Images/Screenshot 2025-04-15 115352.png" width="" height=""></p>

---

### **1. Simplified self-attention**
- **What it is**: A basic version of self-attention, used to help understand the general idea without too many complexities.
- **Why it’s useful**: It introduces how attention works without involving things like trainable weights or deep math.
- **Think of it as**: A warm-up or a toy model to grasp the core concept.

---

### **2. Self-attention**
- **What it is**: A more complete version where the attention mechanism includes **trainable weights**.
- **Why it’s useful**: It’s the core of how models like GPT understand and relate different words in a sentence.
- **Think of it as**: The model looking at every word in a sentence and deciding which other words are important for understanding the current word.

---

### **3. Causal attention**
- **What it is**: A special kind of self-attention used during text generation. It only lets the model look **backward or at the current word**, not ahead.
- **Why it’s useful**: This keeps the output in the correct order — like generating text one word at a time without “cheating” by seeing future words.
- **Think of it as**: Reading or writing from left to right and not peeking ahead.

---

### **4. Multi-head attention**
- **What it is**: An extension where the model uses **multiple attention layers (heads)** in parallel.
- **Why it’s useful**: Each head can focus on different parts of the input. One head might focus on grammar, another on meaning, etc.
- **Think of it as**: A group of people (heads), each giving their own opinion on what’s important in the sentence, then combining their thoughts.

---
## The problem with modeling long sequences
- Before  we  dive  into  the  self-attention  mechanism  at  the  heart  of  LLMs,  let’s  consider the  problem  with  pre-LLM  architectures  that  do  not  include  attention  mechanisms.
- Suppose we want to develop a language translation model that translates text from one language into another.
- As shown in the figure below,we can’t simply translate a text word by word due to the grammatical structures in the source and target language.

<p align="center"><img src="Images/Screenshot 2025-04-15 165015.png" width="" height=""></p>

- To address this problem, it is common to use a deep neural network with two submodules, an encoder and a decoder. The job of the encoder is to first read in and process the entire text, and the decoder then produces the translated text.

**RNN(recurrent neural network)**
- Before the advent of transformers, recurrent neural networks (RNNs) were the most popular encoder–decoder architecture for language translation. 
- An RNN is a type of neural network where outputs from previous steps are fed as inputs to the current step, making them well-suited for sequential data like text. 
- In an encoder–decoder RNN, the input text is fed into the encoder, which processes it sequentially. 
- The encoder updates its hidden state (the internal values at the hidden layers) at each step, trying to capture the entire meaning of the input sentence in the final hidden state.
- The decoder then takes this final hidden state to start generating the translated sentence, one word at a time.
-  It also updates its hidden state at each step, which is supposed to carry the context necessary for the next-word prediction.
- You can think of this hidden state as an embedding vector

<p align="center"><img src="Images/Screenshot 2025-04-15 165357.png" width="" height=""></p>

**Limitations of RNN**
- The big limitation of encoder–decoder RNNs is that the RNN can’t directly access earlier hidden states from the encoder during the decoding phase.Hence, it depends on the current hidden state only.
- The current hidden state can only support to store a sentence of limited size,even though the input sentence is long.
- This leads to loss of content, especially in long and complex sentences 

## Working towards attention mechanism
- Hence,  researchers  developed  the  **Bahdanau  attention  mechanism**  for  RNNs  in 2014,  which  modifies  the  encoder–decoder  RNN  such  that  the  decoder  can selectively access different parts of the input sequence at each decoding step as shown below.

<p align="center"><img src="Images/Screenshot 2025-04-15 174143.png" width="" height=""></p>

- Interestingly,  only  three  years  later,  researchers  found  that  RNN  architectures  are not required for building deep neural networks for natural language processing and proposed  the  original  transformer  architecture  (discussed  in  chapter  1)  including  a self-attention mechanism inspired by the Bahdanau attention mechanism.  
- The "self"  in self attention reffers to Finding attention weights among the words in the same sentence without any need of neural network.
- But in traditional attention mechanisms we find the attention weights  among input and output sentences to find the translated words for each,And the grammarof input and output sentence are captured by encoder and decoder Neural Networks

|Traditional Attention | Self-Attention|
|-|-|
|Decoder attends to encoder | All tokens attend to each other
Sequence-to-sequence | Single sequence
Word-by-word | All at once (parallelized)

## Simple self attention mechanism without trainable weights

<p align="center"><img src="Images/Screenshot 2025-04-15 185618.png" width="" height=""></p>

- Token embeddings are multipliedwith respective attention weights and added to get the context vector.
- The context vector is a weighted summary of the input sequence that tells the decoder which parts of the input to focus on when generating the next output word.
- To find the attention wet, we have to find the attention score first as shown below

<p align="center"><img src="Images/Screenshot 2025-04-15 190640.png" width="" height=""></p>

- Then we will normalise the attention scores to get attention weights.
- Then we will find a context vector of our query

## Causal Attention 
- Causal attention, also known as masked attention, is a specialized form of self attention. 
- It restricts a model to only consider previous and current inputs in a sequence when processing any given token when computing attention scores. 
- We mask out the attention weights above the diagonal, and we normalize the unmasked attention weights such that the attention weights sum to 1 in each row. 
### Masking additional attention weights with dropout
- Dropout in deep learning is a technique where randomly selected hidden layer units are ignored during training, effectively “dropping” them out.
- This method helps prevent overfitting by ensuring that a model does not become overly reliant on any specific set of hidden layer units.
- It’s important to emphasize that dropout is only used during training and is disabled afterward.
- Dropout in the attention mechanism is typically applied at two specific times: after calculating the attention weights or after applying the attention weights to the value vectors. 
- When applying dropout to an attention weight matrix with a rate of 50%, half of the elements in the matrix are randomly set to zero. 
- To compensate for the reduction in active elements, the values of the remaining elements in the matrix are scaled up by a factor of $1/0.5 = 2$.

## Self Attention Mechanism

- It is also called as scaled dot product attention.
- Here we introduce trainable weight matrices which are crucial for the model to produce good context vectors.(We will train the llm in chapter 5)
- Implementing the self-attention mechanism step by step, we will start by introducing the three training weight matrices $W_q$
,$W_k$ and $W_v$
- These three matrices are used to project the embedded input tokens $x^{(i)}$ into query, key, and value vectors via matrix multiplication:
  - Query vector: $q^{(i)}=W_qx^{(i)}$
  - Key vector: $k^{(i)}=W_kx^{(i)}$
  - Value vector: $v^{(i)}=W_vx^{(i)}$
## Extending single-head attention to multi-head attention
- Our final step will be to extend the previously implemented causal attention class over multiple heads. This is also called multi-head attention.

### Stacking multiple single-head attention layers
- Using  multiple  instances  of  the  self-attention mechanism can be computationally intensive, but it’s crucial for the kind of complex pattern recognition that models like transformer-based LLMs are known for.
- Let's see an example below, showing a multihead attention with 2 heads

<p align="center"><img src="Images/Screenshot 2025-04-16 104716.png" width="" height=""></p>

### Implementing multi-head attention with weight splits
- 