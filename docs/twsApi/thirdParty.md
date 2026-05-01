# TWS API DocumentationTWS API 文档

## Third Party API Platforms / 第三方 API 平台

Third party software vendors make use of the TWS’ programming interface (API) to integrate their platforms with Interactive Broker’s. Thanks to the TWS API, well known platforms such as Ninja Trader or Multicharts can interact with the TWS to fetch market data, place orders and/or manage account and portfolio information.

第三方软件供应商利用 TWS 的编程接口（API）将其平台与 Interactive Broker 集成。得益于 TWS API，像 Ninja Trader 或 Multicharts 这样的知名平台可以与 TWS 交互，以获取市场数据、下单以及/或管理账户和投资组合信息。

**It is important to keep in mind that most third party API platforms are not compatible with all IBKR account structures**. Always check first with the software vendor before opening a specific account type or converting an IBKR account type. For instance, many third party API platforms such as NinjaTrader and TradeNavigator are not compatible with IBKR linked account structures, so it is highly recommended to first check with the third party vendor before linking your IBKR accounts.

**需要注意的是，大多数第三方 API 平台并非与所有 IBKR 账户结构兼容**。在开设特定账户类型或转换 IBKR 账户类型之前，务必先与软件供应商确认。例如，许多第三方 API 平台如 NinjaTrader 和 TradeNavigator 与 IBKR 关联账户结构不兼容，因此强烈建议在关联您的 IBKR 账户之前先与第三方供应商确认。

An ongoing list of common [Third Party Connections](https://www.interactivebrokers.com/campus/ibkr-api-page/third-party-connections/) are available within our documentation. This resource will also link out to connection guides detailing how a user can connect with a given platform.

我们的文档中提供了常见的[第三方连接的持续列表](https://www.interactivebrokers.com/campus/ibkr-api-page/third-party-connections/)。此资源还将链接到连接指南，详细说明用户如何与特定平台连接。

A non-exhaustive list of third party platforms implementing our interface can be found in our [Investor’s Marketplace](https://www.interactivebrokers.com/Universal/servlet/MarketPlace.MarketPlaceServlet). As stated in the marketplace, the vendors’ list is in no way a recommendation from Interactive Brokers. If you are interested in a given platform that is not listed, please contact the platform’s vendor directly for further information.

在我们的[投资者市场](https://www.interactivebrokers.com/Universal/servlet/MarketPlace.MarketPlaceServlet)可以找到实现我们接口的第三方平台的非详尽列表。正如市场中所说明的，供应商列表绝非来自 Interactive Brokers 的推荐。如果您对未列出的某个平台感兴趣，请直接联系该平台的供应商以获取更多信息。

### Non-Standard TWS API Languages and Packages / 非标准 TWS API 语言和包

Noted in further depth through our [Architecture](https://www.interactivebrokers.com/ibkr-api-page/twsapi-doc/#architecture) section, the TWS API is built using standardized socket protocol. As a result, users may develop or access alternative third party modules and classes in place of Interactive Brokers default modules through the [TWS API Download](https://www.interactivebrokers.com/ibkr-api-page/twsapi-doc/#find-the-api). While the API is adaptable for client implementations, please understand that Interactive Brokers API Support cannot provide support for non-standard implementations. While we can review your [API logs](https://www.interactivebrokers.com/ibkr-api-page/twsapi-doc/#api-logs) to affirm what content is being submitted, any further assistance will need to take place with the module’s original developer.

通过我们的[架构](https://www.interactivebrokers.com/ibkr-api-page/twsapi-doc/#architecture)部分进一步说明，TWS API 是使用标准化的套接字协议构建的。因此，用户可以通过 [TWS API 下载](https://www.interactivebrokers.com/ibkr-api-page/twsapi-doc/#find-the-api)开发或访问替代的第三方模块和类，以替代 Interactive Brokers 默认的模块。虽然 API 适用于客户端实现，但请理解 Interactive Brokers API 支持无法为非标准实现提供支持。虽然我们可以审查您的 [API 日志](https://www.interactivebrokers.com/ibkr-api-page/twsapi-doc/#api-logs)以确认正在提交的内容，但任何进一步的帮助都需要联系模块的原开发者。

This is neither an endorsement or admonishment of third party implementations. Interactive Brokers will always advise clients use our direct TWS API implementation whenever possible.

这既不是对第三方实现的认可，也不是对其的批评。Interactive Brokers 始终建议客户在可能的情况下使用我们的直接 TWS API 实现。

#### ib_insync and ib_async / ib_insync 和 ib_async

While Interactive Brokers’ API Support is aware of the ib_insync package, we [cannot provide coding assistance](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#troubleshooting) for the package.

虽然 Interactive Brokers 的 API Support 了解 ib_insync 包，但我们[无法为此包提供编码帮助](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#troubleshooting)。

With that in mind, users should be aware that the original ib_insync package is built using a legacy release of the TWS API and is no longer updated. Users who wish to implement the ib_insync structure using supported releases of the Trader Workstation should migrate to the [ib_async package](https://pypi.org/project/ib_async/), which is a modernized implementation of the package by one of its original developers.

考虑到这一点，用户应该意识到原始的 ib_insync 包是使用 TWS API 的旧版本构建的，并且不再更新。希望使用 TWS API 支持的版本实现 ib_insync 结构的用户，应该迁移到 [ib_async 包](https://pypi.org/project/ib_async/)，这是由其原始开发者现代化实现的一个包。

This is neither an endorsement or admonishment of either the ib_insync or ib_async library. Interactive Brokers will always advise clients use our direct TWS API implementation whenever possible.

这既不是对 ib_insync 或 ib_async 库的认可或批评。Interactive Brokers 将始终建议客户在可能的情况下使用我们的直接 TWS API 实现。
