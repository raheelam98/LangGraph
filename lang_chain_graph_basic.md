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

## Tutorial

**Gemini_API_python**

[Q3,Q4,Q5 - Class -11: What are AI Agents and What is AI Agentic Architecture](https://www.youtube.com/watch?v=Sj9c5lX2Y6U&t=7196s)

**LangChain_with_Gemini**

[Q3,Q4,Q5 - Class -13: Messages Classes, LLM Tool Call and Conditional Edges in LangGraph Explained](https://www.youtube.com/watch?v=Rz4mD3KMBe8)

## Tutorial 

[Google AI Python SDK for the Gemini API](https://pypi.org/project/google-generativeai/)


