# 🧠 LangGraph Conversational Memory Agent

*Building Stateful, Personalized Chatbots with Persistent Memory*

## 🎯 Overview

The **LangGraph Conversational Memory Agent** is a project that demonstrates how to build a sophisticated, stateful chatbot capable of remembering user information across an entire conversation. By leveraging the power of LangGraph, this agent goes beyond simple request-response interactions. It intelligently classifies, extracts, and stores user-provided details, leading to a truly personalized and context-aware conversational experience.

## ✨ Key Features

  - **🤖 Stateful Agent Architecture**: Built on LangGraph to manage a robust, multi-step conversational flow and maintain state.
  - **🔍 Personal Information Detection**: Automatically identifies when a user shares personal details like names, hobbies, or interests.
  - **✍️ Structured Data Extraction**: Intelligently summarizes personal details into concise, storable facts (e.g., "User's name is Alex and they love playing chess").
  - **🚫 Duplicate Prevention**: Before saving a new memory, the agent checks if it's a duplicate of existing information, even if phrased differently.
  - **💾 Persistent Conversational Memory**: Saves unique user facts into a memory store, allowing the agent to recall information in later interactions.
  - **🗣️ Context-Aware Responses**: Enriches its prompts with all recalled memories to provide answers that are deeply aware of the user's context.

## 🛠️ Technology Stack

  - **Agent Framework**: LangGraph for creating the stateful agent graph.
  - **LLM Orchestration**: LangChain for core abstractions (prompts, parsers, messages).
  - **LLM Integration**: Ollama (Llama 3.2) for all reasoning and generation tasks.
  - **Core Language**: Python 3.10+
  - **Environment**: Jupyter Notebook

## 📁 Project Structure

```
langgraph-memory-agent/
├── LangGraph-Memory-Agent.ipynb # Main notebook with agent implementation
├── requirements.txt             # Python dependencies
└── README.md                    # This file
```

## 🚀 Quick Start

### Prerequisites

  - Ollama running locally.
  - Python 3.10+ with Jupyter Notebook installed.

## 🎮 How It Works

The agent operates in a cyclical graph, where each node performs a specific task.

### 1\. **Message Classification**

When a user sends a message, it first goes to the `personal_info_classifier` node. This node uses the LLM to determine if the message contains a statement of personal information.

### 2\. **Information Extraction**

If personal info is detected, the workflow moves to the `personal_info_extractor` node. Here, the LLM summarizes the key fact into a concise phrase.

### 3\. **De-duplication & Storage**

The extracted fact is passed to the `personal_info_duplicate_classifier`. This node compares the new information against all existing memories for that user. If the information is genuinely new, it proceeds to the `personal_info_storer` node, which saves it.

### 4\. **Memory-Enhanced Response**

Finally, the agent enters the `retrieve_and_log_memories` node to collect all stored facts for the user. This collection of memories is then injected into the system prompt of the main `call_model` node, which generates a final, contextually-aware response to the user.

## 📊 Sample Conversation

You can test the agent's memory with a conversation like this:

  - **You**: `"Hi, my name is Alex and I love to play chess."`
      - *Agent recognizes and stores this information.*
  - **You**: `"I'm also a big fan of The Lord of the Rings."`
      - *Agent recognizes this new interest, confirms it's not a duplicate, and stores it.*
  - **You**: `"What are my hobbies?"`
      - **Agent, recalling its memory**: `"You enjoy playing chess and being a fan of The Lord of the Rings."`
You got it\! Here's the code and its output, formatted for your README:

## LangGraph Memory Agent Conversation Example

This code demonstrates how to interact with the LangGraph memory agent, showing how it remembers and utilizes previously provided personal information in subsequent turns of a conversation.

```python
# Let's run a continuous conversation
# Use the same thread_id to maintain context
config = {"configurable": {"thread_id": "conversation-1"}}

# --- Turn 1: User introduces themselves ---
input1 = {"messages": [HumanMessage(content="Hi, my name is Alex and I love to play chess.")]}
response = graph.invoke(input=input1, config=config)
print("\nFinal Response 1:", response['messages'][-1].content)



# --- Turn 2: User adds more information ---
input2 = {"messages": [HumanMessage(content="I'm also a big fan of The Lord of the Rings.")]}
response = graph.invoke(input=input2, config=config)
print("\nFinal Response 2:", response['messages'][-1].content)



# --- Turn 3: User asks a question that relies on memory ---
input3 = {"messages": [HumanMessage(content="What are my hobbies?")]}
response = graph.invoke(input=input3, config=config)
print("\nFinal Response 3:", response['messages'][-1].content)
```

### Output

```
Classifier Result: Yes
Extracted Info: User's name is Alex and they love playing chess.
Stored new memory for user user-123: User's name is Alex and they love playing chess.

----- Logging Personal Memory -----
[Memory 1] User's name is Alex and they love playing chess.
-----------------------------------


Final Response 1: Hello Alex! It's great to meet you. I've heard that you're an avid chess player - do you have a favorite opening or strategy? Or perhaps you're looking for some tips or advice on improving your game? I'm here to help!



Classifier Result: Yes
Extracted Info: User is a fan of The Lord of the Rings.
Duplicate Check Result: No
Stored new memory for user user-123: User is a fan of The Lord of the Rings.

----- Logging Personal Memory -----
[Memory 1] User's name is Alex and they love playing chess.
[Memory 2] User is a fan of The Lord of the Rings.
-----------------------------------


Final Response 2: A fan of Middle-earth, eh? Which character is your favorite - is it Frodo, Sam, Aragorn, or perhaps Gollum? Or do you have a special place in your heart for the Elves, Dwarves, or Men? I'm curious to know more about what draws you to J.R.R. Tolkien's epic world!



Classifier Result: No

----- Logging Personal Memory -----
[Memory 1] User's name is Alex and they love playing chess.
[Memory 2] User is a fan of The Lord of the Rings.
-----------------------------------


Final Response 3: You enjoy playing chess and being a fan of The Lord of the Rings.
```

## 🔧 Customization

### Changing the LLM

You can easily switch the underlying language model by modifying the model string in the `ChatOllama` initialization within the notebook.

```python
llm = ChatOllama(model="your-ollama-model", temperature=0)
```

### Modifying Agent Behavior

The agent's logic is defined by the system prompts in each node function (`personal_info_classifier`, `personal_info_extractor`, etc.). You can customize its behavior by editing these prompts to recognize different types of information or to extract facts in a different format.

### Using a Persistent Memory Store

The current implementation uses a volatile `InMemoryStore`. For production use, you can replace this with a persistent key-value store like Redis or a database by implementing a class that inherits from LangChain's `BaseStore`.

## 📝 Requirements

The project requires the following Python packages:

```
langgraph
langchain-ollama
langchain-core
pydantic
ipython
# Note: Other dependencies may be required by langchain libraries.
```

## 🔍 Troubleshooting

### Common Issues

  - **Ollama Connection**: Ensure the Ollama application is running on your machine before starting the notebook.
  - **Model Not Found**: Verify that you have pulled the required model (`llama3.2:latest`) using the `ollama pull` command.
  - **Incorrect JSON Output**: If the LLM fails to return valid JSON for the classifier or de-duper nodes, the agent has built-in error handling to default to a "safe" path, but you may need to adjust the prompts for your specific model to improve reliability.

## 🤝 Contributing

We welcome contributions\! Please feel free to:

  - **Report Issues**: Submit bug reports and suggest new features.
  - **Code Contributions**: Fork the repository and submit pull requests with your enhancements.
  - **Improve Documentation**: Help us make the documentation clearer and more comprehensive.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

*Ready to build smarter, more personal conversational agents? Let's get started\!* 🚀

**Star this repository if you found it helpful\!** ⭐