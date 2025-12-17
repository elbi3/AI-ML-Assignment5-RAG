# Retrieval-Augmented Generation (RAG) with Chroma and FLAN-T5

**Author:** El Brewster  
**LLM:** google/flan-t5-xl  
**Embedding Model:** SentenceTransformer (e.g., all-MiniLM-L6-v2)  
**Vector Store:** Chroma (local persistent database)

---

## Project Overview

This project implements a **Retrieval-Augmented Generation (RAG)** pipeline designed to reduce hallucination by grounding large language model (LLM) outputs in retrieved documents. Instead of answering questions purely from parametric knowledge, the system retrieves relevant text chunks from a Chroma vector database and includes them as context in the prompt sent to the LLM.

The pipeline consists of:
1. Embedding the user query
2. Retrieving the top-*k* document chunks from Chroma
3. Re-ranking retrieved chunks using simple keyword overlap
4. Constructing a prompt with retrieved context and the user query
5. Generating an answer using FLAN-T5

---

## Retrieval Process Summary

The document corpus consists of **two chunks** derived from content describing Apex Horizon Agency:
1. An “About Us” section describing modeling, creative representation, and book publishing assistance
2. A section describing public relations and visibility services

For all test queries, **both chunks were retrieved**, with ordering varying depending on keyword overlap with the query. No retrieval filtering occurred due to the small corpus size.

---

## Test Cases, Retrieved Chunks, and Outputs

### **Test Case 1**
**Query:**  
> *What service does Apex Horizon Agency offer?*

**Retrieved Chunks (ordered):**

**Chunk 1:**  
> Apex Horizon Agency is a full-service modeling and creative representation company dedicated to helping talent build sustainable, visible careers… Beyond modeling, the agency supports writers and thought leaders through book publishing assistance.

**Chunk 2:**  
> The agency also offers public relations and visibility tools, including media strategy support, brand messaging, press kit development, and promotional planning.

**RAG Output:**  
> *modeling and creative representation*

---

### **Test Case 2**
**Query:**  
> *Can you mail things using USPS?*

**Retrieved Chunks (ordered):**

**Chunk 1:**  
> The agency offers public relations and visibility tools, including media strategy support, brand messaging, press kit development, and promotional planning.

**Chunk 2:**  
> Apex Horizon Agency is a full-service modeling and creative representation company… including book publishing assistance.

**RAG Output:**  
> *Yes*

---

### **Test Case 3**
**Query:**  
> *If I were looking for a modeling agent and I was also interested in publishing a book, what services could I expect Horizon Agency to provide?*

**Retrieved Chunks (ordered):**

**Chunk 1:**  
> Apex Horizon Agency is a full-service modeling and creative representation company… Beyond modeling, the agency supports writers and thought leaders through book publishing assistance.

**Chunk 2:**  
> The agency also offers public relations and visibility tools, including media strategy support and promotional planning.

**RAG Output:**  
> *Beyond modeling, Apex Horizon Agency supports writers and thought leaders through book publishing assistance, helping clients navigate proposal development, editing, positioning, and submission to publishers or self-publishing platforms.*

---

## Analysis: Hallucination Mitigation

For in-domain queries (Test Cases 1 and 3), the RAG system successfully grounded the model’s responses in retrieved document content. The answers closely reflect the information present in the retrieved chunks and do not introduce unsupported claims about Apex Horizon Agency’s services.

In Test Case 3, the model effectively synthesized information from the retrieved context to produce a detailed, accurate response combining modeling representation and publishing assistance.

However, Test Case 2 highlights a limitation of the system. The query was out-of-domain relative to the document corpus, and although both chunks were retrieved, they contained no relevant information. As a result, the model defaulted to general world knowledge and answered “Yes.” While factually correct, this response was not grounded in retrieved content, demonstrating that RAG does not prevent hallucination or ungrounded answers when relevant documents are absent.

---

## Conclusion

This project demonstrates that Retrieval-Augmented Generation can reduce hallucination when relevant documents exist in the vector store and are incorporated into the prompt. However, with a small corpus and no relevance thresholding, retrieval may include irrelevant context, allowing the model to rely on prior knowledge for out-of-domain questions.

The results emphasize the importance of document coverage, chunking strategy, and retrieval confidence in building effective RAG systems.

---

## Future Improvements

- Increase corpus size and split documents into smaller chunks
- Apply similarity score thresholds to suppress ungrounded answers
- Explicitly detect out-of-domain queries
- Incorporate retrieval confidence into prompt construction
