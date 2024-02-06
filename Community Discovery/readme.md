# Community Detection

To further explore the relationship between different entities in the graph, CTIKG has a new community detection algorithm that uses the security information provided by the article model and the sentence models to achieve security-oriented community detection and filtering. It has ffollowing steps:

- Type-based Grouping. CTIKG checks each entity’s type assigned during the triple extraction step, and applies the Louvain algorithm on only the security-related entities to identify a group of entities that belong to the same type.
- Community Fusion. CTIKG merges the names of entities in each community into a single sentence, using the BERTencoder layer in the tactics model to compute theembedding vectors of the community. Then CTIKG computes the cosine similarities for each pair of vectors, and mergesthe community pairs whose vector similarity is greater than adefined threshold.
- Community Filtering. CTIKG applies the following rules to filter out the communities that are not closely security-related.
  
In the community detection step, CTIKG uses the following rules to filter the communities:
- Number of Source Articles: The source sentences of a community's edges must be from at least 2 articles.
- Type of Source Article: All the source articles of a community must be classified as CTI articles.
- Tactics Sentences: The source sentences of a community's edges contain at least three types of tactics.

# Evaluation
We evaluate the performance of the community detection algorithm by comparing the results with core expansion, Lpanni, Leiden, Umstmo, and Angle. The results show that CTIKG outperforms the other algorithms in terms of average articles related to community, average correlated security entity number, and precision of community edges. THe following table shows that CTIKG can more accurately detect the communities that are closely related to security. Also the community detected by CTIKG has more correlated security entities and articles than the other algorithms.

<table>
<thead>
  <tr>
    <th>Algorithm</th>
    <th>Avg. Size</th>
    <th>Avg. Edge</th>
    <th>Avg. Articles</th>
    <th>Avg. Correlated Security Entity</th>
    <th>Precision</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>CTIKG</td>
    <td>4.84</td>
    <td>5.769</td>
    <td>5.15</td>
    <td>2.07</td>
    <td>78.56%</td>
  </tr>
  <tr>
    <td>Angel</td>
    <td>8.8</td>
    <td>14.8</td>
    <td>4.8</td>
    <td>2.2</td>
    <td>41.16%</td>
  </tr>
  <tr>
    <td>Umstmo</td>
    <td>20.0</td>
    <td>19.0</td>
    <td>13.0</td>
    <td>1.0</td>
    <td>27.05%</td>
  </tr>
  <tr>
    <td>Core </td>
    <td>8.15</td>
    <td>7.375</td>
    <td>2.08</td>
    <td>0.07</td>
    <td>8.00%</td>
  </tr>
  <tr>
    <td>Leiden</td>
    <td>16.55</td>
    <td>16.19</td>
    <td>3.55</td>
    <td>0.08</td>
    <td>18.00%</td>
  </tr>
  <tr>
    <td>Lpanni</td>
    <td>7.73</td>
    <td>6.90</td>
    <td>1.92</td>
    <td>0.03</td>
    <td>10.00%</td>
  </tr>
</tbody>
</table>

