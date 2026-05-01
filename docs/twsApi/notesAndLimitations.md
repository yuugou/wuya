# TWS API DocumentationTWS API 文档

## Notes & Limitations 注意事项和限制

While Interactive Brokers does maintain a Python, Java, C#, and C++ offering for the TWS API, C# and our Excel offerings are exclusively available for Windows PC. As a result, these features are not available on Linux or Mac OS.

虽然 Interactive Brokers 为 TWS API 提供了 Python、Java、C#和 C++版本，但 C# 和 Excel 版本仅适用于 Windows PC。因此，这些功能在 Linux 或 Mac OS 上不可用。

### Requirements 需求

- A funded and opened IBKR Pro account
- 一个已注资并开启的 IBKR Pro 账户
- The current Stable or Latest release of the TWS or IB Gateway
- TWS 或 IB Gateway 的当前稳定版或最新版
- The current Stable or Latest release of the TWS API
- TWS API 当前稳定版或最新版
- A working knowledge of the programming language our Testbed sample projects are developed in.
- 熟悉我们测试平台示例项目所使用的编程语言。

The minimum supported language version is documented on the right for each of our supported languages.

每个语言的最小支持版本如下：

=== "Python"

    ```python
    Minimum supported Python release is version 3.11.0.

    最低支持的 Python 版本是 3.11.0。
    ```

=== "Java"

    ```Java
    The minimum supported Java version is Java 21.

    最低支持的 Java 版本是 Java 21。
    ```

=== "C++"

    ```C++
    The minimum supported C++ version is C++ 14 Standard.

    最低支持的 C++ 版本是 C++ 14 标准版。
    ```

=== "C#"

    ```C#
    The C# implementation was built using:

    C# 应用使用以下构建：

    - .NET Core 3.1
    - .NET Framework 4.8
    - .NET Standard 2.0
    ```

Please be sure to toggle the indicated language to the language of your choosing.

请确保将指示的语言切换为您选择的语言。

### Limitations 限制

Our programming interface is designed to automate some of the operations a user normally performs manually within the TWS Software such as placing orders, monitoring your account balance and positions, viewing an instrument’s live data… etc. There is no logic within the API other than to ensure the integrity of the exchanged messages. Most validations and checks occur in the backend of TWS and our servers. Because of this it is highly convenient to familiarize with the TWS itself, in order to gain a better understanding on how our platform works. Before spending precious development time troubleshooting on the API side, it is recommended to first experiment with the TWS directly.

我们的编程接口旨在自动化用户在 TWS 软件中通常手动执行的一些操作，例如下单、监控账户余额和头寸、查看金融工具的实时数据等。API 中除了确保交换消息的完整性外，没有其他逻辑。大多数验证和检查都在 TWS 的后端和我们的服务器中发生。由于这个原因，熟悉 TWS 本身会非常方便，以便更好地理解我们的平台是如何工作的。在花费宝贵的时间在 API 方面进行故障排除之前，建议首先直接在 TWS 上进行实验。

**Remember**: If a certain feature or operation is not available in the TWS, it will not be available on the API side either!

**记住**：如果 TWS 中某个功能或操作不可用，API 方面也不会可用！

#### C# for MacOS / MacOS 下的 C\#

The TWS API C# source files are not available through the Mac and Unix distribution download as the language is built around Dynamic Link Library (DLL) files for execution. This is because DLL files are exclusively supported through Windows platforms.

由于 TWS API 的 C#源文件不是通过 Mac 和 Unix 分发下载的，因为该语言围绕动态链接库（DLL）文件执行。这是因为 DLL 文件仅通过 Windows 平台支持。

#### C++ DLLs and Static Linking / C++ 动态链接库和静态链接

Following the TWS API’s recent migration to Protobuf, clients developing in C++ should prioritize static linking over the use of DLLs.

随着 TWS API 最近迁移到 Protobuf，使用 C++开发的客户端应优先考虑静态链接，而不是使用动态链接库。

This recommendation is based on the Google Protobuf documentation. For more information on the reasoning behind it, or questions on enabling DLLs for use with Protobuf, please see DLLs vs static linking.

此建议基于 Google Protobuf 文档。有关其背后的原因或关于启用 DLL 以用于 Protobuf 的问题，请参阅 DLLs vs static linking。

#### Paper Trading模拟交易

If your regular trading account has been approved and funded, you can use your Account Management page to open a Paper Trading Account which lets you use the full range of trading facilities in a simulated environment using real market conditions. Using a Paper Trading Account will allow you not only to get familiar with the TWS API but also to test your trading strategies without risking your capital.

如果您的常规交易账户已获批准并注资，您可以使用账户管理页面开设一个纸面交易账户，该账户让您能在模拟环境中使用真实市场条件，体验全部交易设施。使用纸面交易账户不仅能让您熟悉 TWS API，还能在不冒资本风险的情况下测试您的交易策略。

Please be aware that the Paper Trading Environment relies on more simulated technologies than the Live trading environment. As a result, certain behavior such as order execution may vary

请注意，模拟交易环境比实盘交易环境依赖更多的模拟技术。因此，某些行为（如订单执行）可能会有所不同。

Note the paper trading environment has inherent limitations.

请注意，纸面交易环境具有固有的局限性。
