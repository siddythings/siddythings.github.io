---
author: "sid"
title: "Building a 100% Local MCP Client"
summary: Learn how to build a Model Context Protocol (MCP) client that runs entirely locally, using LlamaIndex, Ollama, and LightningAI. This guide walks you through creating an MCP-powered agent that can connect to tools and data sources while maintaining complete privacy and control.
tags: [
    "ai",
    "llm",
    "mcp",
    "llamaindex",
    "ollama",
    "lightningai"
]
categories: [
    "AI",
    "Development"
]
weight: 98
cover:
    image: /images/mcp-client-title.png
    hiddenInList: true
---

## Introduction
In the rapidly evolving landscape of AI applications, the Model Context Protocol (MCP) has emerged as a crucial component for establishing standardized connections between AI models and external tools. An MCP client acts as a bridge, enabling AI applications (like Cursor) to seamlessly interact with various tools and data sources while maintaining a consistent interface.

## Understanding MCP
The Model Context Protocol (MCP) provides a standardized way for AI applications to:
- Discover available tools and their capabilities
- Execute tool calls in a structured manner
- Handle responses and context management
- Maintain a consistent interface across different implementations

## Tech Stack Overview
For our local implementation, we'll use:
- **LlamaIndex**: For building the MCP-powered Agent
- **Ollama**: To locally serve Deepseek-R1 model
- **LightningAI**: For development and hosting

## Implementation Workflow
Our MCP client follows this workflow:
1. User submits a query
2. Agent connects to the MCP server to discover available tools
3. Based on the query, the agent invokes the appropriate tool
4. Agent processes the context and returns a response

## Building the SQLite MCP Server
For demonstration purposes, we've created a simple SQLite server with two basic tools:
- `add_data`: For inserting new records
- `fetch_data`: For retrieving existing records

This simplified server helps illustrate the core concepts while maintaining the flexibility to connect to any MCP-compatible server.

## Setting Up the Local Environment

### 1. LLM Configuration
We use Deepseek-R1 served locally through Ollama as our LLM. This ensures:
- Complete privacy (all processing happens locally)
- No API costs
- Full control over the model's behavior

### 2. System Prompt Definition
The agent's behavior is guided by a carefully crafted system prompt that instructs it to:
- Use available tools before answering queries
- Maintain context awareness
- Follow best practices for tool usage

### 3. Agent Implementation
The core of our implementation is a LlamaIndex agent that:
- Wraps MCP tools as native tools
- Manages tool discovery and execution
- Handles context and memory
- Processes user queries effectively

## Code Implementation

### Streamlit App Implementation
Let's create a user-friendly interface using Streamlit to interact with our MCP client. Here's the complete implementation:

```python
import streamlit as st
import asyncio
from mcp_client import MCPClient, create_mcp_agent, process_query

# Initialize session state for chat history
if 'messages' not in st.session_state:
    st.session_state.messages = []

# Initialize MCP client and agent
@st.cache_resource
def initialize_agent():
    client = MCPClient()
    tools = client.discover_tools()
    wrapped_tools = wrap_tools_for_llamaindex(tools)
    
    system_prompt = """You are an AI assistant powered by a local MCP client.
    Use the available tools to help users interact with the database.
    Always explain your actions and provide clear responses."""
    
    return create_mcp_agent(wrapped_tools, system_prompt)

# Streamlit UI
st.title("Local MCP Client Demo")
st.write("Interact with your local MCP-powered agent!")

# Sidebar for database operations
with st.sidebar:
    st.header("Database Operations")
    if st.button("View Database Schema"):
        # Add code to fetch and display schema
        pass
    
    if st.button("Clear Chat History"):
        st.session_state.messages = []

# Main chat interface
for message in st.session_state.messages:
    with st.chat_message(message["role"]):
        st.write(message["content"])

# Chat input
if prompt := st.chat_input("What would you like to do?"):
    # Add user message to chat history
    st.session_state.messages.append({"role": "user", "content": prompt})
    with st.chat_message("user"):
        st.write(prompt)
    
    # Get agent response
    with st.chat_message("assistant"):
        with st.spinner("Thinking..."):
            agent = initialize_agent()
            response = asyncio.run(process_query(agent, prompt, st.session_state.messages))
            
            # Display streaming response
            message_placeholder = st.empty()
            full_response = ""
            for chunk in response.response_gen:
                full_response += chunk
                message_placeholder.write(full_response + "▌")
            message_placeholder.write(full_response)
    
    # Add assistant response to chat history
    st.session_state.messages.append({"role": "assistant", "content": full_response})

# Display available tools
with st.expander("Available Tools"):
    st.write("""
    - **add_data**: Insert new records into the database
    - **fetch_data**: Retrieve records from the database
    """)

# Add status information
st.sidebar.markdown("---")
st.sidebar.info("""
    **Status:**
    - MCP Client: Connected
    - LLM: Deepseek-R1 (Local)
    - Database: SQLite
""")
```

This Streamlit app provides:
1. A clean chat interface for interacting with the MCP agent
2. Real-time streaming of agent responses
3. Chat history management
4. Sidebar with database operations
5. Tool information display
6. Status indicators

To run the app:
```bash
streamlit run app.py
```

The app will be available at `http://localhost:8501` by default.

### Defining the Agent
```python
def create_mcp_agent(tools, system_prompt):
    agent = FunctionAgent.from_tools(
        tools=tools,
        system_prompt=system_prompt,
        llm=local_llm,
        verbose=True
    )
    return agent
```

### Agent Interaction
```python
async def process_query(agent, query, context):
    response = await agent.astream_chat(
        query,
        context=context
    )
    return response
```

### MCP Client Initialization
```python
def initialize_mcp_client():
    client = MCPClient()
    tools = client.discover_tools()
    return wrap_tools_for_llamaindex(tools)
```

## Running the Agent
The agent demonstrates its capabilities through:
1. Understanding natural language queries
2. Converting them to appropriate tool calls
3. Executing database operations
4. Presenting results in a user-friendly format

For example:
- When asked to "Add Rafael Nadal...", it generates and executes the appropriate SQL INSERT
- When requested to "fetch data", it runs a SELECT query and formats the results


The implementation we've discussed demonstrates how to create a fully functional MCP client that runs entirely locally while maintaining the ability to connect to any MCP-compatible server.


## References
- [LlamaIndex Documentation](https://docs.llamaindex.ai/)
- [Ollama Documentation](https://ollama.ai/docs)
- [LightningAI Documentation](https://lightning.ai/docs)
- [Model Context Protocol Specification](https://github.com/cursor-ai/mcp) 