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

아직(2023/11/14) Boto3에서 llama2에 대해 지원안하고 있어서 쓸수 없군요. 

```text
[ERROR] ValueError: Error raised by bedrock service: An error occurred (ValidationException) when calling the InvokeModel operation: Malformed input request: 2 schema violations found, please reformat your input and try again.
Traceback (most recent call last):
  File "/var/lang/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1204, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1176, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1147, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 690, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 940, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/var/task/lambda_function.py", line 87, in <module>
    msg = llm(HUMAN_PROMPT+prompt+AI_PROMPT)
  File "/var/lang/lib/python3.11/site-packages/langchain/llms/base.py", line 876, in __call__
    self.generate(
  File "/var/lang/lib/python3.11/site-packages/langchain/llms/base.py", line 656, in generate
    output = self._generate_helper(
  File "/var/lang/lib/python3.11/site-packages/langchain/llms/base.py", line 544, in _generate_helper
    raise e
  File "/var/lang/lib/python3.11/site-packages/langchain/llms/base.py", line 531, in _generate_helper
    self._generate(
  File "/var/lang/lib/python3.11/site-packages/langchain/llms/base.py", line 1053, in _generate
    self._call(prompt, stop=stop, run_manager=run_manager, **kwargs)
  File "/var/lang/lib/python3.11/site-packages/langchain/llms/bedrock.py", line 405, in _call
    return self._prepare_input_and_invoke(prompt=prompt, stop=stop, **kwargs)
  File "/var/lang/lib/python3.11/site-packages/langchain/llms/bedrock.py", line 258, in _prepare_input_and_invoke
    raise ValueError(f"Error raised by bedrock service: {e}")
```
