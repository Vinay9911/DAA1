FarmVaidya-POC: Agricultural Chatbot for Farmers
Project Overview
FarmVaidya-POC is an agricultural chatbot designed to assist farmers with queries related to farming, crops, pests, and diseases. It supports both Telugu and English languages, making it accessible to a wide audience, particularly in Telugu-speaking regions. The chatbot employs a Retrieval-Augmented Generation (RAG) system to provide accurate and context-aware responses based on uploaded agricultural documents, such as PDFs, Docs, or text files. The project is structured into two main components:

Backend: Built with FastAPI, it handles API requests for document management, query processing, and speech processing.
Frontend: Built with Streamlit, it provides a user-friendly interface for uploading documents and interacting with the chatbot via text or voice.

Key Features

Multilingual Support: Processes queries in Telugu and English, with speech-to-text and translation capabilities.
Document-Based Knowledge: Users can upload documents to enhance the chatbot’s knowledge base for tailored responses.
Follow-up Questions: Asks clarifying questions if a query lacks sufficient information, ensuring accurate answers.
Speech Processing: Supports voice input with Telugu transcription and translation to English.
Logging and Monitoring: Logs interactions, including speech processing times, for debugging and improvement.

Workflow
The chatbot’s workflow is designed to handle user queries efficiently:

User Input:

Users interact via text or voice through the Streamlit frontend.
Voice inputs are transcribed to Telugu text using an external Speech-to-Text (STT) API (IndicTranscribe API).


Input Processing:

Telugu text is translated to English using a specialized agricultural dictionary to preserve domain-specific terms.
Queries are classified as greetings, agriculture-related, or non-agriculture using predefined prompts.


Query Handling:

For agriculture-related queries, the system checks if sufficient information is provided.
If insufficient, it initiates a follow-up flow, asking up to five clarifying questions.


RAG System:

The RAG system retrieves relevant information from documents stored in MongoDB GridFS.
A language model (e.g., Gemini) generates a response based on retrieved data and conversation history.


Response Generation:

Responses are generated in English and translated back to Telugu if the input was in Telugu.
The frontend displays the response, streamed character-by-character for a dynamic effect.


Logging:

Interactions, including transcription and translation times, are logged to backend/SPEECH_LOGS.csv.
General logs are stored in backend/logs/ for debugging.



Project Structure
Farmvaidya-POC/
├── backend/
│   ├── api/
│   │   ├── routers/
│   │   │   ├── health.py
│   │   │   ├── documents.py
│   │   │   ├── queries.py
│   │   │   ├── speech.py
│   │   │   └── models.py
│   │   ├── schemas/
│   │   │   └── request_models.py
│   │   ├── __init__.py
│   │   ├── classification.py
│   │   ├── config.py
│   │   └── dependencies.py
│   ├── lightrag_core/
│   │   ├── __init__.py
│   │   ├── controller.py
│   │   ├── exceptions.py
│   │   ├── models.py
│   │   └── utils/
│   │       ├── __init__.py
│   │       ├── document_processor.py
│   │       └── prompt.py
│   ├── utils/
│   ├── logs/
│   ├── rag_workspace/
│   ├── uploads/
│   ├── main.py
│   ├── requirements.txt
│   ├── .env
│   └── SPEECH_LOGS.csv
├── frontend/
│   ├── app.py
│   ├── requirements.txt
│   └── .env

How to Run
Prerequisites

Python 3.8+: Ensure Python is installed (Python Downloads).
MongoDB: Required for document storage, either locally or via MongoDB Atlas (MongoDB Atlas).
API Keys: Obtain a Gemini API key for language model integration (Google Cloud).

Setup

Clone the Repository:
git clone https://github.com/yourusername/FarmVaidya-POC.git
cd FarmVaidya-POC


Create .env Files:

In backend/:GEMINI_API_KEY=your_gemini_api_key
MONGODB_URI=mongodb://localhost:27017/farmvaidya
PORT=8000


In frontend/:API_BASE_URL=http://localhost:8000/api/v1




Install Dependencies:

Backend:cd backend
pip install -r requirements.txt


Frontend:cd ../frontend
pip install -r requirements.txt




Start MongoDB:

For local MongoDB, ensure the service is running (MongoDB Installation).
For MongoDB Atlas, verify the MONGODB_URI in backend/.env.


Run the Backend:
cd backend
uvicorn main:app --reload


Run the Frontend:In a new terminal:
cd ../frontend
streamlit run app.py


Access the Application:Open your browser and navigate to http://localhost:8501 to use the chatbot.


Note: Ensure the backend is running before starting the frontend, as the frontend relies on API calls to the backend.
Dependencies
The project uses separate dependency lists for backend and frontend, specified in their respective requirements.txt files.



Component
Key Dependencies



Backend
fastapi, uvicorn, motor, loguru, tenacity, pydantic


Frontend
streamlit, requests, streamlit-mic-recorder


Install dependencies using pip install -r requirements.txt in each directory.
Configuration
Configuration is managed via .env files in both backend/ and frontend/ directories. Key variables include:



Variable
Description
Default/Example



GEMINI_API_KEY
API key for Gemini language model
your_gemini_api_key


MONGODB_URI
MongoDB connection string
mongodb://localhost:27017/farmvaidya


PORT
Backend API port
8000


API_BASE_URL
Base URL for frontend API calls
http://localhost:8000/api/v1


WORKING_DIRECTORY
Directory for RAG workspace
./rag_workspace


STORAGE_DIR
Directory for uploaded files
./uploads


LOG_DIR
Directory for logs
./logs


CSV_LOG_PATH
Path for speech logs
./SPEECH_LOGS.csv


Ensure these variables are correctly set in the .env files before running the application.
Document Management

Uploading Documents:
Navigate to "Upload Documents" in the frontend sidebar.
Select PDFs, Docs, or text files and click "Submit Documents".
Files are stored in MongoDB GridFS and processed for the RAG system.


Processing:
Documents are saved locally in backend/uploads/ before being uploaded to MongoDB.
The /upload-and-clear endpoint uploads local files to MongoDB and deletes them locally, triggered when clearing the chat.


Retrieval:
Documents are retrieved by the RAG system to provide context for query responses.



Speech Processing

Voice Input:
Users can record voice queries using the microphone icon in the frontend.
Audio is transcribed to Telugu text using an external STT API.


Translation:
Telugu text is translated to English using a specialized dictionary for agricultural terms (e.g., "Black-headed Caterpillar").
Responses are translated back to Telugu if the input was in Telugu.


Performance:
Transcription and translation times are logged in SPEECH_LOGS.csv for monitoring.



Follow-up Questions

The chatbot classifies queries to determine if they are agriculture-related.
If a query lacks sufficient information, it initiates a follow-up flow, asking up to five questions to gather details.
Conversation history is maintained to ensure context-aware responses.

Error Handling

Logging: Errors are logged using loguru in backend/logs/ for debugging.
User Feedback: The frontend displays user-friendly error messages for issues like invalid inputs or API failures.
HTTP Exceptions: The backend raises HTTP 400 (bad request) or 500 (server error) for processing issues.

Usage

Upload Documents:

Go to "Upload Documents" in the sidebar.
Select PDFs, Docs, or text files and click "Submit Documents".
Documents are processed and stored for use in query responses.


Chat with the Assistant:

Switch to "Chat & Voice Assistant" in the sidebar.
Enter a query via text or record a voice message.
The chatbot responds based on uploaded documents and its knowledge base.


Clear Chat:

Click "Clear Chat" to reset the conversation and upload remaining local documents to MongoDB.



Contributing
Contributions are welcome! Please open an issue on GitHub to discuss proposed changes before submitting a pull request.
License
This project is licensed under the MIT License - see the LICENSE file for details.
Key Citations

IndicTranscribe API for Speech-to-Text
Python Official Downloads
MongoDB Atlas Cloud Database
Google Cloud for Gemini API
MongoDB Installation Guide

