# rag_pipeline_chatbot_n8n

This n8n workflow is a **RAG (Retrieval-Augmented Generation) pipeline and chatbot**. It's designed to automatically process documents uploaded to Google Drive and use their content to answer user questions via a chat interface.


### Workflow Overview

The workflow is split into two main parts:

1.  **Ingestion Pipeline**: This part handles new documents.
    -   A **Google Drive Trigger** watches a specific folder ("DEMO"). When a new file is added, it downloads it.
    -   The **Download file** node fetches the new file.
    -   The **Default Data Loader** reads the content of the file.
    -   The **Recursive Character Text Splitter** breaks down the document content into smaller, manageable chunks.
    -   An **Embeddings Mistral Cloud** model converts these text chunks into numerical representations (vectors).
    -   Finally, the **Pinecone Vector Store** saves these vectors into a database called "demo" under the "AI" namespace. This database acts as a knowledge base.

2.  **Chatbot Pipeline**: This part handles user questions.
    -   The **When chat message received** node acts as the trigger for this pipeline, starting when a user sends a message.
    -   The **AI Agent** is the core component that processes the user's message.
    -   It uses a **Pinecone Vector Store1** as a tool. This tool retrieves relevant information from the "demo" database (the one populated by the ingestion pipeline) based on the user's question.
    -   An **Embeddings Mistral Cloud1** model helps the agent find the correct information.
    -   The **OpenRouter Chat Model** (using GPT-3.5-turbo) then uses the retrieved information to formulate an accurate and relevant response to the user's question.

In short, the workflow first creates a searchable knowledge base from new documents and then uses that knowledge base to power a chatbot that can answer questions based on the document content.
