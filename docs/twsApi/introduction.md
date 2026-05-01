# TWS API DocumentationTWS API 文档

## Introduction 介绍

The TWS API is a TCP Socket Protocol API based on connectivity to the Trader Workstation or IB Gateway. The API acts as an interface to retrieve and send data autonomously to Interactive Brokers. Interactive Brokers provides code systems in Python, Java, C++, C#, and Visual Basic.

TWS API 是基于与交易工作站或 IB Gateway 的连接的 TCP Socket Protocol API。该 API 作为接口，可自主从 Interactive Brokers 检索和发送数据。Interactive Brokers 提供了 Python、Java、C++、C# 和 Visual Basic 的代码语言支持。

The TWS API is a message protocol as its core, and any library that implements the TWS API, whether created by IB or someone else, is a tool to send and receive these messages over a TCP socket connection with the IB host platform (TWS or IB Gateway). As such the system can be tweaked and modified into any language of interest given the intention to translate the underlying decoder.

TWS API 的核心是消息协议，任何实现 TWS API 的库，无论是由 IB 创建还是由其他人创建，都是一种工具，通过与 IB 主平台（TWS 或 IB Gateway）的 TCP socket 连接发送和接收这些消息。因此，只要有意翻译底层解码器，该系统就可以被调整和修改为任何感兴趣的语言。

In short, a library written in any other languages must be sending and receiving the same data in the same format as any other conformant TWS API library, so users can look at the documentation for our libraries to see what a given request or response consists of (what it must include, in what form, etc.) and implement them in their own structure.

简而言之，用任何其他语言编写的库都必须以与其他符合规范的 TWS API 库相同的格式发送和接收相同的数据，以便用户可以查看我们库的文档，了解特定请求或响应包含的内容（必须包含什么、以何种形式等），并在自己的结构中实现它们。

Our TWS API components are aimed at experienced professional developers willing to enhance the current TWS functionality. Before you use TWS API, please make sure you fully understand the concepts of OOP (<https://www.geeksforgeeks.org/introduction-of-object-oriented-programming/>) and other Computer Science Concepts. Regrettably, Interactive Brokers cannot offer any programming consulting. Before contacting our API support, please always refer to our available documentation, sample applications and Recorded Webinars.

我们的 TWS API 组件面向经验丰富的专业开发者，他们希望增强当前的 TWS 功能。在使用 TWS API 之前，请确保您充分理解面向对象编程（OOP）的概念（<https://www.geeksforgeeks.org/introduction-of-object-oriented-programming/>）以及其他计算机科学概念。遗憾的是，Interactive Brokers 无法提供任何编程咨询服务。在联系我们的 API 支持之前，请始终参考我们提供的文档、示例应用程序和录制的网络研讨会。

This guide references the Java, VB, C#, C++ and Python Testbed sample projects to demonstrate the TWS API functionality. Code snippets are extracted from these projects and we suggest all those users new to the TWS API to get familiar with them in order to quickly understand the fundamentals of our programming interface. The Testbed sample projects can be found within the samples folder of the TWS API’s installation directory.

本参考指南 Java、VB、C#、C++和 Python 测试平台示例项目来演示 TWS API 的功能。代码片段提取自这些项目，我们建议所有新接触 TWS API 的用户熟悉它们，以便快速理解我们的编程接口基础。测试平台示例项目可以在 TWS API 安装目录的 samples 文件夹中找到。
