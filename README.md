# n8n-RAG-Q&A-and-Embedding-Pipeline

**n8n-RAG-Q&A-and-Embedding-Pipeline** is a powerful automation project built with **n8n** that enables **Retrieval-Augmented Generation (RAG) Q&A** using a custom knowledge base.  
It includes two main workflows: a **RAG Q&A workflow** for answering questions based on documents, and a **Knowledge Base Embedding Pipeline** for ingesting and indexing documents for semantic search.

**Key technologies used:**
- **n8n** – workflow automation  
- **Mistral Cloud** – embeddings and large language models (LLMs)  
- **Pinecone** – vector database for semantic search  
- **Google Drive** – source of knowledge base documents  

---

## How It Works

### 1️⃣ Knowledge Base Embedding Pipeline

This workflow processes documents and stores their embeddings in Pinecone for semantic search. Here's what each node does:

1. **When clicking ‘Execute workflow’ (Manual Trigger)**  
   - Starts the workflow manually when you want to process new documents.

2. **Search files and folders (Google Drive)**  
   - Searches a specific Google Drive folder containing the knowledge base files.

3. **Download file (Google Drive)**  
   - Downloads the files found in the previous step for further processing.

4. **Recursive Character Text Splitter**  
   - Splits the downloaded text into smaller chunks recursively to preserve context.

5. **Default Data Loader**  
   - Loads files into a format suitable for embeddings and attaches metadata like the file name.

6. **Embeddings Mistral Cloud**  
   - Generates vector embeddings for each document chunk using Mistral Cloud’s embedding model.

7. **Pinecone Vector Store**  
   - Inserts the embeddings into Pinecone under the `martial_arts_knowledge_base` namespace for later retrieval.

---

### 2️⃣ RAG Q&A Workflow

This workflow answers user questions based on the knowledge base. Here's what each node does:

1. **When chat message received (Chat Trigger)**  
   - Triggered when a new message arrives from the connected chat interface.

2. **Basic LLM Chain (Mistral Cloud Chat Model)**  
   - Generates 1–3 paraphrased versions of the user question to improve semantic search accuracy.

3. **Structured Output Parser**  
   - Converts the LLM output into a structured JSON format for easier handling.

4. **Split Out**  
   - Splits the paraphrased queries into individual items to process them separately.

5. **Embeddings Mistral Cloud**  
   - Generates embeddings for the paraphrased queries if needed for vector search.

6. **Pinecone Vector Store**  
   - Searches the Pinecone vector database for relevant knowledge corresponding to each paraphrased query.

7. **Filter**  
   - Filters out documents with a similarity score below 0.4 to ensure only relevant content is used.

8. **Remove Duplicates**  
   - Eliminates duplicate documents to avoid redundancy.

9. **Aggregate**  
   - Combines the filtered and deduplicated documents into a single content block.

10. **Basic LLM Chain1 (Mistral Cloud Chat Model)**  
    - Generates the final answer based on the aggregated knowledge and user question, following strict rules for context and clarity.

---

## Workflow Overview

### RAG Q&A Workflow
![RAG Q&A Workflow](assets/RAG%20Q&A%20Workflow.png)

### Knowledge Base Embedding Pipeline
![Knowledge Base Embedding Pipeline](assets/Knowledge%20Base%20Embedding%20Pipeline.png)

---

## Features

- **Real-time Q&A** – Get precise answers from your knowledge base in real-time.
- **RAG-based system** – Combines retrieval and generation for accurate, context-aware responses.
- **Paraphrasing for improved retrieval** – Generates multiple versions of a question to enhance search.
- **Semantic search with embeddings** – Uses Pinecone and Mistral embeddings for high-quality vector search.
- **Document ingestion workflow** – Automatically process and index knowledge base files.
- **Duplicate removal & filtering** – Ensures clean and relevant results.
- **Configurable & extendable** – Customize models, thresholds, and knowledge sources.

---

## Example Use Cases

- **Internal company knowledge base** – Answer employees’ questions using internal documents.
- **Customer support automation** – Provide context-aware answers to FAQs.
- **Training and education** – Query course materials or reference documents intelligently.
- **Research assistance** – Quickly retrieve and summarize information from multiple sources.
