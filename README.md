# DocumentQuery-RAG-LangChain-AI
RAG-based system to make queries to PDF documents. The following tools and models are used in this project:

1. pdfplumber (to extract pdf content)
2. LangChain (to chain differnt components)
3. Embeddings model: HugginFace's sentence-transformers/all-mpnet-base-v2
4. Vector store: FAISS
5. Open AI model: gpt-4o
6. LangChain Expression Language (LCEL) RAG pipeline

Along with the packages in the requirements.txt, the following python packages are needed to be installed:

pdfplumber==0.11.1, sentence-transformers==3.0.1, faiss-cpu==1.8.0, tf-keras, python-dotenv

The ecosystem has the following packages currently:

langchain                 1.2.0
langchain-classic         1.0.1
langchain-community       0.4.1
langchain-core            1.2.5
langchain-huggingface     1.2.0
langchain-openai          1.1.6
langchain-text-splitters  1.1.0
openai                    1.35.0