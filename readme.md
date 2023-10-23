# CTIKG

## Introduction
CTIKG (Cyber Threat Intelligence Knowledge Graph) is an innovative tool designed to efficiently manage and utilize the vast amount of knowledge derived from Cyber Threat Intelligence (CTI) articles. With the fast-paced development of the Internet infrastructure and the increasing cyber threats that target organizations, knowledge of these threats is aggressively collected and shared through CTI. However, the information provided by traditional blacklists is limited and the manual effort required to extract knowledge from CTI articles is not scalable. 

By leveraging advanced NLP model and high-quality Cyber threat dataset from active learning, CTIKG effectively and automatically summarizes sentence and article information from CTI articles. 
By uilizing those information, CTIKG constructs a security-oriented knowledge graph from numerous CTI articles, reveals the relationships among security-related entities and their behaviors across multiple articles, and further provides security-oriented community detection to help security analysts understand the knowledge of cyber threats more efficiently.

## Overview
CTIKG contains three main components. The overview figure is shown below.

![image](https://i.imgur.com/v9Hcxgc.png)

1. [CTI Sentence Identification](https://github.com/CTIKR/CTIKG/tree/main/CTI%20Sentence%Identification): CTI Sentence Identification. By using collected CTI sentences from the ATT&CK knowledge base and several computer security related websites and CTIKG have two RoBERTa model. Thus, CTIKG can identify tactics sentences and behavior sentences from an article. 

2. [CTI Article Classification](https://github.com/CTIKR/CTIKG/tree/main/CTI%20Article%Classification): CTIKG uses sentence classification models to infer the tactics and behavior labels of each sentenc, counts the labeling information as vector, identifies the CTI sentences, and embeds CTI sentences to vector. In this phase, CTIKG transfers article into vector and has a DNN model to identify CTI articles from numerous articles.

3. [Knowledge Graph Construction](https://github.com/CTIKR/CTIKG/tree/main/Knowledge%20Graph%Construction): CTIKG uses the article classification model to extract CTI articles from articles, applies NLP techniques to extract entities and their relations described in CTI sentences, and then matches the entities to join the relations to form the knowledge graph. In further, CTIKG has a new community detection algorithm that uses the security information provided by the article model and sentence models to achieve security-oriented community detection and filtering.

## Tool Requirements
Python Version >= 3.8.0

Python Library:

numpy >= 1.23.4

scikit-learn >= 1.1.2

simpletransformers >= 0.63.9

hdbscan >= 0.8.28

torch >= 1.12.1

networkx >= 2.8.7

cdlib >= 0.2.6