# 推文文案

## X / Twitter 长文案

我做了一个 DevSpace 的 Windows 定制版：让 ChatGPT、Gemini 等 MCP 客户端，在经过授权后，调用本地 DevSpace 工具读取和操作指定工作区。

相对上游 `Waishnav/devspace v1.0.5`，这个版本主要增加：

- Windows 原生 `download_artifact`：可把聊天客户端提供的原生图片、PDF、DOCX 等二进制文件保存到本地工作区；
- `write_binary` Base64 / Data URL 备用写入，适合客户端无法传递原生文件对象的情况；
- 工作区边界、相对路径、覆盖、符号链接 / junction、大小和发布完整性检查；
- 面向 Windows 小白的安装、ngrok、ChatGPT/Gemini 连接和聊天模式 Add plugin 示例。

它不会预先把整个本地项目塞进对话上下文，工具按需调用；但工具实际返回的文件内容、差异或命令输出仍会进入当前对话上下文。服务默认只访问你明确允许的工作区，公网 ChatGPT/Gemini 网页端需要 HTTPS 隧道，本地 MCP 客户端可以直接使用 localhost。

这是基于上游项目的公开定制版，不是官方发行版，也不代表上游作者背书。项目保留 MIT License 和上游版权声明；感谢 [Waishnav/devspace](https://github.com/Waishnav/devspace) 提供基础项目。

项目地址：
https://github.com/cooky-dance/devspace-local-artifacts

上游项目：
https://github.com/Waishnav/devspace

## X 短版

我做了一个 DevSpace 的 Windows 定制版：让 ChatGPT / Gemini 通过 MCP 操作授权的本地工作区，并支持把聊天客户端提供的图片、PDF、DOCX 保存到 Windows 本地；还增加了 Base64 / Data URL 二进制写入和安全路径校验。

基于 [Waishnav/devspace](https://github.com/Waishnav/devspace) v1.0.5，保留 MIT License；这是公开定制版，不是官方版本。

https://github.com/cooky-dance/devspace-local-artifacts
