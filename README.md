---

# AI Agent for Automating Research

A lightweight Python agent that automates short-form research on any query. It searches the web, extracts relevant passages, ranks them using embeddings, and generates a concise extractive summary. Perfect for quick insights without reading dozens of articles.

---

## Features

* **Web Search:** Uses DuckDuckGo to fetch relevant pages without tracking.
* **Content Extraction:** Automatically extracts main text content from web pages, ignoring headers, footers, scripts, ads, and other noise.
* **Chunking:** Splits text into manageable passages for better embedding and ranking.
* **Embedding & Ranking:** Uses [SentenceTransformers](https://www.sbert.net/) to embed passages and rank them by relevance to your query.
* **Extractive Summarization:** Generates a short summary from the most relevant sentences across top passages.
* **Customizable Parameters:** Control search depth, number of passages, summary length, and embedding model.

---

## Installation

### Prerequisites

* Python 3.11+
* `pip`

### Install Required Packages

```bash
pip install beautifulsoup4 requests ddgs numpy sentence-transformers
```

> Optional warnings may appear for Jupyter users about `tqdm` or `ipywidgets`. These can be ignored if you are running the script outside Jupyter.

---

## Usage

### 1. Import the Agent

```python
from research_agent import ShortResearchAgent
```

### 2. Create an Agent Instance

```python
agent = ShortResearchAgent()  # loads the default embedding model
```

### 3. Run a Query

```python
query = "What causes urban heat islands and how can cities reduce them?"
result = agent.run(query)
```

### 4. Access Results

```python
# View top passages
for passage in result["passages"]:
    print(f"- Score: {passage['score']:.3f}")
    print(f"  Source: {passage['url']}")
    print(f"  Text: {passage['passage'][:200]}...\n")

# View extractive summary
print(result["summary"])

# View elapsed time
print(f"Query completed in {result['time']} seconds")
```

---

## Configuration Options

These constants can be modified to customize the agent:

| Parameter           | Default                                    | Description                                    |
| ------------------- | ------------------------------------------ | ---------------------------------------------- |
| `SEARCH_RESULTS`    | 6                                          | Max web pages to fetch per query               |
| `PASSAGES_PER_PAGE` | 4                                          | Max passages to extract from each page         |
| `EMBEDDING_MODEL`   | `"sentence-transformers/all-MiniLM-L6-v2"` | Sentence embedding model                       |
| `TOP_PASSAGES`      | 5                                          | Number of passages to rank and use for summary |
| `SUMMARY_SENTENCES` | 3                                          | Number of sentences in the extractive summary  |
| `TIMEOUT`           | 8                                          | Max seconds to wait when fetching a page       |

---

## How It Works

1. **Search the Web:** Queries DuckDuckGo via `ddgs` and retrieves URLs.
2. **Fetch & Clean Text:** Uses `requests` and `BeautifulSoup` to extract readable content.
3. **Chunk Passages:** Splits large text into smaller chunks (default 120 words).
4. **Embed & Rank:** Encodes passages and the query using `SentenceTransformer` and computes similarity scores.
5. **Extractive Summarization:** Selects top sentences from the highest-scoring passages to generate a concise summary.

---

## Example Output

```
Top passages:
- score 0.875 src https://evytor.vercel.app/blogs/urban-heat-islands-why-cities-feel-hotter
  phenomenon has far-reaching implications, impacting everything from energy consumption and air quality to human health...

- score 0.823 src https://www.siradel.com/urban-heat-island-effect-causes-and-solutions/
  way for cooler, greener and more resilient urban environments. Summary Understanding Urban Heat Islands...

--- Extractive summary ---
So, what exactly causes urban heat islands? (Source: https://evytor.vercel.app/blogs/urban-heat-islands-why-cities-feel-hotter)
Summary Understanding Urban Heat Islands What strategies can be used to mitigate urban heat islands effect? (Source: https://www.siradel.com/urban-heat-island-effect-causes-and-solutions/)
This article dives into the science behind urban heat islands, exploring the causes, impacts, and potential solutions...
--------------------------
Done in 30.6s
```

---

## Notes & Warnings

* Some web pages may block requests; a timeout ensures the agent doesn’t hang indefinitely.
* The extractive summary is based purely on similarity ranking; it does not perform abstractive summarization.
* If running in Jupyter, you may see warnings related to `tqdm` or `tensorflow`. These do not affect functionality.
* Ensure your machine has enough RAM to load the embedding model (`all-MiniLM-L6-v2` is lightweight, ~90MB).

---

## License

This project is MIT licensed.

---

## Future Improvements

* Add support for **multi-query batch processing**.
* Implement **abstractive summarization** using GPT-like models.
* Add caching of search results for faster repeated queries.
* Improve error handling for dynamically loaded web pages (JavaScript-heavy).

---
