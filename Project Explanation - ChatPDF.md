# **Retrieval-Augmented Generation (RAG) with ChatPDF**

## **Objective**

The objective of this project is to build an interactive web application that allows users to upload PDF documents and query them using a fine-tuned large language model (LLM) with the help of Retrieval-Augmented Generation (RAG). The model retrieves relevant chunks of information from the uploaded document and uses that context to generate precise answers to user queries.

---

## **Project Overview**

This project leverages RAG by combining the capabilities of a large language model (LLM) and vector-based retrieval. The core idea is to retrieve the most relevant parts of a document and pass them to a LLM, which then generates an answer based on the retrieved information.

Key components of the project:

* **ChatPDF Class**: Handles the ingestion of PDF documents, retrieval of relevant chunks, and querying of the model.  
* **Streamlit Application**: Provides a user interface to upload PDFs, query the model, and display the results in a chat-like format.

---

## **Code Explanation**

### **ChatPDF Class**

The `ChatPDF` class is the core of the project, responsible for the following:

1. **Ingesting PDF documents** by splitting them into manageable chunks and embedding them for retrieval.  
2. **Retrieving** the relevant parts of the document based on user queries.  
3. **Generating responses** by combining the retrieved content and querying a large language model (LLM).

`class ChatPDF:`   
    `vector_store = None`   
    `retriever = None`   
    `chain = None` 

This initializes placeholders for the vector store, retriever, and chain that will be used to manage retrieval and generation.

#### **1\. Initialization (`__init__`)**

The `__init__` method sets up the necessary components for the model:

* **Model**: It uses the `ChatOllama` model (from the Ollama family) for generating text.  
* **Text Splitter**: Breaks down PDF content into manageable chunks (1024 characters with 100-character overlap) for efficient retrieval.  
* **Prompt**: A template for how questions and context should be structured before being passed to the LLM.

`self.model = ChatOllama(model = "llama3.1")`  
`self.text_splitter = RecursiveCharacterTextSplitter(chunk_size = 1024, chunk_overlap = 100)`

#### **2\. Ingestion (`ingest`)**

This function ingests the PDF file, splits it into chunks, and converts the text into vector embeddings using a `Chroma` vector store. It filters metadata from the document for efficient retrieval and sets up the retriever to find relevant document chunks based on a similarity score.

`def ingest(self, pdf_file_path: str):`  
    `docs = PyPDFLoader(file_path=pdf_file_path).load()`  
    `chunks = self.text_splitter.split_documents(docs)`  
    `chunks = filter_complex_metadata(chunks)`

The `Chroma` vector store stores the document embeddings, and the retriever is set up with a similarity threshold and a limit (`k=3`) for returning the top 3 relevant chunks.

#### 

#### 

#### 

#### **3\. Query Handling (`ask`)**

Once the ingestion is complete, users can ask questions related to the uploaded document. The system retrieves relevant chunks using the retriever and passes them to the LLM model along with the question.

`def ask(self, query: str):`  
    `return self.chain.invoke(query)`

The `chain` combines retrieval and generation using the prompt template, and the response is returned to the user.

#### **4\. Clearing Data (`clear`)**

This method resets the class's internal state, allowing for new documents to be ingested without conflicts from previous documents.

`def clear(self):`  
    `self.vector_store = None`  
    `self.retriever = None`  
    `self.chain = None`

---

### **Streamlit Application**

The Streamlit application provides the user interface for uploading PDFs, querying, and displaying answers in a chat-like format.

#### **1\. File Upload and Ingestion**

Users can upload PDF documents using the `file_uploader` widget. Once a file is uploaded, it triggers the `read_and_save_file` function, which reads the file and ingests it into the ChatPDF instance.

`st.file_uploader(`  
    `"Upload document",`  
    `type=["pdf"],`  
    `on_change=read_and_save_file,`  
    `accept_multiple_files=True,`  
`)`

#### **2\. Chat Interface**

The chat interface is implemented using the `message` component from the `streamlit_chat` library. It displays messages from both the user and the model.

`def display_messages():`  
    `for i, (msg, is_user) in enumerate(st.session_state["messages"]):`  
        `message(msg, is_user=is_user, key=str(i))`

#### **3\. Query Handling**

User input is processed by the `process_input` function, which sends the user's query to the `ChatPDF.ask()` method. The response from the model is then displayed in the chat interface.

`def process_input():`  
    `user_text = st.session_state["user_input"].strip()`  
    `agent_text = st.session_state["assistant"].ask(user_text)`  
    `st.session_state["messages"].append((user_text, True))`  
    `st.session_state["messages"].append((agent_text, False))`

#### **4\. Page Layout**

The main application page (`page()`) organizes the layout of the Streamlit app:

* It displays the file upload section.  
* It handles message display and text input for querying the model.

`def page():`  
    `st.header("ChatPDF")`  
    `st.subheader("Upload a document")`  
    `display_messages()`  
    `st.text_input("Message", key="user_input", on_change=process_input)`

---

## **How the Project Works**

1. **PDF Ingestion**: The user uploads a PDF document, which is split into smaller chunks and embedded into a vector store. These embeddings allow the system to retrieve relevant information based on queries.  
2. **Query and Retrieval**: The user asks a question, and the system uses the retriever to find the most relevant chunks of the document. These chunks are passed to the LLM for generating an answer.  
3. **Answer Generation**: The LLM generates an answer based on the retrieved context and presents it to the user in the chat interface.

---

## **Functions Used**

* **`PyPDFLoader`**: Loads the PDF document for ingestion.  
* **`RecursiveCharacterTextSplitter`**: Splits long documents into smaller chunks.  
* **`Chroma`**: Stores document embeddings and provides similarity-based retrieval.  
* **`ChatOllama`**: A LLM used to generate text based on input prompts.  
* **`PromptTemplate`**: Formats the retrieved context and question for the LLM.  
* **`streamlit_chat`**: Displays messages in a chat format on the web interface.

---

## **Future Scope**

* **Improved Retrieval**: Enhancing the retrieval process by exploring other embedding models and adjusting the similarity threshold for better precision.  
* **Additional File Formats**: Extending the application to support other file formats (e.g., Word documents, plain text).  
* **Deployment**: Deploying the application on the cloud (e.g., AWS, GCP) for broader accessibility.  
* **Multi-Language Support**: Expanding the model to handle multilingual PDFs and queries.  
* **Advanced Query Understanding**: Implementing better natural language understanding for more complex queries and responses.

