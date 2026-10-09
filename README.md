# PlagiaSense — Word-Level Plagiarism Checker

A Data Structures and Algorithms (DSA) based plagiarism checker that compares uploaded PDF documents against reference research papers retrieved through the Semantic Scholar API. It identifies matching text, calculates a word-level similarity percentage, and compares the performance of the **Knuth–Morris–Pratt (KMP)** and **Rabin–Karp** string-matching algorithms.

## Overview

PlagiaSense is a web-based application designed to automate word-level text comparison for short academic documents. Users upload a PDF through a React dashboard, and the Java backend extracts its text, searches for reference papers, and analyzes matching word sequences.

The application uses a custom hash table with separate chaining to index word-level trigrams and identify matching words between the uploaded document and reference papers. It also provides a comparison of KMP and Rabin–Karp using algorithm statistics such as character comparisons and execution time.

## Features

- **PDF Text Extraction:** Extracts text from uploaded PDF documents using Apache PDFBox.
- **Automatic Reference Search:** Uses the Semantic Scholar API to search for relevant research papers.
- **Word-Level Matching:** Generates overlapping three-word sequences, known as trigrams, to identify matching text.
- **Custom Hash Table:** Implements a hash table from scratch using separate chaining for collision handling.
- **Similarity Calculation:** Calculates the percentage of uploaded-document words covered by matching trigrams.
- **Matched Text Details:** Displays matching text and its positions in the uploaded and reference documents.
- **DSA Algorithm Comparison:** Compares KMP and Rabin–Karp using match positions, comparison statistics, and execution time.
- **Multi-Reference Analysis:** Analyzes up to three retrieved reference papers and selects the paper with the highest similarity for the overall result.
- **Interactive Dashboard:** Presents the analysis results through a React-based user interface.
- **Fallback Reference:** Supports a locally stored `source.pdf` fallback when no reference paper can be obtained.

## Technology Stack

| Component | Technologies |
|---|---|
| Backend | Java |
| Frontend | React 18, JavaScript, JSX |
| Frontend Build Tool | Vite |
| PDF Text Extraction | Apache PDFBox |
| JSON Processing | Google Gson |
| Reference Paper Search | Semantic Scholar Graph API |
| HTTP Server | Java `com.sun.net.httpserver` |
| Data Structures | Custom hash table with separate chaining |
| Algorithms | KMP and Rabin–Karp |
| Development Environment | Visual Studio Code |

## Data Structures and Algorithms

### 1. Hash Table with Separate Chaining

A custom hash table stores word-level trigrams from each reference document. Each entry contains a trigram and the positions at which it occurs.

- **Hash function:** Uses a polynomial-style hash calculation with multiplier `31`.
- **Collision handling:** Uses separate chaining with linked-list nodes.
- **Indexing:** Stores trigram occurrences and their positions for efficient lookup.

### 2. N-gram Generation

The application tokenizes document text and generates overlapping word-level trigrams.

For example, the sentence:

`data structures improve search performance`

produces the following trigrams:

- `data structures improve`
- `structures improve search`
- `improve search performance`

These sequences are used to detect exact consecutive-word matches.

### 3. Knuth–Morris–Pratt (KMP)

KMP searches for patterns in reference text using a Longest Prefix Suffix (LPS) array. This helps avoid unnecessary repeated comparisons after mismatches.

### 4. Rabin–Karp

Rabin–Karp uses hash values to compare a pattern with successive text windows. A rolling hash updates the window hash, and character comparisons are performed when the hashes match.

Both algorithms are evaluated separately from the hash-table-based plagiarism score.

## How It Works

1. **Upload:** The user uploads a PDF through the PlagiaSense dashboard.
2. **Extract:** Apache PDFBox extracts the document's text.
3. **Process:** The text is converted to lowercase tokens, and significant words are used to construct a search query.
4. **Retrieve:** The Semantic Scholar API searches for relevant papers. Available open-access PDFs are downloaded and processed.
5. **Index:** The reference text is tokenized, and its trigrams are stored in the custom hash table.
6. **Analyze:** The uploaded document's trigrams are searched against the reference index, and matched word positions are marked.
7. **Calculate:** The system calculates the percentage of uploaded-document words covered by matching trigrams.
8. **Compare Algorithms:** KMP and Rabin–Karp are run to collect algorithm-comparison statistics.
9. **Display:** The dashboard presents the overall similarity score, matched text, reference papers, and DSA comparison results.

### Similarity Formula

The similarity percentage is calculated as:

**Similarity (%) = (Matched Words / Total Words) × 100**

This is a word-level trigram matching metric, not a semantic similarity score or a definitive determination of plagiarism. Paraphrased text and matches in documents outside the retrieved reference set may not be detected.

## System Requirements

- Windows, Linux, or macOS computer
- Java Development Kit (JDK) compatible with the project's Java 25 compilation target
- Node.js and npm
- Internet connection for Semantic Scholar searches and reference PDF downloads
- A valid Semantic Scholar API key configured as an environment variable

## Installation and Setup

### 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_PROJECT_DIRECTORY>
```

Replace the placeholders with your repository URL and project directory.

### 2. Configure the Semantic Scholar API Key

Set the `SEMANTIC_SCHOLAR_API_KEY` environment variable before starting the backend.

**Windows PowerShell:**

```powershell
$env:SEMANTIC_SCHOLAR_API_KEY="YOUR_API_KEY"
```

Replace `YOUR_API_KEY` with your own key. This PowerShell setting applies to the current terminal session.

Do not commit API keys or other secrets to GitHub.

### 3. Start the Java Backend

Open the backend source in your Java IDE or use the build and launch instructions provided with the project. Ensure that the required Java dependencies are available and start the `DashboardServer` application.

The backend is configured to serve the API on:

`http://localhost:8080`

Health endpoint:

`http://localhost:8080/api/health`

### 4. Start the React Frontend

Open a terminal in the frontend directory containing `package.json`, then run:

```bash
npm install
npm run dev
```

Vite typically displays the local development URL in the terminal. In this project, the dashboard is configured for:

`http://localhost:5173`

Ensure the backend is running before performing an analysis.

> **Note:** The precise backend build command depends on the build configuration and dependency files included in the repository.

## API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/health` | Checks backend availability |
| POST | `/api/analyze` | Receives a PDF for plagiarism analysis |

The analysis endpoint is used by the dashboard to submit the document and receive analysis results in JSON format.

## Results and Dashboard

The PlagiaSense dashboard presents:

- Overall similarity percentage
- Number of matched and total words
- Matched text and corresponding positions
- Reference papers analyzed
- KMP versus Rabin–Karp comparison statistics
- Analysis status and summary information

## Project Structure

The backend is organized into the following logical packages:

```text
Backend
├── dsa
│   ├── HashTable
│   ├── KMP
│   ├── RabinKarp
│   └── AlgorithmStats
├── processing
│   ├── TextProcessor
│   ├── NGramGenerator
│   ├── QueryBuilder
│   ├── PDFTextExtractor
│   └── PDFDownloader
├── search
│   └── SemanticScholarClient
├── plagiarism
│   ├── DocumentIndexer
│   ├── PlagiarismEngine
│   ├── MatchDetector
│   └── MultiSourceAnalyzer
├── model
│   └── Analysis and result models
└── server
    └── DashboardServer

Frontend
└── React application built with Vite
```

*This is a logical overview of the packages and classes documented in the project report; the actual directory layout may differ.*

## Limitations

- The analysis depends on the reference papers successfully retrieved and downloaded.
- Only a limited number of reference papers are analyzed per request.
- Exact word-trigram matching does not reliably detect paraphrasing or semantic similarity.
- The similarity percentage represents matching-word coverage, not a universal or institution-approved plagiarism score.
- The quality of PDF text extraction can affect matching accuracy.
- The local `source.pdf` fallback should be distinguished from papers retrieved through Semantic Scholar when interpreting results.

## Future Enhancements

- Support additional document formats, such as DOCX and TXT.
- Make the n-gram size, hash table configuration, and reference count configurable.
- Improve reference retrieval and document matching.
- Enhance matched-passage visualization and reporting.
- Improve handling of PDF extraction errors and unavailable references.

## Authors

**Project:** Word-Level Plagiarism Checker for Short Documents  
**Application:** PlagiaSense  
**Team:** 9

- M. Ananya
- P. Ruhitha

## Acknowledgements

- Semantic Scholar for research-paper discovery.
- Apache PDFBox for PDF text extraction.
- Google Gson for JSON processing.


