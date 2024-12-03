# LangGraph_Fundamentals  
LangGraph Fundamentals

#### [LangChain_LangGraph_Documents](https://github.com/raheelam98/LangChain_LangGraph_documents/blob/main/README.md)
  
**Nodes, Edges, Graphs**

Nodes are just python functions.   

Edges connect the nodes.

Graph are components

print(type())  # displays the type of the object or value passed inside type().

### Create Graph (Work Flow)

#### create schema

```bash
from typing_extensions import TypedDict

class LearningStateClass(TypedDict):
    prompt: str
```

**TypedDict** allows you to define dictionaries with a specific structure and type-checked keys for more reliable code

#### define function : Nodes are just python functions.

```bash
def node_function(state_parameter: LearningStateClass) -> LearningStateClass:
    print("---Node 1 State---", state_parameter)
    return {"prompt": state_parameter['prompt'] +" I am"}
```   

**Construct Graph**

**StateGraph()** class represents a graph structure where nodes correspond to states, and edges define transitions between those states

#### Build graph -  Edges connect the nodes.

```bash
from IPython.display import Image, display # Preview Graph
from langgraph.graph import StateGraph, START, END
from langgraph.graph.state import CompiledStateGraph # type

state_graph_object: StateGraph = StateGraph(state_schema=LearningStateClass)   # Build graph

state_graph_object.add_node("name_str", node_function)   # Define nodes
state_graph_object.add_edge()    # simples edges logic   # Add Edges
graph_varaiable: CompiledStateGraph = state_graph_object.compile()  # Comple Graph
```

graph_varaiable is a variable that holds an instance of the CompiledStateGraph class, created by calling the compile() method on state_graph_object

**add_node()** is used to add a node (or vertex) to a graph

**.compile()** transforms the state graph into a more optimized or executable structure

**.get_graph()** retrieves the current state graph, typically returning its structure (statring and ending points)

print(graph_varaiable.get_graph())

**view graph**
```bash
from IPython.display import Image, display
try:
    display(Image(graph.get_graph().draw_mermaid_png()))  # view graph
except Exception:
    # This requires some extra dependencies and is optional
    pass
```

display(Image(graph_varaiable.get_graph().draw_mermaid_png()))

* Calling `get_graph()` on graph_variable to retrieve the graph object.
* Using `draw_mermaid_png()` to render the graph as a Mermaid diagram in PNG format.
* Passing the generated PNG to Image() for display.

**draw_mermaid_png()** javascript package, creates a graph representing a workflow by converting the graph structure into a visual diagram in PNG format

Note:- When we compile the graph, it provides built-in functions that we can use, and these methods follow the **Runnable** Protocol

### Graph Invocation

[Runnable interface](https://python.langchain.com/v0.1/docs/expression_language/interface/)

graph_varaiable.invoke({"prompt" : "Hi"})

runs the graph with the input {"prompt": "Hi"}, likely triggering a workflow or task based on that input.

**invoke()** runs or triggers a task or function.

Note:- In LangGraph, a reducible function combines results from multiple nodes into a final output.

#### Add LLM functionality to the node function within the same graph.

**to_markdown()** converts data into Markdown format for easy display.



