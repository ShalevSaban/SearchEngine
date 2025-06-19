# 🔍 Mini-Google – Distributed Search Engine with Kafka & DFS Crawling

This project is a simplified, distributed search engine – similar in concept to Google – built using depth-first crawling, batch processing, and modern big-data tools like **Kafka** and **Elasticsearch**.

---

## 📌 Overview

Mini-Google starts from a given URL, crawls the web recursively using a **DFS-based strategy**, extracts all the links from each page, and indexes the content.

Each crawled page is processed asynchronously using **Kafka**, and indexed for search using **Elasticsearch**. The architecture supports scaling by breaking the crawling and indexing into parallel, isolated jobs using **MapReduce-like batching**.

---

## ⚙️ Technologies Used

| Technology       | Purpose |
|------------------|---------|
| 🧭 DFS Crawling   | Recursively visit web pages and extract links |
| 🧵 Kafka          | Message queue for decoupled crawling & indexing |
| 🗃️ Map-Reduce     | Split crawling/indexing into distributed jobs |
| 📥 Read / Write   | Persist raw HTML and indexable data |
| 🔎 Elasticsearch  | Index and search crawled pages |
| ☁️ Deployment     | Can be deployed across multiple containers/nodes |

---

## 🔁 System Flow

1. **Start from a seed URL** provided by the user.
2. **Crawl the page** using DFS:
   - Fetch HTML
   - Extract `<a href="">` links
   - Send each discovered URL to Kafka
3. **Kafka consumers** handle crawling jobs in parallel:
   - Fetch and clean the content
   - Extract keywords and metadata
4. **Processed content** is written into **Elasticsearch** as documents.
5. Users can then **search** any keyword via REST API or UI → results are fetched from the Elasticsearch index.

---

## 🧪 Example Flow

**User provides seed URL:**
```
https://example.com
```

**What happens:**
- Crawler visits the page and finds 5 links.
- Each link is added as a message in Kafka topic `crawl-requests`.
- Multiple consumers process those links concurrently.
- Cleaned page data is transformed and stored in Elasticsearch.

**Search request:**
```
GET /api/search?query=machine+learning
```

**Search response (from Elasticsearch):**
```json
[
  {
    "title": "Introduction to Machine Learning",
    "url": "https://example.com/ml",
    "snippet": "Machine learning is a field of AI that focuses on..."
  },
  ...
]
```

---

## 🏗️ Architecture

- **Producer (Crawler)** → reads pages, extracts links → sends URLs to Kafka.
- **Kafka Topic (`crawl-requests`)** → holds URLs waiting to be processed.
- **Consumers** → fetch URLs from Kafka, download + parse HTML, clean content.
- **Elasticsearch** → indexes cleaned data with metadata and keywords.
- **REST API / UI** → exposes search functionality over indexed content.

---

## 🚀 Deployment

This system is designed for distributed environments:

- Each crawler, processor, and indexer can run in its own container or VM.
- Kafka ensures fault-tolerant communication between stages.
- Elasticsearch handles horizontal scaling of indexing and querying.

---

## 🧠 Future Enhancements

- [ ] Add page rank scoring
- [ ] Detect and avoid duplicate content
- [ ] Handle robots.txt and rate limiting
- [ ] Visual frontend for query results
- [ ] Full-text indexing with NLP enrichment

---

## 🔗 GitHub Repository

[📂 ShalevSaban/SearchEngine](https://github.com/ShalevSaban/SearchEngine)
