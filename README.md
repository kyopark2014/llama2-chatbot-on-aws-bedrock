# AWS Bedrock을 이용한 Llama2 Chatbot만들기

[2023.11월부터 AWS Bedrock에서 Llama2](https://aws.amazon.com/ko/blogs/aws/amazon-bedrock-now-provides-access-to-llama-2-chat-13b-model/)를 API 기반으로 사용할수 있게 되었습니다. 여기서는 AWS Bedrock의 Llama2 LLM를 LangChain 기반으로 이용하는 방법에 대해 설명합니다.

```python
HUMAN_PROMPT = "\n\nUser:"
AI_PROMPT = "\n\nAssistant:"

def get_parameter(modelId):
    if modelId == 'meta.llama2-13b-chat-v1': 
        return {
            'max_gen_len': 1024,
	        'top_p': 0.9,
	        'temperature': 0.2
        }
parameters = get_parameter(modelId)

# langchain for bedrock
llm = Bedrock(
    model_id=modelId, 
    client=boto3_bedrock, 
    streaming=True,
    callbacks=[StreamingStdOutCallbackHandler()],
    model_kwargs=parameters)

```
