# Langchain库的使用

## 1，调用模型

（1）导入模块

```
from langchain_openai import ChatOpenAI
import os
from dotenv import load_dotenv, find_dotenv
```

（2）创建`.env`文件，存储秘钥

```
DEEPSEEK_API_KEY=**************
```

（3）调用模型

```
# 加载 .env 文件中的环境变量
load_dotenv(find_dotenv())

# 创建 ChatOpenAI 实例并传入 API 密钥
chat = ChatOpenAI(
    api_key=os.getenv("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com/v1",
    temperature=0.0, # 越低越严谨，越高越开放
    model='deepseek-chat'
)
```

（4）完整示例

```
from langchain_openai import ChatOpenAI
import os
from dotenv import load_dotenv, find_dotenv

# 加载 .env 文件中的环境变量
load_dotenv(find_dotenv())

# 创建 ChatOpenAI 实例并传入 API 密钥
chat = ChatOpenAI(
    api_key=os.getenv("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com/v1",
    temperature=0.0,
    model='deepseek-chat'
)

message = ("写一首歌")

response = chat.invoke(message)  # invoke()会将输入的消息列表传递给模型，进行推理，生成一个响应。
print(response.content)  # .content 属性获取最终的文本输出
```



## 2,提示模版

### (1),PromptTemplate

（1）导入模块

```
from langchain.prompts import PromptTemplate
```

（2）构造提示词模版

```
# 定义一个提示词模板，动态构造输入
prompt = PromptTemplate(
    input_variables=["theme"], # 输入变量
    template="请写一首关于{theme}的诗" # 模版
)
```

（3）用户输入

```
# 用户给出一个主题
user_theme = input("请输入你想要的诗歌主题：")
```

（4）用模版生成提示词

```
# 用模板生成提示词
message = prompt.format(theme=user_theme)
```

（5）示例

```
from langchain_openai import ChatOpenAI
import os
from dotenv import load_dotenv, find_dotenv
from langchain.prompts import PromptTemplate

# 加载 .env 文件中的环境变量
load_dotenv(find_dotenv())

# 创建 ChatOpenAI 实例并传入 API 密钥
chat = ChatOpenAI(
    api_key=os.getenv("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com/v1",
    temperature=0.0,
    model='deepseek-chat'
)

# 定义一个提示词模板
prompt = PromptTemplate(
    input_variables=["theme"],
    template="请写一首关于{theme}的诗"
)

# 用户给出一个主题
user_theme = input("请输入你想要的诗歌主题：")

# 用模板生成提示词
message = prompt.format(theme=user_theme)

response = chat.invoke(message)  # invoke() 会将输入的消息列表传递给模型，进行推理，生成一个响应。
print(response.content)  # .content 属性获取最终的文本输出
```

### (2),聊天提示模版-ChatPromptTemplate

- **用途**：专为聊天模型（如 GPT-4、DeepSeek Chat）设计。
- **输出**：生成结构化消息列表（包含角色标记，如 `system`/`user`/`ai`）

（1）导入

```
from langchain.prompts import ChatPromptTemplate
```

（2）构造聊天提示词模版

```
# 创建聊天提示模板
prompt = ChatPromptTemplate.from_messages([
    ("system", "你是一位才华横溢的诗人"),
    ("user", "请写一首关于{theme}的诗")
])
```

（3）示例

```
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from dotenv import load_dotenv, find_dotenv
import os

# 载入 API 密钥
load_dotenv(find_dotenv())
api_key = os.getenv("DEEPSEEK_API_KEY")

# 创建模型
chat = ChatOpenAI(
    api_key=api_key,
    base_url="https://api.deepseek.com/v1",
    temperature=0.5,
    model="deepseek-chat"
)

# 创建聊天提示模板
prompt = ChatPromptTemplate.from_messages([ # from_messages多消息结构化提示
    ("system", "你是一位才华横溢的诗人"),
    ("user", "请写一首关于{theme}的诗")
])


# 使用|连接提示和模型
poem_chain = prompt | chat

# 用户输入
user_input = input("请输入诗的主题：")

# 调用链，传入变量字典，自动构建提示 + 调用模型
response = poem_chain.invoke({"theme": user_input})

# 输出结果
print("\n🌸 AI 为你写的诗：\n")
print(response.content)
```





## 3，链 

### (1),Runnable

（1）无需显式调用

（2）使用|链接

```
poem_chain = prompt | chat
```

（3）调用链，传入变量字典，自动构建提示 + 调用模型

```
response = poem_chain.invoke({"theme": user_input})
```

（4）示例

```
from langchain_openai import ChatOpenAI
from langchain.prompts import PromptTemplate
from dotenv import load_dotenv, find_dotenv
import os

# 载入 API 密钥
load_dotenv(find_dotenv())
api_key = os.getenv("DEEPSEEK_API_KEY")

# 创建模型
chat = ChatOpenAI(
    api_key=api_key,
    base_url="https://api.deepseek.com/v1",
    temperature=0.5,
    model="deepseek-chat"
)

# 创建提示模板
prompt = PromptTemplate(
    input_variables=["theme"],
    template="请写一首关于{theme}的诗"
)

# 使用|连接提示和模型
poem_chain = prompt | chat

# 用户输入
user_input = input("请输入诗的主题：")

# 调用链，传入变量字典，自动构建提示 + 调用模型
response = poem_chain.invoke({"theme": user_input})

# 输出结果
print("\n🌸 AI 为你写的诗：\n")
print(response.content)
```

### (2),直通链-RunnablePassthrough

（1）导入模块

```
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough
```

（2）构建prompt模块

```
# Prompt 模板
prompt1 = ChatPromptTemplate.from_template("将以下文本内容翻译为简体中文: {text}")
prompt2 = ChatPromptTemplate.from_template("用10字以内的一句话总结以下文本: {文本翻译}")
prompt3 = ChatPromptTemplate.from_template("根据总结写一个简短回复，总结: {文本总结}")
```

（3）构建链

```
# 构建 LCEL 链，全显示写法，不推荐
runnable_chain = RunnablePassthrough.assign(
    文本翻译=(itemgetter("text") | prompt1 | chat | StrOutputParser())
).assign(
    文本总结=(itemgetter("文本翻译") | prompt2 | chat | StrOutputParser())
).assign(
    回复=( itemgetter("文本总结") | prompt3 | chat | StrOutputParser())
)

# 构建 LCEL链，依赖自动匹配
runnable_chain = RunnablePassthrough.assign(
    文本翻译= prompt1 | chat | StrOutputParser()
).assign(
    文本总结= prompt2 | chat | StrOutputParser()
).assign(
    回复= prompt3 | chat | StrOutputParser()
)
```

`assign` 方法用于向输入字典中添加新的键值对。在这里，我们创建了一个名为 `文本翻译` 的新键。等号右边的部分定义了如何生成这个键对应的值：

- `itemgetter("text")`: 从输入字典 `{"text": ...}` 中提取 `text` 的值。
- `| prompt1`: 将提取的 `text` 传递给 `prompt1` 进行格式化。
- `| chat`: 将格式化后的 prompt 发送给 `chat` 模型获取回复。
- `| StrOutputParser()`: 将 `chat` 模型的输出解析成一个字符串。 因此，`文本翻译` 的值就是原始文本的简体中文翻译。
- `prompt3` 的模板变量名 `{文本总结}`
  会自动匹配字典中的同名字段（LangChain 的智能特性）

（4）完整示例

```
import os
import time
from dotenv import load_dotenv, find_dotenv
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough

# 基本设置
load_dotenv(find_dotenv())
chat = ChatOpenAI(
    api_key=os.getenv("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com/v1",
    temperature=0.0,
    model='deepseek-reasoner'
)

# Prompt 模板
prompt1 = ChatPromptTemplate.from_template("将以下文本内容翻译为简体中文: {text}")
prompt2 = ChatPromptTemplate.from_template("用10字以内的一句话总结以下文本: {文本翻译}")
prompt3 = ChatPromptTemplate.from_template("作为学生根据总结写一个简短回复，总结: {文本总结}")

# 构建 LCEL 链
runnable_chain = RunnablePassthrough.assign(
    文本翻译= prompt1 | chat | StrOutputParser()
).assign(
    文本总结= prompt2 | chat | StrOutputParser()
).assign(
    回复= prompt3 | chat | StrOutputParser()
)

# 输入文本
text = ("I won't approve any leave requests for volunteering during the seventh and eighth periods. We really can't manage without you. If it's absolutely "
        "necessary, have the teacher in charge contact me directly, or ask the supervising teacher there to approve your leave slip")

# 执行与计时
start_time = time.time()
result = runnable_chain.invoke({"text": text})
end_time = time.time()
t = end_time - start_time

# 打印结果
print(f"翻译结果：\n{result['文本翻译']}")
print(f"\n文本总结：\n{result['文本总结']}")
print(f"\n生成的回复：\n{result['回复']}")
print(f"\n响应时间：\n{t:.4f} 秒")
```

（5）非顺序链示例

```
import os
import time
from dotenv import load_dotenv, find_dotenv
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough

# 基本设置
load_dotenv(find_dotenv())
chat = ChatOpenAI(
    api_key=os.getenv("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com/v1",
    temperature=0.0,
    model='deepseek-chat'
)

# Prompt 模板
prompt1 = ChatPromptTemplate.from_template("将{text}翻译为简体中文，回复的时候尽量简洁") #from_template单消息结构化提示
prompt2 = ChatPromptTemplate.from_template("用一句话总结{文本翻译}，回复的时候尽量简洁")
prompt3 = ChatPromptTemplate.from_template("分析{文本翻译}中的情绪，用两个中文单词表示，回复的时候尽量简洁")
prompt4 = ChatPromptTemplate.from_template("假设你是一个学生，根据{文本翻译}和{情绪检测}中的内容，生成一个简短的回复，回复的时候尽量简洁")

# 构建 LCEL 链
runnable_chain = RunnablePassthrough.assign(
    文本翻译= prompt1 | chat | StrOutputParser()
).assign(
    文本总结= prompt2 | chat | StrOutputParser()
).assign(
    情绪检测= prompt3 | chat | StrOutputParser()
).assign(
    回复= prompt4 | chat | StrOutputParser()
)

# 输入文本
text = ("I won't approve any leave requests for volunteering during the seventh and eighth periods. We really can't manage without you. If it's absolutely "
        "necessary, have the teacher in charge contact me directly, or ask the supervising teacher there to approve your leave slip")

# 执行与计时
start_time = time.time()
result = runnable_chain.invoke({"text": text})
end_time = time.time()
t = end_time - start_time

# 打印结果
print(f"翻译结果：\n{result['文本翻译']}")
print(f"\n文本总结：\n{result['文本总结']}")
print(f"\n情绪分析：\n{result['情绪检测']}")
print(f"\n生成的回复：\n{result['回复']}")
print(f"\n响应时间：\n{t:.4f} 秒")
```

### (3),并行链-RunnableParallel

并行处理多个相互独立的任务，所有任务接受相同的原始输入，不保留中间数据

```
import os
import time
from dotenv import load_dotenv, find_dotenv
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnableParallel

# 基本设置
load_dotenv(find_dotenv())
chat = ChatOpenAI(
    api_key=os.getenv("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com/v1",
    temperature=0.0,
    model='deepseek-reasoner'
)

# Prompt 模板
prompt1 = ChatPromptTemplate.from_template("分析{text}中的情绪，用两个中文单词表示，回复的时候尽量简洁") #from_template单消息结构化提示
prompt2 = ChatPromptTemplate.from_template("根据{text}写一个20字以内的文本总结，回复的时候尽量简洁")
prompt3 = ChatPromptTemplate.from_template("作为学生根据{text}写一个简短的回复，回复的时候尽量简洁")

# 构建 LCEL 链
runnable_chain = RunnableParallel(
    文本总结= prompt2 | chat | StrOutputParser(),
    情绪检测= prompt1 | chat | StrOutputParser(),
    回复= prompt3 | chat | StrOutputParser()
)

# 输入文本
text = ("今天中国文化共同体缺旷的学生，今天之内私聊我告诉我原因。并且于每人下周之前写一份1000字检讨交给我。@全体成员")

# 执行与计时
start_time = time.time()
result = runnable_chain.invoke({"text": text})
end_time = time.time()
t = end_time - start_time

# 打印结果
print(f"\n文本总结：\n{result['文本总结']}")
print(f"\n情绪分析：\n{result['情绪检测']}")
print(f"\n生成的回复：\n{result['回复']}")
print(f"\n响应时间：\n{t:.4f} 秒")
```

### (4),路由(分类)链—RunnableLambda

`RunnableLambda` 是 `langchain` 中的一种构造，它允许你将函数包装成一个可执行的可调用对象，使得这个函数可以作为一个“可运行的链”来调用。

（1），定义模版,需要有一个默认模版

```
# 2，定义模版

physics_template = """你是一个非常聪明的物理专家。 \
你擅长用一种简洁并且易于理解的方式去回答问题。\
当你不知道问题的答案时，你承认\
你不知道.

具体问题如下:
{input}"""

math_template = """你是一个非常优秀的数学家。 \
你擅长回答数学问题。 \
你之所以如此优秀， \
是因为你能够将棘手的问题分解为组成部分，\
回答组成部分，然后将它们组合在一起，回答更广泛的问题。

具体问题如下：
{input}"""

history_template = """你是以为非常优秀的历史学家。 \
你对一系列历史时期的人物、事件和背景有着极好的学识和理解\
你有能力思考、反思、辩证、讨论和评估过去。\
你尊重历史证据，并有能力利用它来支持你的解释和判断。

具体问题如下:
{input}"""

computer_template = """ 你是一个成功的计算机科学专家。\
你有创造力、协作精神、\
前瞻性思维、自信、解决问题的能力、\
对理论和算法的理解以及出色的沟通技巧。\
你非常擅长回答编程问题。\
你之所以如此优秀，是因为你知道  \
如何通过以机器可以轻松解释的命令式步骤描述解决方案来解决问题，\
并且你知道如何选择在时间复杂性和空间复杂性之间取得良好平衡的解决方案。

具体问题如下:
{input}"""

other_template = """你是一个ai小助手，\
回答以下问题。\

具体问题如下：
{input}"""
```

（2），定义模版链

```
# 3，定义链

physics_chain = ChatPromptTemplate.from_template(physics_template) | chat | StrOutputParser()

math_chain = ChatPromptTemplate.from_template(math_template) | chat | StrOutputParser()

history_chain = ChatPromptTemplate.from_template(history_template) | chat | StrOutputParser()

computer_chain = ChatPromptTemplate.from_template(computer_template) | chat | StrOutputParser()

other_chain = ChatPromptTemplate.from_template(other_template) | chat | StrOutputParser()
```

（3），定义分类模版和分类链

```
# 4,定义分类模版和分类链
classify_prompt_template = """
请你对以下问题分类分类为"物理"、"数学"、"历史"、"计算机"、"其他"，中的一个，不需要返回多个分类，返回一个即可。
具体问题如下：
{input}"""

classify_chain = ChatPromptTemplate.from_template(classify_prompt_template) | chat | StrOutputParser()
```

（4），使用RunnableLambda构建路由链(分类链)

```
# 5,使用RunnableLambda构建路由链

def route_classification(text):
    # 获取分类结果
    classification = classify_chain.invoke({"input": text})

    # 根据分类结果选择对应的链
    if "物理" in classification:
        return {"category": "物理", "chain": physics_chain}
    elif "数学" in classification:
        return {"category": "数学", "chain": math_chain}
    elif "历史" in classification:
        return {"category": "历史", "chain": history_chain}
    elif "计算机" in classification:
        return {"category": "计算机", "chain": computer_chain}
    else:
        return {"category": "其他", "chain": other_chain}

runnable_chain = RunnableLambda(route_classification)
```

（5），获取输出

```
# 6,获取输出

# 调用并获取分类和链
text = "推荐一下数据结构和算法的学习书籍"
result = runnable_chain.invoke(text)

category = result["category"]
chain = result["chain"]

# 使用分类对应的链进行处理，得到实际回答
response = chain.invoke({"input": text})

# 打印分类和回答
print(f"问题类型: {category}")
print(f"回答: {response}")
```

**`route_classification`** 返回的是一个包含 `category` 和 `chain` 的字典。`category` 是问题的分类，`chain` 是对应的处理链。

（6），完整示例

```
import os
from dotenv import load_dotenv, find_dotenv
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnableLambda

# 1,加载模型

# 加载 .env 文件中的环境变量
load_dotenv(find_dotenv())

# 创建 ChatOpenAI 实例并传入 API 密钥
chat = ChatOpenAI(
    api_key=os.getenv("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com/v1",
    temperature=0.0,
    model='deepseek-chat'
)

# 2，定义模版

physics_template = """你是一个非常聪明的物理专家。 \
你擅长用一种简洁并且易于理解的方式去回答问题。\
当你不知道问题的答案时，你承认\
你不知道.

具体问题如下:
{input}"""

math_template = """你是一个非常优秀的数学家。 \
你擅长回答数学问题。 \
你之所以如此优秀， \
是因为你能够将棘手的问题分解为组成部分，\
回答组成部分，然后将它们组合在一起，回答更广泛的问题。

具体问题如下：
{input}"""

history_template = """你是以为非常优秀的历史学家。 \
你对一系列历史时期的人物、事件和背景有着极好的学识和理解\
你有能力思考、反思、辩证、讨论和评估过去。\
你尊重历史证据，并有能力利用它来支持你的解释和判断。

具体问题如下:
{input}"""

computer_template = """ 你是一个成功的计算机科学专家。\
你有创造力、协作精神、\
前瞻性思维、自信、解决问题的能力、\
对理论和算法的理解以及出色的沟通技巧。\
你非常擅长回答编程问题。\
你之所以如此优秀，是因为你知道  \
如何通过以机器可以轻松解释的命令式步骤描述解决方案来解决问题，\
并且你知道如何选择在时间复杂性和空间复杂性之间取得良好平衡的解决方案。

具体问题如下:
{input}"""

other_template = """你是一个ai小助手，\
回答以下问题。\

具体问题如下：
{input}"""

# 3，定义链

physics_chain = ChatPromptTemplate.from_template(physics_template) | chat | StrOutputParser()

math_chain = ChatPromptTemplate.from_template(math_template) | chat | StrOutputParser()

history_chain = ChatPromptTemplate.from_template(history_template) | chat | StrOutputParser()

computer_chain = ChatPromptTemplate.from_template(computer_template) | chat | StrOutputParser()

other_chain = ChatPromptTemplate.from_template(other_template) | chat | StrOutputParser()

# 4,定义分类模版和分类链
classify_prompt_template = """
请你对以下问题分类分类为"物理"、"数学"、"历史"、"计算机"、"其他"，中的一个，不需要返回多个分类，返回一个即可。
具体问题如下：
{input}"""

classify_chain = ChatPromptTemplate.from_template(classify_prompt_template) | chat | StrOutputParser()

# 5,使用RunnableLambda构建路由链

def route_classification(text):
    # 获取分类结果
    classification = classify_chain.invoke({"input": text})

    # 根据分类结果选择对应的链
    if "物理" in classification:
        return {"category": "物理", "chain": physics_chain}
    elif "数学" in classification:
        return {"category": "数学", "chain": math_chain}
    elif "历史" in classification:
        return {"category": "历史", "chain": history_chain}
    elif "计算机" in classification:
        return {"category": "计算机", "chain": computer_chain}
    else:
        return {"category": "其他", "chain": other_chain}

runnable_chain = RunnableLambda(route_classification)

# 6,获取输出

# 调用并获取分类和链
text = "推荐一下数据结构和算法的学习书籍"
result = runnable_chain.invoke(text)

category = result["category"]
chain = result["chain"]

# 使用分类对应的链进行处理，得到实际回答
response = chain.invoke({"input": text})

# 打印分类和回答
print(f"问题类型: {category}")
print(f"回答: {response}")
```



## 4，构建文档检索问答系统

### (1),基于向量检索

（1）**文档加载器 (`TextLoader`)**

- **作用**：将存储在本地的文本文件加载到程序中，作为检索的文档源。

```\
from langchain_community.document_loaders import TextLoader  # text文档加载器

# 文档加载器，指定文件路径及编码方式，加载文本文件
loader = TextLoader(file_path="Clothing_Introduction.txt", encoding="utf-8")  # 指定UTF-8编码
```

（2）**嵌入模型 (`HuggingFaceEmbeddings`)**

- **作用**：将加载的文档转换为嵌入向量，便于在高维空间中进行相似度计算，以实现高效的文档检索。

```
from langchain_huggingface import HuggingFaceEmbeddings

# 使用 Hugging Face 的嵌入模型，将文本转换为嵌入向量
embedding = HuggingFaceEmbeddings(model_name='sentence-transformers/all-MiniLM-L6-v2')
```

（3）**向量索引 (`VectorstoreIndexCreator`)**

- **作用**：根据嵌入向量创建一个向量数据库，用于高效的相似度检索。

```
# 创建向量索引，使用文档加载器加载的文档，转换为向量并存储
index = VectorstoreIndexCreator(
    vectorstore_cls=DocArrayInMemorySearch,  # 向量存储类
    embedding=embedding  # 嵌入模型
).from_loaders([loader])  # 使用加载器加载文档
```

（4）**检索器 (`Retriever`)**

- **作用**：根据用户提出的问题，从向量数据库中检索出与问题相关的文档片段。

```
# 将向量存储转换为检索器，用于根据向量进行文档检索
retriever = index.vectorstore.as_retriever()
```

（5） **处理链 (`RetrievalQA`)**

- **作用**：将聊天模型与检索器结合，形成一个问答流程。它先从数据库中检索相关文档片段，然后将这些片段传递给聊天模型以生成答案。

```
# 创建检索问答链，结合聊天模型和检索器
qa_stuff = RetrievalQA.from_chain_type(
    llm=chat,  # 使用的语言模型
    chain_type="stuff",  # 检索方式："stuff" 类型的链，先检索相关信息再生成答案
    retriever=retriever  # 检索器，用于从文档中检索信息
)

```

（6）完整示例

```
import os
from dotenv import load_dotenv, find_dotenv
from langchain_openai import ChatOpenAI
from langchain.chains import RetrievalQA  # 用于实现基于文档检索的问答链。
from langchain_community.document_loaders import TextLoader  # 文档加载器
from langchain_community.vectorstores import DocArrayInMemorySearch  # 用于在内存中存储文档的向量表示。
from langchain_huggingface import HuggingFaceEmbeddings
from langchain.indexes import VectorstoreIndexCreator # 用于创建和管理向量索引。

# 基本设置：加载环境变量
load_dotenv(find_dotenv())

# 加载聊天模型，指定 API 密钥、模型类型和其他参数
chat = ChatOpenAI(
    openai_api_key=os.getenv("DEEPSEEK_API_KEY"),  # 从环境变量中获取 API 密钥
    openai_api_base="https://api.deepseek.com/v1",  # 使用的 API 基地址
    model="deepseek-reasoner",  # 使用的模型类型
    temperature=0.0  # 设置为 0.0，表示生成的回答更加确定性
)

# 文档加载器，指定文件路径及编码方式，加载文本文件
loader = TextLoader(file_path="Clothing_Introduction.txt", encoding="utf-8")  # 指定UTF-8编码

# 使用 Hugging Face 的嵌入模型，将文本转换为嵌入向量
embedding = HuggingFaceEmbeddings(model_name='sentence-transformers/all-MiniLM-L6-v2')

# 创建向量索引，使用文档加载器加载的文档，转换为向量并存储
index = VectorstoreIndexCreator(
    vectorstore_cls=DocArrayInMemorySearch,  # 向量存储类
    embedding=embedding  # 嵌入模型
).from_loaders([loader])  # 使用加载器加载文档

# 用户的问题
question = "这件衣服有什么特点"

# 将向量存储转换为检索器，用于根据向量进行文档检索
retriever = index.vectorstore.as_retriever()

# 创建检索问答链，结合聊天模型和检索器
qa_stuff = RetrievalQA.from_chain_type(
    llm=chat,  # 使用的语言模型
    chain_type="stuff",  # "stuff" 类型的链，先检索相关信息再生成答案
    retriever=retriever  # 检索器，用于从文档中检索信息
)

# 执行问答任务，传入用户问题并获取模型生成的回答
response = qa_stuff.invoke(question)

# 输出模型的回答
print(f"回复：\n{response['result']}")
```

### (2),基于RAG

（1）导入文档

```
from langchain_community.document_loaders import TextLoader

try:
    loader = TextLoader(
        "Clothing_Introduction.txt",
        encoding="utf-8",
        autodetect_encoding=True  # 添加自动编码检测
    )
    docs = loader.load()
except Exception as e:
    raise RuntimeError(f"文档加载失败: {str(e)}")
```

（2）嵌入模型

```
from langchain_huggingface import HuggingFaceEmbeddings

embedding = HuggingFaceEmbeddings(
    model_name='sentence-transformers/all-MiniLM-L6-v2',
    model_kwargs={'device': 'cpu'},  # 明确指定运行设备
    encode_kwargs={'normalize_embeddings': True} # 归一化向量
)
```

（3）向量索引

```
from langchain_community.vectorstores import FAISS

# 创建向量数据库
vectorstore = FAISS.from_documents(docs, embedding)
```

作用：将文档分割为块 → 生成向量 → 建立高效索引

（4）创建提示模版

```
from langchain.prompts import ChatPromptTemplate

prompt_template = """
你是一个专业的服装知识助手，请严格根据提供的上下文信息回答问题。
如果不知道答案，请如实告知。

上下文信息：
{context}

用户问题：{input}
"""
prompt = ChatPromptTemplate.from_template(prompt_template)
```

（5）创建处理链

```
# 创建检索问答链
from langchain.chains import create_retrieval_chain
from langchain.chains.combine_documents import create_stuff_documents_chain

combine_chain = create_stuff_documents_chain(
    llm=llm,
    prompt=prompt
)

retrieval_chain = create_retrieval_chain(
    retriever=vectorstore.as_retriever(
        search_type="similarity",  # 使用相似度搜索
        search_kwargs={"k": 3}     # 返回前3个相关结果
    ),
    combine_docs_chain=combine_chain
)
```

**`create_stuff_documents_chain`**: 这是一个函数，接受一个 **LLM** 和一个 **prompt** 来创建文档处理链

**`create_retrieval_chain`**: 这也是一个函数，接受 **retriever**（信息检索器）和 **combine_docs_chain**（文档处理链）作为输入，创建一个整体的检索和文档处理流程。其主要作用是通过`retriever`检索相关文档并传递给 `combine_docs_chain` 来处理结果。

`vectorstore` 是一个存储文档向量（通常用于向量搜索）的对象，它能够将用户输入与文档库中的向量进行对比，找出最相关的文档。

`as_retriever(...)` 是一种将 `vectorstore` 转换为检索器的方式。它通过指定搜索类型 `similarity` 来进行相似性搜索，并设置 `search_kwargs={"k": 3}` 来限制检索结果的数量（即检索出与查询最相关的 **3** 个文档）

问：`vectorstore` 本身能够实现文档的 **相似性搜索**，为什么还需要`as_retriever()` 这个转换步骤？

答：虽然 `vectorstore` 可以进行基础的向量检索，但 `as_retriever()` 的主要作用是提供一个更灵活、更统一的接口，便于与其他组件集成，方便参数配置和优化检索过程，并简化与文档处理链（例如 `combine_docs_chain`）的交互。

（6）问答处理

```
def ask_question(question):
    try:
        response = retrieval_chain.invoke({"input": question})
        return response["answer"]
    except Exception as e:
        return f"处理请求时发生错误: {str(e)}"
        
if __name__ == "__main__":
    question = "这件衣服有什么特点？"
    answer = ask_question(question)
    print(f"问题：{question}")
    print(f"回复：{answer}")
```

解释代码流程：

1. 用户提问（ `"这件衣服有什么特点？"`）。
2. `retrieval_chain` 使用这个问题在向量数据库中进行检索，得到与问题相关的文档内容。
3. 检索到的文档内容被填充到 `prompt_template` 的 `{context}` 部分。
4. 最终生成的提示信息会传递给聊天模型 `llm` 进行回答。

（7）完整示例

```
import os
from dotenv import load_dotenv, find_dotenv
from langchain_openai import ChatOpenAI
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_community.vectorstores import FAISS
from langchain_community.document_loaders import TextLoader
from langchain.chains import create_retrieval_chain
from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain.prompts import ChatPromptTemplate

# 基本设置：加载环境变量
load_dotenv(find_dotenv())

# 初始化模型时添加详细参数说明
llm = ChatOpenAI(
    openai_api_key=os.getenv("DEEPSEEK_API_KEY"),
    openai_api_base="https://api.deepseek.com/v1",
    model="deepseek-chat",  # 确认使用正确的模型名称
    temperature=0.0,
    max_tokens=512  # 生成的最大 token 数量限制为 512
)

# 1. 文档加载
try:
    loader = TextLoader(
        "Clothing_Introduction.txt",
        encoding="utf-8",
        autodetect_encoding=True  # 添加自动编码检测
    )
    docs = loader.load()
except Exception as e:
    raise RuntimeError(f"文档加载失败: {str(e)}")

# 2. 嵌入模型
embedding = HuggingFaceEmbeddings(
    model_name='sentence-transformers/all-MiniLM-L6-v2',
    model_kwargs={'device': 'cpu'},  # 明确指定运行设备
    encode_kwargs={'normalize_embeddings': True} # 归一化向量
)

# 3. 向量数据库创建
vectorstore = FAISS.from_documents(docs, embedding)

# 4. 创建提示模板
prompt_template = """
你是一个专业的服装知识助手，请严格根据提供的上下文信息回答问题。
如果不知道答案，请如实告知。

上下文信息：
{context}

用户问题：{input}
"""
prompt = ChatPromptTemplate.from_template(prompt_template)

# 5. 构建处理链
combine_chain = create_stuff_documents_chain(
    llm=llm,
    prompt=prompt
)

retrieval_chain = create_retrieval_chain(
    retriever=vectorstore.as_retriever(
        search_type="similarity",  # 使用相似度搜索
        search_kwargs={"k": 3}     # 返回前3个相关结果
    ),
    combine_docs_chain=combine_chain
)

# 6. 问答函数
def ask_question(question):
    try:
        response = retrieval_chain.invoke({"input": question})
        return response["answer"]
    except Exception as e:
        return f"处理请求时发生错误: {str(e)}"

# 示例使用
if __name__ == "__main__":
    question = "这件衣服合适在什么场所穿？"
    answer = ask_question(question)
    print(f"问题：{question}")
    print(f"回复：{answer}")
```



## 5，智能体

### （1）使用LangGraph构建一个可以使用搜索工具的聊天机器人

（1）导入库

```
from typing import Annotated, Typeddict # 用于类型声明
import os  # 操作系统接口，用于访问环境变量
from dotenv import load_dotenv, find_dotenv  # 用于加载.env文件中的环境变量
from langchain_openai import ChatOpenAI  # OpenAI聊天模型接口
from langchain_community.tools.tavily_search import TavilySearchResults  # Tavily工具，用于搜索
from langgraph.graph import StateGraph  # LangGraph的图形结构
from langgraph.graph.message import add_messages  # 用于处理消息的函数
from langgraph.prebuilt import ToolNode, tools_condition  # 预定义的工具节点和条件分支
```

（2）为聊天机器人定义一个**统一的数据格式**，并创建StateGraph实例

```
class State(TypedDict):
    messages: Annotated[list, add_messages]
```

- 首先定义了一个数据类型`State`,它是个字典类型，但必须包含字段`messages`（类型为list）

```
# 示例
State = {
    "messages": [
        {"role": "user", "content": "你好！"},
        {"role": "assistant", "content": "你好！有什么我可以帮忙的吗？"},
    ]
}
```

- `list` 后续添加的内容：

```
[{"role": "user", "content": "Hello!"},
  {"role": "assistant", "content": "Hi there!"}]
  
# 这是 OpenAI Chat API 的标准消息格式，每条消息由 `role` 和 `content` 两部分组成。
```

- `Annotated`: 为list添加附加消息，并被langgraph识别并使用。

- `add_messages`:langgraph框架在聊天过程中会识别这个附加信息，把新消息 append 到里面，而不是重新赋值整个列表（覆盖）。

```
graph_builder = StateGraph(State)
```

`StateGraph` 是 LangGraph 中的核心类，用于构建和管理对话流程图

所有节点处理的‘状态信息’（也就是上下文、数据）都必须符合 `State` 类型（数据格式）

（3）创建工具节点

设置工具

```
tool = TavilySearchResults(max_results=2) # 最多返回两个搜索结果
tools = [tool] # 将 tool 放入一个工具列表 tools 中
```

将工具绑定到 LLM

```
llm_with_tools = llm.bind_tools(tools)
```

`bind_tools()`: 是一个方法，它**把工具的调用能力“绑定”到这个 LLM 上**。

`llm_with_tools`: 得到一个新的模型实例，它在推理时会自动识别是否应该调用工具，并返回工具调用请求。

- 通过`bind_tools()`方法将工具描述信息（名称、描述、参数格式）注入模型的系统提示中

创建工具节点

```
tool_node = ToolNode(tools=[tool])
```

`Toolnode`：预定义的工具调用函数

（4）创建llm节点

定义`chatbot`函数

```
def chatbot(state: State):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}
```

定义了一个 `chatbot` 函数，它接受一个 `State` 类型的参数 `state`。

该函数将当前对话的消息传递给模型，并返回生成的回复（通过 `llm_with_tools.invoke` 调用模型）

（5）添加节点和边并编译图

```
# 添加llm节点
graph_builder.add_node("chatbot", chatbot)
# 添加工具节点
graph_builder.add_node("tools", tool_node)
# 添加条件边
graph_builder.add_conditional_edges(
    "chatbot",
    tools_condition,
)
# 添加从工具节点返回到llm节点的边
graph_builder.add_edge("tools", "chatbot")
# 添加入口点
graph_builder.set_entry_point("chatbot")

# 编译图
graph = graph_builder.compile()
```

- `dd_conditional_edges` 方法添加了条件分支，用于在 `chatbot` 节点之后根据某些条件（如工具是否被调用）进行分支。
- `tools_condition` 是一个预定义的条件函数，用于决定是否执行工具节点或返回END。
- `add_edge` 设置了从 `tools` 节点到 `chatbot` 节点的边，表示当工具节点执行后会返回到 `chatbot` 节点。
- `set_entry_point("chatbot")` 设置了图的入口点为 `chatbot`，即从 `chatbot` 节点开始对话流程。
- `compile()` 方法将构建的图形编译成一个可以执行的对话流程

（6）启动图并流式输出

```
def stream_graph_updates(user_input: str):
    for event in graph.stream({"messages": [{"role": "user", "content": user_input}]}): # 输入数据符合State格式
        for value in event.values():
            print("Assistant:", value["messages"][-1].content)
```

`graph.stream(...) `:流式输出函数：

LangGraph 会启动你定义的流程图，从**起始节点**开始处理。

- 在处理过程中，图中的每个节点都会用这个输入数据（`{"messages": [{"role": "user", "content": user_input}]}`）作为初始状态，逐步向下传递。
- 比如，你的第一个节点可能是 `chatbot`，它会收到这些 `messages`，然后产生自己的输出（可能是模型的回复）。

每当一个节点执行完毕，它就会产出一个“事件（event）”。这个 `event` 是一个字典，key 是节点的名字，value 是这个节点的输出状态（符合 `State` 类型）。

比如：

```
{
  "chatbot": {
    "messages": [{"role": "assistant", "content": "你好，有什么可以帮你？"}]
  }
}
```

每次调用 `graph.stream()`，都会有一个**中间结果（event）**流出来。

`event.values()`:获取每个节点的输出状态

`value["messages"][-1].content`:输出最后一个也就是最新的消息内容

（7）主循环

```
while True:
    try:
        user_input = input("User: ")
        if user_input.lower() in ["quit", "exit", "q"]:
            print("Goodbye!")
            break
        stream_graph_updates(user_input)
    except:
        # fallback if input() is not available
        user_input = "What do you know about LangGraph?"
        print("User: " + user_input)
        stream_graph_updates(user_input)
        break
```

- 主循环接收用户输入并调用 `stream_graph_updates` 来处理输入并显示助手的回应。
- 如果用户输入 `quit`、`exit` 或 `q`，则退出循环并结束对话。
- 如果 `input()` 不可用（可能是在某些环境中），则会使用默认输入 `"What do you know about LangGraph?"` 进行测试。

（8）完整示例

```
import os  # 操作系统接口，用于访问环境变量
from dotenv import load_dotenv, find_dotenv  # 用于加载.env文件中的环境变量
from langchain_openai import ChatOpenAI  # OpenAI聊天模型接口

from typing import Annotated, TypedDict
from langgraph.graph import StateGraph
from langgraph.graph.message import add_messages
from langgraph.prebuilt import ToolNode, tools_condition
from langchain_community.tools.tavily_search import TavilySearchResults

# 1,设置llm模型
load_dotenv(find_dotenv())  # 加载环境变量

llm = ChatOpenAI(
    openai_api_key=os.getenv("OPENAI_API_KEY"),  # API密钥
    model="gpt-4o",  # 使用的模型
    temperature=0.0,  # 生成的确定性（0最确定）
    openai_api_base='http://apikey888.top/v1'
)

# 2，创建StateGraph实例
class State(TypedDict):
    messages: Annotated[list, add_messages]

graph_builder = StateGraph(State)

# 3，创建工具节点
tool = TavilySearchResults(max_results=2) # 最多返回两个搜索结果
tools = [tool] # 将 tool 放入一个工具列表 tools 中
llm_with_tools = llm.bind_tools(tools)
tool_node = ToolNode(tools=[tool])

# 4，创建llm节点
def chatbot(state: State):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

# 5，构造并编译图
# 添加llm节点
graph_builder.add_node("chatbot", chatbot)
# 添加工具节点
graph_builder.add_node("tools", tool_node)
# 添加条件边
graph_builder.add_conditional_edges(
    "chatbot",
    tools_condition,
)
# 添加从工具节点返回到llm节点的边
graph_builder.add_edge("tools", "chatbot")
# 添加入口点
graph_builder.set_entry_point("chatbot")

# 编译图
graph = graph_builder.compile()

# 6，流式输出运行图
def stream_graph_updates(user_input: str):
    for event in graph.stream({"messages": [{"role": "user", "content": user_input}]}): # 输入数据符合State格式
        for value in event.values():
            print("Assistant:", value["messages"][-1].content)

# 7，主循环
while True:
    try:
        user_input = input("User: ")
        if user_input.lower() in ["quit", "exit", "q"]:
            print("Goodbye!")
            break
        stream_graph_updates(user_input)
    except:
        print("没有输入")
        break
```



### （2）为聊天机器人添加内存

（1）导入模块

```
from langgraph.checkpoint.memory import MemorySaver
```

提供了一个检查点管理组件，允许你保存图执行过程中的**状态信息**，并在必要时进行**恢复**

（2）创建`MemorySaver` 实例

```
memory = MemorySaver()

graph = graph_builder.compile(checkpointer=memory)  # 添加记忆功能

config = {"configurable": {"thread_id": "1"}} # 定义配置项
```

`checkpointer=memory` 传入了 `MemorySaver` 实例，意味着在图执行时，LangGraph 会根据 `MemorySaver` 的逻辑将每个节点的状态保存到内存中。

（3）设置配置并传入`graph.stream()`

```
config = {"configurable": {"thread_id": "1"}} # 定义配置项

def stream_graph_updates(user_input: str):
    for event in graph.stream({"messages": [{"role": "user", "content": user_input}]}, config): # 传入配置
        for value in event.values():
            print("Assistant:", value["messages"][-1].content)

```

定义一个 `config` 配置字典，用于存储与执行相关的**可配置参数**

`thread_id` 这里就表示对话的“线程 ID”。

- 如果 thread_id 相同，每次新的 user_input 都会用同一个状态继续推理（有记忆）。
- 如果 thread_id 不同，LangGraph 就会认为是新的对话。



### （3）补充：流式执行和非流式执行

（1）流式执行

**流式执行**指的是数据或者事件的处理是逐步进行的，并且实时生成输出。

每当某一部分任务完成时，结果就会立刻返回，用户可以立即看到输出。

（2）非流式执行

**非流式执行**指的是整个任务的执行过程会一次性完成，直到最终结果才能返回

用户需要等待所有的任务执行完毕，才能获得最终结果

（3）完整示例

```
import os  # 操作系统接口，用于访问环境变量
from dotenv import load_dotenv, find_dotenv  # 用于加载.env文件中的环境变量
from langchain_openai import ChatOpenAI  # OpenAI聊天模型接口

from typing import Annotated, TypedDict
from langgraph.graph import StateGraph
from langgraph.graph.message import add_messages
from langgraph.prebuilt import ToolNode, tools_condition
from langchain_community.tools.tavily_search import TavilySearchResults

from langgraph.checkpoint.memory import MemorySaver

# 1,设置llm模型
load_dotenv(find_dotenv())  # 加载环境变量

llm = ChatOpenAI(
    openai_api_key=os.getenv("OPENAI_API_KEY"),  # API密钥
    model="gpt-4o",  # 使用的模型
    temperature=0.0,  # 生成的确定性（0最确定）
    openai_api_base='http://apikey888.top/v1'
)

# 2，创建StateGraph实例
class State(TypedDict):
    messages: Annotated[list, add_messages]

graph_builder = StateGraph(State)

# 3，创建工具节点
tool = TavilySearchResults(max_results=2) # 最多返回两个搜索结果
tools = [tool] # 将 tool 放入一个工具列表 tools 中
llm_with_tools = llm.bind_tools(tools)
tool_node = ToolNode(tools=[tool])

# 4，创建llm节点
def chatbot(state: State):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

# 5，构造并编译图
# 添加llm节点
graph_builder.add_node("chatbot", chatbot)
# 添加工具节点
graph_builder.add_node("tools", tool_node)
# 添加条件边
graph_builder.add_conditional_edges(
    "chatbot",
    tools_condition,
)
# 添加从工具节点返回到llm节点的边
graph_builder.add_edge("tools", "chatbot")
# 添加入口点
graph_builder.set_entry_point("chatbot")

# 编译图
memory = MemorySaver()
graph = graph_builder.compile(checkpointer=memory) # 添加记忆功能

config = {"configurable": {"thread_id": "1"}}

def stream_graph_updates(user_input: str):
    for event in graph.stream({"messages": [{"role": "user", "content": user_input}]}, config): # 输入数据符合State格式
        for value in event.values():
            print("Assistant:", value["messages"][-1].content)

# # 非流式的实现
# def graph_updates(user_input: str):
#     result = graph.invoke({"messages": [{"role": "user", "content": user_input}]}, config) # 输入数据符合State格式
#     print("Assistant:", result["messages"][-1].content)

while True:
    try:
        user_input = input("User: ")
        if user_input.lower() in ["quit", "exit", "q"]:
            print("Goodbye!")
            break
        stream_graph_updates(user_input)
    except Exception as e:
        print(f"发生错误{e}")
        break
```



### （4）完整实现

```
import os  # 操作系统接口，用于访问环境变量
import json
from dotenv import load_dotenv, find_dotenv  # 用于加载.env文件中的环境变量
from langchain_openai import ChatOpenAI  # OpenAI聊天模型接口

from typing import Annotated, TypedDict
from langgraph.graph import StateGraph, END, START
from langgraph.graph.message import add_messages
from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_core.messages import ToolMessage

from langgraph.checkpoint.memory import MemorySaver

# 1,设置llm模型
load_dotenv(find_dotenv())  # 加载环境变量

llm = ChatOpenAI(
    openai_api_key=os.getenv("OPENAI_API_KEY"),  # API密钥
    model="gpt-4o",  # 使用的模型
    temperature=0.0,  # 生成的确定性（0最确定）
    openai_api_base='http://apikey888.top/v1'
)

# 2，创建StateGraph实例
class State(TypedDict):
    messages: Annotated[list, add_messages]

graph_builder = StateGraph(State)

# 3，创建工具节点
tool = TavilySearchResults(max_results=2) # 最多返回两个搜索结果
tools = [tool] # 将 tool 放入一个工具列表 tools 中
llm_with_tools = llm.bind_tools(tools)

class ToolNode:  # 构建工具调用函数

    def __init__(self, tools: list) -> None:
        self.tools_by_name = {tool.name: tool for tool in tools}

    def __call__(self, inputs: dict):
        messages = inputs.get("messages", [])
        if messages:
            message = messages[-1]
        else:
            raise ValueError("No message found in input")
        outputs = []
        for tool_call in message.tool_calls:
            tool_result = self.tools_by_name[tool_call["name"]].invoke(
                tool_call["args"]
            )
            outputs.append(
                ToolMessage(
                    content=json.dumps(tool_result),
                    name=tool_call["name"],
                    tool_call_id=tool_call["id"],
                )
            )
        return {"messages": outputs}

tool_node = ToolNode(tools=[tool])

# 4，创建llm节点
def chatbot(state: State):
    return {"messages": [llm_with_tools.invoke(state["messages"])]}

# 5, 创建决策边判断函数
def route_tools(state: State):
    messages = state.get("messages", [])
    if messages:
        message = messages[-1]
    else:
        raise ValueError(f"No messages found in input state to tool_edge: {state}")

    if hasattr(message, "tool_calls") and len(message.tool_calls) > 0:
        return "tools"

    return END

# 6，构造并编译图
graph_builder.add_node("chatbot", chatbot) # 添加llm节点
graph_builder.add_node("tools", tool_node) # 添加工具节点
graph_builder.add_conditional_edges(       # 添加条件边
    "chatbot",
    route_tools,
    {"tools": "tools", END: END},
)
graph_builder.add_edge("tools", "chatbot") # 添加从工具节点返回到llm节点的边
graph_builder.add_edge(START, "chatbot")  # 添加入口点

memory = MemorySaver()   # 添加记忆功能
graph = graph_builder.compile(checkpointer=memory) # 编译图

# 7，设置流式输入
config = {"configurable": {"thread_id": "1"}} # 配置项

def stream_graph_updates(user_input: str):
    for event in graph.stream({"messages": [{"role": "user", "content": user_input}]}, config): # 输入数据符合State格式
        for value in event.values():
            print("Assistant:", value["messages"][-1].content)

# 8，主循环
while True:
    try:
        user_input = input("User: ")
        if user_input.lower() in ["quit", "exit", "q"]:
            print("Goodbye!")
            break
        stream_graph_updates(user_input)
    except Exception as e:
        print(f"发生错误{e}")
        break
```



```
class ToolNode:  # 构建工具调用函数
	
	# 类的初始化方法。它接收一个 tools 列表（包含多个工具对象）
    def __init__(self, tools: list) -> None:
        self.tools_by_name = {tool.name: tool for tool in tools}

	# 这是一个特殊的方法，使得 ToolNode 类的实例可以像函数一样被调用。
    def __call__(self, inputs: dict):
        messages = inputs.get("messages", [])
        if messages:
            message = messages[-1]
        else:
            raise ValueError("No message found in input")
        outputs = []
        # 遍历 message 中的每个工具调用。
        for tool_call in message.tool_calls:
            tool_result = self.tools_by_name[tool_call["name"]].invoke(
                tool_call["args"]
            )
            outputs.append(
                ToolMessage(
                    content=json.dumps(tool_result), # 工具结果的 JSON 字符串。
                    name=tool_call["name"],
                    tool_call_id=tool_call["id"],
                )
            )
        return {"messages": outputs}
```

`messages = inputs.get("messages", [])`
获取消息：从输入的 inputs 字典中获取 "messages" 键对应的值。如果 messages 存在，它将是一个包含消息的列表；如果 messages 不存在，则默认为空列表。

对于每个 `tool_call`（工具调用），通过工具名称查找对应的工具（`self.tools_by_name[tool_call["name"]]`）。然后调用该工具的 `invoke` 方法，并传入 `tool_call["args"]` 作为参数



```5, 创建决策边判断函数
# 5, 创建决策边判断函数
def route_tools(state: State):
    messages = state.get("messages", [])
    if messages:
        message = messages[-1]
    else:
        raise ValueError(f"No messages found in input state to tool_edge: {state}")

    if hasattr(message, "tool_calls") and len(message.tool_calls) > 0:
        return "tools"
        
    return END
```

`hasattr` 是 Python 内置的函数，用来检查一个对象是否具有某个属性。

这里检查 `message` 对象是否有名为 `tool_calls` 的属性。`tool_calls` 通常是一个列表，包含当前消息中所需调用的工具的信息。

如果 `message` 对象有 `tool_calls` 属性，`hasattr` 返回 `True`，否则返回 `False`

如果 `message` 包含 `tool_calls` 属性，并且该属性的长度大于零（即有工具调用），则返回字符串 `"tools"`，表示消息中存在工具调用。



## 6，MCP

模型上下文协议，**标准化让模型（智能体）和外部数据（工具/资源）的交互**。

MCP协议提供了一种**标准化的接口**，使得**不同开发者的智能体**和**不同的工具**之间可以直接交互，而不需要重新实现接口



 
