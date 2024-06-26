

# Getting Started

- [Install dependencies](#install-depedencies)
- [Set environment variables](#set-environment-variables)
- [Authenticate against azure](#authenticate-against-azure)
- [Run a basic code sample](#run-a-basic-code-sample)
- [Explore more code snippets](#explore-more-samples)

## 1. Install dependencies

Install Azure AI Inferencing package using the following command:

```
pip install azure-ai-inference
```

## 2. Set environment variables
Get the model endpoint url and use it in the code below by exporting it as an environment variable

```bash
export MODEL_ENDPOINT="<your-model-endpoint-goes-here>"
```

Set model name in an env variable:

```bash
export MODEL_NAME=gpt-4o
```

## 3. Run a basic code sample

This sample demonstrates a basic call to the chat completion API.
It is leveraging your endpoint and key. The call is synchronous.


```python
import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import SystemMessage, UserMessage
from azure.identity import DefaultAzureCredential

endpoint = os.environ["MODEL_ENDPOINT"]
model_name = os.environ["MODEL_NAME"]

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=DefaultAzureCredential(),
)


response = client.complete(
    messages=[
        SystemMessage(content="You are a helpful assistant."),
        UserMessage(content="What is the capital of France?"),
    ],
    model_extras={"model": model_name},
)

print(response.choices[0].message.content)
```


## 4. Explore more samples

For a better user experience, you will want to stream the response
of the model so that the first token shows up early and you avoid waiting for long responses.


### Run a multi-turn conversation

This sample demonstrates a multi-turn conversation with the chat completion API.
When using the model for a chat application, you'll need to manage the history
of that conversation and send the latest messages to the model.


```python
import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import AssistantMessage, SystemMessage, UserMessage
from azure.identity import DefaultAzureCredential

endpoint = os.environ["MODEL_ENDPOINT"]
model_name = os.environ["MODEL_NAME"]

client = ChatCompletionsClient(
    endpoint=endpoint, DefaultAzureCredential(),
)

messages = [
    SystemMessage(content="You are a helpful assistant."),
    UserMessage(content="What is the capital of France?"),
    AssistantMessage(content="The capital of France is Paris."),
    UserMessage(content="What about Spain?"),
]

response = client.complete(messages=messages, model_extras={"model": model_name})

print(response.choices[0].message.content)
```


### Stream the output

For a better user experience, you will want to stream the response
of the model so that the first token shows up early and you avoid waiting for long responses.


```python
import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import SystemMessage, UserMessage
from azure.identity import DefaultAzureCredential

endpoint = os.environ["MODEL_ENDPOINT"]
model_name = os.environ["MODEL_NAME"]

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=DefaultAzureCredential(),
)

response = client.complete(
    stream=True,
    messages=[
        SystemMessage(content="You are a helpful assistant."),
        UserMessage(content="Give me 5 good reasons why I should exercise every day."),
    ],
    model_extras={"model": model_name},
)

for update in response:
    print(update.choices[0].delta.content or "", end="")

client.close()
```

