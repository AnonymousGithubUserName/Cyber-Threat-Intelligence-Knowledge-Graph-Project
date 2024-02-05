# CTIKG

## Introduction
CTIKG (Cyber Threat Intelligence Knowledge Graph) is an innovative tool with Large Language Model (LLM) and machine learning techniques. Its goal is to effectively build a security-oriented knowledge graph from CTI articles and perform community detection on the built knowledge graph to identify security-oriented communities.

With the fast-paced development of the Internet infrastructure and the increasing cyber threats that target organizations, knowledge of these threats is aggressively collected and shared through CTI. However, the information provided by traditional blacklists is limited and the manual effort required to extract knowledge from CTI articles is not scalable. 

By leveraging advanced NLP model and high-quality Cyber threat dataset from active learning, CTIKG effectively and automatically summarizes sentence and article information from CTI articles. 
By uilizing LLM agent and segment processing with dual memory design, CTIKG constructs a security-oriented knowledge graph from numerous CTI articles, which has higher effectiveness than SOTA methods. It reveals the relationships among security-related entities and their behaviors across multiple articles, and further provides security-oriented community detection to help security analysts understand the knowledge of cyber threats more efficiently.

## Overview
CTIKG contains three main components. The overview figure is shown below.

![image](https://i.ibb.co/mFkMLB3/overview.png)

1. [CTI Article Classification](https://github.com/AnonymousGithubUserName/Cyber-Threat-Intelligence-Knowledge-Graph-Project/tree/main/CTI%20Article%20Classification): CTIKG uses [sentence classification models](https://github.com/AnonymousGithubUserName/Cyber-Threat-Intelligence-Knowledge-Graph-Project/tree/main/CTI%20Sentence%20Identification) to infer the tactics and behavior labels of each sentenc, counts the labeling information as vector, identifies the CTI sentences, and embeds CTI sentences to vector. In this phase, CTIKG transfers article into vector and has a DNN model to identify CTI articles from numerous articles.

2. [Knowledge Graph Construction](https://github.com/AnonymousGithubUserName/Cyber-Threat-Intelligence-Knowledge-Graph-Project/tree/main/Knowledge%20Graph%20Construction): CTIKG uses CTI articles, which are identified by the article classification model as input, deploys multiple LLM agents to extract all the knowledge within an CTI article in the form of triples and combine them to form a knowledge graph. The knowledge graph construction consists of five types of LLM agents: worker, integrator, refiner, merger, and checker. Those LLM agents worker, integrator, refiner are designed to extract entities and their relations from a segment of CTI articles, and then merger merge the them to form the knowledge graph. The checker is used to check the result of other LLM agents.

3. [Community Discovery](https://github.com/AnonymousGithubUserName/Cyber-Threat-Intelligence-Knowledge-Graph-Project/tree/main/Community%20Discovery) CtiKG performs community detection on constructed knowledge graph and leverages the CTI sentence and article classication model outputs and other edge statistics to identify the security-oriented communities.


## Tool Requirements
Python Version >= 3.8.0

Python Library:

scalellm==0.0.5

openai==0.28.0

numpy >= 1.23.4

scikit-learn >= 1.1.2

simpletransformers >= 0.63.9

hdbscan >= 0.8.28

torch >= 1.12.1

networkx >= 2.8.7

cdlib >= 0.2.6
