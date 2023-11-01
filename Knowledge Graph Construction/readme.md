Knowledge Graph Construction

## Introduction 
CTIKG can utilizes the article classification model to extract CTI articles from articles, applies NLP techniques to extract entities and their relations described in the CTI sentences, and then match the entities to join the relations, forming the knowledge graph. 
In the matching step,the Levenstain distance is used to compute string distances, which is defined as:
![Lev(s1, s2)](https://latex.codecogs.com/svg.latex?%5Cdpi%7B300%7D%20Lev%28s_1%2C%20s_2%29%20%3D%20%5Cbegin%7Bcases%7D%20%20%20max%5C%7B%7Cs_1%7C%2C%20%7Cs_2%7C%5C%7D%20%26%20%5Ctext%7Bif%20%24%7Cs_1%7C%3D0%24%20or%20%24%7Cs_2%7C%3D0%24%7D%5C%5C%20%20%20Lev%28s_1%5B2%3A%5D%2C%20s_2%5B2%3A%5D%29%20%26%20%5Ctext%7Bif%20%24s_1%5B0%5D%20%3D%20s_2%5B0%5D%24%7D%5C%5C%20%20%201%20%2B%20min%20%5Cbegin%7Bcases%7D%20%20%20%20%20%20Lev%28s_1%5B2%3A%5D%2C%20s_2%29%20%5C%5C%20%20%20%20%20%20Lev%28s_1%2C%20s_2%5B2%3A%5D%29%20%5C%5C%20%20%20%20%20%20Lev%28s_1%5B2%3A%5D%2C%20s_2%5B2%3A%5D%29%20%5C%5C%20%20%20%5Cend%7Bcases%7D%20%26%20%5Ctext%7Botherwise%7D%20%5Cend%7Bcases%7D)
    
Then, $CTIKG$ computes the relative Levenstein distance as:

![RLev(s1, s2)](https://latex.codecogs.com/svg.latex?RLev_{(s_1,s_2)}=\frac{Lev_{(s_1,%20s_2)}}{max\{|s_1|,%20|s_2|\}})

CTIKG merges two entities if their entity types are the same and their text representations meet the following conditions:

- Text similarity: The relative Levenshtein distance is below 0.1.
- Semantics similarity: The cosine similarity of the mean Word2Vec embedding is above 0.7.

To further explore the relationship between different entities in the graph, CTIKG has a new community detection algorithm that uses the security information provided by the article model and the sentence models to achieve security-oriented community detection and filtering. It has ffollowing steps:

- Type-based Grouping. CTIKG checks each entity’s type assigned during the triple extraction step, and applies the Louvain algorithm on only the security-related entities to identify a group of entities that belong to the same type.
- Community Fusion. CTIKG merges the names of entities in each community into a single sentence, using the BERTencoder layer in the tactics model to compute theembedding vectors of the community. Then CTIKG computes the cosine similarities for each pair of vectors, and mergesthe community pairs whose vector similarity is greater than adefined threshold.
- Community Filtering. CTIKG applies the following rules to filter out the communities that are not closely security-related.
  
In the community detection step, CTIKG uses the following rules to filter the communities:
- Number of Source Articles: The source sentences of a community's edges must be from at least 2 articles.
- Type of Source Article: All the source articles of a community must be classified as CTI articles.
- Tactics Sentences: The source sentences of a community's edges contain at least three types of tactics.
  
## Evaluation
The sample inspection shows that the entity detection model can correctly extract 76.06% of triples containing a specific CVE name, 71.09% of triples containing a specific malware name, and 72.72% of triples containing an APT group name.
For the community detection, We also use a score called Comm_Sec to evaluate the security-relatedness of the communities. The score is defined as:

![score](https://latex.codecogs.com/svg.image?Comm_{Sec}=(R_{sec}+R_{tac}+R_{beh})*(\frac{R_{com}}{E(R_{com})}%20)*\sqrt{\frac{M}{E(M)}%20}*\frac{E(S)}{E(S)+\left%20|\bar{S}%20-E(S)%20\right%20|%20})

Where $R_{sec}$ denotes the proportion of nodes being security-related entities in the community, $R_{tac}$ denotes the proportion of edges in the community that is related to tactics sentences, $R_{beh}$ denotes the proportion of edges in the community that is related to behavior sentences, $R_{com}$ denotes the proportion of security-related communities through manual inspection, $M$ denotes the Erdos-Renyi Modularity, $\bar{S}$ denotes the average size of communities, $E(S)$ denotes the expected size of communities, $E(M)$ denotes the expected value of Erdos-Renyi Modularity, and $E(R_{com})$ denotes the expected proportion of security-related communities.

The random sample inspection shows that the result from CTIKG contine more security-related entities than the result from the other community detection algorithm. 

<p align="center">
  <b> Figure: Comparison with other community detection algorithms </b>
</p>

| Algorithm         | Security Community % | Erdos-Renyi Modularity | Avg. Community Size | Security Entity % | Behavior Edge % | Tactic Edge % | Comm_Sec |
|-------------------|----------------------|------------------------|---------------------|-------------------|----------------|---------------|----------|
| CTIKG (with filter) | 60.00%               | 0.36                   | 6.83                | 33.49%           | 31.20%         | 43.88%       | 1.08     |
| CTIKG (no filter)   | 6.00%                | 0.32                   | 6.78                | 7.68%            | 5.91%          | 23.47%       | 0.03     |
| Core Expansion     | 46.67%               | 0.34                   | 8.47                | 49.69%           | 24.74%         | 27.63%       | 0.65     |
| Graph Entropy      | 40.00%               | 0.48                   | 1.45                | 7.40%            | 3.80%          | 4.20%        | 0.07     |
| Lpanni             | 33.33%               | 0.39                   | 6.99                | 49.26%           | 25.59%         | 28.62%       | 0.61     |
| Slpa               | 33.33%               | 0.40                   | 24.57               | 50.62%           | 22.53%         | 25.11%       | 0.17     |
| umstmo             | 0.00%                | 0.41                   | 1.68                | 0.03%            | 0.02%          | 0.02%        | 0.00     |
| ebgc               | 16.66%               | 0.35                   | 5.49                | 9.89%            | 4.13%          | 4.69%        | 0.04     |

## Workflow
1. Run the *Entity Extraction.ipynb* to extract triples and their relationship from articles and save them as pkl files.
2. Run the *Graph Generation.ipynb* to generate overall graph, and a optional graph that only contains CTI article's triples.
3. Run the *Community Detection.ipynb* to perform the CTIKG community detection algorithm.
