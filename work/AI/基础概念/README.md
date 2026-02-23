- 模型：可以理解成多层网络结构+一堆可训练参数组成的数学表示；

- LM/语言模型

- LLM/大语言模型（Large Language Model）：模型参数更大，更“聪明”

```
闭源收费：GPT-4、Claude（Anthropic，创始团队是 前OpenAI 的成员，更在意AI安全避免有害）、Gemini（Google）、Kimi（月之暗面/Moonshot AI，是中国 AI 独角兽公司）；
开源可本地部署：Llama（Meta）、千问Qwen（阿里）、DeepSeek；

请求LLM的输入格式形如（分了角色更准，因为模型训练的时候对话数据就被标记成了不同角色）：
{
  "model":"deepseed/xxx",
  "messages":[
    {
      "role":"system",  // 系统prompt，设定上下文、角色和规则，可以用于安全防注入
      "content": "你是一位专业的数学老师，请xxx"
    },
    {
      "role":"user",  //  用户第1轮输入的信息（如果上下文经过多轮对话之后特别长，这里可以压缩成用户第1轮~第x轮对话的摘要，摘要写完下一个紧跟着的是role第x+1轮的问题）
      "content": "1+1等于多少"
    },
    {
      "role":"assistant",  // 模型第1轮生成的回复
      "content": "等于2"
    },
    {
      "role":"user",  //  用户第2轮输入的信息
      "content": "为什么呢？"
    },
    ...
  ]
}
```

- context/memory：上下文/背景信息，每次调用LLM都会把历史的对话（可以经过压缩以减少token）放到这里面再调用；因为模型本身没有记忆，所以多轮对话的中的每一次对话对LLM来说都认为是“第一次对话”。。。你只能把历史对话塞进上下文里一起调用LLM，token爆炸。。。。白花花的银子

- prompt：提示词/最终指令，也算context的一部分

- RAG（Retrieval Augmented Generation）
是结合信息检索（Retrieval）和生成式LM（Generation）的技术框架；通过检索（通常使用向量数据库）知识库，把检索到的信息作为上下文，输入到生成式LM中，从而生成更准确、更有依据的答案或内容；优点是 能实时更新知识库、减少AI幻觉；能应用于：问答、客服、搜索；

- AI Agent(智能体)：LLM + 手脚（Tools Functions）、记忆（短期+长期）、目标感、行动循环，从“像人一样回答”变成“像人一样做”；（在RAG的基础上加了更多能力，Agent是更上层的，大部分应用都是直接使用agent，间接调用LLM和工具）；

![AI应用形态](https://ywsswy.top/imagehub/?path=AI应用形态.png)

- function calling：agent跟LLM、工具之间约定的对话格式（类似于前端和后端约定的接口格式）；也有agent（如Cline）用XML格式跟LLM沟通的

- MCP（Model Context Protocol）：agent跟工具之间的桥梁，agent（使用MCP协议格式，像json）调用MCP server（不一定是服务器-SSE传输类型，本地不联网的也可以叫MCP server-stdio传输类型，只要对外的输入输出满足协议要求就算）发现/获取工具列表并调用工具；MCP server的内部可以封装一堆工具；

- MCP client/host：MCP的调用方，可以算是AI agent；

- rule：可以理解成一段固定被填充进上下文的规则；

- skills：领域知识包（包含Prompt、流程、资源（按需读给上下文）和脚本（按需运行）的文件夹集合）；token是按需加载的（采用了渐进式披露机制）；例如准备10个不同领域的SKILL.md，开始只加载里面的 name 和 description字段，让系统知道「具备哪些可用能力」，决定使用哪个skill时再加载里面的正文（更详细的Prompt、流程，再进一步按需可选地加载里面的二级资源或者运行脚本）；（比rules好在token更节省，有结构规范好维护；比MCP好在注入了 SOP 可以把流程说的很细，并且也是按需暴露能力，避免MCP那种万一成千上万个工具AI都傻掉了-类似的工具不知道调用哪个）

- subagent：对于一些独立的子任务，可以单独在这个子agent中完成。本质上是做了上下文隔离，子agent产生的上下文不会保留在主agent中。

- ReAct（Reasoning + Acting）：论文提到的一个思想，让AI模型“边想边做”的推理框架。思考、规划、结合工具调用从而解决复杂问题；

- vibe coding：年度科技词汇，并定义为：通过自然语言提示与AI协作生成可运行代码的新编程方式；出自前OpenAI联合创始人、前特斯拉人工智能主管Andrej Karpathy 发布的一条推文，大意是说： Vibe 编程，它让你完全沉浸在氛围中，甚至忘记代码的存在。



![AI技术矩阵](https://ywsswy.top/imagehub/?path=AI技术矩阵.png)