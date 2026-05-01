# TWS API Documentation / TWS API 文档

## Architecture / 架构

The TWS API is a BSD implementation that communicates request and response values across TCP socket using a end-line-delimited message protocol. While the underlying structure of the message will vary by request, requests typically follow a patter of indicating a message identifier, request identifier, and then directly relevant content for the request such as contract details or market data parameters.

TWS API 是一个 BSD 实现的协议，通过 TCP 套接字使用行尾分隔的消息协议来通信请求和响应值。虽然消息的底层结构会因请求而异，但请求通常遵循一个模式：首先标识消息标识符，然后是请求标识符，接着是直接与请求相关的内容，例如合约详情或市场数据参数。

The provided TWS API package use two distinct classes to accommodate the request / response functionality of the socket protocol, EClient and EWrapper respectively.

提供的 TWS API 包使用两个不同的类来适应套接字协议的请求/响应功能，分别是 EClient 和 EWrapper。

The EWrapper class is used to receive all messages from the host and distribute them amongst the affiliated response functions. The EReader class will retrieve the messages from the socket connection and decode them for distribution by the EWrapper class.

EWrapper 类用于接收来自主机的所有消息，并将它们分发到相关的响应函数中。EReader 类将从套接字连接中获取消息，并对其进行解码，以便 EWrapper 类进行分发。

=== "python"

```python
class TestWrapper(wrapper.EWrapper):
```

EClient or EClientSocket is used to send requests to the Trader Workstation. This client class contains all the available methods to communicate with the host. Up to 32 clients can be connected to a single instance of the host Trader Workstation or IB Gateway simultaneously.

EClient 或 EClientSocket 用于向 TWS 发送请求。这个客户端类包含了所有可用的与主机通信的方法。最多可以有 32 个客户端同时连接到一个 TWS 或 IB 网关的实例。

The primary distinction in EClient and EClientSocket is the involvement of the EReader Class to trigger when requests should be processed. EClient is unique to the Python implementation and utilizes the Python Queue module in place of the EReaderSignal directly. Both the EReaderSignal and Python Queue module handle the queueing process for submitting messages across the socket connection. In either scenario, the EWrapper class must be implemented first to acknowledge the EClient requests.

EClient 和 EClientSocket 的主要区别在于 EReader 类的参与，用于触发何时处理请求。EClient 是 Python 实现的专有特性，它使用 Python 的 Queue 模块代替直接使用 EReaderSignal。EReaderSignal 和 Python Queue 模块都负责通过套接字连接提交消息的队列处理。在任一情况下，都必须首先实现 EWrapper 类来确认 EClient 的请求。

=== "python"

```python
class TestClient(EClient):
    def __init__(self, wrapper):
        EClient.__init__(self, wrapper)
...
class TestApp(TestWrapper, TestClient):
    def __init__(self):
        TestWrapper.__init__(self)
        TestClient.__init__(self, wrapper=self)
```

Note: The EReaderSignal class is not used for Python API. The Python Queue module is used for inter-thread communication and data exchange.

注意：EReaderSignal 类不用于 Python API。Python Queue 模块用于线程间通信和数据交换。

### The Trader Workstation / TWS

Our market maker-designed IBKR Trader Workstation (TWS) lets traders, investors, and institutions trade stocks, options, futures, forex, bonds, and funds on over 100 markets worldwide from a single account. The TWS API is a programming interface to TWS, and as such, for an application to connect to the API there must first be a running instance of TWS or IB Gateway.

我们市场做市商设计的 IBKR 交易工作站（TWS）让交易者、投资者和机构能够通过一个账户在全球 100 多个市场上交易股票、期权、期货、外汇、债券和基金。TWS API 是 TWS 的编程接口，因此，要让应用程序连接到 API，首先必须有正在运行的 TWS 或 IB Gateway 实例。

#### The IB Gateway / IB Gateway

As an alternative to TWS for API users, IBKR also offers IB Gateway (IBGW). From the perspective of an API application, IB Gateway and TWS are identical; both represent a server to which an API client application can open a socket connection after the user has authenticated. With either application (TWS or IBGW), the user must manually enter their username and password into a login window. For security reasons, a headless session of TWS or IBGW without a GUI is not supported. From the user’s perspective, IB Gateway may be advantageous because it is a lighter application which consumes about 40% fewer resources.

作为 API 用户对 TWS 的替代方案，IBKR 还提供 IB Gateway（IBGW）。从 API 应用程序的角度来看，IB Gateway 和 TWS 是相同的；两者都代表一个用户经过身份验证后 API 客户端应用程序可以打开套接字连接的服务器。使用任一应用程序（TWS 或 IBGW），用户必须手动将用户名和密码输入登录窗口。出于安全原因，不支持没有 GUI 的 TWS 或 IBGW 的无头会话。从用户的角度来看，IB Gateway 可能具有优势，因为它是一个更轻量级的应用程序，资源消耗约减少 40%。

Both TWS and IBGW were designed to be restarted daily. This is necessary to perform functions such as re-downloading contract definitions in cases where contracts have been changed or new contracts have been added. Beginning in version 974+ both applications offer an auto-restart feature that allows the application to restart daily without user intervention. With this option enabled, TWS or IBGW can potentially run from Sunday to Sunday without re-authenticating. After the nightly server reset on Saturday night it will be necessary to again enter security credentials.

TWS 和 IBGW 都被设计为每日重启。这是为了在合约变更或新增合约的情况下重新下载合约定义等操作所必需的。从版本 974 开始，这两个应用程序都提供了自动重启功能，允许应用程序每日自动重启而无需用户干预。启用此选项后，TWS 或 IBGW 有可能从周日运行到周日而无需重新认证。在周六晚上的夜间服务器重置后，将需要再次输入安全凭证。

The advantages of TWS over IBGW is that it provides the end user with many tools (Risk Navigator, OptionTrader, BookTrader, etc) and a graphical user interface which can be used to monitor an account or place orders. For beginning API users, it is recommended to first become acquainted with TWS before using IBGW.

TWS 相对于 IBGW 的优势在于它为终端用户提供了许多工具（如风险导航器、期权交易器、订单簿交易器等）以及一个图形用户界面，可用于监控账户或下单。对于初学者 API 用户，建议在使用 IBGW 之前先熟悉 TWS。

For simplicity, this guide will mostly refer to the TWS although the reader should understand that for the TWS API’s purposes, TWS and IB Gateway are synonymous.

为简化起见，本指南将主要提及 TWS，尽管读者应理解，对于 TWS API 而言，TWS 和 IB Gateway 是同义的。
