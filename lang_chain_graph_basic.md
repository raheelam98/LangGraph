#### Gemini_API_python

**SDK (Software Development Kit)** 
An SDK is a set of tools to build software for a particular platform. These tools also allow an app developer to build an app which can integrate with another program

```bash
!pip install -q -U google-generativeai
```

* **! (exclamation mark)**   Linux Command start with exclamation mark  .
* **pip install**   installs Python packages   .
* **-q**   (quiet mode) less verbose  .
* **-U**   (upgrade) latest version  .
* **google-generativeai**   library, which you can use to interact with Google's AI services programmatically

**Note**  Python is an interpreted scripting language, which means it executes code line by line

pass credential to pass google key
```bash
from google.colab import userdata
userdata.get('secretName')
```

#### pass credential to pass GEMINI_API_KEY
```bash
import google.generativeai as genai

# Or use `os.getenv('GEMINI_API_KEY')` to fetch an environment variable.
GEMINI_API_KEY : str = userdata.get("GEMINI_API_KEY")
genai.configure(api_key=GEMINI_API_KEY)
```
**configure(api_key="value")**  method sets up the authentication for your code by linking it with your API key

#### Use list_models to see the available Gemini models

* `gemini-1.5-flash`: optimized for multi-modal use-cases where speed and cost are important. This should be your go-to model.
* `gemini-1.5-pro`: optimized for high intelligence tasks, the most powerful Gemini model

```bash
# print list of models
for m in genai.list_models():
  if "generateContent" in m.supported_generation_methods:
    print(m.name)
```

Python For Loop
```bash
for var in iterable:
    # statements
    pass
```

#### Generate text from text inputs ( `gemini-1.5-flash` - lightweight, multimodal AI model)
Multimodal processing (handl text, audio, and images)

```bash
from google.generativeai import GenerativeModel
model : GenerativeModel = genai.GenerativeModel("gemini-1.5-flash")
```
GenerativeModel("pass-model") 

**%%time** use magic function ( show the utilization of CPU and time)

**Langchain Schema Type:**

**LLM** - AIMessage

**User** - HumanMessage

**System Instraction** - SystemMessage

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


## Tutorial

## Tutorial

**Gemini_API_python**

[Q3,Q4,Q5 - Class -11: What are AI Agents and What is AI Agentic Architecture](https://www.youtube.com/watch?v=Sj9c5lX2Y6U&t=7196s)

[Q3,Q4,Q5 - Class -12: Introduction to LangGraph. A Simple Graph to understand State, Node & Edges](https://www.youtube.com/watch?v=9eBqA9cQAAc)

**LangChain_with_Gemini**

[Q3,Q4,Q5 - Class -13: Messages Classes, LLM Tool Call and Conditional Edges in LangGraph Explained](https://www.youtube.com/watch?v=Rz4mD3KMBe8)


## Tutorial 

[Google AI Python SDK for the Gemini API](https://pypi.org/project/google-generativeai/)


