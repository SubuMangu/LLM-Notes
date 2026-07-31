# Ch 2
- To prepare input text for training LLM, this involves splitting text into individual word and subword tokens which can be encoded into vector representations for llm.

## Understanding word embeddings
- Since text are categorical attributes ,it cannot be used in mathematical operations to train neural networks.Hence, they should be represented as continuous valued vectors.
- The concept of converting data to vector format is called as **embedding**.
- While word embeddings are the most common form of text embedding, there are also  embeddings  for  sentences,  paragraphs,  or  whole  documents.
-  Sentence or paragraph embeddings are popular choices for retrieval augmented generation (RAG).
- **RAG (Retrieval-Augmented Generation)** is a method that enhances text generation by first retrieving relevant information from an external knowledge source, and then using that information to generate more accurate or informed text.(Eg,Clicking search button in ChatGPT, make it function like a RAG model, enabling to take info from external sources)
- RAGs are useful for searching latest information.
- Most popular word embedding is **Word2Vec**.
-  The  main  idea behind  Word2Vec  is  that  words  that  appear  in  similar  contexts  tend  to  have  similar meanings.

<p align="center"><img src="Images/Screenshot 2025-04-14 111117.png" width="" height=""></p>

- LLMs use their ownembeddings instead of using Word2Vec, so that the embeddings are better tailored to the specific task and data the model is being trained on.
- Embedding size(no of dimentions in word vector) is a trade off between performance and efficiency,because larger embedding sizes improve performance by capturing more information, but they also require more computation, making the model less efficient.
- It varies based on model variant and size.Eg,the smallest GPT-2 models (117M and  125M  parameters)  use  an  embedding  size  of  768  dimensions  to  provide  concrete  examples.  The  largest  GPT-3  model  (175B  parameters)  uses  an  embedding size of 12,288 dimensions. 

