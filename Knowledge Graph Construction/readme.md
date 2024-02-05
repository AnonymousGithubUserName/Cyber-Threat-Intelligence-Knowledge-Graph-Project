# Knowledge Graph Construction

## Introduction
CtiKG takes as input the CTI articles identified in the first phase, deploys LLM agents to extract the cyber threat knowledge from the CTI articles in the form of triples, and combines the extracted triples to form a knowledge graph. 

# Dual Memory Design
CtiKG uses a dual memory design to overcome the problems of using LLM directly. Specifically, instead of using the whole article as input, CtiKG divides the article into segments and extracts the triples from each segment. The triples for a segment form the short-term memory. Meanwhile, CtiKG keeps a long list of all triples extracted from different segments of the article, which is called the long-term memory.


# Short-Term Memory Construction
To construct the short-term memory of a text segment, CtiKG
deploys four types of LLM agents: Worker, Integrator, Refiner and
Checker.

- Worker Agent. The worker agent receives the text segment as input and extracts the security-related triples from the text.

- Integrator Agent. The integrator agent is used to extract a final triple from the results of the worker agents. Due to the randomness of LLM, multiple workers may use different triples to express the same entity relation in the text. The integrator summarizes the valid triples with identical meanings and outputs a representative triple.

- Refiner Agent. The refiner agent is designed to perform the normalization and resolution tasks. The refiner agent takes the triples from the integrator agent as input and refines the triples to ensure that the entities in the triples are normalized and resolved.

# Long-Term Memory Construction
CTIKG uses the merger agent to further refine the triples in short-term memory to fit the context of long-term memory. After refinement, CTIKG directly adds the triples into the long-term memory.

- Merger Agent. This agent determines whether the subject or object of the triples in short-term memory and long-term memory refer to the same entities. If so, this agent will modify the subject or the object of the triples in the short-term memory to unify the representation of the same entities.

# Retry Mechanism
-Checker Agent. The checker agent is designed to check whether the output of LLM agents is correct, based on whether the output of other agents contains the example text, or refuse performing the task. If the output of an LLM agent is found to be incorrect, CtiKG then instructs the LLM agent to re-perform the task.
