## Update: 1st Aug 2018
- DGEM
	- DGEM model: Ran for all the questions and collected the results in a [spreadsheet](https://docs.google.com/spreadsheets/d/151zuO4OEE7Z1zyyDnMPC5DXp-aeJ31ROvm_7-edUVa8/edit?usp=sharing).
	- Going through each question and the corresponding options and support manually.
	- Check if analysis is similar to the analysis done in KG<sup>2</sup> paper.

- New Paper [KG<sup>2</sup>](https://arxiv.org/pdf/1805.12393.pdf)
	- Claims: 31.70% on challenge set.
	- Claims: 51% difficulty in choosing correct answer is due to insufficient support.
		- **_Can use definitions here along with the provided dataset._**
		- **_Use something other than elastic search to find bettersupport sentences._**
	- Source code: One of the authors of the paper replied that **The current paper is preliminary, and they are still working on it. They'll release the code after they submit a formal paper to some venue.**

## Update: 27th July 2018
- Source code is not available for: [KG<sup>2</sup>: 31.70%](https://arxiv.org/pdf/1805.12393.pdf) paper.
- ~~Its been 3 days since I contacted the authors asking whether they are planning to opensource the code or not.~~
- ~~Haven't received any reply yet~~
- ~~Meanwhile running DGEM-OpenIE~~

## Update: 23rd July 2018

### New Paper Published: [KG<sup>2</sup>](https://arxiv.org/pdf/1805.12393.pdf)

- Best model yet! - Claims 31.70% on challenge set.


	|Method | Test Scores|
	|------ | -----------|
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


- Based on **Contextual Knowledge Graph Embeddings.**
- Accuracy on Easy Set not known yet.

&nbsp; **Ideas**

- Wordnet
	- connects words from same POS.
	- Can we use it to improve the edge embedding learned in DGEM?
	- Convert Hierarchial Connections to edge embedding? How?
	- How much scince is in dataset?

- [Universal Language Model Fine-tuning for Text Classification](https://arxiv.org/pdf/1801.06146.pdf)
	- These Fine Tuning Techniques may be helpful (Like Transfer learning in CV)

**TODOs**

- [X] Understand allenNLP API (ongoing)
- [X] Debug the remaining two neural models (ongoing)
- [X] Read WordTree A corpus of Explanation Graphs 
- [ ] Go through the code of DGEM paper to better understand the architecture
- [ ] See if the errors can be reduced using WordNet, WebChild or changes in the architecture.


## Update: 16th July 2018
Back to work
## June 15 to June 29
Family Trip
## June 05 to June 12
Went to hometown

### Update: 30-05-2018
### TL;DR

**1. Papers Read**

- [x] Information Retrieval 
- [x] PMI
- [x] Table ILP 2016
- [x] Tuple Inference 2017
- [x] Decompositional Attention 2016
- [x] BiDAF 2017
- [x] DGEM 2018: **Model is complicated. Need to go thorugh the code to understand it better**
- [ ] WordTree A corpus of Explanation Graphs

Note: Available code is implemented using allenNLP and elasticsearch libraries (So, need to learn these frameworks to avoid re-implementing neural models)

**2. Models/Error Analysis**

- Code/Model only available for Neural Models: Decompositional, BiDAF and DGEM
- Even after spending considerable time Couldn't Run the models on my laptop (4 GB Memory wasn't enough)
- ~~Was facing some connectivity issues on the DAIR lab machine~~
- Connection Issue on DAIR machine is now resolved, ~~Will try to run models on it~~.
- Tried models, ran into pytorch errors, probably because code written in older version of pytorch.
- ~~Need access to servers!~~

---

### Few Benchmark queries (apart from the ones given [here](http://ai2-website.s3.amazonaws.com/publications/AI2ReasoningChallenge2018.pdf))

- **(Factual)**<br/>
Which property of air does a barometer measure?<br/>
a) pressure b) speed c) humidity d) temperature

- **(Semantic Relation)**<br/>
What is one way to change water from a liquid to a solid?<br/>
a) Decrease Temp b) Increase Temp c) Decrease Mass d) Increase Mass

- **(Parallel Evidence)**<br/> 
Sleet, rain, snow and hail are forms of<br/>
a) Erosion b) evaporation c) groundwater d) precipitation

- **(Perturbed Questions/Options)**<br/> 
In New York State, the longest period of daylight occurs during which month?<br/>
a) Eastern b) June c) history d) years.

- **(Multifact Reasoning)**<br/>
Which object in our solar system reflects light and is a satellite that orbits around one planet?<br/>
a) Earth b) Mercury c) Sun d) Moon

- **(Missing Important Words)**<br/>
Which material will spread out to **completely** fill a larger container?<br/>
a) air b) ice c) sand d) water

- **(Bad Alignment) - CO<sub>2</sub> aligned with breathe out**<br/>
Which of the following gases is necessary for humans to breathe in order to live?<br/>
a) Oxygen b) Carbon dioxide c) Helium d) Water vapor

---

### Info from various Papers 

- **q** -> question
- **a<sub>i</sub>** -> ith option (answer)

### BaseLine Solvers to divide dataset

#### 1. Information trieval

- Waterloo corpus 10<sup>10</sup> tokens (in original paper) [ARC dataset for this]
- query: *q + a<sub>i</sub>* -> Search Engine (Elasticsearch) -> top retrieved sentence **s** + score
- s should have at least one non-stopword overlap with q, and at least one with ai to ensures s has some relevance to both q and a<sub>i</sub> .
- repeat for all a<sub>i</sub> and report a<sub>i</sub> with highest score

#### 2. PMI

- Waterloo corpus
- Extract n-grams from q. 
- Calculate average PMI of each a<sub>i</sub> over all n-grams.
- Report a<sub>i</sub> with largest PMI


### Easy set vs Challenge Set

Challenge questions are answered incorrectly by both of the baseline solvers.

Basically removes most of the factoid questions(more likely to be present in the corpus)


### Other Non Neural Solvers


#### 1. Table ILP 2016, Score: 26.97
	
- ILP over semi structured knowledge base (Tables).
- components of q, a<sub>i</sub>, table_celss, headers -> nodes of graph.
- edge weights: similaity and entailment using WordNet. [Tuple Inference paper claims that this gives unreliable scores on longer phrases]
- find subgraph that **best** supports an option.
- **best** -> ILP formalism representing structural and semantic constraints.
- solve ILP -> using SCIP

&nbsp; **Updates**

- Table created manually + query based interactive automatic table building tool.
- Table of Subject verb object extracted using OpenIE.
- Edge denotes equality between nodes.
- Equality: Phrase Level entailment -> accounts for generalization, and lexical variability

&nbsp; **Problems/Doubts**

- Model not available
- Manually defining table schemas!! -> Time consuming
- ~~IKE, Open IE (SVO)?~~
- penalizing evidence chaining..Good or Bad?
- Irrelevent Evidence chaining may bring noise

#### 2. Tuple Inference 2017, Score: 23.83

- Tuple: (subject; predicate; objects)
- Generation of tuples: Elasticsearch (q, a<sub>i</sub>) on trainset. Open IE on top 200 hits??
- (q, <sub>i</sub>) from test set. take top 50 tuples based on tf-idf scoring.
- q, a<sub>i</sub>, tuples -> vertices
- Rest similar to TableILP

&nbsp; **Problems/Doubts**

- Model Not available
- Open IE?? (lossy nature? use jaccard score?)
- Conversion to tuples may cause loss of important bits of information.

### Neural Solvers

**Need to go through their codes to better understand the architecture**

#### 1. Deompositional Attention 2016, 24.34

&nbsp; **Adaptation**

- q + a<sub>i</sub> = hypothesis(h<sub>i</sub>). 
- Use h<sub>i</sub> as search query to retriev sentences(elasticsearch?) t<sub>ij</sub>.
- Calculate entailment scores between h<sub>i</sub> and t<sub>ij</sub>
- Report a<sub>i</sub> with maximum overall entailment score.

&nbsp; **Original Idea**

- Sentence a, Sentence b as input
- Output y belongs to {**E**ntailment, **N**eutral, **C**ontradiction}
- Each word is a pure embedding (GLOVE) or augmented with intra sentence attention.
- Attention (soft alignment matrix) -> Comparison -> Aggregation ->  output y

#### 2. BiDAF: Direct Answer System 2017, 26.54

&nbsp; **Adaptation**

- Create paragrph of bunch of retrieved sentences for q
- Apply BiDAF to find a answer span for q(context) from this paragraph
- pick a<sub>i</sub> that maximally overlaps with this answer span

&nbsp; **Original Idea**

- Hierarchial Architecture
	- Character Embedding Layer: **Char CNNs**
	- Word Embedding Layer: **Pre-trained word Embedding Model**
	- Contextual Embedding Layer
	- Attention Layer
	- Modeling Layer: **RNN**
	- Output Layer 

#### 3. DGEM 2018, 27.11

&nbsp; **Adaptation**

&nbsp; Same as Decompositional Attention

&nbsp; **Original Idea**

&nbsp;&nbsp;**Incorporate semantic and structural knowledge into model explicitly using graphical structure (similar to Tuple ILP)**

- Intution: Knowledge **--entails-->** (q, a<sub>correct</sub>)
- Create entailment dataset of above form.
- Three types of data point: Complete Entailment, Partial Entailment, Unrelated
- Uses OpenIE tuples and graph similar to Tuple Inference.
- edges: OpenIE tags, prepositions
- Node Attention: attention of the words in the node over the words in the premise. softmax of (dot product of embeddings)
- Each node represented using attention weighted LSTM
- Learn embedding for each edge label
- Probability calculation?

&nbsp; **Other Problems/Doubts**

- Manual creation of entailment dataset 
- Find Type of entailment. Done manually using Amazon Mechanincal Turk
- Manual convesion of every (q, a<sub>i</sub>) to entailment form.
