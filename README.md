## **RAG‑Based Document Query System**

This project implements a Retrieval Augmented Generation (RAG) workflow that allows users to query the content of PDF documents using OpenAI model. It extracts text from PDFs, embeds the text using HuggingFace sentence transformers, stores embeddings in a FAISS vector index and uses OpenAI GPT-4o model (via LangChain) to generate grounded answers based on retrieved context.

**Features**

- PDF text extraction using pdfplumber
- Chunking with RecursiveCharacterTextSplitter 
- Semantic embeddings via HuggingFaceEmbeddings (all-mpnet-base-v2)
- Vector storage and similarity search using FAISS
- Retriever setup with configurable k (top‑k relevant chunks)
- Custom RAG prompt built with ChatPromptTemplate
- LLM response generation using ChatOpenAI
- LangChain Expression Language (LCEL) pipeline combining retriever → prompt → LLM into a single chain

**Requirements**

- Depndencies:
    - pdfplumber
    - sentence-transformer
    - faiss-cpu
    - python-dotenv
    - langchain
    - langchain-core
    - langchain-community
    - langchain-huggingface
    - langchain-openai
    - langchain-text-splitters
    - openai
    - ipykernel

  To install the dependencies: pip install -r requirements.txt

- .env file contains the OpenAI API key (OPENAI_API_KEY).

**Steps**

- Load PDFs and extract texts
    - Tool: pdfplumber
    - Reads all .pdf files from the files/ directory and brings all extracted texts together into one combined text.

- Split texts into chunks
    - Component: RecursiveCharacterTextSplitter
    - Splits the combined text into overlapping chunks (chunk_size=800, chunk_overlap=200) suitable for retrieval.

- Create embeddings and vector store
    - Tools: HuggingFaceEmbeddings with sentence-transformers/all-mpnet-base-v2, FAISS (FAISS.from_texts)
    - Converts text chunks into embeddings and stores them in a FAISS vector index for fast similarity search.

- Build a retriever
    - Component: db.as_retriever()
    - Configured with k=12 to return the 12 most similar chunks for a given query.

- Define the RAG prompt
    - Component: ChatPromptTemplate
    - Prompt template:
        Answer the question based only on the following context:
        {context}

        Question: {input}

- Construct the RAG pipeline (LCEL)
    - Components: RunnablePassthrough, ChatOpenAI, ChatPromptTemplate, FAISS retriever
    - Pipeline:
        rag_chain = (
            {
                "context": retriever,
                "input": RunnablePassthrough()
            }
            | prompt
            | llm
        )

    - For a user query:
        - the retriever returns relevant chunks as context,
        - the prompt formats context + input,
        - the LLM generates the answer.

**Example usage**
rag_chain.invoke("Query 1: Ask something about the document content") 
rag_chain.invoke("Query 2: Another document‑based question")