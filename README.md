# LangGraph
LangGraph

[LangGraph - Documents](https://langchain-ai.github.io/langgraph/tutorials/introduction/)

[LangGraph - Part 1 & 2 : 1 Nov 2024](https://www.youtube.com/watch?v=eZ2yFnGi9hE&t=800s)

[GitHub - 00_quickstart_part1_to_part7.ipynb](https://github.com/panaversity/learn-applied-generative-ai-fundamentals/blob/main/03_langchain_ecosystem/langgraph/chatbot/docs/00_quickstart_part1_to_part7.ipynb)

[colab - Lang Graph part 1 ](https://github.com/raheelam98/LangGraph/blob/main/03_langchain_ecosystem/langgraph/chatbot/docs/00_quickstart_part1_to_part7.ipynb)

First, install the required packages:
```bash
%%capture --no-stderr
%pip install -U langgraph langsmith langchain_google_genai
```

API keys:
```bash
import os
from google.colab import userdata
os.environ["LANGCHAIN_API_KEY"] = userdata.get('LANGCHAIN_API_KEY')
os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_PROJECT"] = "quickstart"

gemini_api_key = userdata.get('GEMINI_API_KEY')
```

```bash
from langchain_google_genai import ChatGoogleGenerativeAI

llm = ChatGoogleGenerativeAI(
    model="gemini-1.5-flash",
    max_retries=2,
    api_key=gemini_api_key
)

llm.invoke("greet me")
```
### Part 1: Build a Basic Chatbot

 ```bash
 class State(TypedDict):
    messages: Annotated[list, add_messages]

graph_builder = StateGraph(State)
 ```

Defines a `State` class as a typed dictionary with a single key `messages`, which is a list annotated with the `add_messages` function.

**`add_messages`** function is responsible for processing and adding messages to the state in the graph

 **`StateGraph(State)`** initializes a directed graph structure using the defined `State` schema to manage the flow of data and operations

 **`graph_builder.add_node("node_name", node_function)`** add a node, which represents a function or operation, to the directed graph

### Part 2: Enhancing the Chatbot with Tools

```bash
os.environ["TAVILY_API_KEY"] = userdata.get("TAVILY_API_KEY")
```

Next, define the tool
```bash
from langchain_community.tools.tavily_search import TavilySearchResults

tool = TavilySearchResults(max_results=2)
tools = [tool]
tool.invoke("What's a 'node' in LangGraph?")
```

 **`TavilySearchResults(max_results=2)`** initializes a search tool that retrieves and returns up to 2 search results.

```bash
tools = [search_tool, calculator_tool]
llm_with_tools = llm.bind_tools(tools)
```
 **`llm.bind_tools(tools)`** integrates the specified tools with the language model, enabling the model to invoke these tools during its execution.

 `llm_with_tools = llm.bind_tools(tools)`
 This line integrates the tools list with the language model llm, resulting in a new instance (llm_with_tools) that can use these tools during its operations.

[ PIAIC GenAI Classes - Lang Graph Part 3 & 4 : 2 Nov -2024](https://www.youtube.com/watch?v=UhfcycocwkU&t=138s)

### Part 3: Adding Memory to the Chatbot

#### MemorySaver() : it saves it all in-memory
Compile the graph with memory checkpointing

**`graph = graph_builder.compile(checkpointer=MemorySaver())`**

#### Integrating tools for search results and memory checkpointing

**`graph = graph_builder.compile(checkpointer=MemorySaver()) `** compiles the graph with memory checkpointing, allowing it to save and restore the state of the graph's nodes during its execution.

**This code is to create a chatbot using the LangChain framework, integrating tools for search results and memory checkpointing**

```bash

from typing import Annotated

from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_core.messages import BaseMessage
from typing_extensions import TypedDict

from langgraph.checkpoint.memory import MemorySaver  # Import memory checkpointing
from langgraph.graph import StateGraph  # Import StateGraph to build the graph
from langgraph.graph.message import add_messages  # Import add_messages function
from langgraph.prebuilt import ToolNode  # Import ToolNode

memory = MemorySaver()  # check require to crreate instance

# Define State with messages
class State(TypedDict):
    messages: Annotated[list, add_messages]

# Initialize the StateGraph with State
graph_builder = StateGraph(State)

# Initialize the search tool and list of tools
tool = TavilySearchResults(max_results=2)
tools = [tool]

# Initialize the language model and bind it with tools
llm_with_tools = llm.bind_tools(tools)

# Define the chatbot function to invoke the language model
def chatbot(state: State):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

# Add the chatbot node to the graph
graph_builder.add_node("chatbot", chatbot)

# Add the tools node to the graph
tool_node = ToolNode(tools=[tool])
graph_builder.add_node("tools", tool_node)

# Add conditional edges between nodes
graph_builder.add_conditional_edges(
    "chatbot",
    tools_condition,
)

# Add an edge from the tools node to the chatbot node
graph_builder.add_edge("tools", "chatbot")

# Set the chatbot as the entry point of the graph
graph_builder.set_entry_point("chatbot")

# Compile the graph with memory checkpointing
graph = graph_builder.compile(checkpointer=MemorySaver())  # Compile the graph with memory checkpointing
```

### Part 4: Human-in-the-loop
#### LangGraph's interrupt_before functionality to always break the tool node.
First get aprovel and then call tool, example - before going to pay money ask, can i pay the money

