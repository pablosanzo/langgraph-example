# QuickStart: Launch Local LangGraph Server
[](https://github.com/langchain-ai/langgraph/edit/main/docs/docs/tutorials/langgraph-platform/local-server.md "Edit this page")

This is a quick start guide to help you get a LangGraph app up and running locally.

Requirements

*   Python >= 3.11
*   [LangGraph CLI](https://langchain-ai.github.io/langgraph/cloud/reference/cli/): Requires langchain-cli\[inmem\] >= 0.1.58

Install the LangGraph CLI
-------------------------------------------------------------------------

```
pip install --upgrade "langgraph-cli[inmem]"

```


🌱 Create a LangGraph App
----------------------------------------------------------------------

Create a new app from the `react-agent` template. This template is a simple agent that can be flexibly extended to many tools.

Python ServerNode Server

```
langgraph new path/to/your/app --template react-agent-python 

```


```
langgraph new path/to/your/app --template react-agent-js

```


Additional Templates

If you use `langgraph new` without specifying a template, you will be presented with an interactive menu that will allow you to choose from a list of available templates.

Install Dependencies
---------------------------------------------------------------

In the root of your new LangGraph app, install the dependencies in `edit` mode so your local changes are used by the server:

Create a `.env` file
------------------------------------------------------------

You will find a `.env.example` in the root of your new LangGraph app. Create a `.env` file in the root of your new LangGraph app and copy the contents of the `.env.example` file into it, filling in the necessary API keys:

```
LANGSMITH_API_KEY=lsv2...
TAVILY_API_KEY=tvly-...
ANTHROPIC_API_KEY=sk-
OPENAI_API_KEY=sk-...

```


Get API Keys

*   **LANGSMITH\_API\_KEY**: Go to the [LangSmith Settings page](https://smith.langchain.com/settings). Then clck **Create API Key**.
*   **ANTHROPIC\_API\_KEY**: Get an API key from [Anthropic](https://console.anthropic.com/).
*   **OPENAI\_API\_KEY**: Get an API key from [OpenAI](https://openai.com/).
*   **TAVILY\_API\_KEY**: Get an API key on the [Tavily website](https://app.tavily.com/).

🚀 Launch LangGraph Server
------------------------------------------------------------------------

This will start up the LangGraph API server locally. If this runs successfully, you should see something like:

> Ready!
> 
> *   API: [http://localhost:2024](http://localhost:2024/)
>     
> *   Docs: [http://localhost:2024/docs](http://localhost:2024/docs)
>     
> *   LangGraph Studio Web UI: [https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024](https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024)
>     

In-Memory Mode

The `langgraph dev` command starts LangGraph Server in an in-memory mode. This mode is suitable for development and testing purposes. For production use, you should deploy LangGraph Server with access to a persistent storage backend.

If you want to test your application with a persistent storage backend, you can use the `langgraph up` command instead of `langgraph dev`. You will need to have `docker` installed on your machine to use this command.

LangGraph Studio Web UI
---------------------------------------------------------------------

LangGraph Studio Web is a specialized UI that you can connect to LangGraph API server to enable visualization, interaction, and debugging of your application locally. Test your graph in the LangGraph Studio Web UI by visiting the URL provided in the output of the `langgraph dev` command.

> *   LangGraph Studio Web UI: [https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024](https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024)

Connecting to a server with a custom host/port

If you are running the LangGraph API server with a custom host / port, you can point the Studio Web UI at it by changing the `baseUrl` URL param. For example, if you are running your server on port 8000, you can change the above URL to the following:

```
https://smith.langchain.com/studio/baseUrl=http://127.0.0.1:8000

```


Safari Compatibility

Currently, LangGraph Studio Web does not support Safari when running a server locally.

Test the API
-----------------------------------------------

Python SDK (Async)Python SDK (Sync)Javascript SDKRest API

**Install the LangGraph Python SDK**

```
pip install langgraph-sdk

```


**Send a message to the assistant (threadless run)**

```
from langgraph_sdk import get_client

client = get_client(url="http://localhost:2024")

async for chunk in client.runs.stream(
    None,  # Threadless run
    "agent", # Name of assistant. Defined in langgraph.json.
    input={
        "messages": [{
            "role": "human",
            "content": "What is LangGraph?",
        }],
    },
    stream_mode="updates",
):
    print(f"Receiving new event of type: {chunk.event}...")
    print(chunk.data)
    print("\n\n")

```


**Install the LangGraph Python SDK**

```
pip install langgraph-sdk

```


**Send a message to the assistant (threadless run)**

```
from langgraph_sdk import get_sync_client

client = get_sync_client(url="http://localhost:2024")

for chunk in client.runs.stream(
    None,  # Threadless run
    "agent", # Name of assistant. Defined in langgraph.json.
    input={
        "messages": [{
            "role": "human",
            "content": "What is LangGraph?",
        }],
    },
    stream_mode="updates",
):
    print(f"Receiving new event of type: {chunk.event}...")
    print(chunk.data)
    print("\n\n")

```


**Install the LangGraph JS SDK**

```
npm install @langchain/langgraph-sdk

```


**Send a message to the assistant (threadless run)**

```
const { Client } = await import("@langchain/langgraph-sdk");

// only set the apiUrl if you changed the default port when calling langgraph dev
const client = new Client({ apiUrl: "http://localhost:2024"});

const streamResponse = client.runs.stream(
    null, // Threadless run
    "agent", // Assistant ID
    {
        input: {
            "messages": [
                { "role": "user", "content": "What is LangGraph?"}
            ]
        },
        streamMode: "messages",
    }
);

for await (const chunk of streamResponse) {
    console.log(`Receiving new event of type: ${chunk.event}...`);
    console.log(JSON.stringify(chunk.data));
    console.log("\n\n");
}

```


```
curl -s --request POST \
    --url "http://localhost:2024/runs/stream" \
    --header 'Content-Type: application/json' \
    --data "{
        \"assistant_id\": \"agent\",
        \"input\": {
            \"messages\": [
                {
                    \"role\": \"human\",
                    \"content\": \"What is LangGraph?\"
                }
            ]
        },
        \"stream_mode\": \"updates\"
    }" 

```


Auth

If you're connecting to a remote server, you will need to provide a LangSmith API Key for authorization. Please see the API Reference for the clients for more information.

Next Steps
-------------------------------------------

Now that you have a LangGraph app running locally, take your journey further by exploring deployment and advanced features:

### 🌐 Deploy to LangGraph Cloud

*   **[LangGraph Cloud Quickstart](https://langchain-ai.github.io/langgraph/cloud/quick_start/)**: Deploy your LangGraph app using LangGraph Cloud.

### 📚 Learn More about LangGraph Platform

Expand your knowledge with these resources:

*   **[LangGraph Platform Concepts](https://langchain-ai.github.io/langgraph/concepts/#langgraph-platform)**: Understand the foundational concepts of the LangGraph Platform.
*   **[LangGraph Platform How-to Guides](https://langchain-ai.github.io/langgraph/how-tos/#langgraph-platform)**: Discover step-by-step guides to build and deploy applications.

### 🛠️ Developer References

Access detailed documentation for development and API usage:

*   **[LangGraph Server API Reference](https://langchain-ai.github.io/langgraph/cloud/reference/api/api_ref.html)**: Explore the LangGraph Server API documentation.
*   **[Python SDK Reference](https://langchain-ai.github.io/langgraph/cloud/reference/sdk/python_sdk_ref/)**: Explore the Python SDK API Reference.
*   **[JS/TS SDK Reference](https://langchain-ai.github.io/langgraph/cloud/reference/sdk/js_ts_sdk_ref/)**: Explore the Python SDK API Reference.