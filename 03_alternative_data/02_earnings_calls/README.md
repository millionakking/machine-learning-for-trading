## 如何抓取收益电话会议记录

> 更新：不幸的是，seekingalpha 已经更新了他们的网站，需要输入验证码，因此不再可以按照此处描述的方式进行自动下载。

文本数据是必不可少的替代数据源。文本信息的一个例子是财报电话会议记录，在电话会议上，高管们不仅展示了最新的财务结果，而且还回答了财务分析师提出的问题。投资者使用成绩单来评估情绪变化、对特定主题的强调或沟通方式。

我们将说明收益电话会议记录的抓取和解析。

### 指引

> 注意：与所有其他示例不同，代码编写为在主机上运行而不是使用 Docker 映像，因为它依赖于浏览器。该代码仅在 Ubuntu 和 Mac 上进行了测试。

本节包含用于从 Seeking Alpha 检索收益电话会议记录的代码。

运行 `python sa_selenium.py` 文件以抓取成绩单并将结果存储在成绩单/部分下。
- 内容：陈述和问答内容
- 参与者：通过寻求 alpha 列出
- 收益：电话所指收益的日期和公司

这需要 [geckodriver](https://github.com/mozilla/geckodriver/releases) 和 [Firefox](https://www.mozilla.org/en-US/firefox/new/)。

- 在 macOS 上，您可以使用 ```brew install geckodriver```。
- 请参阅 [此处](https://askubuntu.com/questions/870530/how-to-install-geckodriver-in-ubuntu) 了解 Ubuntu。






