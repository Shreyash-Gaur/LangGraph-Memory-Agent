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
├── test.ipynb                # Main notebook with agent implementation
├── requirements.txt            # Python dependencies
└── README.md                   # This file
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

## 🎨 Visualization Features

The project includes a built-in method to visualize the agent's graph structure. The notebook uses `graph.get_graph().draw_mermaid_png()` to generate a clear diagram of all the nodes and the conditional edges connecting them, making it easy to understand the agent's decision-making flow.

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

## 🙏 Acknowledgments

  - **LangChain & LangGraph Teams** for creating the powerful frameworks that make building complex agents possible.
  - **Ollama** for providing an incredible platform for running local LLMs with ease.
  - **The Open Source Community** for their continuous innovation and support.

*Ready to build smarter, more personal conversational agents? Let's get started\!* 🚀

**Star this repository if you found it helpful\!** ⭐