# 📚 AI Research Paper Intelligence System

Finding relevant research papers using only keywords often misses papers that discuss the same idea with different wording. This project solves that problem by using semantic search instead of traditional keyword matching.

The application searches through around **50,000 Machine Learning papers from ArXiv**, finds papers that are semantically similar to a user's query, generates a short summary for each result, and extracts important keywords to help users understand the paper quickly.

This project was developed as part of the **Coding Blocks Internship Program**.

---

# Features

- Search research papers using natural language queries
- Semantic search using sentence embeddings
- Fast similarity search with FAISS
- Automatic abstract summarization using a Transformer model
- Keyword extraction using KeyBERT
- Simple and interactive Streamlit interface

---

# Example Query

Instead of searching with exact keywords, users can enter queries like:

> **"deep learning for medical image analysis"**

The system then:

- Converts the query into a sentence embedding
- Finds papers with similar meaning
- Generates a concise summary of each abstract
- Extracts important keywords from every paper
- Displays the most relevant results in the web application

---

# Project Workflow

```
                     ArXiv ML Dataset
                            │
                            ▼
                  Data Cleaning & Processing
                            │
                            ▼
          Sentence Transformer Embeddings
                            │
                            ▼
                    FAISS Vector Index
                            │
                 User Search Query
                            │
                            ▼
               Semantic Similarity Search
                            │
            ┌───────────────┴───────────────┐
            ▼                               ▼
     Abstract Summarization          Keyword Extraction
            │                               │
            └───────────────┬───────────────┘
                            ▼
                     Streamlit Web App
```

---

# Technologies Used

| Component | Library / Tool |
|-----------|----------------|
| Dataset | Hugging Face ML-ArXiv-Papers |
| Embeddings | Sentence Transformers (all-MiniLM-L6-v2) |
| Vector Search | FAISS |
| Summarization | DistilBART |
| Keyword Extraction | KeyBERT |
| Backend | Python |
| Data Processing | Pandas, NumPy |
| Frontend | Streamlit |

---

# Folder Structure

```
AI-Research-Paper-Intelligence-System/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   └── README.md
│
├── notebooks/
│   ├── 01_EDA_and_Embeddings.ipynb
│   └── 02_Search_Engine.ipynb
│
└── src/
    ├── data_prep.py
    ├── build_index.py
    ├── search_engine.py
    └── app.py
```

The notebooks explain each stage of the project, from data exploration to semantic search. The `src` folder contains the modular implementation used by the Streamlit application.

---

# Installation

Clone the repository and install the required packages.

```bash
pip install -r requirements.txt
```

---

# Build the Search Index

The first time you run the project, embeddings need to be generated for the dataset and indexed using FAISS.

```bash
python src/data_prep.py
python src/build_index.py
```

Since embeddings are created for roughly **50,000 papers**, this step may take around **20–30 minutes on CPU**. The generated files are saved locally, so this process only needs to be done once.

---

# Run the Application

```bash
streamlit run src/app.py
```

After the server starts, open the local Streamlit URL shown in the terminal (typically `http://localhost:8501`).

---

# Using the Search Engine in Python

```python
from src.search_engine import PaperSearchEngine

engine = PaperSearchEngine()

results = engine.full_report(
    "deep learning for medical image analysis",
    k=5
)

for paper in results:
    print(paper["title"])
    print(paper["summary"])
    print(paper["keywords"])
```

---

# Design Choices

### Why FAISS?

FAISS provides efficient similarity search over dense vector embeddings. For a dataset of around 50,000 papers, an exact index (`IndexFlatIP`) offers fast retrieval without sacrificing accuracy.

### Why Sentence Transformers?

The `all-MiniLM-L6-v2` model produces compact 384-dimensional embeddings while maintaining good semantic understanding. It offers a practical balance between speed and performance.

### Why Normalize Embeddings?

Normalizing embeddings allows cosine similarity to be computed efficiently through inner-product search in FAISS.

### Why Save the Index?

Generating embeddings for the entire dataset is time-consuming. Saving the embeddings and FAISS index avoids repeating this step every time the application starts.

---

# Future Improvements

Some possible enhancements include:

- Filtering results by publication year or research category
- Supporting larger datasets with approximate FAISS indexes
- Deploying the application online
- Adding citation networks between related papers
- Allowing users to bookmark or export search results

---

# Author

**Harshit Shakya**

Coding Blocks Internship Project
