# CTI Sentence Identification

The first step of CTIKG is to train classification model and automatically obtain different types of CTI sentences from CTI articles using NLP model. We use the python library **simpletransformers** to perform model training and inference.

## Workflow
1. Manually label Cyber threat sentences from selective articles, to create initial **CTI_sentences.xlsx**.
2. Utilize **CTI Sentence Identification Model Training.ipynb** to train an initial sentence text classification model. For the specific Cyber tactic where model show poor performance, use **Active learning uncertainty sampling.ipynb** to selectively extract sentences from all articles, label them and update **CTI_sentences.xlsx** file.
3. After repeated iterations of the active learning, utilize **CTI Sentence Identification Model Training.ipynb** to train high performance sentence text classification models.

## Introduction
The file **CTI_sentences.xlsx** contains sentences from CTI articles for the purpose of training of the classification model and following evaluation. The 'Sentence' column contains the sentence text, the 'Tactics' column contains the sentence's Cyber attack tactic label, and the 'Behavior' column contains the sentence's Cyber attack behavior label.

We labeled every sentences of the 672 article from ATT&CK knowledge base, and got 8,408 sentences. Then, we use active learning to label 9,012 sentences from 5,110 CTI articles. The  active learning uses uncertainty sampling method to select the sentences. The uncertainty score is calculated by Classification Uncertainty, Classification Margin, and Classification Entropy. For $n$ tactics, the model accepts a sentence $s$ as input and predicts a probability (ranging from 0 to 1) for each tactics, denoted as $p_1, \ldots, p_n$. 

The Classification Uncertainty is defined as: $p_{max}$ and 1, i.e., $1-p_{max}$, which computes the difference between the highest probability value.

The Classification Margin is defined as: $p_{max-1}$, i.e., $p_{max}$ - $p_{max-1}$, which computes the difference between the highest probability value $p_{max}$ and second highest value $p_{max-1}$.

The Classification Entropy is defined as: $-\sum\limits_{i=1}^{n}\left(p_{i}\log_{2}{p_{i}}\right)$, which computes the information entropy of all probability values.

    
In total, 17,420 sentences are labeled with the Cyber attack tactics. Among the 17,420 sentences, we further labeled 1,023 sentences describing attack behavior and 940 sentences not describing attack behavior as the negative samples. The following figure and table shows the distribution of the tactic labels and behavior labels in dataset. 

<p align="center">
  <b> Figure: Distribution of Attack Tactics </b>
</p>
<p align="center" style="background-color: white;">
  <img src="https://i.imgur.com/9cThmEe.jpg" style="border: 10px solid white;">
</p>


<b> Table : Frequency Distribution Of Attack Tactics </b>
| Attack Tactics Distribution In Dataset 	| Sentences Count 	|
|----------------------------------------	|----------------:	|
| Sentences with 0 attack tactic         	|            4022 	|
| Sentences with 1 attack tactic         	|            8437 	|
| Sentences with 2 attack tactics        	|            4220 	|
| Sentences with 3 attack tactics        	|             673 	|
| Sentences with 4 attack tactics        	|              37 	|
| Sentences with 5 attack tactics        	|              10 	|
| Sentences with 6 attack tactics        	|               1 	|

## Tool implement 
The CTIKG already provide two trained model, you can use the following code to load the model and predict the CTI sentences. 

The active learning greatly improves the performance of tactic model. Our tactic model has average **88% precision** and **90% recall**, and our behavior model has average **83% precision** and **82% recall**. The following table shows tactic model performance on individual tactics before and after active learning.

<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky"></th>
    <th class="tg-c3ow" colspan="4">Before Active Learning&nbsp;&nbsp;</th>
    <th class="tg-c3ow" colspan="4">After Active Learning&nbsp;&nbsp;</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">Attack Tactic</td>
    <td class="tg-dvpl">Precision</td>
    <td class="tg-dvpl">Recall</td>
    <td class="tg-dvpl">F1-score</td>
    <td class="tg-dvpl">Support</td>
    <td class="tg-dvpl">Precision</td>
    <td class="tg-dvpl">Recall</td>
    <td class="tg-dvpl">F1-score</td>
    <td class="tg-dvpl">Support</td>
  </tr>
  <tr>
    <td class="tg-0pky">Initial Access</td>
    <td class="tg-dvpl">0.67</td>
    <td class="tg-dvpl">0.75</td>
    <td class="tg-dvpl">0.71</td>
    <td class="tg-dvpl">129</td>
    <td class="tg-dvpl">0.88</td>
    <td class="tg-dvpl">0.89</td>
    <td class="tg-dvpl">0.88</td>
    <td class="tg-dvpl">1556</td>
  </tr>
  <tr>
    <td class="tg-0pky">Execution</td>
    <td class="tg-dvpl">0.17</td>
    <td class="tg-dvpl">0.11</td>
    <td class="tg-dvpl">0.13</td>
    <td class="tg-dvpl">9</td>
    <td class="tg-dvpl">0.90</td>
    <td class="tg-dvpl">0.92</td>
    <td class="tg-dvpl">0.91</td>
    <td class="tg-dvpl">4836</td>
  </tr>
  <tr>
    <td class="tg-0pky">Defense Evasion</td>
    <td class="tg-dvpl">0.50</td>
    <td class="tg-dvpl">0.14</td>
    <td class="tg-dvpl">0.22</td>
    <td class="tg-dvpl">7</td>
    <td class="tg-dvpl">0.85</td>
    <td class="tg-dvpl">0.86</td>
    <td class="tg-dvpl">0.86</td>
    <td class="tg-dvpl">1686</td>
  </tr>
  <tr>
    <td class="tg-0pky">Command and Control</td>
    <td class="tg-dvpl">0.05</td>
    <td class="tg-dvpl">0.06</td>
    <td class="tg-dvpl">0.05</td>
    <td class="tg-dvpl">17</td>
    <td class="tg-dvpl">0.84</td>
    <td class="tg-dvpl">0.87</td>
    <td class="tg-dvpl">0.85</td>
    <td class="tg-dvpl">2261</td>
  </tr>
  <tr>
    <td class="tg-0pky">Privilege Escalation</td>
    <td class="tg-dvpl">0.35</td>
    <td class="tg-dvpl">0.39</td>
    <td class="tg-dvpl">0.37</td>
    <td class="tg-dvpl">23</td>
    <td class="tg-dvpl">0.89</td>
    <td class="tg-dvpl">0.90</td>
    <td class="tg-dvpl">0.89</td>
    <td class="tg-dvpl">918</td>
  </tr>
  <tr>
    <td class="tg-0pky">Persistence</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">2</td>
    <td class="tg-dvpl">0.91</td>
    <td class="tg-dvpl">0.93</td>
    <td class="tg-dvpl">0.92</td>
    <td class="tg-dvpl">965</td>
  </tr>
  <tr>
    <td class="tg-0pky">Lateral Movement</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">3</td>
    <td class="tg-dvpl">0.90</td>
    <td class="tg-dvpl">0.91</td>
    <td class="tg-dvpl">0.90</td>
    <td class="tg-dvpl">989</td>
  </tr>
  <tr>
    <td class="tg-0pky">Data Leak</td>
    <td class="tg-dvpl">0.18</td>
    <td class="tg-dvpl">0.12</td>
    <td class="tg-dvpl">0.14</td>
    <td class="tg-dvpl">17</td>
    <td class="tg-dvpl">0.88</td>
    <td class="tg-dvpl">0.89</td>
    <td class="tg-dvpl">0.89</td>
    <td class="tg-dvpl">3352</td>
  </tr>
  <tr>
    <td class="tg-0pky">Exfiltration</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">4</td>
    <td class="tg-dvpl">0.84</td>
    <td class="tg-dvpl">0.89</td>
    <td class="tg-dvpl">0.86</td>
    <td class="tg-dvpl">1073</td>
  </tr>
  <tr>
    <td class="tg-0pky">Impact</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0.00</td>
    <td class="tg-dvpl">0</td>
    <td class="tg-dvpl">0.94</td>
    <td class="tg-dvpl">0.92</td>
    <td class="tg-dvpl">0.93</td>
    <td class="tg-dvpl">1464</td>
  </tr>
  <tr>
    <td class="tg-0pky">Macro Average</td>
    <td class="tg-dvpl">0.21</td>
    <td class="tg-dvpl">0.17</td>
    <td class="tg-dvpl">0.18</td>
    <td class="tg-dvpl">211</td>
    <td class="tg-dvpl">0.88</td>
    <td class="tg-dvpl">0.90</td>
    <td class="tg-dvpl">0.89</td>
    <td class="tg-dvpl">19100</td>
  </tr>
  <tr>
    <td class="tg-0pky">Weighted Average</td>
    <td class="tg-dvpl">0.49</td>
    <td class="tg-dvpl">0.53</td>
    <td class="tg-dvpl">0.50</td>
    <td class="tg-dvpl">211</td>
    <td class="tg-dvpl">0.88</td>
    <td class="tg-dvpl">0.90</td>
    <td class="tg-dvpl">0.89</td>
    <td class="tg-dvpl">19100</td>
  </tr>
</tbody>
</table>

To use the model, download the **'tactic model.zip'** or **'behavior model.zip'** file from our [huggingface repo](https://huggingface.co/CTIKR/CTIKR/tree/main) and unzip at the same folder. Open the jupyter notebook CTI Sentence Identification Model Training.ipynb, modify the variable ** my_best_model_dir** to './model'. Then only run the **'Setup'** and **'Evaluation'** cell. The variable y_pred contains the list of inference results. 

For train own model, you should input the training data as the 'sentence.pkl' at the **'Setup'** cell, then run the **'Training'** cell. The model training and evaluation follow the k-fold cross validation. At each fold, the input format dataframe should be [[text1, label1], [text2，label2], ..., [textn，labeln]] with integer label . After running the 'Training' cell, the trained model will be saved at the 'clfmodel/k/best_model' folder.

## Tool requirement 

simpletransformers >= 0.63.9


