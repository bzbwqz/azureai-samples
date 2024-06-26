

# Getting Started

- [Create a personal access token](#create-a-personal-access-token)
- [Set environment variables](#set-environment-variables)
- [Run a basic code sample](#run-a-basic-code-sample)
- [Explore more code snippets](#explore-more-samples)

## 1. Create a personal access token

You'll need this to enable free API access. The token will be sent to a Microsoft service but does not need any permissions.

## 2. Set environment variables
Update or create an environment variable to set your token as the key for the client code.

```bash
export TOKEN="<your-key-goes-here>"

```
Get the model endpoint url and use it in the code below by exporting it as an environment variable

```bash
export MODEL_ENDPOINT="<your-model-endpoint-goes-here>"
```

Set model name in an env variable:

```bash
export MODEL_NAME=mistral-large
```

## 3. Call basic code sample

Paste the following into a shell:


```console
curl -X GET "$MODEL_ENDPOINT/completions" \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer $TOKEN" \
    -d '{
        "messages": [
            {
                "role": "system",
                "content": "You are a helpful assistant."
            },
            {
                "role": "user",
                "content": "What is the capital of France?"
            }
        ],
        "model": $MODEL_NAME
    }'
```


## 4. Explore more samples


### Run a multi-turn conversation

Call the chat completion API and pass the chat history:


```console
curl -X GET "$MODEL_ENDPOINT/completions" \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer $TOKEN" \
    -d '{
        "messages": [
            {
                "role": "system",
                "content": "You are a helpful assistant."
            },
            {
                "role": "user",
                "content": "What is the capital of France?"
            },
            {
                "role": "assistant",
                "content": "The capital of France is Paris."
            },
            {
                "role": "user",
                "content": "What about Spain?"
            }
        ],
        "model": $MODEL_NAME
    }'
```


### Stream the output

This is an example of calling the endpoint and streaming the response.


```console
curl -X GET "$MODEL_ENDPOINT/completions" \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer $TOKEN" \
    -d '{
        "messages": [
            {
                "role": "system",
                "content": "You are a helpful assistant."
            },
            {
                "role": "user",
                "content": "Give me 5 good reasons why I should exercise every day."
            }
        ],
        "stream": true,
        "model": $MODEL_NAME
    }'
```

