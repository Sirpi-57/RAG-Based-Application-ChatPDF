# RAG-Based-Application-ChatPDF

**Objective**

The objective of this project is to create an intelligent document-querying system that leverages fine-tuned Large Language Models (LLMs) in conjunction with Retrieval-Augmented Generation (RAG). This system enables users to upload PDF documents, extract key information, and query the content interactively in natural language. By combining RAG with fine-tuning, the model provides more accurate, context-specific responses and enhances its ability to retrieve relevant information from large, unstructured documents. This setup can be used in scenarios like research, legal document review, and data-heavy industries where extracting precise information is critical.

**Explanations of Concepts:**

**Retrieval-Augmented Generation (RAG):**

RAG is a hybrid approach that combines document retrieval with language generation. Instead of relying solely on the generative capabilities of a large language model, RAG retrieves relevant documents from a knowledge base (in this case, PDFs or other documents) and then generates responses based on the retrieved content. This ensures that the answers are not just fluent but also factually accurate and contextually relevant.

**Fine-tuning a Large Language Model (LLM):**

Fine-tuning involves customizing a pre-trained model, in our case **Ollama (Llama 3.1)**, by adapting it for a specific task. In this case, the task is question-answering based on PDF content. The model is fine-tuned using a custom prompt template to provide concise and accurate answers based on the PDF context. The use of ChatOllama and FastEmbedEmbeddings enables efficient vector-based document retrieval and response generation.

**Recursive Character Text Splitting:**

PDFs often contain large blocks of text. To make the data manageable, the RecursiveCharacterTextSplitter is used to break down the document into smaller chunks. This allows for better indexing, retrieval, and processing by the LLM.

**Vector Store and Retrieval:**

To perform efficient search and retrieval, the PDF data is converted into vector embeddings using the Chroma vector store. These embeddings help in finding the most relevant pieces of information (chunks) when a user asks a question. A similarity-based retrieval method is used to fetch context based on a score threshold.

**Project Deliverables:**

**PDF Ingestion:** Users can upload PDF files, and the app will process and ingest the content using the PyPDFLoader.

**Question Answering System:** After ingestion, users can ask questions, and the system will generate answers based on the content retrieved from the PDF.

**Streamlit-based UI:** The application is built with Streamlit, featuring an easy-to-use interface where users can upload documents, input queries, and see responses in real-time.

**Real-time Interaction:** The app provides a chat-like interface using streamlit_chat for real-time interactions.

**Ingestion Feedback:** The user gets a feedback message indicating the time taken to ingest each document, ensuring transparency and clarity.

**Future Scope:**

**Support for Other Document Types:** Extending the functionality to ingest and process documents in formats such as DOCX, TXT, or HTML.

**Advanced Document Search:** Implementing more sophisticated search methods, such as keyword-based or metadata-based filtering, for more accurate retrieval.

**Multi-language Support:** Allowing question-answering in multiple languages to cater to a broader audience.

**Improved Model Performance:** Fine-tuning the model further with more document-specific training data to enhance accuracy.

**Model Selection Options:** Providing the user with different LLMs to choose from, depending on the complexity and type of document.

![Screenshot (164)](https://github.com/user-attachments/assets/e21cb14b-5f5f-4296-ae06-dbe4369a269e)
