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

**This code is to create a chatbot using the LangChain framework, integrating tools for search results, memory checkpointing (part 3), interrup_node (part4)**

```bash

from typing import Annotated

from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_core.messages import BaseMessage
from typing_extensions import TypedDict

from langgraph.checkpoint.memory import MemorySaver  # Import memory checkpointing
from langgraph.graph import StateGraph  # Import StateGraph to build the graph
from langgraph.graph.message import add_messages  # Import add_messages function
from langgraph.prebuilt import ToolNode  # Import ToolNode

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

# Compile the graph with memory checkpointing  - part 3
#graph = graph_builder.compile(checkpointer=MemorySaver())  

memory = MemorySaver()

#interrupt_before=["tools"],  # This is new!   -  part 4
# Note: can also interrupt **after** actions, if desired.
# interrupt_after=["tools"]

graph = graph_builder.compile(checkpointer=memory, interrupt_before=["tools"])
    


```
