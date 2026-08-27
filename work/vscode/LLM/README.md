VSCode内置（built-in/无需手动安装）了Copilot插件（chat）

使用自己的OpenAI Compatible的API Provider(Language Model)

参见：https://code.visualstudio.com/docs/agent-customization/language-models#_add-a-custom-endpoint-model

下面的auto切换：选择“Manage Models”，打开JSON格式手动编辑chatLanguageModels.json，格式示例：

```
[
	{
		"name": "MyLLM",
		"vendor": "customendpoint",
		"apiKey": "<Your token>",
		"apiType": "chat-completions",
		"models": [
			{
				"id": "claude-opus-4-7",
				"name": "claude-opus-4-7(myllm)",
				"url": "http://<baseurl>",
				"toolCalling": true,
				"vision": true,
				"maxInputTokens": 200000,
				"maxOutputTokens": 64000,
				"requestHeaders": {
					"Authorization": "Bearer <Your token>"
				}
			}
		]
	}
]
```
注意
chat的 model列表中 相同的模型id只会显示一条，所以id不要重名
外层的name是组名可以自己起名；内层的name是显示的名称也可以自己起名；
常见报错1：No lowest priority node found：maxInputTokens配置太小；
常见报错2：temperature is deprecated for this model：没有使用正确的代理url；
常见报错3：gateway request exception, please auth first：没有写对requestHeaders；
apiType，默认是"chat-completions"，还有"messages"，取决于模型提供商支持哪种，一般openAI用"chat-completions"(/v1/chat/completions)，而anthropic用"messages"（/v1/messages）
如果显式配置了apiType，那么baseurl就不用写后缀（vscode自动帮拼接好了），否则可能需要写/v1/messages这种到url里；



配置rule/instructions（~/.claude/CLAUDE.md），参考：
https://code.visualstudio.com/docs/agent-customization/custom-instructions?referrer=in-product#_use-a-claudemd-file