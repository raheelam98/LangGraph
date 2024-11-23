
  ## langchain-academy  

[LangChain Academy](https://academy.langchain.com/courses/intro-to-langgraph)
  
[langchain-academy - Github](https://github.com/langchain-ai/langchain-academy/tree/main)

[langgraph/course-notebooks](https://github.com/panaversity/learn-applied-generative-ai-fundamentals/blob/main/03_langchain_ecosystem/langgraph/course-notebooks/module-0/basics.ipynb)

[Class-01: Mastering LangGraph In a New Way: Core Concepts & Implementation Node, Edges, States - Nov 8, 2024](https://www.youtube.com/watch?v=jIX9P12IkQM)

## Moduel 0
```bash
%%capture --no-stderr
!pip install -q langchain_google_genai langchain_core langchain_community tavily-python
```

**Description of Libraries**
* `langchain-google-genai:` LangChain doesn't have its own chat model, so we use Google's Gemini.
* `langchain-core`: Utilizes LangChain core libraries, e.g., human-message, ai-message.
* `langchain_community:` Tavily-python is part of the LangChain community package.
* `tavily-python: `Used for web search.
Note: Use both langchain_community and tavily-python together.

```bash
from langchain_google_genai import ChatGoogleGenerativeAI
from google.colab import userdata

# Get the GEMINI API key from user data
GEMINI_API_KEY = userdata.get('GEMINI_API_KEY')

# Initialize the ChatGoogleGenerativeAI with the Gemini model
llm = ChatGoogleGenerativeAI(
    model="gemini-1.5-flash",
    api_key=GEMINI_API_KEY,
    temperature=0
)

# Invoke the LLM with a query
result = llm.invoke("Rain is expected in Karachi starting today?")
result
```

**Description of LLM methods**
* `stream`: Stream back chunks of the response (useful for streaming results).
* `invoke`: Call the chain on an input (provides a complete result from start to end).

#### Search Tools - TAVILY

* TAVILY is a search engine created by the LangChain open community. It provides the latest accurate data from the web.
* LLMs do not have access to real-time information, e.g., weather forecasts.
* LLMs do not have the latest information. That's why we use TAVILY to access real-time information, e.g., weather forecasts.

```bash
import os
from google.colab import userdata

# Get the TAVILY API key from user data and set the environment variable
TAVILY_API_KEY = userdata.get('TAVILY_API_KEY')
os.environ["TAVILY_API_KEY"] = TAVILY_API_KEY
```
## Moduel 2

  
