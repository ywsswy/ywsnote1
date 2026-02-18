使用方式：
1）安装（有的需要安装，有的无需安装）
python3 -m pip install my_weather_mcp

2）在agent（例如claude code）里配置mcp，形如：

```
{
  ...
  "weather": {
    "time": {
      "command": "python3",
      "args": [
        "-m",
        "my_weather_mcp"
      ],
      "timeout": 1800
    }
  }
}

```

3）然后就可以开始用啦，内部流程如下图：
![MCP](https://ywsswy.top/imagehub/?path=MCP.png)

