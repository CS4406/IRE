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
Understanding design choices, their time and cost implications, necessity of wide-area systems and partitioning, and impact on consistency and avaialability. This is to help understand that there is no one-size-fits-all and inevitably need to tailor solutions (like suitable data stores) for specific usecases (including IR and different subproblems within) and do so being consious of the trade-off involved and how we manage undesirable outcomes. We covered examples including deep-dive of latency budget when serving cross-continent requests that include disk seek+read and network trip of the data. Challenges even with simple applications serving diverse use-cases like transaction consistency with a primary DB versus high availability with a cache layer and warehouse for batch-query analytics. Example of local master as in Yahoo vs remote master as in Facebook from [1]. Brief introduction to consistency and availability trade-offs in the presence of such partitions, reasoning through the choices with invariants and operations with examples like collaborative editors, auto-teller machines, etc.
1. Main reading material: CAP Twelve Years Later: How the "Rules" Have Changed
1. Additional reading:
   1. Jeff's talks on Google's early evolution: [here](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/stanford-295-talk.pdf) and [here](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/Stanford-DL-Nov-2010.pdf).
## 4. Data store
Discusses the challenges with storing and accessing large volume of data including latencies and costs across disks (HDDs and SSDs). The necessity of clever indexing, trade-offs with B-Trees and LMS-Trees. Understanding design with attention to the undelying hardware (disks, network, faults, etc), for example, sequential read/write to make access faster, using log-structured writes and avoiding random access, etc. We looked at B-Trees and LSM-Trees and how they fare on various performance metrics (read/write throughputs, latency crash recovery, concurrency, lookups vs rangesearch, write patterns (random vs sequential, amplification, compression, disk fragmentation).
1. Main reading material: Section 3.6 `SEIRP` and Chapter 3 of `DDIA`
1. Additional references
   1. [The ubiquitous B-Tree](https://carlosproal.com/ir/papers/p121-comer.pdf)
   1. [The Log-structured Merge-tree](https://www.cs.umb.edu/~poneil/lsmtree.pdf)
   1. [Bigtable: A Distributed Storage System for Structured Data](https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)
## 5. Deduplication - minhash
Capturing set similarity with Jaccard measure, using shingling for set representation of documents, understanding the cost of naive deduplication at scale with space (50KB docs taking up 500KB space for 10-shingles) and time (quadractic complexity of pairwie comparison). Using minhash-signatures to generate compressed representation of documents (250 length signatures cutting down space to 1KB, 500x improvement). Understanding the estimation and why it is an unbiased estimator of Jaccard similarity and the choice of signature length.
1. Main reading: Section 3.7 `SEIRP` and Chapter 3 of `MMDS`
## 6. Deduplication - LSH
Recap of minhash, banding technique to reduce quadratic complexity down for all pairs comparison, further filtering for high-similarity matches. General theory of Locality sensitive hashing and simhash. Quick bridef on relations to rounding for approximation in optimization, Jonhson-Lindenstrauss for length preserving random projections, and (p1,p2,d1,d2)-sensitivity. Applications of data sketching to different use-cases like association rule mining, clustering at web scale, set membership (bloom filters), etc.
1. Main reading: Chapter 3 of `MMDS`
2. Additional reading
   1. On the resemblance and containment of documents. Broder, Andrei Z.
   1. Similarity Estimation Techniques from Rounding Algorithms. Moses S Charikar.
   2. Detecting Near-Duplicates for Web Crawling. Gurmeet Singh Manku, Arvind Jain, and Anish Das Sarma.
   3. Optionally Chapter 4 of `MMDS` for data sketching.
## 7. Text processing
Query observes the true user intent through an noisy channel that could corrupt it for various reasons like user's language limitation, etc. We receive feedback on relevance post-hoc only after the user acts on the presented results. In order to retrieve the relevant docs for a given intent, they should all be indexed against the corresponding query tokens that requires pre-processing of text that apprears in the documents. Pre-processing of crawled text to prepare it for indexing that includes extracting informative words (less-frequent vs stopwords), word variations (misspellings, stemming, capitalization, hyphes, apostrophes, etc), common bigrams and phrases, information extraction (names entities, POS tags, and also different data types like dates in all its variations, locations, etc), and varying importance of word depending on where it appears and how many times. Disucssion also on using Zipf's and Heap'slaws for query result size estimation, determining efficient order of conjuctions, etc. Use of artificial vocabulary induced to leverage lexical indexing for retrieving semantics and concepts. Necessity to work backwards from ranking to pre-compute all relevant information to store in the index for retrieval at the time of query processing. A brief of emerging challenges; a sense of the large space of rankings and inevitable underexploration of this space owing to how the lifecycle is structured, and the multi-party view of platform as opposed to the traditional two-party study (and looking at spamming, search engine optimization, digital ads, jobs, etc).
1. Main reading: Chapter 4 `SEIRP`
## 8. Link analysis
Among the many features that determine the relevance of a document to a query (most of which could be context specific) there are some generic ones like quality of the webpage. High quality pages are reliable sources of information while poor ones are either outdated copies, misleading sites or spam/click-baits. PageRank is one of the earliest algorithms with similar ones like HITS invented to determine the quality of a page. We discuss graph structure of the web, applying PageRank to determine page quality, and distributed implementation amenable for MapReduce.
1. Main reading Section 4.5 of `SEIRP` and Chapter 5 of `MMDS`.
## 9. Indexing
Term-doc incidence matrix and storing it as inverted-index with postings list for each term. Supporting various queries (OR, AND, NOT, PHRASE, INFIELD) on the index using running merge operation on the retrieved postings list. We also looked at how a two 1MB posting lists could take about 0.05s to to read from the disk while it only takes 0.001s (50x faster) to run a merge operation for them on a 5GHz processor. Correposndingly, we could process much more data if only we could improve the IO rate even if at the cost of compute speeds and compression help us leverage this trade-off.
1. Main reading: Chapter 5 `SEIRP`
## Lecture 10
The exact choice of compression algorithm depends on the specific IO and processor speeds or rather the whole hierarchy of disk, network, memory and compute rates. We discussed delta-encoding to make it amenable for compression, using unary and Elias-$`\gamma,\delta`$ bit-aligned codes and v-byte byte-aligned codes. We the look into list skipping and necessity of skip pointers to have binary search working with compression and d-gaps. Further, using word lists index to quickly seek in the relevant terms and the corresponding files and also retrieving the relevant text blurb for display. 
We next look at index construction and the utility of structuring the processing as map-reduce. Updating indexes using index-merging and result-merging. The term-at-a-time and document-at-a-time processing of queries. Various optimizations like list-skipping, conjuctive processing, dynamically/statically setting thresholds $`\tau`$ for chopping out documents/postings that are unlikely to score high enough to be in top=$`k`, and the importance of ordering. A note on structured queries, distributed evaluation and caching.
1. Main reading: Chapter 5 `SEIRP` and Chapter 2 of `MMDS`
