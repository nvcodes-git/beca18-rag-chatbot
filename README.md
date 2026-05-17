# beca18-rag-chatbot

## Purpose

This project builds a Retrieval-Augmented Generation (RAG) pipeline that answers questions about the official Beca 18 scholarship regulations from PRONABEC (Peru). The source document is `beca18_reglamento.pdf`, the official regulatory PDF published by MINEDU/VMGI-PRONABEC.

## Pipeline Summary

The pipeline reads the PDF page by page, cleans the text, and splits it into 400-token chunks with 60-token overlap using `RecursiveCharacterTextSplitter`. Each chunk is embedded with `gemini-embedding-001` (768 dimensions) and stored in a persistent ChromaDB collection using cosine similarity. When a user asks a question, it gets embedded and the top-k most relevant chunks are retrieved and sent as context to `gemini-2.5-flash`, which answers only from those fragments and cites page numbers when possible.

## Installation and Setup

**1. Clone the repository**
```bash
git clone https://github.com/your-username/beca18-rag-chatbot.git
cd beca18-rag-chatbot
```

**2. Install dependencies**
```bash
pip install -r requirements.txt
```

**3. Set up your API key**

Copy `.env.example` to `.env` and fill in your Gemini API key:
```bash
cp .env.example .env
```
Then open `.env` and set:
```
GEMINI_API_KEY=your_gemini_api_key_here
```
Note: never commit the `.env` file. It is already covered by `.gitignore`.

## How to Run the Notebook

Open the notebook in Jupyter:
```bash
jupyter notebook notebooks/beca18_rag_chatbot.ipynb
```

Run all cells from top to bottom (Kernel > Restart and Run All). The first run will index all 321 chunks into ChromaDB, which takes around 3 minutes. On subsequent runs it will detect the existing collection and skip the indexing step automatically.

## How to Use the Chat Interface

The last cell displays an interactive widget with the following controls:

- **Text input**: type your question about Beca 18
- **Ask button**: submits the question and shows the answer
- **Clear button**: resets the input and output
- **k slider**: sets how many document fragments to retrieve (1 to 10)
- **Fuentes recuperadas**: an expandable accordion showing each source fragment with its page number and similarity distance

The chatbot only answers based on the official regulations. If a question is outside the scope of the document, it will respond with: "El documento no contiene información sobre este tema."
