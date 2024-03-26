# Learning LangChain

Created: March 6, 2024 8:32 PM
Tags: Personal

LangChain is an open-source developer framework for building LLM applications.
It has Python and TypeScript packages.
Focused on composition and modularity.

LangChain key values add modular components that can be used in conjunction with each other or by themselves and the other key value is the use cases where common ways to combine the components to end to end applications.
LangChain includes prompts, models, indexes, chains. 
Prompts include prompt templates, output parsers
Models include 20+ integrations of LLMs, chat models and text embedding models (10+ integrations)
Indexes are ways for ingesting data and combining it with models. It contains document loaders, text splitters, vector stores and retrievers
Chains are more end-to-end use cases can be used as building blocks for other chains
Agents consist of algorithms for getting LLMs to use tools and uses the model as a reasoning engine. 

Models refers to the LLMs, prompts refers to the style of creating inputs to pass in models and parsers involves taking the output of these models and parsing it into a more structured format. 

Langchain supports output parsing which works with prompt templates (it has built in prompts). Generating its output in a certain format with key words. 

![Screenshot 2024-03-07 at 5.24.27 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-07_at_5.24.27_PM.png)

Large Language Models for a chat converstaion are “stateless”. The LLM does not remember the history of the conversation. Each transaction, each call to the api endpoint is independent. Chatbots appear to have memory by providing the full conversation as “context”. And so the LLM depends on the memory to generate as it seems the whole conversation as output. However, as the conversation becomes long the amount of memory needed increases and the cost of tokens become more expensive. LangChain provides several kinds of “memory” to store and accumulate the conversation.

![Screenshot 2024-03-18 at 2.34.12 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-18_at_2.34.12_PM.png)

![Screenshot 2024-03-18 at 2.36.27 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-18_at_2.36.27_PM.png)

LangCHain LLM Chains:

Sequential Chains:

![Screenshot 2024-03-18 at 2.54.50 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-18_at_2.54.50_PM.png)

Briefly, simplesequentialchain is useful for single inputs and outputs. 

![Screenshot 2024-03-18 at 3.05.10 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-18_at_3.05.10_PM.png)

For multiple inputs and outputs regular SequentialChain is more appropriate. 

![Screenshot 2024-03-18 at 3.05.57 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-18_at_3.05.57_PM.png)

## LLM’s on Documents

LLM’s can only inspect a few thousand words at a time. So how can we get the LLM to answer everything within huge documents. 

Embeddings create numerical representations for pieces of texts. This numerical representation captures the semantic meaning of the piece of text its been run over. Pieces of text with similar content will have similar vectors. This lets us compare pieces of text in the vector space. 

![Screenshot 2024-03-18 at 5.02.16 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-18_at_5.02.16_PM.png)

This allows to analyse which pieces of text are similar. 

A vector database is a way to store these vector representations. The way we create a vector db is populating it with chunks of texts coming from incoming documents. When getting a big document we first break it up in smaller chunks. It helps in creating pieces of text that are smaller than the original document and is useful as we may not be able to pass the whole document to the LLM. So we want to create small chunks to pass only the relevant ones. We then create embeddings for each of these chunks and then we store these in a vector db. 

This is what happens when we create an index, we can use it during run time to find the pieces of text most relevant to an incoming query. When a query comes in we create an embedding for that query we then compare it to all vectors in the vector db and pick the n most similar. 
These are then returned and we can pass those in the prompt to the LLM to get back the final answer. 

![Screenshot 2024-03-18 at 5.12.22 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-18_at_5.12.22_PM.png)

## Stuff Method:

![Screenshot 2024-03-18 at 7.31.26 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-18_at_7.31.26_PM.png)

It is used for small documents or spreadsheets.

There are three additional methods:

- Map_reduce: Takes all the chunks, passes them along with the question to the LLM gets back a response and uses another LLM call to summarize all individual responses into a final answer. It is very powerful because it can operate over any number of documents. It does take a lot more calls though. Its great for summarization.
- Refine: Used to loop over many documents and it builds upon the answer from the previous document.  Good for combining information and building up an answer over time. Leads to longer answers, and is also not as fast as the calls depend on the result of previous calls.
- Map_rerank: Is a bit more experimental, where we do a single call to the LLM for each document and ask it to return scores and select the highest score. This relies on the LLM to know what the score should be. Refined instructions would need to be made.

# LangChain Expression Language

LangChain can compose chains of different components together. 

LCEL and the runnable protocol define:
An allowed set of input types, and a corresponding allowed set of output types. 

Required methods like `invoke` `stream` and `batch` that all runnables will expose. 
There are also ways of modifying parameter at runtime like `bind`

Composition can use the Linux pipe syntax:
Chain = prompt | llm | OutputParser

The interface that we expect all runnables to expose:

`invoke`calls runnable on a single input 

`stream` calls runnable on a single input and streams back a response

`batch` calls runnable on a list of inputs

For all of these synchronous there is asynchronous methods like ainvoke, astream and etc. 

![Screenshot 2024-03-21 at 3.00.37 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-21_at_3.00.37_PM.png)

![Screenshot 2024-03-21 at 3.05.31 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-21_at_3.05.31_PM.png)

# Pydantic

![Screenshot 2024-03-21 at 5.21.52 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-21_at_5.21.52_PM.png)

![Screenshot 2024-03-21 at 5.22.51 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-21_at_5.22.51_PM.png)

## **Pydantic Syntax**

Pydantic data classes are a blend of Python's data classes with the validation power of Pydantic.

They offer a concise way to define data structures while ensuring that the data adheres to specified types and constraints.

In standard python you would create a class like this:

```python
class User:
	def **init**(self, name: str, age: int, email: str):
		[self.name](http://self.name/) = name
		self.age = age
		self.email = email
```

# Tagging and Extraction

![Screenshot 2024-03-21 at 6.11.20 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-21_at_6.11.20_PM.png)

Using tagging, we pass in unstructured piece of text along structured description. We use LLM to generate structured output and reason over the input text and create a response in the format of the structured description that we passed in. 

Extraction we extract specific entities from a text. These entities are also represented by a structured description. But rather than using LLM to reason over the text and respond with a single output in the structured description, we are using the LLM to look over the text and extract a list of these elements.

![Screenshot 2024-03-21 at 6.18.35 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-21_at_6.18.35_PM.png)

# Tools and Routing

The LLM can decide which function to use and what inputs to that function shuld be. Then its calling that function with those inputs. 
Langchain combine these two ideas into something called a tool, which is a schema definition for the function which we can convert in the OpenAi function spec and a callable (call the model). 

![Screenshot 2024-03-22 at 1.48.19 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-22_at_1.48.19_PM.png)

# Agents:

![Screenshot 2024-03-22 at 5.16.10 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-22_at_5.16.10_PM.png)

# Retrieval Augmented Generation:

![Screenshot 2024-03-25 at 12.19.31 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-25_at_12.19.31_PM.png)

In retrieval augmented generation (RAG), an LLM retrieves contextual documents from an external dataset as part of its execution.

This is useful if we want to ask question about specific documents (e.g., our PDFs, a set of videos, etc).

![Untitled](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Untitled.png)

# Loaders

![Screenshot 2024-03-25 at 1.02.09 PM.png](Learning%20LangChain%208bdb6f448cad46b3a344b7b14db17657/Screenshot_2024-03-25_at_1.02.09_PM.png)