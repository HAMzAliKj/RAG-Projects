# mistral_ocr_guide

Mistral OCR + Gemini Vision RAG Pipeline

Extract rich knowledge from PDFs using OCR, image understanding, and Retrieval-Augmented Generation (RAG).

Features

✅ OCR using Mistral OCR

✅ Automatic Table Extraction

✅ Formula Preservation

✅ Figure Extraction

✅ AI Image Description using Gemini 2.5 Flash

✅ Markdown Reconstruction

✅ LangChain Chunking

✅ HuggingFace Embeddings

✅ Qdrant Cloud Vector Database

✅ Retrieval-Augmented Generation (RAG)

Workflow
PDF
 │
 ▼
Mistral OCR
 │
 ├── Text
 ├── Tables
 ├── Formulas
 └── Images
         │
         ▼
 Gemini Vision
         │
         ▼
 Image Descriptions
         │
         ▼
 Merge Markdown
         │
         ▼
 Chunking
         │
         ▼
 HuggingFace Embeddings
         │
         ▼
 Qdrant Cloud
         │
         ▼
 Retriever
         │
         ▼
 Gemini 2.5 Flash
         │
         ▼
 AI Answers
Tech Stack
Python
Mistral OCR
Gemini 2.5 Flash
LangChain
HuggingFace Embeddings
Qdrant
Markdown
OCR
RAG

Original PDF

↓

Markdown

↓

Image Description

↓

Chunks

↓

Embeddings

↓

RAG

Project Structure
project/

│

├── notebook.ipynb

├── README.md

├── requirements.txt

├── sample.pdf

├── images/

├── outputs/

└── screenshots/
