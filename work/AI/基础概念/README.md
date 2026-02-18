- 模型：可以理解成多层网络结构+一堆可训练参数组成的数学表示；

- LM/语言模型

- LLM/大语言模型（Large Language Model）：模型参数更大，更“聪明”

```
闭源收费：GPT-4、Claude（Anthropic，创始团队是 前OpenAI 的成员，更在意AI安全避免有害）、Gemini（Google）、Kimi（月之暗面/Moonshot AI，是中国 AI 独角兽公司）；
开源可本地部署：Llama（Meta）、千问Qwen（阿里）、DeepSeek；
```

- AI Agent：LLM + 手脚（Tools Functions Actions）、记忆（短期+长期）、目标感、行动循环，从“像人一样回答”变成“像人一样做”；

- RAG（Retrieval Augmented Generation）
是结合信息检索（Retrieval）和生成式LM（Generation）的技术框架；通过检索（通常使用向量数据库）知识库，把检索到的信息作为上下文，输入到生成式LM中，从而生成更准确、更有依据的答案或内容；
优点是 能实时更新知识库、减少AI幻觉；能应用于：问答、客服、搜索；

- vibe coding：年度科技词汇，并定义为：通过自然语言提示与AI协作生成可运行代码的新编程方式；出自前OpenAI联合创始人、前特斯拉人工智能主管Andrej Karpathy 发布的一条推文，大意是说： Vibe 编程，它让你完全沉浸在氛围中，甚至忘记代码的存在。



【Q】
- Agent 框架（LangChain、LlamaIndex、AutoGen、CrewAI 等）