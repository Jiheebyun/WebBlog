<details>
  <summary>AI</summary>
  
### what a prompt template
사용자의 입력을 LLM에게 전달하여 형식 또는 문장구조를 의미

### Chat Model
대화 형식으로 입력과 출력을 주고 받는 구조

### Chain
단독으로 LLM을 사용하는것은 간단한 작업으로는 괜찮지만, 복잡한 작업에는 여러가지 LLM을 호출을 체인처런 연결해야 할 필요가 있음 즉, 체인은 여러 구성요소를 순서대로 연결한 흐름대로 작업을 묶어서 순서대로 처리할수 있게 하는 구조.


```python
import os 
from dotenv import load_dotenv
from langchain_core.prompts import PromptTemplate
from langchain_openai import ChatOpenAI

information = """Elon Reeve Musk (/ˈiːlɒn/ EE-lon; born June 28, 1971) is a businessman. He is known for his leadership of Tesla, SpaceX, X (formerly Twitter), and the Department of Government Efficiency (DOGE). Musk has been considered the wealthiest person in the world since 2021; as of May 2025, Forbes estimates his net worth to be US$424.7 billion."""
print(os.environ.get("OPENAI_API_KEY"))
if __name__ == '__main__':
    print('HI')
    summery_template = """
        given the information {information} about a person from i want you to create:
        1. a short summery
        2. two interesting facts about them
    """

    summery_prompt_template = PromptTemplate(
        input_variables=["information"], template=summery_template
    )
    
    llm = ChatOpenAI(temperature=0, model_name="gpt-3.5-turbo")

    chain = summery_prompt_template | llm
    res = chain.invoke(input={"information": information})

    print(res)

```




```python

```


  
</details>