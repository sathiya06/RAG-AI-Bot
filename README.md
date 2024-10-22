# RAG-Based AI Chat Bot

This project is a Retrieval-Augmented Generation (RAG)-based chatbot powered by OpenAI's GPT model, which uses a vector database to enhance search and response capabilities. The bot processes data stored in the `data` folder using OpenAI's embeddings, with a text processing pipeline built on `RecursiveCharacterTextSplitter` to manage larger documents. The Chroma vector database is employed to store and query the processed data.

## Features
- **Data Source**: The bot processes text files from the `data` folder.
- **Embedding Creation**: It generates embeddings using OpenAI's `OPENAIEmbeddings`.
- **Vector Database**: The bot stores and retrieves vectors using **Chroma**.
- **Text Preprocessing**: It uses `RecursiveCharacterTextSplitter` to handle and split large text documents.

## Installation

### 1. Clone the repository
Clone the repository using the command:

    git clone <repository-url>
    cd <repository-directory>

### 2. Install the required packages
To install all dependencies, use the command:

    pip install -r requirements.txt

### 3. Set up OpenAI API Key
You will need an OpenAI API key to run the chatbot. Create a `.env` file in the root directory and add your API key in the following format:

    OPENAI_API_KEY=your_openai_api_key_here

Replace `your_openai_api_key_here` with your actual OpenAI API key.

## Usage

Once the environment is set up, you can run the bot using the command:

    python app.py

The bot will use the data stored in the `data` folder as its source for generating responses. It will preprocess the text files, create embeddings, and use the Chroma vector database to handle vector storage and retrieval.

## File Structure

    ├── data/                  # Folder containing source text files
    ├── app.py                 # Main script to run the bot
    ├── requirements.txt       # List of dependencies
    ├── .env                   # Environment file for OpenAI API key
    ├── README.md              # Project documentation
    └── other necessary scripts or files

## Requirements
- Python 3.8+
- OpenAI API Key

## Key Libraries
- **Chroma**: Vector database for storing and querying embeddings.
- **OpenAI**: Used to access the GPT model and create embeddings.
- **LangChain**: For handling embedding creation and text splitting.

## Contributing
Feel free to submit a pull request or open an issue if you find a bug or have a suggestion!
