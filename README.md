# RAG with LangChain & Neo4j

Welcome to the **Hospital RAG Chatbot** repository! This project showcases a Retrieval-Augmented Generation (RAG) chatbot that uses LangChain for orchestration and Neo4j for data storage and vector indexing. The chatbot is designed to answer questions about patient experiences (unstructured data) and hospital system details (structured data), all in a single conversation interface.

---

## Overview
This chatbot applies modern **RAG techniques** for question-answering over both **structured** (Neo4j graph database) and **unstructured** data (patient reviews). It demonstrates:

- **Embedding & Vector Retrieval**: Patient reviews are stored and retrieved using vector similarity.  
- **Text-to-Cypher**: Seamlessly converts user queries into Cypher queries for insights from Neo4j.  
- **Dynamic Few-Shot Prompting**: Retrieves similar examples from a vector store to help generate better Cypher queries.  
- **Tool-Calling**: Leverages multiple tools under an AI agent to generate responses and fetch relevant data.  
- **Modular Architecture**: Docker Compose orchestrates FastAPI, Streamlit apps, and the Neo4j database.

---

## Features

1. **RAG for Patient Reviews**  
   - Indexes unstructured patient reviews for semantic search and quick retrieval.

2. **Structured Data Querying**  
   - Uses natural language to query hospital data (doctors, payers, visits, etc.) stored in Neo4j.

3. **Few-Shot Prompting**  
   - Dynamically fetches relevant question-query pairs to help the model generate more accurate Cypher.

4. **Streamlit Apps**  
   - **Main Chatbot UI**: Interact with the agent.  
   - **Cypher Example Self-Service**: Submit your own question-Cypher pairs to improve the chatbot’s few-shot examples.

5. **Asynchronous FastAPI**  
   - The chatbot agent is deployed through a FastAPI endpoint for easy integration with other services.

---

## Quick Start

1. **Clone this repository**:
   ```bash
   git clone https://github.com/gupta-nakul/langchain-neo4j-rag.git
   cd langchain-neo4j-rag
   ```

2. **Create your `.env` file** in the root directory. Use the variables described in [Configuration](#configuration).

3. **Install Docker and Docker Compose** (if not already installed):
   - Follow the official [Docker Compose installation guide](https://docs.docker.com/compose/install/).

4. **Build and Run**:
   ```bash
   docker-compose up --build
   ```

5. Access the services:
   - FastAPI: [http://localhost:8000/docs](http://localhost:8000/docs)
   - Main Chatbot (Streamlit): [http://localhost:8501](http://localhost:8501)
   - Cypher Example Self-Service Portal (Streamlit): [http://localhost:8502](http://localhost:8502)

---

## Configuration

Create a `.env` file in the repository’s root with the following variables:

```.env
NEO4J_URI=<YOUR_NEO4J_URI>
NEO4J_USERNAME=<YOUR_NEO4J_USERNAME>
NEO4J_PASSWORD=<YOUR_NEO4J_PASSWORD>

OPENAI_API_KEY=<YOUR_OPENAI_API_KEY>

HOSPITALS_CSV_PATH=https://raw.githubusercontent.com/gupta-nakul/langchain-neo4j-rag-chatbot/main/data/hospitals.csv
PAYERS_CSV_PATH=https://raw.githubusercontent.com/gupta-nakul/langchain-neo4j-rag-chatbot/main/data/payers.csv
PHYSICIANS_CSV_PATH=https://raw.githubusercontent.com/gupta-nakul/langchain-neo4j-rag-chatbot/main/data/physicians.csv
PATIENTS_CSV_PATH=https://raw.githubusercontent.com/gupta-nakul/langchain-neo4j-rag-chatbot/main/data/patients.csv
VISITS_CSV_PATH=https://raw.githubusercontent.com/gupta-nakul/langchain-neo4j-rag-chatbot/main/data/visits.csv
REVIEWS_CSV_PATH=https://raw.githubusercontent.com/gupta-nakul/langchain-neo4j-rag-chatbot/main/data/reviews.csv
EXAMPLE_CYPHER_CSV_PATH=https://raw.githubusercontent.com/gupta-nakul/langchain-neo4j-rag-chatbot/main/data/example_cypher.csv

CHATBOT_URL=http://host.docker.internal:8000/hospital-rag-agent

HOSPITAL_AGENT_MODEL=gpt-4o-mini
HOSPITAL_CYPHER_MODEL=gpt-4o-mini
HOSPITAL_QA_MODEL=gpt-4o-mini

NEO4J_CYPHER_EXAMPLES_INDEX_NAME=questions
NEO4J_CYPHER_EXAMPLES_NODE_NAME=Question
NEO4J_CYPHER_EXAMPLES_TEXT_NODE_PROPERTY=question
NEO4J_CYPHER_EXAMPLES_METADATA_NAME=cypher
```

> **Note**  
> - `NEO4J_URI`, `NEO4J_USERNAME`, and `NEO4J_PASSWORD` connect to your Neo4j AuraDB instance.  
> - `OPENAI_API_KEY` is your OpenAI API key for LLM queries.

---

## Data Sources

This project uses synthetic hospital system data, split across CSV files. During container startup, a small ETL process ingests these CSVs into Neo4j. The patient reviews are also embedded and indexed for RAG:

- **Hospitals**  
- **Physicians**  
- **Patients**  
- **Visits**  
- **Reviews**  
- **Cypher Example Queries**

---

## Future Enhancements

Some ideas I'm considering to further improve the chatbot:

- **Memory with Redis**  
  Persist conversation state to handle multi-turn dialogue more effectively.

- **Hybrid RAG**  
  Combine vector-based retrieval with advanced search techniques for better results.

- **Multi-modal Support**  
  Potentially integrate image or other data types into the chatbot.

- **Data Visualizations**  
  Display query results graphically within the Streamlit app.

- **Performance Evaluation**  
  Track metrics and experiment results with robust logging and reporting.

- **Cloud Infrastructure**  
  Use Terraform or other IaC tools to provision resources for production.
