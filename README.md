# LangGraph
 
### Agentic Chatbots:
Autonomous systems that perform tasks, make decisions, and interact with users with minimal human intervention. They leverage advanced AI to understand context, learn from interactions, and provide personalized response

[Lang Graph - Tutorials](https://langchain-ai.github.io/langgraph/tutorials/)

[Lang Graph - Agentic Chatbot : Quick Start](https://langchain-ai.github.io/langgraph/tutorials/introduction/)

[Building Chatbots with LangGraph - Github](https://github.com/panaversity/learn-applied-generative-ai-fundamentals/tree/main/03_langchain_ecosystem/langgraph/chatbot)

[PIAIC GenAI Classes - Part 1 & 2 : video - 1 Nov 2024](https://www.youtube.com/watch?v=eZ2yFnGi9hE&t=800s)

[PIAIC GenAI Classes - Part 3,4,5,6 : video - Nov 2, 2024](https://www.youtube.com/watch?v=UhfcycocwkU&t=138s)

[PIAIC GenAI Classes - Revision Part 1 to 6  and Part 7 : video - Nov 7, 2024](https://www.youtube.com/watch?v=hRzyWYUpJKs)

[colab - Lang Graph - Chatbots Agent](https://github.com/raheelam98/LangGraph/blob/main/03_langchain_ecosystem/langgraph/chatbot/docs/00_quickstart_part1_to_part7.ipynb)


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

## Tool Usage in Agentic Chatbots

```bash
 class State(TypedDict):
    messages: Annotated[list, add_messages]
 ```
Defines a `State` class as a typed dictionary with a single key `messages`, which is a list annotated with the `add_messages` function.

**`add_messages`** function is responsible for processing and adding messages to the state in the graph

 **`StateGraph(State)`** initializes a directed graph structure using the defined `State` schema to manage the flow of data and operations

 **`TavilySearchResults(max_results=2)`** initializes a search tool that retrieves and returns up to 2 search results.

```bash
tools = [search_tool, calculator_tool]
llm_with_tools = llm.bind_tools(tools)
```
 **`llm.bind_tools(tools)`** integrates the specified tools with the language model, enabling the model to invoke these tools during its execution.

 **`llm_with_tools = llm.bind_tools(tools)`**
 This line integrates the tools list with the language model llm, resulting in a new instance (llm_with_tools) that can use these tools during its operations.

 **`graph_builder.add_node("node_name", node_function)`** add a node, which represents a function or operation, to the directed graph

**`graph = graph_builder.compile(checkpointer=MemorySaver()) `** compiles the graph with memory checkpointing, allowing it to save and restore the state of the graph's nodes during its execution.

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

**`graph_builder.compile()`** method creates a CompiledGraph, finalizing the state graph's construction and preparing it for execution with all defined nodes and connections
 

 ```bash
def chatbot(state: State):
    return {"messages": [llm.invoke(state["messages"])]}

# Check if the node already exists before adding it
if "chatbot" not in graph_builder.nodes:
    graph_builder.add_node("chatbot", chatbot)
else:
    print("Node 'chatbot' already present.")

# # Adding nodes
graph_builder.add_edge(START, "chatbot")
graph_builder.add_edge("chatbot", END)

# Compile the graph
graph = graph_builder.compile()
 ```

### Part 2: Enhancing the Chatbot with Tools

#### Integrating tools for search results

```bash
%%capture --no-stderr
%pip install -U tavily-python langchain_community
```

Next, define the tool

**`TavilySearchResults(max_results=2)`** initializes a search tool that retrieves and returns up to 2 search results.

Initializes a search tool that retrieves and returns up to 2 search results.
```bash
tool = TavilySearchResults(max_results=2)
tools = [tool]
```

**`bind_tools()`** Integrates the specified tools with the language model, enabling the model to invoke these tools during its execution

```bash
llm_with_tools = llm.bind_tools(tools)
```
This line integrates the tools list with the language model llm, resulting in a new instance (llm_with_tools) that can use these tools during its operations.

**`add_conditional_edges`** Define conditional edges for the chatbot node based on tool usage
```bash
graph_builder.add_conditional_edges(
    "chatbot",
    tools_condition
)
```

[ PIAIC GenAI Classes - Lang Graph Part 3 & 4 : 2 Nov -2024](https://www.youtube.com/watch?v=UhfcycocwkU&t=138s)

### Part 3: Adding Memory to the Chatbot

[PIAIC GenAI Classes - Nov 2, 2024](https://www.youtube.com/watch?v=UhfcycocwkU&t=138s)

time 04

#### MemorySaver() : it saves it all in-memory
Compile the graph with memory checkpointing

**`graph = graph_builder.compile(checkpointer=MemorySaver())`**

#### Memory checkpointing

**`graph = graph_builder.compile(checkpointer=MemorySaver()) `** compiles the graph with memory checkpointing, allowing it to save and restore the state of the graph's nodes during its execution.

### Part 4: Human-in-the-loop

[PIAIC GenAI Classes - Nov 2, 2024](https://www.youtube.com/watch?v=UhfcycocwkU&t=138s)

#### Freezer and un unfreez node
#### LangGraph's interrupt_before functionality to always break the tool node.
First get aprovel and then call tool, example - before going to pay money ask, can i pay the money

Compile the graph, specifying to **`interrupt_before`** the tools node.
```bash
graph = graph_builder.compile(checkpointer=memory, interrupt_before=["tools"])
```

    interrupt_before=["tools"],  # This is new!
    # Note: can also interrupt __after__ tools, if desired.
    # interrupt_after=["tools"]

**`get_state()`**  Retrieve the current state or status of a particular object
**`.next`** Find out what the next node to execute is
**`.config`** Show the current configuration of the state
**`.value`**  Show all values

* `snapshot = graph.get_state(config)`  # Retrieve the current state or status of the graph
* `snapshot.next`  # Find out what the next node to execute is
* `snapshot.config`  # Show the current configuration of the state

```bash
snapshot = graph.get_state(config)  # Retrieve the current state or status of the graph
snapshot.next  # Find out what the next node to execute is
snapshot.config  # Show the current configuration of the state
snapshot.values  # Show values
snapshot.values['messages']  # Show messages

existing_message = snapshot.values["messages"][-1]  # Find out the last message
existing_message.pretty_print()  # Print message

existing_message.tool_calls  # Get tool data
```

```bash
# `None` will append nothing new to the current state, letting it resume as if it had never been interrupted
events = graph.stream(None, config, stream_mode="values")  # Stream events based on the current state and configuration
for event in events:  # Iterate through the streamed events
    if "messages" in event:  # Check if the event contains messages
        event["messages"][-1].pretty_print()  # Print the last message in the event
```
### Part 5: Manually Updating the State   

[PIAIC GenAI Classes - Nov 2, 2024    ](https://www.youtube.com/watch?v=UhfcycocwkU&t=138s)  

**`update_state()`** method would be used to modify or refresh the current state of the graph based on new information or changes in data.

### Part 6: Customizing State

[PIAIC GenAI Classes - Nov 2, 2024](https://www.youtube.com/watch?v=UhfcycocwkU&t=138s)

time 1:30

```bash
class State(TypedDict):
    messages: Annotated[list, add_messages]
    ask_human: bool  # This flag is new
```

```bash
# We can bind the llm to a tool definition, a pydantic model, or a json schema
llm_with_tools = llm.bind_tools(tools + [RequestAssistance])
```

**`[RequestAssistance]`**  class use to relay the user's 'request'

**Note** now llm get two tools first is search tool and second is 



