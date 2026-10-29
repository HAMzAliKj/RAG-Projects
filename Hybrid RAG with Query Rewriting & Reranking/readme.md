Hybrid RAG with Cohere Embeddings, Qdrant, and BM25
A Python-based Hybrid Retrieval-Augmented Generation (RAG) project that retrieves relevant information from PDF lecture notes using both semantic vector search and keyword-based BM25 search.

The notebook combines results from both retrieval methods into one list, preparing them for a reranking step or use by an LLM.

Features
PDF loading: Loads PDF pages using LangChain's PyPDFLoader.
Text chunking: Splits page text into chunks of 900 characters with 150 characters of overlap.
Embeddings: Uses Cohere's embed-english-v3.0 embedding model.
Vector database: Stores and retrieves embedded chunks using Qdrant Cloud.
Semantic search: Finds relevant chunks based on meaning using Qdrant.
Keyword search: Uses BM25 to find chunks based on query terms.
Combined retrieval: Merges BM25 and vector-search results into one list for a subsequent reranking or LLM step.
Local serialization: Saves and reloads the document chunks with Python pickle.
How It Works
Load the PDF lecture notes.
Split each page into smaller text chunks.
Add lecture and page-number markers to each chunk.
Generate Cohere embeddings and store the chunks in a Qdrant collection.
Load the existing Qdrant collection for vector search.
Save the chunks to my_list.pkl, then load them for BM25 keyword retrieval.
Retrieve the top four results from BM25 and the top four from vector search.
Combine both sets of results into Result_Ready_For_Reranking.
PDF Lecture Notes
       |
       v
Load Pages and Split into Chunks
       |
       +---------------------------+
       |                           |
       v                           v
Cohere Embeddings              Document Chunks
       |                           |
       v                           v
Qdrant Vector Search           BM25 Keyword Search
       |                           |
       +-------------+-------------+
                     |
                     v
             Combine Retrieved Results
                     |
                     v
          Ready for Reranking / LLM
Technologies Used
Python
LangChain
Qdrant Cloud
Cohere Embeddings (embed-english-v3.0)
BM25 Retriever
PyPDFLoader
python-dotenv
pickle
Requirements
Python 3.10 or a compatible Python version supported by the selected packages
A Qdrant Cloud URL and API key
A Cohere API key
A PDF file containing searchable text
Setup
1. Clone the repository
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_REPOSITORY_FOLDER>
2. Install the dependencies
pip install langchain-community langchain-text-splitters langchain-cohere langchain-qdrant qdrant-client rank-bm25 pypdf python-dotenv
3. Configure environment variables
Create a .env file in the project directory:

QDRANT_URL=your_qdrant_cloud_url
QDRANT_API_KEY=your_qdrant_api_key
COHERE_API_KEY=your_cohere_api_key
Replace the example values with your own credentials. Never commit your .env file or expose API keys in your notebook or repository.

4. Set your PDF path
In the notebook, replace the placeholder in PyPDFLoader with the path to your PDF:

loader = PyPDFLoader("path/to/your_lecture_notes.pdf")
5. Run the notebook
Open main.ipynb in Jupyter Notebook, JupyterLab, or VS Code and execute the cells in order.

Configuration
Setting	Value in notebook
Embedding model	embed-english-v3.0
Chunk size	900 characters
Chunk overlap	150 characters
Qdrant collection	Universty Lecture Notes
BM25 results	4
Vector-search results	4
The Qdrant collection name is intentionally shown exactly as it appears in the notebook. Use the same name when accessing the existing collection.

Example Query
The notebook demonstrates retrieval with this query:

query = "Honeybee foraging & the waggle dance"
BM25 and Qdrant vector search each retrieve up to four relevant chunks. The results are combined:

Result_Ready_For_Reranking = query_to_bm25 + result
This produces a combined list of retrieved documents. It does not itself rerank the results or generate a final natural-language answer; those steps would need to be implemented separately.

Important Notes
Run the document-ingestion cells before trying to retrieve from a new PDF.
The Qdrant collection must exist before from_existing_collection() can access it.
The notebook saves document chunks in my_list.pkl so they can be loaded for BM25 retrieval. If you change the PDF, regenerate the pickle file so it matches the new content.
The notebook's current BM25 call uses a private method (_get_relevant_documents). LangChain's public retrieval interface is generally preferable for new code.
The notebook currently concatenates the two result lists. It does not deduplicate repeated chunks or apply a reranking model.
Ensure that page-number metadata and document content are preserved as expected when preparing citations or source references.
Project Structure
.
├── main.ipynb       # PDF ingestion and hybrid retrieval workflow
├── .env             # API credentials (keep private; do not commit)
└── my_list.pkl      # Serialized document chunks created by the notebook
my_list.pkl is generated when the relevant notebook cell runs. It may be excluded from Git if you prefer to regenerate it.

Future Improvements
Add a reranking model to sort the combined results by relevance.
Remove duplicate chunks from the combined results.
Pass the retrieved context to an LLM to generate an answer grounded in the PDF.
Preserve page numbers as structured metadata for easier source citations.
Add a reusable query function and evaluation tests.
License
Add a license if you intend to distribute or open-source this project.
