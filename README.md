# [WIP] Information Rretrieval and Extraction

The course initially covers various methods to crawl, index and query text documents for information. 
It introduces the challenges with designing and building such a system, various techniques from literature and the involved trade-offs.
Initial lecture will focus on enabling lexical search on text data with inverted-indexing while later lectures will dwell into semantic search including non-textual data.
At about mid-semester after discussing methods to rank of candidates and evaluation of results, it will get into relevant aspects practice of IR systems including A/B-testing.
Subsequqently, it will cover various recommender systems that require retrieving information without specific queries and the various applications of the discussed concepts to different use-cases.
The course it intended to be a breadth course to introduce its students to the application of various computer science, engineering and machine learning methods to the building of retrieval systems and the challenges in their practice.
The following will serve as central reference matetial across lectures with additional references shared for inidividual ones as appropriate.
1. `SEIRP`: Search Engines. Information Retrieval in Practice. W. Bruce Croft, Donald Metzler, and Trevor Strohman.
2. `MMDS`: Mining Massive Datasets. Jure Lescovec, Anand Rangarajan, and Jeffrey David Ullman.
3. `DDIA`: Designing Data-Intensive Applications. Martin Kleppmann.


## 1. Introduction
Course overview with what is expected, which topics are covered and that which is omitted, grading, projects, etc.
## 2. IR system overview
Challenges with enabling search at scale, undestanding engineering (latency through-puts, etc) and functional parameters (relevance). 
Indexing and querying phases, evaluation of IR system (offline/online), deployment with A/B testing, varying requirements with use-cases. 
We also looked at data crawling including references to focussed crawling and modeling age of cache and challenges with data volume and duplicates.
1. Main reading material: Chapter 1 and parts of chapters 2 & 3 from `SEIRP`.
## 3. IR system design
Understanding time and cost of various design choices, necessity of wide-area and partitioning, impact on consistency and avaialability.
1. Main reading material: CAP Twelve Years Later: How the "Rules" Have Changed
1. Additional reading:
   1. Jeff's talks on Google's early evolution: [here](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/stanford-295-talk.pdf) and [here](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/Stanford-DL-Nov-2010.pdf).
## 4. Data store
Discusses the challenges with storing and accessing large volume of data including latencies and costs across disks (HDDs and SSDs). The necessity of clever indexing, trade-offs with B-Trees and LMS-Trees.
1. Main reading material: Section 3.6 `SEIRP` and Chapter 3 of `DDIA`
1. Additional references
   1. [The ubiquitous B-Tree](https://carlosproal.com/ir/papers/p121-comer.pdf)
   1. [The Log-structured Merge-tree](https://www.cs.umb.edu/~poneil/lsmtree.pdf)
   1. [Bigtable: A Distributed Storage System for Structured Data](https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)
## 5. Deduplication - minhash
Capturing document similarity with Jaccard measure, shingling, cost of naive deduplication at scale, its estimator based on minhash-signatures.
1. Main reading: Section 3.7 `SEIRP` and Chapter 3 of `MMDS`
## 6. Deduplication - LSH
Banding technique to reduce quadratic complexity for all pairs comparison, locality sensitive hashing with simhash and relations to optimization.
1. Main reading: Chapter 3 and 4 of `MMDS`
2. Additional reading
   1. On the resemblance and containment of documents. Broder, Andrei Z.
   1. Similarity Estimation Techniques from Rounding Algorithms. Moses S Charikar.
   2. Detecting Near-Duplicates for Web Crawling. Gurmeet Singh Manku, Arvind Jain, and Anish Das Sarma.
## 7. Text processing
Pre-processing of crawled text including extraction and link analysis to prepare it for indexing.
1. Main reading: Chapter 4 `SEIRP`
