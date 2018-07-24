## Notes on the paper "KG2: Learning to Reason Science Exam Questions with Contextual Knowledge Graph Embeddings"

[Link to paper](https://arxiv.org/pdf/1805.12393.pdf)

|Method | Test Scores|
|:-----:|:----------:|
|KG<sup>2</sup> | 31.70|
|TableILP | 26.97|
|BiDAF | 26.54| 
|DGEM-OpenIE | 26.41 |
|			 |(27.11 using some propiretry parser) | 
|Guess-all / Random | 25.02|
|DecompAttn | 24.34|
|TupleInference | 23.83|
|IR-Google | 21.58|
|IR-ARC | 20.26|

- Using Google Search API to select the answer option with most hits doesn't even crosses the Guess-all/Random baseline. 
- TODO: Does this mean collecting more senetences won't help?
- Hypothesis *h<sub>i</sub> = question + a<sub>i</sub>*
- Knowledge Graph
- Graph Embedding
- Graph Ranking Problem?

### Approach

&nbsp; **1. Generating Hypothesis**

- h<sub>i</sub> : ith hypothesis
- q<sub>i</sub> : ith question
- c<sub>i</sub><sup>j</sup> : jth option for ith question
- a<sub>i</sub> : correct option for ith question 
- h<sub>i</sub> = q<sub>i</sub> + c<sub>i</sub><sup>j</sup>
- automatically generate hypothesis by substituting wh-words in questions with candidate option

&nbsp; **2. Searching Potential Supports**

- ElasticSearch: hypothesis as query to corpus
- Top 20 (filtered clean) sentences are used as potential support sentence.

&nbsp; **3. Constructing Knowledge Graphs**

- Open IE to extract triples from each support sentence.
- Triples: T(subj, predicate, obj)
- Collect them to construct a contextual knowledge graph.
- For e.g. **he** <-subj- **play** -ball-> **ball*
- Create Hypothesis Graph (G<sup>hypo</sup>) and Support Graph(G<sup>supp</sup>)


&nbsp; **4. TODO: Learning with graph embeddings?**

- Choosing right answer becomes a Graph Ranking Problem *How?*
- Graph Scoring Function f: G<sup>hypo</sup> X G<sup>supp</sup> --> R should give highest score to correct pair?
- Point wise ranking objective?
- Read Graph Embeddings



### Analysis

![](difficulty_distribution_in_ARC.png)

- Estimated upper bound: Correctly answer all learnable questions, and randomly guess others: **36.25%**. If this is the case curating more data might help.
- 51% difficulty is due to insufficient support. Use different method to find support sentences?
- May be try better method to extract triples? Sentence Parsing instead of Open IE?