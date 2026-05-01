# TWS API Documentation / TWS API 文档

## Introduction 介绍

## Notes & Limitations 注意事项和限制

## Download TWS or IB Gateway 下载 TWS 或 IB Gateway

## TWS Settings / TWS 设置

## Download the TWS API / 下载 TWS API

## TWSAPI Basics Tutorial  TWSAPI 基础教程

## Third Party API Platforms / 第三方 API 平台

## Unique Configurations / 独特的配置

## Troubleshooting & Support / 故障排除与支持

## Architecture / 架构

## Pacing Limitations / 节流限制

## Connectivity / 连接性

## Synchronous API / 同步 API

## Account & Portfolio Data / 账户与投资组合数据

## Bulletins / 公告

## Contracts (Financial Instruments) / 合约（金融工具）

An IBApi.Contract object represents trading instruments such as a stocks, futures or options. Every time a new request that requires a contract (i.e. market data, order placing, etc.) is sent to TWS, the platform will try to match the provided contract object with a single candidate.
一个 IBApi.Contract 对象代表交易工具，如股票、期货或期权。每次向 TWS 发送需要合约的新请求（例如市场数据、订单下达等），平台都会尝试将提供的合约对象与单个候选对象进行匹配。
The Contract Object  合约对象

The Contract object is an object used throughout the TWS API to define the target of your requests. Contract objects will be used for market data, portfolios, orders, executions, and even some news request. This is the staple structure used for all of the TWS API.
合约对象是 TWS API 中用于定义请求目标的对象。合约对象将用于市场数据、投资组合、订单、成交记录，甚至某些新闻请求。这是 TWS API 中使用的核心结构。

In all contracts, the minimum viable structure requires at least a conId and exchange; or a symbol, secType, exchange, primaryExchange, and currency. Derivatives will require additional fields, such as lastTradeDateOrExpiration, tradingClass, multiplier, strikes, and so on.
所有合约中，最小的可用结构至少需要 conId 和交易所；或者需要符号、secType、交易所、primaryExchange 和货币。衍生品将需要额外的字段，如 lastTradeDateOrExpiration、tradingClass、multiplier、strikes 等。

The values to the right represent the most common Contract values to pass for complete contracts. For a more comprehensive list of contract structures, please see the Contracts page.
右侧的值代表最常用的完整合约传递的合约值。有关更全面的合约结构列表，请参阅合约页面。
Contract()

ConId: int. Identifier to specify an exact contract.
ConId: int. 用于指定确切合约的标识符。

Symbol: String. Ticker symbol of the underlying instrument.
Symbol: String. 标的基础工具的股票代码。

SecType: String. Security type of the traded instrument.
SecType: String. 交易工具的证券类型。

Exchange: String. Exchange for which data or trades should be routed.
Exchange: String. 数据或交易应路由的交易所。

PrimaryExchange: String. Primary listing exchange of the instrument.
PrimaryExchange: String。该工具的主要上市交易所。

Currency: String. Base currency the instrument is traded on.
Currency: String。该工具交易的基准货币。

LastTradeDateOrContractMonth: String. For derivatives, the expiration date of the contract.
LastTradeDateOrContractMonth: String。对于衍生品，合约的到期日。

Strike: double. For derivatives, the strike price of the instrument.
Strike: double。对于衍生品，该工具的行权价。

Right: String. For derivatives, the right (P/C) of the instrument.
右侧：字符串。对于衍生品，指合约的权利（买入/卖出）。

TradingClass: String. For derivatives, the trading class of the instrument.
交易类别：字符串。对于衍生品，指合约的交易类别。
May be used to indicate between a monthly or a weekly contract.
可用于区分月合约或周合约。

Given additional structures for contracts are ever evolving, it is recommended to review the relevant Contract class in your programming language for a comprehensive review of what fields are available.
鉴于合约的额外结构在不断演变，建议查阅您编程语言中的相关合约类，以全面了解可用字段。
Contract Class Reference
合约类参考
Finding Contract Details in Trader Workstation
在交易工作台查找合约详情

If there is more than one contract matching the same description, TWS will return an error notifying you there is an ambiguity. In these cases the TWS needs further information to narrow down the list of contracts matching the provided description to a single element.
如果存在多个合约匹配同一描述，交易工作台将返回错误提示存在歧义。在这种情况下，交易工作台需要更多信息来将匹配所提供描述的合约列表缩小到一个元素。

The best way of finding a contract’s description is within TWS itself. Within TWS, you can easily check a contract’s description either by double clicking it or through the Financial Instrument Info -> Description menu, which you access by right-clicking a contract in TWS:
查找合约描述的最佳方式是在交易工作台内部。在交易工作台内，您可以通过双击合约或通过右键点击交易工作台中的合约访问"金融工具信息"->"描述"菜单，轻松检查合约的描述。

Right click menu containing Financial Instrument Info.

The description will then appear:
描述随后将显示：

Note: you can see the extended contract details by choosing Contract Info -> Details. This option will open a web page showing all available information on the contract.
注意：您可以通过选择合同信息 -> 详细信息来查看扩展的合同详情。此选项将打开一个网页，显示合同的所有可用信息。

Contract Description Window

Whenever a contract description is provided via the TWS API, the TWS will try to match the given description to a single contract. This mechanism allows for great flexibility since it gives the possibility to define the same contract in multiple ways.
每当通过 TWS API 提供合约描述时，TWS 会尝试将给定的描述与单个合约进行匹配。这种机制提供了极大的灵活性，因为它允许以多种方式定义同一个合约。

The simplest way to define a contract is by providing its symbol, security type, currency, exchange, and primary exchange. The vast majority of stocks, CFDs, Indexes or FX pairs can be uniquely defined through these four attributes. More complex contracts such as options and futures require some extra information due to their nature. Below are several examples for different types of instruments.
定义合约最简单的方法是提供其符号、安全类型、货币、交易所和主要交易所。绝大多数股票、CFD、指数或外汇对都可以通过这四个属性唯一确定。由于期权和期货等更复杂的合约的性质，需要一些额外信息。以下是不同类型工具的几个示例。
Contract Details  合约详情

Complete details about a contract in IB’s database can be retrieved using the function IBApi.EClient.reqContractDetails. This includes information about a contract’s conID, symbol, local symbol, currency, etc. which is returned in a IBApi.ContractDetails object. reqContractDetails takes as an argument a Contract object which may uniquely match one contract, and unlike other API functions it can also take a Contract object which matches multiple contracts in IB’s database. When there are multiple matches, they will each be returned individually to the function IBApi::EWrapper::contractDetails.
可以使用 IBApi.EClient.reqContractDetails 函数从 IB 的数据库中检索合约的完整详细信息。这包括合约的 conID、符号、本地符号、货币等信息，这些信息以 IBApi.ContractDetails 对象的形式返回。reqContractDetails 需要一个 Contract 对象作为参数，该对象可以唯一匹配一个合约，与其他 API 函数不同，它也可以接受一个匹配 IB 数据库中多个合约的 Contract 对象。当存在多个匹配项时，它们将分别返回给函数 IBApi::EWrapper::contractDetails。

Request for Bond details will be returned to IBApi::EWrapper::bondContractDetails instead. Because of bond market data license restrictions, there are only a few available fields to be returned in a bond contract description, namely the minTick, exchange, and short name.
请求债券详细信息将返回到 IBApi::EWrapper::bondContractDetails。由于债券市场数据许可限制，债券合约描述中只有几个可返回的字段，即 minTick、交易所和简称。

Note: Invoking reqContractDetails with a Contract object which has currency = USD will only return US contracts, even if there are non-US instruments which have the USD currency.
注意：使用具有货币=USD 的 Contract 对象调用 reqContractDetails，即使存在具有 USD 货币的非美国工具，也只会返回美国合约。

Another function of IBApi::EClient::reqContractDetails is to request the trading schedule of an instrument via the TradingHours and LiquidHours fields. The corresponding timeZoneId field will then indicate the time zone for the trading schedule of the instrument. TWS sends these timeZoneId strings to the API from the schedule responses as-is, and may not exactly match the time zones displayed in the TWS contract description.
IBApi::EClient::reqContractDetails 的另一个功能是通过 TradingHours 和 LiquidHours 字段请求工具的交易时间表。相应的 timeZoneId 字段将指示工具交易时间表所在的时区。TWS 会原样将这些 timeZoneId 字符串从时间表响应中发送给 API，可能与 TWS 合约描述中显示的时区不完全匹配。

Possible timeZoneId values are:
可能的 timeZoneId 值有：

    Europe/Riga
    Australia/NSW  澳大利亚/新南威尔士
    Europe/Warsaw  欧洲/华沙
    US/Pacific  美国/太平洋
    Europe/Tallinn  欧洲/塔林
    Japan  日本
    US/Eastern  美国/东部时间
    Europe/London  欧洲/伦敦
    Africa/Johannesburg  非洲/约翰内斯堡
    Israel  以色列
    Europe/Vilnius  欧洲/维尔纽斯
    MET
    Europe/Helsinki  欧洲/赫尔辛基
    US/Central
    Europe/Budapest
    Asia/Calcutta
    Hongkong
    Europe/Moscow  欧洲/莫斯科
    GMT  格林尼治标准时间

Request Contract Details  请求合约详情

EClient.reqContractDetails (

reqId: int. Request identifier to track data.
reqId: int. 用于跟踪数据的请求标识符。

contract: ContractDetails. the contract used as sample to query the available contracts.
contract: ContractDetails. 作为查询可用合约的样本合约。
Typically contains at least the Symbol, SecType, Exchange, and Currency.
通常至少包含 Symbol、SecType、Exchange 和 Currency。
)

Upon requesting EClient.reqContractDetails, all contracts matching the requested Contract Object will be returned to EWrapper.contractDetails or EWrapper.bondContractDetails.
在请求 EClient.reqContractDetails 时，所有与请求的 Contract Object 匹配的合约将被返回到 EWrapper.contractDetails 或 EWrapper.bondContractDetails。

self.reqContractDetails(reqId, contract)
Receive Contract Details  接收合约详情

EWrapper.contractDetails (

reqId: int. Request identifier to track data.
reqId: int. 请求标识符，用于跟踪数据。

contract: ContractDetails. Contains the full contract object contents including all information about a specific traded instrument.
contract: ContractDetails. 包含特定交易工具的所有信息，包括完整的合约对象内容。
)

Receives the full contract’s definitions This method will return all contracts matching the requested via EClientSocket::reqContractDetails. For example, one can obtain the whole option chain with it.
接收完整合约的定义。此方法将返回通过 EClientSocket::reqContractDetails 请求的所有匹配合约。例如，可以使用它获取整个期权链。

def contractDetails(self, reqId: int, contractDetails: ContractDetails):
  print(reqId, contractDetails)

EWrapper.contractDetailsEnd (

reqId: int. Request identifier to track data.
reqId: int. 请求标识符，用于跟踪数据。
)

After all contracts matching the request were returned, this method will mark the end of their reception.
当所有符合请求的合约都返回后，该方法将标记其接收的结束。

def contractDetailsEnd(self, reqId: int):
  print("ContractDetailsEnd. ReqId:", reqId)

Receive Bond Details  接收债券详情

EWrapper.bondContractDetails (

reqId: int. Request identifier to track data.
reqId: int. 请求标识符，用于跟踪数据。

contract: ContractDetails. Contains the full contract object contents including all information about a specific traded instrument.
contract: ContractDetails. 包含特定交易工具的所有信息，包括完整的合约对象内容。
)

Delivers the Bond contract data after this has been requested via reqContractDetails.
在通过 reqContractDetails 请求后，将提供债券合约数据。

def bondContractDetails(self, reqId: int, contractDetails: ContractDetails):
  printinstance(reqId, contractDetails)

Option Chains  期权链

The option chain for a given security can be returned using the function EClient.reqContractDetails. If an option contract is incompletely defined (for instance with the strike undefined) and used as an argument to EClient.reqContractDetails, a list of all matching option contracts will be returned.
使用函数 EClient.reqContractDetails 可以返回特定证券的期权链。如果期权合约定义不完整（例如行权价未定义），并将其作为 EClient.reqContractDetails 的参数使用，将返回所有匹配的期权合约列表。

One limitation of this technique is that the return of option chains will be throttled and take a longer time the more ambiguous the contract definition. The function EClient.reqSecDefOptParams was introduced that does not have the throttling limitation.
这项技术的局限性在于，期权链的返回将被限流，并且当合约定义越模糊时，所需时间越长。引入了函数 EClient.reqSecDefOptParams，该函数没有限流限制。

    It is not recommended to use EClient.reqContractDetails to receive complete option chains on an underlying, e.g. all combinations of strikes/rights/expiries.
    不建议使用 EClient.reqContractDetails 来接收标的物完整的期权链，例如所有行权价/期权类型/到期日的组合。
    For very large option chains returned from EClient.reqContractDetails, unchecking the setting in TWS Global Configuration at API -> Settings -> “Expose entire trading schedule to the API” will decrease the amount of data returned per option and help to return the contract list more quickly.
    对于从 EClient.reqContractDetails 返回的非常大的期权链，取消勾选 TWS 全局配置中的 API -> 设置 -> “向 API 暴露整个交易时间表”选项，将减少每个期权的返回数据量，并有助于更快地返回合约列表。

EClient.reqSecDefOptParams returns a list of expiries and a list of strike prices. In some cases, it is possible there are combinations of strike and expiry that would not give a valid option contract.
EClient.reqSecDefOptParams 返回到期日列表和行权价列表。在某些情况下，可能存在行权价和到期日的组合无法形成有效期权合约。
Request Option Chains  请求期权链

EClient.reqSecDefOptParams (

reqId: int. The ID chosen for the request
reqId: int. 为请求选择的 ID

underlyingSymbol: String. Contract symbol of the underlying.
underlyingSymbol: String. 标的合约的合约符号

futFopExchange: String. The exchange on which the returned options are trading. Can be set to the empty string “” for all exchanges.
futFopExchange: String. 返回的期权交易的交易所。可以设置为空字符串“”以涵盖所有交易所。

underlyingSecType: String. The type of the underlying security, i.e. STK
underlyingSecType: String. 标的证券的类型，即 STK

underlyingConId: int. The contract ID of the underlying security.
underlyingConId: int. 标的证券的合约 ID。
)

Requests security definition option parameters for viewing a contract’s option chain.
请求查看合约期权链的证券定义选项参数。

self.reqSecDefOptParams(0, "IBM", "", "STK", 8314)

Receive Option Chains  接收期权链

EWrapper.securityDefinitionOptionParameter (
EWrapper.securityDefinitionOptionParameter

reqId: int. ID of the request initiating the callback.
reqId: int. 发起回调的请求的 ID。

underlyingConId: int. The conID of the underlying security.
underlyingConId: int. 标的证券的 conID。

tradingClass: String. The option trading class.
tradingClass: String. 期权交易类别。

multiplier: String. The option multiplier.
multiplier: String. 期权乘数。

exchange: String. Exchange for which the derivative is hosted.
交易所：String。衍生品所在的交易所。

expirations: HashSet. A list of the expiries for the options of this underlying on this exchange.
到期日：HashSet。该标的在交易所上的期权到期日列表。

strikes: HashSet. A list of the possible strikes for options of this underlying on this exchange.
行权价：HashSet。该标的在交易所上的期权可能行权价列表。
)

Returns the option chain for an underlying on an exchange specified in reqSecDefOptParams There will be multiple callbacks to securityDefinitionOptionParameter if multiple exchanges are specified in reqSecDefOptParams
返回指定交易所上标的的期权链。如果 reqSecDefOptParams 中指定了多个交易所，securityDefinitionOptionParameter 将会有多个回调。

def securityDefinitionOptionParameter(self, reqId: int, exchange: str, underlyingConId: int, tradingClass: str, multiplier: str, expirations: SetOfString, strikes: SetOfFloat):
  print("SecurityDefinitionOptionParameter.", "ReqId:", reqId, "Exchange:", exchange, "Underlying conId:", underlyingConId, "TradingClass:", tradingClass, "Multiplier:", multiplier, "Expirations:", expirations, "Strikes:", strikes)

Stock Symbol Search  股票代码搜索

The function IBApi::EClient::reqMatchingSymbols is available to search for stock contracts. The input can be either the first few letters of the ticker symbol, or for longer strings, a character sequence matching a word in the security name. For instance to search for the stock symbol ‘IBKR’, the input ‘I’ or ‘IB’ can be used, as well as the word ‘Interactive’. Up to 16 matching results are returned.
函数 IBApi::EClient::reqMatchingSymbols 可用于搜索股票合约。输入可以是股票代码的前几个字母，或者对于较长的字符串，可以是与证券名称中某个词匹配的字符序列。例如，要搜索股票代码“IBKR”，可以使用输入“I”或“IB”，也可以使用单词“Interactive”。最多返回 16 个匹配结果。

There must be an interval of at least 1 second between successive calls to reqMatchingSymbols
连续调用 reqMatchingSymbols 之间必须至少间隔 1 秒

Matching stock contracts are returned to IBApi::EWrapper::symbolSamples with information about types of derivative contracts which exist (warrants, options, dutch warrants, futures).
匹配的股票合约会返回到 IBApi::EWrapper::symbolSamples，并附带关于存在衍生合约类型的信息（认股权证、期权、荷兰式认股权证、期货）。
Request Stock Contract Search
请求股票合约搜索

EClient.reqMatchingSymbols (

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

pattern: String. Either start of ticker symbol or (for larger strings) company name.
pattern: String. 要么是股票代码的开始，要么（对于较长的字符串）是公司名称。
)

Requests matching stock symbols.
匹配股票代码的请求。

self.reqMatchingSymbols(reqId, "IBM")

Receive Searched Stock Contract
接收搜索的股票合约

EWrapper.symbolSamples (  EWrapper.symbolSamples

reqID: int. Request identifier used to track data.
reqID: int. 用于跟踪数据的请求标识符。

contractDescription: ContractDescription[]. Provide an array of contract objects matching the requested descriptoin.
contractDescription: ContractDescription[]。提供一个与请求描述匹配的合约对象数组。
)

Returns array of sample contract descriptions
返回样本合约描述数组

def symbolSamples(self, reqId: int, contractDescriptions: ListOfContractDescription):
  print("Symbol Samples. Request Id: ", reqId)
  for contractDescription in contractDescriptions:
    derivSecTypes = ""
    for derivSecType in contractDescription.derivativeSecTypes:
      derivSecTypes += " "
      derivSecTypes += derivSecType
      print("Contract: conId:%s, symbol:%s, secType:%s primExchange:%s, "
        "currency:%s, derivativeSecTypes:%s, description:%s, issuerId:%s" % (
        contractDescription.contract.conId,
        contractDescription.contract.symbol,
        contractDescription.contract.secType,
        contractDescription.contract.primaryExchange,
        contractDescription.contract.currency, derivSecTypes,
        contractDescription.contract.description,
        contractDescription.contract.issuerId))

Event Trading  事件交易

Forecast and Event Contracts enable investors to trade their opinion on specific yes-or-no questions on economic indicators such as the Consumer Price Index and the Fed Funds Rate, climate indicators including temperatures and atmospheric CO2, key futures markets including  energy, metals, and equity indexes.
预测和事件合约使投资者能够对消费者价格指数和联邦基金利率等经济指标、包括温度和大气中二氧化碳的气候指标，以及能源、金属和股票指数等关键期货市场的特定是或否问题进行交易。
Introduction  介绍

Interactive Brokers models Event Contract instruments on options (for ForecastEx products) and futures options (for CME Group products).
富途证券在期权（适用于 ForecastEx 产品）和期货期权（适用于 CME Group 产品）上对事件合约工具进行建模。

Event Contracts can generally be thought of as options products in the TWS API, and their discovery workflow follows a familiar options-like sequence. This guide will make analogies to conventional index options for both ForecastEx and CME Group products.
事件合约通常可以被视为 TWS API 中的期权产品，其发现工作流程遵循熟悉的期权类序列。本指南将针对 ForecastEx 和 CME Group 产品，对传统指数期权进行类比说明。
ForecastEx Forecast Contracts
ForecastEx 预测合约

Forecast Contracts let you trade your view on the outcomes of various economic, government and environmental indicators, elections and tight races.
预测合约允许您交易对各种经济、政府和环境指标、选举及竞争激烈的赛果的看法。

Each contract pays USD 1.00 at expiry if expiring in-the-money, and your max profit per contract is USD 1.00 minus the premium you paid to purchase the contract. Forecast Contracts are quoted in USD 0.01 increments.
每个合约在到期时如果处于盈利状态，将支付 1.00 美元，并且每个合约的最大利润为 1.00 美元减去您购买合约时支付的溢价。预测合约以 0.01 美元为最小变动单位报价。

ForecastEx Website: https://forecastex.com/
ForecastEx 网站：https://forecastex.com/
CME Event Contracts  CME 事件合约

CME event contracts let you trade your view on whether the price of key futures markets will move up or down by the end of each day’s trading session.
CME 事件合约允许您通过交易，对主要期货市场的价格在每日交易会结束时是上涨还是下跌进行押注。

Each contract pays USD 100.00 at expiry if expiring in-the-money, and your max profit per contract is USD 100.00 minus the premium you paid to purchase the contract (plus fees and commissions). CME event contracts are quoted in USD 1.00 increments.
每个合约在到期时如果处于盈利状态，将支付 100.00 美元，每个合约的最大利润是 100.00 美元减去您购买合约时支付的溢价（加上费用和佣金）。CME 事件合约以 1.00 美元的增量报价。

ForecastEx Website: https://www.cmegroup.com/activetrader/event-contracts.html
ForecastEx 网站：https://www.cmegroup.com/activetrader/event-contracts.html
Contract Definition & Discovery
合约定义与发现

IB’s Event Contract instrument records use the following fields inherited from the options model:
IB 的事件合约工具记录使用以下从期权模型继承的字段：

    An underlier, which may or may not be artificial:
    一个标的物，可能是也可能不是人造的：
        For CME products, a tradable Event Contract will have the relevant CME future as its underlier. Therefore, the security type of the CME contract will be a futures option, or “FOP”.
        对于 CME 产品，一个可交易的 Event 合约将以相关的 CME 期货作为其标的物。因此，CME 合约的证券类型将是期货期权，或“FOP”。
        For ForecastEx products, IB has generated an artificial underlying index which serves as a container for related Event Contracts in the same product class. These artificial indices do not have any associated reference values and are purely an artifact of the option instrument model used to represent these Event Contracts. However, these artificial underlying indices can be used to search for groups of related Event Contracts, just as with index options. Therefore, the security type of ForecastEx products are always options, or “OPT”.
        对于 ForecastEx 产品，IB 生成了一个人造的标的指数，该指数作为同一产品类别中相关 Event 合约的容器。这些人造指数没有任何关联的参考值，纯粹是用于表示这些 Event 合约的期权工具模型的产物。然而，这些人造标的指数可以像指数期权一样用于搜索相关的一组 Event 合约。因此，ForecastEx 产品的证券类型总是期权，或“OPT”。
    An Exchange value will reflect the listing exchange of the given Event contract.
    一个交易所值将反映给定 Event 合约的上市交易所。
        ForecastEx contracts will always use “FORECASTX” as the exchange value. Note the value does not include the final “E” in “ForecastEx”.
        ForecastEx 合约始终使用“FORECASTX”作为交易所值。请注意，该值不包括“ForecastEx”中的最终“E”。
        A CME product may use “CBOT”, “CME”, “COMEX”, or “NYMEX” depending on the contract’s listing.
        CME 产品可能使用“CBOT”、“CME”、“COMEX”或“NYMEX”，具体取决于合约的上市情况。
    A Symbol value which matches the symbol of the underlier, and which reflects the issuer’s product code.
    一个与标的物符号匹配的 Symbol 值，并反映发行商的产品代码。
    A Trading Class which also reflects the issuer’s product code for the instrument, and in the case of CME Group products, is used to differentiate Event Contracts from CME futures options.
    一个 Trading Class，也反映该工具的发行商产品代码，对于 CME 集团产品，用于区分事件合约与 CME 期货期权。
        Note that many CME Group Event Contracts, which resolve against CME Group futures, are assigned a Trading Class prefixed with “EC” and followed by the symbol of the relevant futures product, to avoid naming collisions with other derivatives (i.e., proper futures options listed on the same future).
        请注意，许多 CME 集团事件合约，其结算与 CME 集团期货挂钩，会被分配一个以“EC”为前缀、后接相关期货产品符号的交易类别，以避免与其他衍生品（即同一期货上挂牌的期权）的命名冲突。
    A Put or Call (Right) value, where Call = Yes and Put = No.
    一个看涨或看跌（权利）值，其中看涨=是，看跌=否。
        Note that ForecastEx instruments do not permit Sell orders. Instead, ForecastEx positions are flattened or reduced by buying the opposing contract. CME Group Event Contracts permit both buying and selling.
        请注意，ForecastEx 工具不允许卖出订单。相反，ForecastEx 头寸通过买入相反合约来平仓或减少。CME Group 活动合约允许买入和卖出。
    An artificial Contract Month value, again used primarily for searching and filtering available instruments. Most Event Contract products do not follow monthly series as is common with index or equity options, so these Contract Month values are typically not a meaningful attribute of the instrument. Rather, they permit filtering of instruments by calendar month.
    一个人工的合约月份值，再次主要用于搜索和过滤可用工具。大多数事件合约产品并不像指数或股票期权那样遵循月度系列，因此这些合约月份值通常不是工具的有意义属性。相反，它们允许按日历月份过滤工具。
        Requesting Contract Details for a given instrument will return a “realExpirationDate”, which will correspond with the same values printed in the ForecastTrader page.
        请求给定工具的合约详情将返回一个“实际到期日”，它将与 ForecastTrader 页面中打印的相同值对应。
    A Last Trade Date, Time, and Millisecond values, which together indicate precisely when trading in an Event Contract will cease, just as with index options.
    一个最后交易日期、时间和毫秒值，它们一起精确地指示事件合约中的交易何时停止，就像指数期权一样。
    A Strike value, which is the numerical value on which the event resolution hinges. Though numerical, this value need not represent a price.
    一个行权价值，它是事件解决所依赖的数值。尽管是数值，但这个值不必代表价格。
    An instrument description (or “local symbol”) in the form "PRODUCT EXPIRATION STRIKE RIGHT", where:
    一个工具描述（或“本地符号”），形式为 "PRODUCT EXPIRATION STRIKE RIGHT" ，其中：
        PRODUCT is the issuer’s product identifier
        PRODUCT 是发行商的产品标识符
        EXPIRATION is the date of the instrument’s resolution in the form MmmDD'YY, e.g., “Sep26’24”
        EXPIRATION 是工具解决日期，形式为 MmmDD'YY ，例如“Sep26’24”
        STRIKE is the numerical value that determines the contract’s moneyness at expiration
        STRIKE 是决定合约到期时价值的数值
        RIGHT is a value YES or NO
        RIGHT 是 YES 或 NO 的值

ForecastEx Contract Example
ForecastEx 合约示例

Given the information above, we can establish a working example against the Global Carbon Dioxide Emissions contract on the ForecastTrader Website.
根据上述信息，我们可以在 ForecastTrader 网站上针对全球二氧化碳排放合约建立一个工作示例。

Reviewing the page to the right, we can see all of the contract details necessary to get started.
查看右侧页面，我们可以看到所有开始交易所需的合约详细信息。

    Above the chart next to the contract name, we can see the Symbol, “GCE”.
    在合约名称旁边的图表上方，我们可以看到符号“GCE”。
    On the left side of the web page, we can find the contract’s expiration date, June 30, 2026.
    在网页的左侧，我们可以找到合约的到期日期，2026 年 6 月 30 日。
    Equally important is the value on the right, “Market closes in 287 days.”
    同样重要的是右侧的值“市场将在 287 天后关闭”。
    The bolded excess on the top, 40,5000, indicates our strike price. This can be corroborated by the table on the left which acts like an Option Chain table users may be more familiar with.
    顶部加粗的超额部分，40,5000，表示我们的行权价。这可以通过左侧的表格得到证实，该表格类似于用户可能更熟悉的期权链表。

While not explicitly stated in the web page, there are several details that may be inferred based on the information present:
虽然网页上没有明确说明，但根据现有信息可以推断出以下几点：

    All ForecastEx contracts use the “OPT” security type, as mentioned in the Contract Definition & Discovery section above.
    所有 ForecastEx 合约都使用“OPT”安全类型，如上文合同定义与发现部分所述。
    The ForecastEx exchange value is always listed as “FORECASTX”.
    ForecastEx 交易所的值始终显示为“FORECASTX”。
    All currently offered Event Contracts are hosted in the United States of America, and therefore will always use “USD” as their currency value.
    所有当前提供的 Event 合约均托管在美国，因此其货币价值始终使用“USD”。
    “Yes” or “No” contracts are based on option rights, “Call” and “Put” respectively.
    “是”或“否”合约基于期权权利，分别为“看涨”和“看跌”。

Displays an example of a Forecast Contract being shown in the Forecast Trader.

In order to request our specific contract, we will need to focus on the “Market closes in 287 days” statement. This value indicates the last day the contract may be traded.
为了申请我们的特定合约，我们需要关注“市场将在 287 天后关闭”这一说明。这个值表示合约最后可以交易的日子。

This document is written on the 19th of March, 2025. That is the 78th day of the calendar year.
本文撰写于 2025 年 3 月 19 日。这是该年的第 78 天。

Given the context that this is day 78, and the market will close in 287 days, the contract’s last trade date would then be the 365th day of the year, or December 31st, 2025.
考虑到今天是第 78 天，而市场将在 287 天后关闭，那么合约的最后交易日将是该年的第 365 天，即 2025 年 12 月 31 日。

Given the TWS API date standards, this will be written as 20251231.
根据 TWS API 的日期标准，这将表示为 20251231。

This information can now be distilled into a standard TWS API contract definition:
这些信息现在可以提炼为标准的 TWS API 合约定义：

Symbol: “GCE”

SecType: “OPT”

Exchange: “FORECASTX”

Currency: “USD”  货币：“USD”

LastTradeDateOrContractMonth: “20251231”
最后交易日期或合约月份：“20251231”

Right: “C”  右：“C”

Strike: 40500  敲定价：40500

contract= Contract()
contract.symbol = "GCE"
contract.secType = "OPT"
contract.currency = "USD"
contract.exchange = "FORECASTX"
contract.lastTradeDateOrContractMonth = "20251231"
contract.right = "C"
contract.strike = 40500

Market Data  市场数据

Requesting market data for event contracts will follow the same request structure as for any other security type.
请求事件合约的市场数据将遵循与其他任何证券类型相同的请求结构。

Noted in our Contract Definition & Discovery section, ForecastEx instruments do not support buying and selling. Therefore, “BID” and “ASK” values will not correlate to buy and sell values, but the “Highest Bid” and “Buy Yes Now at” prices for the Bid and Ask respectively.
如我们在合约定义与发现部分所述，ForecastEx 工具不支持买入和卖出。因此，“BID”和“ASK”值不会与买入和卖出值相关联，但“最高买入价”和“买入立即成交价”将分别对应于 Bid 和 Ask 的价格。

Because “BID” and “ASK” do not correctly directly to Buying and Selling, historical “Trades” nor real-time “Last” prices will not be available.
由于“BID”和“ASK”不能直接对应买入和卖出，因此历史“交易”价格和实时“最后”价格将不可用。
Order Submission  订单提交

Order Submission for Event Contracts function the same as any other instrument offered at Interactive Brokers.
事件合约的订单提交功能与其他在 Interactive Brokers 提供的任何工具相同。

There are some unique order behaviors for both CME Group and ForecastEx contracts:
对于 CME Group 和 ForecastEx 合约，有一些独特的订单行为：

    Event Contracts only support Limit Orders
    事件合约仅支持限价单
    Event Contracts only support a Time in Force of Day, Good till Canceled, or Immediate-Or-Cancel.
    事件合约仅支持当日有效、有效至撤销或立即成交撤销的时间有效方式。
    Event Contracts do not support Cash Quantity in the TWS API. Orders must be submitted as whole-share values.
    事件合约在 TWS API 中不支持现金数量。订单必须以整股值提交。
    CME Group instruments can be bought and sold and function as normal futures options.
    CME Group 的合约可以买卖，并像普通期货期权一样运作。
    ForecastEx instruments cannot be sold, only bought. To exit or reduce a position, one must buy the opposing Event Contract, and IB will net the opposing positions together automatically.
    ForecastEx 仪器不能卖出，只能买入。要退出或减少头寸，必须买入相反的事件合约，IB 将自动将相反的头寸进行结算。

Event Contracts cannot be sold short.
事件合约不能卖空。

Order Example  订单示例

Reviewing the same material as our Contract Example, we have all the tools needed to submit our order with some additional context available in the Order Ticket, featured on the right.
与我们的合同示例相同的内容进行审查，我们已经拥有提交订单所需的所有工具，并且在订单票证中提供了额外的上下文信息，如右侧所示。

We are already aware that:
我们已经清楚：

    ForecastEx contracts are always “BUY” orders.
    ForecastEx 合约总是“买入”订单。
    Event Contracts only support “LMT” as the Order Type.
    事件合约仅支持“限价”作为订单类型。

This leaves us to decide the quantity, limit price, and time-in-force values.
这让我们需要决定数量、限价和时间有效值。

We can set our limit price based on the values shown in the Order Ticket, or base the value on the Bid and Ask Price from our Requested Market Data.
我们可以根据订单票中显示的值设置我们的限价，或者根据请求的市场数据中的买卖价来设置该值。

Displays an example of an order ticket being filled out for a Forecast Contract.

Given the information above, we are able to create a full order ticket.
根据上述信息，我们能够创建一个完整的订单票。

Action: “BUY”  操作：“买入”

TotalQuantity: 1000  总数量：1000

OrderType: “LMT”  订单类型：“限价”

LmtPrice: 0.57  限价：0.57

Tif: “DAY”  Tif: “日”

order = Order()
order.action = "BUY"
order.orderType = "LMT"
order.totalQuantity = 1000
order.lmtPrice = 0.57

Other Functionality  其他功能

    Event Contracts fundamentally behave like Options or Futures Options. As a result, instrument rules, position information, and instrument-specific behavior will follow the same presentation in the Trader Workstation as those other instruments.
    事件合约在本质上表现得像期权或期货期权。因此，合约规则、头寸信息和特定合约行为在交易工作站中的显示将与那些其他合约相同。
    Market Scanners are not currently available to research Event Contracts. Users will need to discover Event Contract symbols through Interactive Brokers’ ForecastTrader.
    市场扫描器目前无法用于研究事件合约。用户需要通过 Interactive Brokers 的 ForecastTrader 发现事件合约符号。

Error Handling  错误处理

When a client application sends a message to TWS which requires a response which has an expected response (i.e. placing an order, requesting market data, subscribing to account updates, etc.), TWS will almost either always 1) respond with the relevant data or 2) send an error message to EWrapper.error().
当客户端应用程序向 TWS 发送需要响应的消息时（例如下单、请求市场数据、订阅账户更新等），TWS 几乎总是会 1)返回相关数据或 2)向 EWrapper.error()发送错误消息。

    Exceptions when no response can occur: Also, if a request is made prior to full establishment of connection (denoted by a returned 2104 or 2106 error code “Data Server is Ok”), there may not be a response from the request.
    无响应的异常情况：此外，如果在连接完全建立之前（通过返回的 2104 或 2106 错误代码“数据服务器正常”）发出请求，则可能不会收到响应。

Error messages sent by the TWS are handled by the EWrapper.error() method. The EWrapper.error() event contains the originating request Id (or the orderId in case the error was raised when placing an order), a numeric error code and a brief description. It is important to keep in mind that this function is used for true error messages as well as notifications that do not mean anything is wrong.
TWS 发送的错误消息由 EWrapper.error()方法处理。EWrapper.error()事件包含发起请求的请求 ID（如果错误是在下单时引发的，则为 orderId）、数值错误代码和简要描述。重要的是要记住，此函数既用于真正的错误消息，也用于表示没有问题的通知。

API Error Messages when TWS is not set to the English Language
当 TWS 未设置为英语语言时的 API 错误消息

    Currently on the Windows platform, error messages are sent using Latin1 encoding. If TWS is launched in a non-Western language, it is recommended to enable the setting at Global Configuration -> API -> Settings to “Show API error messages in English”.
    目前，在 Windows 平台上，错误消息使用 Latin1 编码发送。如果 TWS 以非西文语言启动，建议在全局配置 -> API -> 设置中启用“以英文显示 API 错误消息”的设置。

Understanding Message Codes
理解消息代码

The TWS uses the EWrapper.error method not only to deliver errors but also warnings or informative messages. This is done mostly for simplicity’s sake. Below is a table with all the messages which can be sent by the TWS/IB Gateway. All messages delivered by the TWS are usually accompanied by a brief but meaningful description pointing in the direction of the problem.
TWS 不仅使用 EWrapper.error 方法发送错误，还用于发送警告或信息性消息。这主要是为了简化操作。以下是 TWS/IB 网关可以发送的所有消息的表格。TWS 发送的所有消息通常都附带简短但具有意义的描述，指明问题所在。

Remember that the TWS API simply connects to a running TWS/IB Gateway which most of times will be running on your local network if not in the same host as the client application. It is your responsibility to provide reliable connectivity between the TWS and your client application.
请记住，TWS API 仅连接到正在运行的 TWS/IB 网关，而该网关大多数时间将运行在您的本地网络中，如果不在客户端应用程序的同一主机上。确保 TWS 和您的客户端应用程序之间的可靠连接是您的责任。
System Message Codes  系统消息代码

The messages in the table below are not a consequence of any action performed by the client application. They are notifications about the connectivity status between the TWS and our servers. Your client application must pay special attention to them and handle the situation accordingly. You are very likely to lose connectivity to our servers at least once a day due to our daily server maintenance downtime as clearly detailed in our Current System Status page. Note that after the system reset, the TWS/IB Gateway will automatically reconnect to our servers and you can resume your operations normally.
下表中的消息并非由客户端应用程序的任何操作引起。它们是关于 TWS 与我们的服务器之间连接状态的通知。客户端应用程序必须特别关注这些消息，并相应地处理情况。由于我们每日服务器维护停机时间（如我们在当前系统状态页面中详细说明的那样），您很可能每天至少会失去一次与我们的服务器的连接。请注意，系统重置后，TWS/IB 网关将自动重新连接到我们的服务器，您可以正常恢复操作。

Note:  注意：

    During a reset period, there may be an interruption in the ability to log in or manage orders. Existing orders (native types) will operate normally although execution reports and simulated orders will be delayed until the reset is complete. It is not recommended to operate during the scheduled reset times.
    在重置期间，登录或管理订单的能力可能会中断。现有订单（原生类型）将正常运行，尽管执行报告和模拟订单会延迟到重置完成。不建议在计划的重置时间内进行操作。

Code  代码	TWS message  TWS 消息	Additional notes  附加说明
1100	Connectivity between IB and the TWS has been lost.
IB 与 TWS 之间的连接已丢失。	Your TWS/IB Gateway has been disconnected from IB servers. This can occur because of an internet connectivity issue, a nightly reset of the IB servers, or a competing session.
您的 TWS/IB 网关已与 IB 服务器断开连接。这可能是因为网络连接问题、IB 服务器的夜间重置或存在竞争会话。
1101	Connectivity between IB and TWS has been restored- data lost.*
IB 与 TWS 之间的连接已恢复-数据丢失。*	The TWS/IB Gateway has successfully reconnected to IB’s servers. Your market data requests have been lost and need to be re-submitted.
TWS/IB 网关已成功重新连接到 IB 的服务器。您的市场数据请求已丢失，需要重新提交。
1102	Connectivity between IB and TWS has been restored- data maintained.
IB 与 TWS 之间的连接已恢复-数据保持。	The TWS/IB Gateway has successfully reconnected to IB’s servers. Your market data requests have been recovered and there is no need for you to re-submit them.
TWS/IB 网关已成功重新连接到 IB 的服务器。您的市场数据请求已恢复，您无需重新提交。
1300	TWS socket port has been reset and this connection is being dropped. Please reconnect on the new port – <port_num>
TWS 套接字端口已重置，此连接将被断开。请在新端口上重新连接——<port_num>	The port number in the TWS/IBG settings has been changed during an active API connection.
在活动 API 连接期间，TWS/IBG 设置中的端口号已更改。
Error Codes  错误代码

Error codes in different ranges have different indications.
不同范围内的错误代码具有不同的指示。
Code  代码	TWS message  TWS 消息	Additional notes  附加说明
100	Max rate of messages per second has been exceeded.
每秒消息数已超过最大速率。	The client application has exceeded the rate of 50 messages/second. The TWS will likely disconnect the client application after this message.
客户端应用程序已超过每秒 50 条消息的速率。TWS 可能会在收到此消息后断开客户端应用程序。
101	Max number of tickers has been reached.
已达到最大股票代码数量。	““The current number of active market data subscriptions in TWS and the API altogether has been exceeded. This number is calculated based on a formula which is based on the equity, commissions, and quote booster packs in an account. Active lines can be checked in Tws using the Ctrl-Alt-= combination””
““TWS 和 API 中当前活跃的市场数据订阅总数已超出。该数量是根据账户中的股票、佣金和报价加速包计算得出的。可以在 TWS 中使用 Ctrl-Alt-=组合键检查活跃线路”“”
102	Duplicate ticker ID.  重复的股票代码 ID。	A market data request used a ticker ID which is already in use by an active request.
一个市场数据请求使用了已被活跃请求使用的股票代码 ID。
103	Duplicate order ID.  重复的订单 ID。	An order was placed with an order ID that is less than or equal to the order ID of a previous order from this client
使用了一个小于或等于此客户先前订单订单 ID 的订单 ID 提交了订单
104	Can’t modify a filled order.
无法修改已成交的订单。	An attempt was made to modify an order which has already been filled by the system.
尝试修改系统已成交的订单。
105	Order being modified does not match original order.
正在修改的订单与原始订单不匹配。	An order was placed with an order ID of a currently open order but basic parameters differed (aside from quantity or price fields)
已使用当前一个开放订单的订单 ID 提交订单，但基本参数不同（除数量或价格字段外）
106	Can’t transmit order ID:  无法传输订单 ID：	Order ID may not be transmitted. This is most often caused by an invalid order type or order formatting.
订单 ID 可能无法传输。这通常是由无效的订单类型或订单格式错误引起的。
107	Cannot transmit incomplete order.
无法传输不完整的订单。	Order is missing a required field.
订单缺少必填字段。
109	Price is out of the range defined by the Percentage setting at order defaults frame. The order will not be transmitted.
价格超出订单默认设置中百分比定义的范围。订单将不会被传输。	Price entered is outside the range of prices set in TWS or IB Gateway Order Precautionary Settings
输入的价格不在 TWS 或 IB Gateway 订单预防设置中设定的价格范围内。
110	The price does not conform to the minimum price variation for this contract.
价格不符合此合约的最小价格变动要求。	An entered price field has more digits of precision than is allowed for this particular contract. Minimum increment information can be found on the IB Contracts and Securities Search page.
输入的价格字段比此特定合约允许的精度位数更多。最小增量信息可以在 IB 合约和证券搜索页面上找到。
111	The TIF (Tif type) and the order type are incompatible.
TIF（Tif 类型）和订单类型不兼容。	The time in force specified cannot be used with this order type. Please refer to order tickets in TWS for allowable combinations.
指定的有效时间无法与此订单类型一起使用。请参考 TWS 中的订单票以查找允许的组合。
113	The Tif option should be set to DAY for MOC and LOC orders.
Tif 选项应设置为 DAY，用于 MOC 和 LOC 订单。	Market-on-close or Limit-on-close orders should be sent with time in force set to ‘DAY’
市价收盘或限价收盘订单应设置时间为 DAY
114	Relative orders are valid for stocks only.
相对订单仅适用于股票。	This error is deprecated.
此错误已弃用。
115	““Relative orders for US stocks can only be submitted to SMART, SMART_ECN, INSTINET, or PRIMEX.””
““美国股票的相对订单只能提交至 SMART、SMART_ECN、INSTINET 或 PRIMEX。””	This error is deprecated.
此错误已弃用。
116	The order cannot be transmitted to a dead exchange.
订单无法传输至已关闭的交易所。	Exchange field is invalid.
交易所字段无效。
117	The block order size must be at least 50.
限价单的尺寸至少为 50。	Caused by a block order submission using a quantity less than 50.
由提交使用小于 50 数量的限价单引起。
118	VWAP orders must be routed through the VWAP exchange.
VWAP 订单必须通过 VWAP 交易所路由。
119	Only VWAP orders may be placed on the VWAP exchange.
仅可在 VWAP 交易所下设置 VWAP 订单。	““When an order is routed to the VWAP exchange, the type of the order must be defined as ‘VWAP’.””
““当订单路由至 VWAP 交易所时，订单类型必须定义为‘VWAP’。””
120	It is too late to place a VWAP order for today.
今天已经太晚，无法提交 VWAP 订单了。	The cutoff has passed for the current day to place VWAP orders.
当前日期的 VWAP 订单截止时间已过。
121	“Invalid BD flag for the order. Check “”Destination”” and “”BD”” flag.”
订单的 BD 标志无效。请检查“目的地”和“BD”标志。	This error is deprecated.
此错误已过时。
122	No request tag has been found for order:
未找到订单的请求标签：	Caused when request encoding to socket improperly formed.
由请求编码到套接字格式不正确引起。
123	No record is available for conid:
没有为 conid 记录：	The specified contract ID cannot be found. This error is deprecated.
指定的合约 ID 无法找到。此错误已弃用。
124	No market rule is available for conid:
没有为 conid 提供市场规则：	Returned in the event a market rule is not applied to a given contract identifier. May be caused when interacting with a non-tradeable instrument such as an Index.
当未对特定合约标识符应用市场规则时返回。可能是与非交易工具（如指数）交互时引起。
125	Buy price must be the same as the best asking price.
买入价格必须与最优卖价相同。	Caused by a Buy order exceptionally above the Best Ask price. Please request market data to identify the NBO.
由买入订单异常高于最优卖价引起。请请求市场数据以识别 NBO。
126	Sell price must be the same as the best bidding price.
卖出价格必须与最优买价相同。	Caused by a Sell order exceptionally below the Best Bid price. Please request market data to identify the NBB.
由卖单异常低于最佳买价引起。请请求市场数据以识别 NBB。
129	VWAP orders must be submitted at least three minutes before the start time.
VWAP 订单必须在开始时间前至少提交三分钟。	The start time specified in the VWAP order is less than 3 minutes after when it is placed.
VWAP 订单中指定的开始时间小于下单后三分钟。
131	““The sweep-to-fill flag and display size are only valid for US stocks routed through SMART, and will be ignored.””
“扫单填充标志和显示尺寸仅适用于通过 SMART 路由的美国股票，将被忽略。””	Sweep-to-fill used on an unsupported order type.
在不受支持的订单类型上使用了扫仓填充。
132	This order cannot be transmitted without a clearing account.
此订单在没有清算账户的情况下无法传输。	Order parameters do not include a valid clearing account.
订单参数不包含有效的清算账户。
133	Submit new order failed.  提交新订单失败。	Failure in order submission. May be caused by order parameters or network connectivity.
提交订单失败。可能由订单参数或网络连接问题引起。
134	Modify order failed.  修改订单失败。	Unable to modify an existing order. The order may have already been executed or cancelled. Please request open orders to verify current order status.
无法修改现有订单。订单可能已成交或取消。请请求查看当前订单以确认订单状态。
135	Can’t find order with ID =
找不到 ID 为=的订单。	An attempt was made to cancel an order not currently in the system.
尝试取消一个当前不在系统中的订单。
136	This order cannot be cancelled.
此订单无法取消。	““An attempt was made to cancel an order than cannot be cancelled, for instance because””
““尝试取消一个无法取消的订单，例如因为””
137	VWAP orders can only be cancelled up to three minutes before the start time.
VWAP 订单只能在开始前最多三分钟取消。	VWAP order cancellation taking place within three minutes of submission.
提交后三分钟内发生的 VWAP 订单取消。
138	Could not parse ticker request:
无法解析股票代码请求：	“Ticker symbol cannot be parsed, likely due to the inclusion of invalid symbols or content.”
“股票代码无法解析，可能是由于包含了无效符号或内容。”
139	Parsing error:  解析错误：	Error in command syntax generated parsing error.
命令语法错误导致解析错误。
140	The size value should be an integer:
大小值应为整数：	The size field in the Order class has an invalid type.
订单类中的 size 字段类型无效。
141	The price value should be a double:
价格值应为双精度浮点数：	A price field in the Order type has an invalid type.
订单类型中的价格字段类型无效。
142	Institutional customer account does not have account info
机构客户账户没有账户信息	Institutional account structure is not including account details in order submission.
机构账户结构在订单提交时不包含账户详情。
143	Requested ID is not an integer number.
请求的 ID 不是一个整数。	The IDs used in API requests must be integer values.
API 请求中使用的 ID 必须是整数值。
144	““Order size does not match total share allocation. To adjust the share allocation, right-click on the order and select Modify > Share Allocation ””
““订单数量与总股份数量不匹配。要调整股份数量分配，请右键单击订单并选择修改 > 股份数量分配 ””
145	Error in validating entry fields –
验证输入字段时出错 –	An error occurred with the syntax of a request field.
请求字段语法发生错误。
146	Invalid trigger method.  无效的触发方法。	The trigger method specified for a method such as stop or trail stop was not one of the allowable methods.
对于停止或跟踪停止等操作指定的触发方法不是允许的方法之一。
147	The conditional contract info is incomplete.
条件合约信息不完整。
148	“Conditional submission of orders is supported for Limit, Market, MidPrice, Relative and Snap order types only. Conditional cancelation of orders is supported for Limit and MidPrice order types only.”
“仅支持 Limit、Market、MidPrice、Relative 和 Snap 订单类型的条件下单。仅支持 Limit 和 MidPrice 订单类型的条件撤单。”
151	This order cannot be transmitted without a user name.
此订单在未提供用户名的情况下无法传输。	In DDE the user name is a required field in the place order command.
在 DDE 中，用户名是下单命令中的必填字段。
152	“The “”hidden”” order attribute may not be specified for this order.”
““隐藏””订单属性不适用于此订单。	The order in question cannot be placed as a hidden order. See- https://www.interactivebrokers.com/en/index.php?f=596
该订单不能作为隐藏订单下单。请参考-https://www.interactivebrokers.com/en/index.php?f=596
153	EFPs can only be limit orders.
EFPs 只能作为限价单。	This error is deprecated.
此错误已弃用。
154	Orders cannot be transmitted for a halted security.
当证券暂停交易时，订单无法传输。	A security was halted for trading when an order was placed.
在订单提交时，该证券被暂停交易。
155	A sizeOp order must have a user name and account.
尺寸 Op 订单必须包含用户名和账户。	This error is deprecated.
此错误已弃用。
156	A SizeOp order must go to IBSX
一个 SizeOp 订单必须发送到 IBSX	This error is deprecated.
这个错误已经过时。
157	An order can be EITHER Iceberg or Discretionary. Please remove either the Discretionary amount or the Display size.
一个订单可以是冰山订单或任意订单。请移除任意订单量或显示大小。	In the Order class extended attributes the fields ‘Iceberg’ and ‘Discretionary’ cannot
在订单类的扩展属性中，'Iceberg' 和 'Discretionary' 字段不能
158	You must specify an offset amount or a percent offset value.
您必须指定一个偏移量或百分比偏移值。	TRAIL and TRAIL STOP orders must have an absolute offset amount or offset percentage specified.
TRAIL 和 TRAIL STOP 订单必须指定一个绝对偏移量或偏移百分比。
159	The percent offset value must be between 0% and 100%.
百分比偏移值必须在 0%到 100%之间。	A percent offset value was specified outside the allowable range of 0% and 100%.
指定的百分比偏移值超出了 0%到 100%的允许范围。
160	The size value cannot be zero.
大小值不能为零。	The size of an order must be a positive quantity.
订单的大小必须为正数。
161	Cancel attempted when order is not in a cancellable state. Order permId =
尝试取消订单时，订单不在可取消状态。订单 permId =	An attempt was made to cancel an order not active at the time.
尝试取消一个在取消时非活跃的订单。
162	Historical market data Service error message.
历史市场数据服务错误消息。
163	The price specified would violate the percentage constraint specified in the default order settings.
指定的价格将违反默认订单设置中指定的百分比约束。	The order price entered is outside the allowable range specified in the Order Precautionary Settings of TWS or IB Gateway
输入的订单价格超出 TWS 或 IB Gateway 订单预防设置中指定的允许范围。
164	There is no market data to check price percent violations.
没有市场数据用于检查价格百分比违规。	No market data is available for the specified contract to determine whether the specified price is outside the price percent precautionary order setting.
没有市场数据可用于指定的合约，以确定指定的价格是否超出价格百分比预防性订单设置。
165	Historical market Data Service query message.
历史市场数据服务查询消息。	““There was an issue with a historical data request, such is no such data in IB’s database. Note this message is not specific to the API.””
““历史数据请求出现问题，例如 IB 数据库中不存在该数据。请注意此消息并非特定于 API。””
166	HMDS Expired Contract Violation.
HMDS 到期合约违规。	Historical data is not available for the specified expired contract.
指定的到期合约历史数据不可用。
167	VWAP order time must be in the future.
VWAP 订单时间必须在未来。	The start time of a VWAP order has already passed.
VWAP 订单的起始时间已经过去。
168	Discretionary amount does not conform to the minimum price variation for this contract.
自由裁量金额不符合该合约的最小价格变动。	The discretionary field is specified with a number of degrees of precision higher than what is allowed for a specified contract.
自由裁量字段指定的精度高于指定合约允许的精度。
200	No security definition has been found for the request.
未找到请求的安全定义。	““The specified contract does not match any in IB’s database, usually because of an incorrect or missing parameter.””
“指定的合约与 IB 数据库中的任何合约都不匹配，通常是因为参数错误或缺失。”
200	The contract description specified for is ambiguous
指定的合约描述不明确	Ambiguity may occur when the contract definition provided is not unique.
当提供的合约定义不唯一时，可能会出现歧义。
200		““For some stocks that has the same Symbol, Currency and Exchange, you need to specify the IBApi.Contract.PrimaryExch attribute to avoid ambiguity. Please refer to a sample stock contract here.””
“对于 Symbol、Currency 和 Exchange 都相同的某些股票，您需要指定 IBApi.Contract.PrimaryExch 属性以避免歧义。请参考此处的一个股票合约示例。”
200		““For futures that has multiple multipliers for the same expiration, You need to specify the IBApi.Contract.Multiplier attribute to avoid ambiguity. Please refer to a sample futures contract here.””
“对于到期日相同的具有多个乘数号的期货，您需要指定 IBApi.Contract.Multiplier 属性以避免歧义。请参考此处的一个期货合约示例。”
201	Order rejected – Reason:  订单被拒绝 – 原因：	An attempted order was rejected by the IB servers. See Order Placement Considerations for additional information/considerations for these errors.
尝试的订单被 IB 服务器拒绝。有关这些错误的更多信息/注意事项，请参阅订单放置注意事项。
202	Order cancelled – Reason:
订单取消 – 原因：	An active order on the IB server was cancelled. See Order Placement Considerations for additional information/considerations for these errors.
IB 服务器上的活跃订单已被取消。有关这些错误的更多信息/注意事项，请参阅“订单放置注意事项”。
203	The security is not available or allowed for this account.
此账户无法使用或未被允许使用该证券。	The specified security has a trading restriction with a specific account.
指定的证券对该特定账户有交易限制。
203	The contract description specified for %S is ambiguous; you must specify the currency.
为%S 指定的合约描述不明确；您必须指定货币。	The contract definition is incomplete. The currency must be included.
合约定义不完整。必须包含货币。
300	Can’t find EId with ticker Id:
找不到与股票代码 ID 匹配的 EId：	An attempt was made to cancel market data for a ticker ID that was not associated with a current subscription. With the DDE API this occurs by clearing the spreadsheet cell.
尝试取消与当前订阅无关的股票代码 ID 的市场数据。使用 DDE API 时，这通过清除电子表格单元格发生。
301	Invalid ticker action:  无效的股票操作：
302	Error parsing stop ticker string:
解析停止交易代码字符串时出错：
303	Invalid action:  无效操作：	An action field was specified that is not available for the account. For most accounts this is only BUY or SELL. Some institutional accounts also have the options SSHORT or SLONG available.
指定了一个对账户不可用的操作字段。对大多数账户来说，这只能是买入或卖出。一些机构账户也有 SSHORT 或 SLONG 的选项可用。
304	Invalid account value action:
无效账户值操作：
305	““Request parsing error, the request has been ignored.””
“请求解析错误，请求已被忽略。”	The syntax of a DDE request is invalid.
DDE 请求的语法无效。
306	Error processing DDE request:
处理 DDE 请求时出错：	An issue with a DDE request prevented it from processing.
DDE 请求出现问题，导致无法处理。
307	Invalid request topic:  无效的请求主题：	The ‘topic’ field in a DDE request is invalid.
DDE 请求中的“topic”字段无效。
308	Unable to create the ‘API’ page in TWS as the maximum number of pages already exists.
由于已达到最大页面数量，无法在 TWS 中创建“API”页面。	““An order placed from the API will automatically open a new page in classic TWS, however there are already the maximum number of pages open.””
“从 API 下达的订单将自动在经典 TWS 中打开新页面，但目前已达到最大页面数量。”
309	““Max number (3) of market depth requests has been reached. Note: TWS currently limits users to a maximum of 3 distinct market depth requests. This same restriction applies to API clients, however API clients may make multiple market depth requests for the same security.””
“已达到市场深度请求的最大数量（3 个）。注意：TWS 目前限制用户最多只能发起 3 个不同的市场深度请求。相同的限制也适用于 API 客户端，但 API 客户端可以对同一证券发起多个市场深度请求。”	“Maximum market depth requests exceeded. Please see our Market Data Line Documentation for more information.”
超出最大市场深度请求数量。请参阅我们的市场数据线路文档以获取更多信息。”
310	Can’t find the subscribed market depth with tickerId:
找不到具有 tickerId 的订阅市场深度：	An attempt was made to cancel market depth for a ticker not currently active.
尝试取消当前未激活的证券的市场深度请求。
311	The origin is invalid.  来源无效。	The origin field specified in the Order class is invalid.
Order 类中指定的 origin 字段无效。
312	The combo details are invalid.
组合详情无效。	Combination contract specified has invalid parameters.
指定的组合合约参数无效。
313	The combo details for leg ” are invalid.
leg ”的组合详情无效。	A combo leg was not defined correctly.
组合腿未正确定义。
314	Security type ‘BAG’ requires combo leg details.
Security 类型‘BAG’需要组合腿详情。	When specifying security type as ‘BAG’ make sure to also add combo legs with details.
当指定安全类型为“BAG”时，请确保同时添加包含详细信息的组合腿。
315	Stock combo legs are restricted to SMART order routing.
股票组合腿仅限于 SMART 订单路由。	Make sure to specify ‘SMART’ as an exchange when using stock combo contracts.
在使用股票组合合同时，请确保将交易所指定为“SMART”。
316	Market depth data has been HALTED. Please re-subscribe.
市场深度数据已暂停。请重新订阅。	You need to re-subscribe to start receiving market depth data again.
您需要重新订阅才能再次接收市场深度数据。
317	Market depth data has been RESET. Please empty deep book contents before applying any new entries.
市场深度数据已被重置。请在应用任何新条目前清空深度书内容。
319	Invalid log level  无效的日志级别	Make sure that you are setting a log level to a value in range of 1 to 5.
请确保您设置的日志级别在 1 到 5 的范围内。
320	Server error when reading an API client request.
读取 API 客户端请求时服务器出错。
321	Server error when validating an API client request.
验证 API 客户端请求时服务器出错。
322	Server error when processing an API client request.
处理 API 客户端请求时出现服务器错误。
323	Server error: cause – s
服务器错误：原因 – s
324	Server error when reading a DDE client request (missing information).
读取 DDE 客户端请求时出现服务器错误（信息缺失）。	Make sure that you have specified all the needed information for your request.
请确保您已为您的请求指定了所有所需信息。
325	Discretionary orders are not supported for this combination of exchange and order type.
这种交易所和订单类型的组合不支持任意指令订单。	Make sure that you are specifying a valid combination of exchange and order type for the discretionary order.
请确保您为任意指令订单指定了有效的交易所和订单类型组合。
326	Unable to connect as the client id is already in use. Retry with a unique client id.
无法连接，因为客户端 ID 已被使用。请使用唯一的客户端 ID 重试。	Another client application is already connected with the specified client id.
另一个客户端应用程序已使用指定的客户端 ID 连接。
327	Only API connections with clientId set to 0 can set the auto bind TWS orders property.
只有将 clientId 设置为 0 的 API 连接才能设置自动绑定 TWS 订单的属性。
328	Trailing stop orders can be attached to limit or stop-limit orders only.
跟踪止损单只能附加到限价单或止损限价单上。	Indicates attempt to attach trail stop to order which was not a limit or stop-limit.
表示尝试将跟踪止损附加到非限价单或止损限价单的订单上。
329	Order modify failed. Cannot change to the new order type.
订单修改失败。无法更改为新的订单类型。	You are not allowed to modify initial order type to the specific order type you are using.
不允许将初始订单类型修改为你正在使用的特定订单类型。
330	Only FA or STL customers can request managed accounts list.
只有 FA 或 STL 客户可以请求管理账户列表。	Make sure that your account type is either FA or STL.
确保你的账户类型是 FA 或 STL。
331	Internal error. FA or STL does not have any managed accounts.
内部错误。FA 或 STL 没有任何管理账户。	You do not have any managed accounts.
您没有任何受管理的账户。
332	The account codes for the order profile are invalid.
订单配置文件中的账户代码无效。	You need to check that the account codes you specified for your request are valid.
您需要检查您在请求中指定的账户代码是否有效。
333	Invalid share allocation syntax.
股份分配语法无效。
334	Invalid Good Till Date order
无效的到期日订单	Check you order settings.
检查您的订单设置。
335	Invalid delta: The delta must be between 0 and 100.
无效的 delta：delta 必须在 0 和 100 之间。
336	““The time or time zone is invalid. The correct format is hh:mm:ss xxx where xxx is an optionally specified time-zone. E.g.: 15:59:00 EST Note that there is a space between the time and the time zone. If no time zone is specified, local time is assumed.””
““时间或时区无效。正确的格式是 hh:mm:ss xxx，其中 xxx 是可选指定的时间区。例如：15:59:00 EST 注意时间和时区之间有一个空格。如果没有指定时区，则假定使用本地时间。””
337	““The date, time, or time-zone entered is invalid. The correct format is yyyymmdd hh:mm:ss xxx where yyyymmdd and xxx are optional. E.g.: 20031126 15:59:00 ESTNote that there is a space between the date and time, and between the time and time-zone.””
“输入的日期、时间或时区无效。正确的格式是 yyyymmdd hh:mm:ss xxx，其中 yyyymmdd 和 xxx 是可选的。例如：20031126 15:59:00 EST 注意日期和时间之间，以及时间和时区之间有一个空格。”
338	Good After Time orders are currently disabled on this exchange.
该交易所目前禁用 Good After Time 订单。
339	Futures spread are no longer supported. Please use combos instead.
不再支持期货价差，请使用组合交易代替。
340	Invalid improvement amount for box auction strategy.
箱型拍卖策略的改进金额无效。
341	“Invalid delta. Valid values are from 1 to 100. You can set the delta from the “Pegged to Stock” section of the Order Ticket Panel, or by selecting Page/Layout from the main menu and adding the Delta column.”
“无效的 delta。有效值范围从 1 到 100。您可以在订单票面板的“盯盘股票”部分设置 delta，或通过从主菜单选择“页面/布局”并添加 Delta 列来设置。”
342	Pegged order is not supported on this exchange.
在此交易所不支持盯市订单。	You can review all order types and supported exchanges on the Order Types and Algos page.
您可以在“订单类型和算法”页面查看所有订单类型和受支持的交易所。
343	““The date, time, or time-zone entered is invalid. The correct format is yyyymmdd hh:mm:ss xxx””
“输入的日期、时间或时区无效。正确格式为 yyyymmdd hh:mm:ss xxx”
344	The account logged into is not a financial advisor account.
登录的账户不是金融顾问账户。	You are trying to perform an action that is only available for the financial advisor account.
您正在尝试执行仅金融顾问账户可用的操作。
345	Generic combo is not supported for FA advisor account.
通用组合不支持 FA 顾问账户。
346	Not an institutional account or an away clearing account.
不是机构账户或异地清算账户。
347	Short sale slot value must be 1 (broker holds shares) or 2 (delivered from elsewhere).
卖空仓位值必须是 1（经纪人持有股票）或 2（从其他地方提供）。	Make sure that your slot value is either 1 or 2.
确保您的仓位值是 1 或 2。
348	Order not a short sale – type must be SSHORT to specify short sale slot.
订单不是卖空——类型必须是 SSHORT 来指定卖空槽位。	Make sure that the action you specified is ‘SSHORT’.
确保你指定的操作是‘SSHORT’。
349	“Generic combo does not support “”Good After”” attribute.”
“通用组合不支持”“好之后”属性。”
350	Minimum quantity is not supported for best combo order.
最佳组合订单不支持最小数量。
351	“The “”Regular Trading Hours only”” flag is not valid for this order.”
“此订单不适用“仅限常规交易时段”标志。”
352	Short sale slot value of 2 (delivered from elsewhere) requires location.
短售槽位值 2（从其他地方交付）需要位置。	You need to specify designatedLocation for your order.
您需要为您的订单指定 designatedLocation。
353	Short sale slot value of 1 requires no location be specified.
短售槽位值 1 无需指定地点。	You do not need to specify designatedLocation for your order.
您无需指定 designatedLocation 为您的订单。
354	Requested market data is not subscribed. Check API status by selecting the Account menu then under Management choose Market Data Subscription Manager and/or availability of delayed data.
请求的市场数据未订阅。通过选择账户菜单，然后在管理下选择市场数据订阅管理器以及/或延迟数据可用性来检查 API 状态。	You do not have live market data available in your account for the specified instruments. For further details please refer to our Market Data Subscriptions page.
您在账户中没有指定工具的实时市场数据。详情请参阅我们的市场数据订阅页面。
355	Order size does not conform to market rule.
订单大小不符合市场规则。	Check order size parameters for the specified contract from the TWS Contract Details.
从 TWS 合约详情中检查指定合约的订单大小参数。
356	Smart-combo order does not support OCA group.
智能组合订单不支持 OCA 组。	Remove OCA group from your order.
移除您的订单中的 OCA 组。
357	Your client version is out of date.
您的客户端版本已过时。
358	Smart combo child order not supported.
智能组合子订单不受支持。
359	Combo order only supports reduce on fill without block(OCA).
组合订单仅支持无阻塞的成交减少（OCA）。
360	No whatif check support for smart combo order.
智能组合订单不支持 what-if 检查。	Pre-trade commissions and margin information is not available for this type of order.
此类订单无法提供交易前佣金和保证金信息。
361	Invalid trigger price.  触发价格无效。
362	Invalid adjusted stop price.
无效的调整后止损价。
363	Invalid adjusted stop limit price.
无效的调整后止损限价。
364	Invalid adjusted trailing amount.
无效的调整后跟踪金额。
365	No scanner subscription found for ticker id:
未找到对应 ticker id 的扫描器订阅。	Scanner market data subscription request with this ticker id has either been cancelled or is not found.
使用此股票代码 ID 的扫描器市场数据订阅已被取消或不存在。
366	No historical data query found for ticker id:
未找到对应股票代码 ID 的历史数据查询：	Historical market data request with this ticker id has either been cancelled or is not found.
使用此股票代码 ID 的历史市场数据请求已被取消或不存在。
367	Volatility type if set must be 1 or 2 for VOL orders. Do not set it for other order types.
对于 VOL 订单，波动率类型若设置必须是 1 或 2。其他订单类型无需设置。
368	Reference Price Type must be 1 or 2 for dynamic volatility management. Do not set it for non-VOL orders.
动态波动管理中，Reference Price Type 必须为 1 或 2。非 VOL 订单无需设置。
369	Volatility orders are only valid for US options.
波动率订单仅适用于美国期权。	Make sure that you are placing an order for US OPT contract.
请确保您正在为美国 OPT 合约下单。
370	““Dynamic Volatility orders must be SMART routed, or trade on a Price Improvement Exchange.””
“动态波动率订单必须通过 SMART 路由，或在价格改进交易所交易。”
371	VOL order requires positive floating point value for volatility. Do not set it for other order types.
VOL 订单需要正浮点数值表示波动率。其他订单类型不要设置它。
372	Cannot set dynamic VOL attribute on non-VOL order.
非 VOL 订单不能设置动态 VOL 属性。	Make sure that your order type is ‘VOL’.
确保您的订单类型是‘VOL’。
373	Can only set stock range attribute on VOL or RELATIVE TO STOCK order.
只能在 VOL 或 RELATIVE TO STOCK 订单上设置股票范围属性。
374	““If both are set, the lower stock range attribute must be less than the upper stock range attribute.””
“如果两者都设置，则较低的股票范围属性必须小于较高的股票范围属性。”
375	Stock range attributes cannot be negative.
股票范围属性不能为负。
376	The order is not eligible for continuous update. The option must trade on a cheap-to-reroute exchange.
该订单不符合连续更新资格。期权必须在易于重新路由的交易所交易。
377	Must specify valid delta hedge order aux. price.
必须指定有效的 Delta 对冲订单辅助价格。
378	Delta hedge order type requires delta hedge aux. price to be specified.
Delta 对冲订单类型需要指定 Delta 对冲辅助价格。	Make sure your order has delta attribute.
确保您的订单具有 Delta 属性。
379	Delta hedge order type requires that no delta hedge aux. price be specified.
Delta 对冲订单类型要求不指定 Delta 对冲辅助价格。	Make sure you do not specify aux. delta hedge price.
确保不要指定辅助 Delta 对冲价格。
380	This order type is not allowed for delta hedge orders.
此订单类型不允许用于 Delta 对冲订单。	““Limit, Market or Relative orders are supported.””
““支持限价、市价或相对订单。””
381	Your DDE.dll needs to be upgraded.
您的 DDE.dll 需要升级。
382	The price specified violates the number of ticks constraint specified in the default order settings.
指定的价格违反了默认订单设置中规定的价格变动单位约束。
383	The size specified violates the size constraint specified in the default order settings.
指定的大小违反了默认订单设置中规定的大小约束。
384	Invalid DDE array request.
无效的 DDE 数组请求。
385	Duplicate ticker ID for API scanner subscription.
API 扫描器订阅中存在重复的 ticker ID。	Make sure you are using a unique ticker ID for your new scanner subscription.
确保您为新的扫描器订阅使用唯一的 ticker ID。
386	Duplicate ticker ID for API historical data query.
API 历史数据查询中存在重复的 ticker ID。	Make sure you are using a unique ticker ID for your new historical market data query.
确保您为新的历史市场数据查询使用唯一的 ticker ID。
387	Unsupported order type for this exchange and security type.
不支持的订单类型，适用于此交易所和证券类型。	You can review all order types and supported exchanges on the Order Types and Algos page.
您可以在“订单类型和算法”页面查看所有订单类型和支持的交易所。
388	Order size is smaller than the minimum requirement.
订单大小小于最小要求。	Check order size parameters for the specified contract from the TWS Contract Details.
从 TWS 合约详情中检查指定合约的订单大小参数。
389	Supplied routed order ID is not unique.
提供的路由订单 ID 不唯一。
390	Supplied routed order ID is invalid.
提供的路由订单 ID 无效。
391	The time or time-zone entered is invalid. The correct format is hh:mm:ss xxx
输入的时间或时区无效。正确格式为 hh:mm:ss xxx
392	Invalid order: contract expired.
无效订单：合约已到期。	You can not place an order for the expired contract.
不能为过期合约下单。
393	Short sale slot may be specified for delta hedge orders only.
仅限 Delta 对冲订单可指定短期卖空时段。
394	Invalid Process Time: must be integer number of milliseconds between 100 and 2000. Found:
无效的处理时间：必须是 100 到 2000 毫秒之间的整数。发现：
395	““Due to system problems, orders with OCA groups are currently not being accepted.””
““由于系统问题，目前不接受具有 OCA 组的订单。””	Check TWS bulletins for more information.
请查阅 TWS 公告以获取更多信息。
396	““Due to system problems, application is currently accepting only Market and Limit orders for this contract.””
“由于系统问题，应用程序目前仅接受此合约的市场订单和限价订单。”	Check TWS bulletins for more information.
请查阅 TWS 公告以获取更多信息。
397	““Due to system problems, application is currently accepting only Market and Limit orders for this contract.””
“由于系统问题，应用程序目前仅接受此合约的市场订单和限价订单。”
398	cannot be used as a condition trigger.
不能用作条件触发条件。	Please make sure that you specify a valid condition
请确保您指定一个有效的条件
399	Order message error  订单消息错误
400	Algo order error.  算法订单错误。
401	Length restriction.  长度限制。
402	Conditions are not allowed for this contract.
此合约不允许条件。	Condition order type does not support for this contract
条件订单类型不适用于此合约
403	Invalid stop price.  无效的停价。	The Stop Price you specified for the order is invalid for the contract
您为该订单指定的停价无效
404	Shares for this order are not immediately available for short sale. The order will be held while we attempt to locate the shares.
该订单的股票暂不可用于卖空。我们将尝试寻找股票，期间订单将被暂挂。	You order is held by the TWS because you are trying to sell a contract but you do not have any long position and the market does not have short sale available. You order will be transmitted once there is short sale available on the market
您的订单被 TWS 暂挂，因为您试图卖出一个合约，但您没有任何多头仓位，且市场不允许卖空。一旦市场允许卖空，您的订单将被传送。
405	The child order quantity should be equivalent to the parent order size.
子订单数量应与主订单大小相等。	This error is deprecated.
这个错误已经过时。
406	The currency is not allowed.
货币不被允许。	Please specify a valid currency
请指定有效的货币
407	The symbol should contain valid non-unicode characters only.
符号应仅包含有效的非 Unicode 字符。	Please check your contract Symbol
请检查您的合约符号
408	Invalid scale order increment.
无效的规模订单增量。
409	Invalid scale order. You must specify order component size.
无效的规模订单。你必须指定订单组件大小。	ScaleInitLevelSize specified is invalid
指定的 ScaleInitLevelSize 无效
410	Invalid subsequent component size for scale order.
规模订单后续组件大小无效。	ScaleSubsLevelSize specified is invalid
指定的 ScaleSubsLevelSize 无效
411	“The “”Outside Regular Trading Hours”” flag is not valid for this order.”
““非交易日””标志对此订单无效。	Trading outside of regular trading hours is not available for this security
此证券在非交易日无法交易
412	The contract is not available for trading.
该合约不可交易。
413	What-if order should have the transmit flag set to true.
假设订单应将传输标志设置为 true。	You need to set IBApi.Order.Transmit to TRUE
您需要将 IBApi.Order.Transmit 设置为 TRUE
414	Snapshot market data subscription is not applicable to generic ticks.
快照市场数据订阅不适用于通用报文。	You must leave Generic Tick List to be empty when requesting snapshot market data
在请求快照市场数据时，您必须将通用报文列表留空
415	Wait until previous RFQ finishes and try again.
等待上一个 RFQ 完成后再尝试。
416	RFQ is not applicable for the contract. Order ID:
此合约不适用 RFQ。订单 ID：
417	Invalid initial component size for scale order.
比例订单的初始组件大小无效。	ScaleInitLevelSize specified is invalid
指定的 ScaleInitLevelSize 无效
418	Invalid scale order profit offset.
无效的尺度订单利润偏移。	ScaleProfitOffset specified is invalid
指定的 ScaleProfitOffset 无效
419	Missing initial component size for scale order.
尺度订单缺少初始组件大小。	You need to specify the ScaleInitLevelSize
您需要指定 ScaleInitLevelSize
420	Invalid real-time query.  实时查询无效。	Information about pacing violations
有关超时违规的信息
421	Invalid route.  无效的路径。	This error is deprecated.
这个错误已经过时。
422	The account and clearing attributes on this order may not be changed.
此订单上的账户和清算属性可能无法更改。
423	Cross order RFQ has been expired. THI committed size is no longer available. Please open order dialog and verify liquidity allocation.
交叉订单 RFQ 已过期。THI 承诺的规模不再可用。请打开订单对话框并验证流动性分配。
424	FA Order requires allocation to be specified.
市价单需要指定分配。	This error is deprecated.
这个错误已经过时。
425	FA Order requires per-account manual allocations because there is no common clearing instruction. Please use order dialog Adviser tab to enter the allocation.
FA 订单需要按账户手动分配，因为没有通用的清算指令。请使用订单对话框的顾问选项卡输入分配。	This error is deprecated.
这个错误已经过时。
426	None of the accounts have enough shares.
没有哪个账户有足够的股票。	You are not able to enter short position with Cash Account
您无法使用现金账户建立空头头寸
427	Mutual Fund order requires monetary value to be specified.
共同基金订单需要指定货币价值。	This error is deprecated.
这个错误已经过时。
428	Mutual Fund Sell order requires shares to be specified.
共同基金卖出订单需要指定股票数量。	This error is deprecated.
这个错误已经过时。
429	Delta neutral orders are only supported for combos (BAG security type).
Delta 中性订单仅支持组合（BAG 证券类型）。
430	““We are sorry, but fundamentals data for the security specified is not available.””
““抱歉，但指定证券的基本面数据不可用。””
431	What to show field is missing or incorrect.
缺少或错误显示“要显示”字段。	This error is deprecated.
这个错误已经过时。
432	Commission must not be negative.
佣金不能为负。	This error is deprecated.
这个错误已经过时。
433	“Invalid “”Restore size after taking profit”” for multiple account allocation scale order.”
“多个账户分配比例订单的“获利后恢复大小”无效。”
434	The order size cannot be zero.
订单大小不能为零。
435	You must specify an account.
你必须指定一个账户。	The function you invoked only works on a single account
您调用的功能仅在单个账户上有效
436	““You must specify an allocation (either a single account, group, or profile).””
“你必须指定一个分配（可以是单个账户、组或配置文件）。”	““When you try to place an order with a Financial Advisor account, you must specify the order to be routed to either a single account, a group, or a profile.””
“当你尝试使用财务顾问账户下单时，你必须指定订单路由到单个账户、组或配置文件。”
437	Order can have only one flag Outside RTH or Allow PreOpen.
订单只能有一个标志：Outside RTH 或 Allow PreOpen。	This error is deprecated.
这个错误已经过时。
438	The application is now locked.
应用程序现在已被锁定。	This error is deprecated.
这个错误已经过时。
439	Order processing failed. Algorithm definition not found.
订单处理失败。未找到算法定义。	Please double check your specification for IBApi.Order.AlgoStrategy and IBApi.Order.AlgoParams
请仔细检查您的 IBApi.Order.AlgoStrategy 和 IBApi.Order.AlgoParams 规范
440	Order modify failed. Algorithm cannot be modified.
订单修改失败。算法无法修改。
441	Algo attributes validation failed:
算法属性验证失败：	Please double check your specification for IBApi.Order.AlgoStrategy and IBApi.Order.AlgoParams
请仔细检查 IBApi.Order.AlgoStrategy 和 IBApi.Order.AlgoParams 的规范
442	Specified algorithm is not allowed for this order.
指定的算法不允许用于此订单。
443	Order processing failed. Unknown algo attribute.
订单处理失败。未知算法属性。	Specification for IBApi.Order.AlgoParams is incorrect
IBApi.Order.AlgoParams 的规范不正确
444	Volatility Combo order is not yet acknowledged. Cannot submit changes at this time.
波动率组合订单尚未确认。当前无法提交更改。	The order is not in a state that is able to be modified
该订单不处于可修改的状态
445	The RFQ for this order is no longer valid.
此订单的 RFQ 已失效。
446	Missing scale order profit offset.
缺少尺度订单利润偏移。	ScaleProfitOffset is not properly specified
ScaleProfitOffset 未正确指定
447	Missing scale price adjustment amount or interval.
缺少尺度价格调整金额或间隔。	ScalePriceAdjustValue or ScalePriceAdjustInterval is not specified properly
ScalePriceAdjustValue 或 ScalePriceAdjustInterval 未正确指定
448	Invalid scale price adjustment interval.
无效的价格调整间隔。	ScalePriceAdjustInterval specified is invalid
指定的 ScalePriceAdjustInterval 无效
449	Unexpected scale price adjustment amount or interval.
意外的价格调整金额或间隔。	ScalePriceAdjustValue or ScalePriceAdjustInterval specified is invalid
指定的 ScalePriceAdjustValue 或 ScalePriceAdjustInterval 无效
481	Order size reduced.  订单大小已减小。
501	Already Connected.  已连接。	Your client application is already connected to the TWS.
您的客户端应用程序已连接到 TWS。
502	“Couldn’t connect to TWS. Confirm that “”Enable ActiveX and Socket Clients”” is enabled and connection port is the same as “”Socket Port”” on the TWS “”Edit->Global Configuration…->API->Settings”” menu.”
“无法连接到 TWS。请确认“启用 ActiveX 和 Socket 客户端”已启用，并且连接端口与 TWS“编辑->全局配置…->API->设置”菜单中的“Socket 端口”相同。”	When you receive this error message it is either because you have not enabled API connectivity in the TWS and/or you are trying to connect on the wrong port. Refer to the TWS’ API Settings as explained in the error message. See also Connectivity
当你收到此错误消息时，要么是因为你未在 TWS 中启用 API 连接性，要么是因为你正在尝试连接到错误的端口。请参考错误消息中解释的 TWS 的 API 设置。另见连接性
503	The TWS is out of date and must be upgraded.
TWS 已过时，必须升级。	Indicates TWS or IBG is too old for use with the current API version. Can also be triggered if the TWS version does not support a specific API function.
表示 TWS 或 IBG 版本过旧，无法与当前 API 版本一起使用。如果 TWS 版本不支持特定 API 功能，也可能触发此错误。
504	Not connected.  未连接。	You are trying to perform a request without properly connecting and/or after connection to the TWS has been broken probably due to an unhandled exception within your client application.
您尝试在不正确连接和/或连接到 TWS 后断开的情况下执行请求，这可能是由于客户端应用程序中的未处理异常导致的。
505	Fatal Error: Unknown message id.
致命错误：未知消息 ID。
506	Unsupported Version (not used in Python client)
不支持的版本（Python 客户端不使用）
507	Bad Message Length (Java-only)
消息长度错误（仅限 Java）	““Indicates EOF exception was caught while reading from the socket. This can occur if there is an attempt to connect to TWS with a client ID that is already in use, or if TWS is locked, closes, or breaks the connection. It should be handled by the client application and used to indicate that the socket connection is not valid.””
“指示在从套接字读取时捕获到 EOF 异常。这可能是尝试使用已在使用的客户端 ID 连接到 TWS，或者 TWS 被锁定、关闭或断开连接时发生的。应由客户端应用程序处理，并用于指示套接字连接无效。”
508	Bad Message  错误消息
509	Exception caught while reading socket
读取套接字时捕获到异常	(not used in Python C# client)
(在 Python C#客户端中未使用)
510	Request Market Data Sending Error –
请求市场数据发送错误 –
511	Cancel Market Data Sending Error –
取消市场数据发送错误 –
512	Order Sending Error –  订单发送错误 –
513	Account Update Request Sending Error –
账户更新请求发送错误 –
514	Request For Executions Sending Error –
发送执行请求错误 –
515	Cancel Order Sending Error –
取消订单发送错误 –
516	Request Open Order Sending Error –
请求开仓订单发送错误 –
517	Unknown contract. Verify the contract details supplied. (not used in Python C# client)
未知合约。请核实所提供的合约详情。（在 Python C#客户端中未使用）
518	Request Contract Data Sending Error –
请求合约数据发送错误 –
519	Request Market Depth Sending Error –
请求市场深度发送错误 –
520	Failed to create socket (not used in C# client)
创建套接字失败（C#客户端未使用）
521	Set Server Log Level Sending Error –
设置服务器日志级别发送错误 –
522	FA Information Request Sending Error –
FA 信息请求发送错误 –
523	FA Information Replace Sending Error –
FA 信息替换发送错误 –
524	Request Scanner Subscription Sending Error –
请求扫描器订阅发送错误 –
525	Cancel Scanner Subscription Sending Error –
取消扫描器订阅发送错误 –
526	Request Scanner Parameter Sending Error –
请求扫描器参数发送错误 –
527	Request Historical Data Sending Error –
请求历史数据发送错误 –
528	Request Historical Data Sending Error –
请求历史数据发送错误 –
529	Request Real-time Bar Data Sending Error –
请求实时 K 线数据发送错误 –
530	Cancel Real-time Bar Data Sending Error –
取消实时 K 线数据发送错误 –
531	Request Current Time Sending Error –
请求当前时间发送错误 –
532	Request Fundamental Data Sending Error –
请求基本面数据发送错误 –
533	Cancel Fundamental Data Sending Error –
取消基础数据发送错误 –
534	Request Calculate Implied Volatility Sending Error –
请求计算隐含波动率发送错误 –
535	Request Calculate Option Price Sending Error –
请求计算期权价格发送错误 –
536	Cancel Calculate Implied Volatility Sending Error –
取消计算隐含波动率发送错误 –
537	Cancel Calculate Option Price Sending Error –
取消计算期权价格发送错误 –
538	Request Global Cancel Sending Error –
请求全局取消发送错误 –
539	Request Market Data Type Sending Error –
请求市场数据类型发送错误 –
540	Request Positions Sending Error –
请求仓位发送错误 –
541	Cancel Positions Sending Error –
取消仓位发送错误 –
542	Request Account Data Sending Error –
请求账户数据发送错误 –
543	Cancel Account Data Sending Error –
取消账户数据发送错误 –
544	Verify Request Sending Error –
验证请求发送错误 –
545	Verify Message Sending Error –
验证消息发送错误 –
546	Query Display Groups Sending Error –
查询显示组发送错误 –
547	Subscribe To Group Events Sending Error –
订阅组事件发送错误 –
548	Update Display Group Sending Error –
更新显示组发送错误 –
549	Unsubscribe From Group Events Sending Error –
取消订阅组事件发送错误 –
550	Start API Sending Error –
开始 API 发送错误 –
551	Verify And Auth Request Sending Error –
验证和认证请求发送错误 –
552	Verify And Auth Message Sending Error –
验证和认证消息发送错误 –
553	Request Positions Multi Sending Error –
请求头寸多发送错误 –
554	Cancel Positions Multi Sending Error –
取消头寸批量发送错误 –
555	Request Account Updates Multi Sending Error –
请求账户更新批量发送错误 –
556	Cancel Account Updates Multi Sending Error –
取消账户更新批量发送错误 –
557	Request Security Definition Option Params Sending Error –
请求安全定义选项参数批量发送错误 –
558	Request Soft Dollar Tiers Sending Error –
请求软美元层级发送错误 –
559	Request Family Codes Sending Error –
请求家族代码发送错误 –
560	Request Matching Symbols Sending Error –
请求匹配符号发送错误 –
561	Request Market Depth Exchanges Sending Error –
请求市场深度交易所发送错误 –
562	Request Smart Components Sending Error –
请求智能组件发送错误 –
563	Request News Providers Sending Error –
请求新闻提供者发送错误 –
564	Request News Article Sending Error –
请求新闻文章发送错误 –
565	Request Historical News Sending Error –
请求历史新闻发送错误 –
566	Request Head Time Stamp Sending Error –
请求头时间戳发送错误 –
567	Request Histogram Data Sending Error –
请求直方图数据发送错误 –
568	Cancel Request Histogram Data Sending Error –
取消请求直方图数据发送错误 –
569	Cancel Head Time Stamp Sending Error –
取消发送头时间戳错误 –
570	Request Market Rule Sending Error –
请求市场规则发送错误 –
571	Request PnL Sending Error –
请求 PnL 发送错误 –
572	Cancel PnL Sending Error –
取消 PnL 发送错误 –
573	Request PnL Single Error –
请求 PnL 单个错误 –
574	Cancel PnL Single Sending Error –
取消 PnL 单个发送错误 –
575	Request Historical Ticks Error –
请求历史报错 –
576	Request Tick-By-Tick Data Sending Error –
请求逐笔数据发送报错 –
577	Cancel Tick-By-Tick Data Sending Error –
取消逐笔数据发送报错 –
578	Request Completed Orders Sending Error –
请求已完成订单发送报错 –
579	Invalid symbol in string –
字符串中无效的符号—
580	Request WSH Meta Data Sending Error –
请求 WSH 元数据发送错误—
581	Cancel WSH Meta Data Sending Error –
取消发送 WSH 元数据错误 –
582	Request WSH Event Data Sending Error –
请求发送 WSH 事件数据错误 –
583	Cancel WSH Event Data Sending Error –
取消发送 WSH 事件数据错误 –
584	Request User Info Sending Error –
请求发送用户信息错误 –
585	“FA Profile is not supported anymore, use FA Group instead”
“FA Profile 已不再支持，请使用 FA Group”	“Indicates FaDataTypeEnum.PROFILES is deprecated. Use FaDataTypeEnum.GROUPS or 1 instead”
“表示 FaDataTypeEnum.PROFILES 已弃用。请使用 FaDataTypeEnum.GROUPS 或 1”
586	Failed to read message because not connected (Used only in Java client)
因未连接而读取消息失败（仅在 Java 客户端中使用）
587	Request Current Time In Millis Sending Error –
请求当前时间（毫秒）发送错误 –
588	Error encoding protobuf  编码 protobuf 出错	(Used only in Java client)
（仅用于 Java 客户端）
589	Cancel Market Depth Sending Error –
取消市场深度发送错误 –
2100	New account data requested from TWS. API client has been unsubscribed from account data.
从 TWS 请求新账户数据。API 客户端已取消订阅账户数据。	““The TWS only allows one IBApi.EClient.reqAccountUpdates request at a time. If the client application attempts to subscribe to a second account without canceling the previous subscription, the new request will override the old one and the TWS will send this message notifying so.””
““TWS 一次只允许一个 IBApi.EClient.reqAccountUpdates 请求。如果客户端应用程序在取消之前的订阅之前尝试订阅第二个账户，新的请求将覆盖旧的请求，TWS 将发送此消息通知。””
2101	Unable to subscribe to account as the following clients are subscribed to a different account.
无法订阅账户，因为以下客户端已订阅不同的账户。	““If a client application invokes IBApi.EClient.reqAccountUpdates when there is an active subscription started by a different client, the TWS will reject the new subscription request with this message.””
““如果客户端应用程序在由不同客户端启动的活跃订阅时调用 IBApi.EClient.reqAccountUpdates，TWS 将使用此消息拒绝新的订阅请求。””
2102	Unable to modify this order as it is still being processed.
无法修改此订单，因为它仍在处理中。	““If you attempt to modify an order before it gets processed by the system, the modification will be rejected. Wait until the order has been fully processed before modifying it. See Placing Orders for further details.””
““如果您在系统处理订单之前尝试修改订单，修改将被拒绝。请等待订单完全处理后再进行修改。有关详细信息，请参阅“下单”部分。””
2103	A market data farm is disconnected.
一个市场数据农场已断开连接。	““Indicates a connectivity problem to an IB server. Outside of the nightly IB server reset, this typically indicates an underlying ISP connectivity issue.””
““表示与 IB 服务器的连接问题。除了夜间 IB 服务器重置外，这通常表示潜在的 ISP 连接问题。””
2104	Market data farm connection is OK
市场数据农场连接正常	““A notification that connection to the market data server is ok. This is a notification and not a true error condition, and is expected on first establishing connection.””
““连接到市场数据服务器成功的通知。这是一个通知，不是真正的错误状态，在首次建立连接时是预期的。””
2105	A historical data farm is disconnected.
历史数据农场断开连接。	““Indicates a connectivity problem to an IB server. Outside of the nightly IB server reset, this typically indicates an underlying ISP connectivity issue.””
““表示与 IB 服务器的连接问题。除了夜间 IB 服务器重置外，这通常表示潜在的 ISP 连接问题。””
2106	A historical data farm is connected.
历史数据农场已连接。	““A notification that connection to the market data server is ok. This is a notification and not a true error condition, and is expected on first establishing connection.””
““已成功连接到市场数据服务器。这是一个通知，并非真正的错误状态，在首次建立连接时是预期的。””
2107	A historical data farm connection has become inactive but should be available upon demand.
一个历史数据农场连接已变为非活动状态，但应根据需求可用。	““Whenever a connection to the historical data farm is not being used because there is not an active historical data request, the connection will go inactive in IB Gateway. This does not indicate any connectivity issue or problem with IB Gateway. As soon as a historical data request is made the status will change back to active.””
““每当由于没有活动的历史数据请求而未使用与历史数据农场的连接时，该连接将在 IB Gateway 中变为非活动状态。这并不表示任何连接问题或 IB Gateway 的问题。一旦做出历史数据请求，状态将恢复为活动状态。””
2108	A market data farm connection has become inactive but should be available upon demand.
一个市场数据农场连接已变得不活跃，但应根据需求可用。	““Whenever a connection to our data farms is not needed, it will become dormant. There is nothing abnormal nor wrong with your client application nor with the TWS. You can safely ignore this message.””
“当不需要连接到我们的数据农场时，它将进入休眠状态。您的客户端应用程序或 TWS 没有任何异常或错误，您可以安全地忽略此消息。”
2109	“Order Event Warning: Attribute “”Outside Regular Trading Hours”” is ignored based on the order type and destination. PlaceOrder is now processed.”
“订单事件警告：根据订单类型和目的地，属性“”非交易日外””被忽略。现在正在处理 PlaceOrder。”	Indicates the outsideRth flag was set for an order for which there is not a regular vs outside regular trading hour distinction
表示为订单设置了 outsideRth 标志，该订单没有常规交易时段与非常规交易时段的区别
2110	Connectivity between TWS and server is broken. It will be restored automatically.
TWS 与服务器之间的连接已断开。它将自动恢复。	Indicates a connectivity problem between TWS or IBG and the IB server. This will usually only occur during the IB nightly server reset; cases at other times indicate a problem in the local ISP connectivity.
指示 TWS 或 IBG 与 IB 服务器之间的连接问题。这通常只在 IB 夜间服务器重置时发生；其他时间发生的情况表明是本地 ISP 连接问题。
2111	“The Start and/or End Time for algo order BUY/SELL a contract was adjusted to use the next trading date. To modify this setting, use the Auto-adjust algo order date item on the Orders configuration page”
“算法订单 BUY/SELL 合约的开始和/或结束时间已调整为使用下一个交易日。要修改此设置，请使用订单配置页面上的自动调整算法订单日期项”	Please go to TWS Global Configuration – “Orders” – “Settings” to correct the configuration.
请前往 TWS 全局配置——“订单”——“设置”以修正配置。
2119	Market data farm is connecting.
市场数据农场正在连接。
2130	Warning: products are trading on the basis of currency price with factor.
警告：产品基于货币价格与因素进行交易。
2137	Cross Side Warning  交叉侧警告	““This warning message occurs in TWS version 955 and higher. It occurs when an order will change the position in an account from long to short or from short to long. To bypass the warning, a new feature has been added to IB Gateway 956 (or higher) and TWS 957 (or higher) so that once can go to Global Configuration > Messages and disable the “”Cross Side Warning””.””
““此警告消息在 TWS 版本 955 及以上版本中发生。当订单将账户中的头寸从多头变为空头或从空头变为多头时会发生此警告。为绕过此警告，IB Gateway 956（或更高版本）和 TWS 957（或更高版本）中添加了新功能，允许用户进入全局配置 > 消息并禁用“交叉侧警告”。””
2152	Market depth smart depth exchanges.
市场深度智能深度交易所。
2158	Sec-def data farm connection is OK
Sec-def 数据农场连接正常	““A notification that connection to the Security definition data server is ok. This is a notification and not a true error condition, and is expected on first establishing connection.””
““连接到安全定义数据服务器的通知正常。这是一个通知，不是真正的错误状态，在首次建立连接时是预期的。””
2168	Etrade Only Not Supported Warning
仅限 Etrade 警告	The EtradeOnly IBApi.Order attribute is no longer supported. Error received with TWS versions 983+. Remove attribute to place order.
EtradeOnly IBApi.Order 属性不再受支持。在使用 TWS 版本 983 及更高版本时收到错误。移除属性以提交订单。
2169	Firm Quote Only Not Supported Warning
仅支持公司报价警告	The firmQuoteOnly IBApi.Order attribute is no longer supported. Error received with TWS versions 983+. Remove attribute to place order.
firmQuoteOnly IBApi.Order 属性不再受支持。在使用 TWS 版本 983 及更高版本时收到错误。请移除该属性以提交订单。
10000	Cross currency combo error.
跨货币组合错误。
10001	Cross currency vol error.
跨货币波动率错误。
10002	Invalid non-guaranteed legs.
无效的非保证合约。
10003	IBSX not allowed.  IBSX 不允许。
10005	Read-only models.  只读模型。
10006	Missing parent order.  缺少父订单。	The parent order ID specified cannot be found. In some cases this can occur with bracket orders if the child order is placed immediately after the parent order; a brief pause of 50 ms or less will be necessary before the child order is transmitted to TWS/IBG.
指定的父订单 ID 不存在。在某些情况下，这可能会发生在括号订单中，如果子订单立即在父订单之后下单；在子订单传输到 TWS/IBG 之前，需要暂停 50 毫秒或更少的时间。
10007	Invalid hedge type.  对冲类型无效。
10008	Invalid beta value.  贝塔值无效。
10009	Invalid hedge ratio.  无效的对冲比率。
10010	Invalid delta hedge order.
无效的 Delta 对冲订单。
10011	Currency is not supported for Smart combo.
智能组合不支持该货币。
10012	Invalid allocation percentage
无效的分配百分比	FaPercentage specified is not valid
指定的百分比无效
10013	Smart routing API error (Smart routing opt-out required).
智能路由 API 错误（需要关闭智能路由）。
10014	PctChange limits.  百分比变化限制。	This error is deprecated  此错误已弃用
10015	Trading is not allowed in the API.
API 中不允许交易。
10016	Contract is not visible.  合约不可见。	This error is deprecated  此错误已弃用
10017	Contracts are not visible.
合约不可见。	This error is deprecated  这个错误已弃用
10018	Orders use EV warning.  订单使用 EV 警告。
10019	Trades use EV warning.  交易使用 EV 警告。
10020	Display size should be smaller than order size./td>
显示尺寸应小于订单尺寸./td>	The display size should be smaller than the total quantity
显示尺寸应小于总数量
10021	Invalid leg2 to Mkt Offset API.
无效的 leg2 到 Mkt Offset API。	This error is deprecated  这个错误已弃用
10022	Invalid Leg Prio API.  无效的 Leg Prio API。	This error is deprecated  这个错误已弃用
10023	Invalid combo display size API.
无效的组合显示大小 API。	This error is deprecated  这个错误已弃用
10024	Invalid don’t start next legin API.
无效的 don’t start next legin API。	This error is deprecated  这个错误已弃用
10025	Invalid leg2 to Mkt time1 API.
无效的 leg2 to Mkt time1 API。	This error is deprecated  此错误已弃用
10026	Invalid leg2 to Mkt time2 API.
无效的 leg2 到 Mkt time2 API。	This error is deprecated  此错误已弃用
10027	Invalid combo routing tag API.
无效的组合路由标签 API。	This error is deprecated  此错误已弃用
10090	Part of requested market data is not subscribed.
请求的市场数据部分未订阅。	““Indicates that some tick types requested require additional market data subscriptions not held in the account. This commonly occurs for instance if a user has options subscriptions but not the underlying stock so the system cannot calculate the real time greek values (other default ticks will be returned). Or alternatively, if generic tick types are specified in a market data request without the associated subscriptions.””
““表示请求的部分行情类型需要额外的市场数据订阅，而账户中未持有这些订阅。这种情况通常发生在例如用户订阅了期权但未订阅标的股票，因此系统无法计算实时希腊值（其他默认行情将被返回）。或者，如果市场数据请求中指定了通用行情类型而没有相应的订阅。””
10147	Order to be canceled was not found.
要取消的订单未找到。
10148	““OrderId that needs to be cancelled can not be cancelled, state:””
“需要取消的订单不能取消，状态：””	An attempt was made to cancel an order that had already been filled by the system.
尝试取消系统已成交的订单。
10186	Requested market data is not subscribed. Delayed market data is not enabled
请求的市场数据未订阅。延迟市场数据未启用	See Market Data Types on how to enable delayed data.
请参阅“市场数据类型”了解如何启用延迟数据。
10187	Failed to request historical ticks:No market data permissions
请求历史 K 线失败：无市场数据权限
10189	Failed to request tick-by-tick data. Invalid Real-time Query
请求逐笔数据失败。实时查询无效	“Trading TWS session is connected from a different IP address. Or, No market data permissions”
“交易 TWS 会话从不同 IP 地址连接。或，无市场数据权限”
10197	No market data during competing session
竞赛会话期间无市场数据	““Indicates that the user is logged into the paper account and live account simultaneously trying to request live market data using both the accounts. In such a scenario preference would be given to the live account, for more details please refer: https://ibkr.info/node/1719””
““表示用户同时登录了模拟账户和实盘账户，并尝试使用这两个账户请求实盘市场数据。在这种情况下，优先级将给予实盘账户，更多详情请参考：https://ibkr.info/node/1719””
10225	““Bust event occurred, current subscription is deactivated. Please resubscribe real-time bars immediately””
““发生断开事件，当前订阅已停用。请立即重新订阅实时 K 线””
10230	““You have unsaved FA changes. Please retry ‘request FA’ operation later, when ‘replace FA’ operation is complete””
““您有未保存的财务顾问（FA）更改。请在‘替换 FA’操作完成后，稍后再尝试‘请求 FA’操作””	There are pending Financial Advisor configuration changes. See Financial Advisors
有待处理的财务顾问配置更改。查看 财务顾问
10231	The following Groups and/or Profiles contain invalid accounts:
以下组别和/或配置文件包含无效账户：	““If the account(s) inside Groups or Profiles is/are incorrect in xml-formatted configuration string of replaceFA request, then the error shows list of such Groups and/or Profiles.””
““如果组别或配置文件中的账户在 replaceFA 请求的 xml 格式配置字符串中不正确，则会显示此类组别和/或配置文件的列表。””
10233	Defaults were inherited from CASH preset during the creation of this order.
此订单创建时继承了 CASH 预设的默认值。
10234	The Decision Maker field is required and not set for this order (non-desktop).
决策者字段对此订单（非桌面端）是必填项且未设置。
10235	The Decision Maker field is required and not set for this order (ibbot).
决策者字段对此订单（ibbot）是必填项且未设置
10236	Child has to be AON if parent order is AON
如果父订单是 AON，子订单必须是 AON
10237	All or None ticket can route entire unfilled size only
全或无订单只能路由全部未成交部分
10238	Some error occured during communication with Advisor Setup web-app
与顾问设置 Web 应用通信过程中发生了一些错误
10239	This order will affect one or more accounts that are flagged because they do not fit the required risk score criteria prescribed by the group/profile/model allocation.
此订单将影响一个或多个因不符合集团/配置文件/模型分配所规定的风险评分标准而被标记的账户。
10240	You must enter a valid Price Cap.
您必须输入一个有效的价格上限。
10241	Order Quantity is expressed in monetary terms. Modification is not supported via API. Please use desktop version to revise this order.
订单数量以货币单位表示。通过 API 不支持修改。请使用桌面版修改此订单。
10242	Fractional-sized order cannot be modified via API. Please use desktop version to revise this order.
分数大小的订单无法通过 API 修改。请使用桌面版修改此订单。
10243	Fractional-sized order cannot be placed via API. Please use desktop version to place this order.
无法通过 API 下单分数大小的订单。请使用桌面版下单此订单。
10244	Cash Quantity cannot be used for this order
现金数量不能用于此订单
10245	This financial instrument does not support fractional shares trading
此金融工具不支持碎股交易
10246	This order doesn’t support fractional shares trading
此订单不支持碎股交易
10247	Only IB SmartRouting supports fractional shares
仅 IB SmartRouting 支持碎股交易
10248	doesn’t have permission to trade fractional shares
没有权限交易零碎股
10249	“=””””> order doesn’t support fractional shares”
“=””””> 订单不支持零碎股”
10250	The size does not conform to the minimum variation of for this contract
大小不符合该合约的最小变动量
10251	Fractional shares are not supported for allocation orders
分配订单不支持碎股
10252	This non-close-position order doesn’t support fractional shares trading
这种非平仓订单不支持碎股交易
10253	Clear Away orders are not supported for multi-leg combo with attached hedge.
带有对冲的复合多腿订单不支持清除订单
10254	Invalid Order: bond expired
无效订单：债券已到期
10268	The ‘EtradeOnly’ order attribute is not supported
‘EtradeOnly’订单属性不受支持	The EtradeOnly IBApi.Order attribute is no longer supported. Error received with TWS versions 983+
EtradeOnly IBApi.Order 属性不再受支持。在使用 TWS 版本 983 及更高版本时收到错误
10269	The ‘firmQuoteOnly’ order attribute is not supported
‘firmQuoteOnly’订单属性不受支持	The firmQuoteOnly IBApi.Order attribute is no longer supported. Error received with TWS versions 983+
firmQuoteOnly IBApi.Order 属性不再受支持。在使用 TWS 版本 983 及更高版本时收到错误
10270	The ‘nbboPriceCap’ order attribute is not supported
‘nbboPriceCap’订单属性不受支持	The nbboPriceCap IBApi.Order attribute is no longer supported. Error received with TWS versions 983+
TWS 版本 983 及更高版本不再支持 nbboPriceCap IBApi.Order 属性。收到错误信息
10276	News feed is not allowed
不允许使用新闻源	The API client is not permissioned for receiving WSH news feed.
API 客户端没有接收 WSH 新闻源的权限。
10277	News feed permissions required
需要新闻源权限	The API client is not subscribed to receive WSH news feed
API 客户端未订阅接收 WSH 新闻源
10278	Duplicate WSH metadata request
重复的 WSH 元数据请求	A request is already pending for the same API client.
对于同一个 API 客户端，已经有一个请求正在等待。
10279	Failed request WSH metadata
请求失败 WSH 元数据	A general error occurred when processing the request.
处理请求时发生了通用错误。
10280	Failed cancel WSH metadata
取消请求失败 WSH 元数据	A general error occurred when processing the request.
处理请求时发生一般错误。
10281	Duplicate WSH event data request
重复 WSH 事件数据请求	A request is already pending for the same API client.
一个针对同一 API 客户端的请求已经挂起。
10282	WSH metadata not requested
未请求 WSH 元数据	WSH metadata was not requested by first sending a reqWshMetaData request.
未通过先发送 reqWshMetaData 请求来请求 WSH 元数据。
10283	Fail request WSH event data
请求 WSH 事件数据失败	A general error occurred when processing the request.
处理请求时发生了通用错误。
10284	Fail cancel WSH event data
取消 WSH 事件数据失败	A general error occurred when processing the request.
处理请求时发生了通用错误。
10285	Your API version does not support fractional sizing rules. Please upgrade to at least version 163
您的 API 版本不支持分数尺寸规则。请升级到至少版本 163
10286	%s field cannot contain more than %s decimals.
%s 字段不能包含超过 %s 位小数。
10287	Cryptocurrency order is not confirmed
加密货币订单未确认
10288	Market order confirmation dialog title for cryptocurrencies
加密货币市价单确认对话框标题
10289	You must set Cash Quantity for this order
您必须为该订单设置现金数量
10290	This order only supports CashQty trading.
此订单仅支持现金数量交易。
10291	Orders to harvest Capital Loss must use the DAY time-in-force.
平仓亏损订单必须使用日有效期限。
10292	Order type/action restriction
订单类型/操作限制
10293	Cryptocurrency Cash Quantity order cannot specify size
加密货币现货数量订单不能指定大小
10294	Cash quantity set on the order does not match total monetary amount of the Group.
订单上设置的现金数量与集团总金额不匹配。
10295	Orders to harvest Capital Loss must use the DAY time-in-force.
平仓亏损订单必须使用 DAY 时间有效期。
10295	Only daily resolution supported for Schedule requests
计划请求仅支持每日分辨率。
10296	“The Smart Routing features \””Seek Price Improvement\”” (aka \””Route to Dark Pools\””) and \””Do not route to Dark Pools\”” are mutually exclusive.
“智能路由功能”中的“寻求价格改善”（又名“路由至暗池”）和“不路由至暗池”是互斥的。
Enabling both will result in the order being rejected. Please choose only one of these commands.%s”
同时启用两者会导致订单被拒绝。请仅选择其中一个命令。%
10297	Not Held attribute is invalid for this order.
不持有属性对此订单无效。
10298	Cannot trade an instrument with currency different from model currency
不能交易与模型货币不同的货币的合约。
10299	Expected what to show is %s
预期显示的是 %s	please use that instead of %s.
请使用 %s 代替。
10300	%s: The date  %s: 日期	time  时间
10301	%s: The date  %s: 日期	time  时间
10302	Min trade trade quantity is not allowed for this order
此订单不允许最小交易数量
10303	Invalid min trade quantity value (%s).
无效的最小交易数量值 (%s)。
It must be a positive integer
必须是正整数	not exceeding the total order size.
不超过总订单量。
10304	Minimum Competing Size value must be non-negative.
最小竞争尺寸值必须为非负数。
10305	Compete against best bid or offer Offset dollar value must be positive
与最优买价或卖价竞争的偏移美元值必须为正数	multiple of a cent.  必须是美分倍数。
10306	Mid offsets are not allowed
中间偏移量是不允许的
10307	Invalid MidOffsetAtWhole and/or MidOffsetAtHalf attribute values
无效的 MidOffsetAtWhole 和/或 MidOffsetAtHalf 属性值
10308	Revision to Post to ATS value presence is not allowed.
不允许修改 Post to ATS 值的存在。
10309	Invalid WSH event data request.
无效的 WSH 事件数据请求。
10310	The Solicited field should be used for orders initiated or recommended by the broker or advisor that were approved by the client (by phone
Solicited 字段应用于由经纪商或顾问发起或推荐的订单，且已获得客户（通过电话）的批准。	email
10311	This order will be directly routed to %s. Direct routed orders may result in higher trade fees.
此订单将直接路由至 %s。直接路由的订单可能导致交易费用更高。
10312	The order type Volatility is currently not supported for this combination of financial instrument and account type
对于这种金融工具和账户类型的组合，目前不支持波动率订单类型。
10314	%s: The date  %s: 日期	time  时间
10315	%s: The time entered is invalid. The correct format is hh:mm:ss. E.g.: 15:00:00 in UTC. No date should be specified
%s: 输入的时间无效。正确的格式是 hh:mm:ss。例如：UTC 时间 15:00:00。不应指定日期	current date is assumed.  默认为当前日期。
10316	Trigger Outside RTH was deprecated. Please upgrade your API Client software to submit order with Outside RTH attribute instead.
"在交易时段外触发"已被弃用。请升级您的 API 客户端软件，使用"在交易时段外"属性提交订单。
10317	The Cash Quantity size for the below contracts does not conform to minimum variation of %s
下述合约的现金数量大小不符合%s 的最小变动量
10318	This order doesn’t support fractional quantity trading
此订单不支持分数数量交易
10319	Placing orders for Municipal Bonds via API is currently disabled
目前通过 API 下单购买市政债券已被禁用
10321	Placing orders for Municipal Bonds is currently disabled for attached and OCA orders.
目前为附加订单和 OCA 订单下单购买市政债券已被禁用
10322	This API request for All is not supported for Dynamic Account Addition
这个 API 请求用于全部操作不支持动态账户添加
10324	Invalid parameters for OCA group for exchange %s. Overfill Protection is implied.
OCA 组参数无效，对于交易所%s，隐含了超额保护
10325	OCA group is not supported
OCA 组不受支持
10326	OCA group revision is not allowed
不允许修改 OCA 组
10327	OCA group type revision is not allowed
OCA 分组类型修订不允许
10328	Connection lost  连接丢失	order data could not be resolved
订单数据无法解析
10329	This order will be directly routed to %s.
此订单将直接路由到 %s。
10330	The expiry date/time format is invalid.\nThe correct format is yyyyMM
过期日期/时间格式无效。\n 正确格式为 yyyyMM	yyyyMMdd HH:mm:ss (operator or instrument time zone) or yyyyMMdd-HH:mm:ss (UTC time zone).
yyyyMMdd HH:mm:ss（操作员或合约时间区）或 yyyyMMdd-HH:mm:ss（协调世界时）。
10331	Any stop warning  任何停损警告
10332	Cryptocurrency volatility warning
加密货币波动警告
10333	Option Exercise at-the-money warning
期权行权价平值警告
10334	Confirm Omnibus Order Account
确认总订单账户
10335	“Order presets cannot be applied as configured. Please review
“订单预设无法按配置应用。请检查
%s Settings and Rapid Order Entry Configuration for consistency.”
%s 设置和快速订单输入配置的一致性。”
10336	Per-leg executing broker configuration is not supported
单腿执行经纪商配置不受支持
10337	Misc options key=%s is invalid in %s request. Valid keys are: %s
杂项选项键=%s 在 %s 请求中无效。有效的键是：%s
10338	Misc options value=%s is invalid for key=%s in %s request. Valid values are: %s
杂项选项值=%s 在键=%s 的 %s 请求中无效。有效的值是：%s
10339	Setting end date/time for continuous future security type is not allowed
不允许为连续期货安全类型设置结束日期/时间
10340	The following order attribute is not supported: %s
不支持的订单属性：%s
10341	Parent order id cannot be modified
父订单 ID 不能修改
10342	The ‘ImbalanceOnly’ order attribute may not be specified for this order.
此订单不能指定“仅不平衡”订单属性。
10343	Selling Event Contracts is neither allowed directly
直接销售事件合约既不被允许	nor as an attached profit taker.
也不可以作为附加盈利取引。
10344	Price value must be between 0.02 and 0.99 with a maximum of two decimal places.
价格值必须在 0.02 和 0.99 之间，最多有两位小数。
10345	You cannot trade a %s
您不能交易%s
10346	Market data for %s cannot be delivered because ticker for the same financial instrument is displayed on %s
由于同一金融工具的股票代码在%s 上显示，因此无法提供%s 的市场数据
10347	This security has limited liquidity. If you choose to trade this security
该证券流动性有限。如果您选择交易该证券	there is a heightened risk that you may not be able to close your position
存在无法平仓头寸的高度风险
at the time you wish
在你希望的时候
WinError 10038	An operation was attempted on something that is not a socket.
尝试对非套接字对象执行操作。	This indicates socket connection was closed improperly.
这表示套接字连接未正确关闭。
Receiving Error Messages  接收错误消息

EWrapper.error(

reqId: int. The request identifier corresponding to the most recent reqId that maintained the error stream.
reqId: int. 对应于最近维护错误流的最新 reqId 的请求标识符。
This does not pertain to the orderId from placeOrder, but whatever the most recent requestId is.
这并不涉及 placeOrder 的 orderId，而是无论最新的 requestId 是什么。

errorTime: int. The Unix timestamp of when the error took place.
errorTime: int. 错误发生时的 Unix 时间戳。
Note: This is only implemented for TWS API 10.33+
注意：这仅适用于 TWS API 10.33 及以上版本。

errorCode: int. The code identifying the error.
errorCode: int. 识别错误的代码。

errorMsg: String. The error’s description.
errorMsg: String. 错误的描述。

advancedOrderRejectJson: String. Advanced order reject description in json format.
advancedOrderRejectJson: String. 以 JSON 格式表示的高级订单拒绝描述。
)

def error(self, reqId: TickerId, errorTime: int, errorCode: int, errorString: str, advancedOrderRejectJson = ""):
  print("Error. Id:", reqId, errorTime, "Code:", errorCode, "Msg:", errorString, "AdvancedOrderRejectJson:", advancedOrderRejectJson)

Common Error Resolution  常见错误解决方法

The content below references some of the most common errors received by clients at Interactive Brokers, and offers direct resolutions for the matters in most instances. If further information is required, please feel to contact Customer Service for additional insight.
以下内容涉及 Interactive Brokers 客户收到的一些最常见错误，并在大多数情况下提供直接解决方案。如需更多信息，请随时联系客服获取进一步指导。

Market data farm connection is OK
市场数据农场连接正常

Error code 2104, 2106, and 2158 all generally state that farm connection is OK. What this means is that the API has successfully connected to Trader Workstation or the IB Gateway, and that connection is able to reach Interactive Brokers servers. There is no issue with the connection, and it is a sign you connected successfully.
错误代码 2104、2106 和 2158 通常都表示农场连接正常。这意味着 API 已成功连接到交易工作站或 IB 网关，并且该连接能够到达 Interactive Brokers 服务器。连接没有问题，这是您成功连接的标志。

While using IB Gateway, users may encounter the error, “A historical data farm connection has become inactive but should be available upon demand.” This means that while no historical data requests are being sent, the connection is halted. Once a historical data request is sent over the API connection, the market data farm will reconnect and supply market data.
在使用 IB 网关时，用户可能会遇到错误信息：“历史数据农场连接已变得不活跃，但应按需可用。”这意味着在没有发送历史数据请求时，连接会被暂停。一旦通过 API 连接发送历史数据请求，市场数据农场将重新连接并提供市场数据。
Requested market data requires additional subscription for API. See link in 'Market Data Connections' dialog for more details.Delayed market data is available.
请求的市场数据需要额外的 API 订阅。有关详细信息，请参阅“市场数据连接”对话框中的链接。延迟市场数据可用。

Error 10089 notes that clients are requesting market data when they do not maintain a valid market data subscription. To resolve this issue, users must add a market data subscription to the specific user they are requesting market data with. Alternatively, users must request delayed market data prior to requesting market data.
错误 10089 指出，当客户没有维护有效的市场数据订阅时，他们正在请求市场数据。要解决此问题，用户必须向他们请求市场数据的特定用户添加市场数据订阅。或者，用户必须在请求市场数据之前请求延迟市场数据。

Market data availability is different in TWS versus the API. As a result, market data you can receive in Trader Workstation may not be available in the API.
TWS 与 API 中的市场数据可用性不同。因此，您可以在交易工作站中接收的市场数据可能不在 API 中可用。

Interactive Brokers lists many of our most popular market data subscriptions here.
Interactive Brokers 在此列出了我们许多最受欢迎的市场数据订阅。
Financial Advisors  财务顾问

Financial Advisors are able to manage their allocation groups from the TWS API.
财务顾问能够通过 TWS API 管理他们的分配组。

Note: Modifications made through the API will effect orders placed through TWS, the TWS API, Client Portal, and the Client Portal API.
注意：通过 API 进行的修改将影响通过 TWS、TWS API、客户门户和客户门户 API 下达的订单。
Request FA Groups and Profiles
请求 FA 组别和配置文件

EClient.requestFA (

faDataType: int. The configuration to change. Set to 1 or 3 as defined in the table below.
faDataType: int. 要更改的配置。设置为下表中所定义的 1 或 3。
)

Requests the FA configuration as set in TWS for the given FA Group or Profile.
请求为指定的 FA 组或配置文件在 TWS 中设置的 FA 配置。

self.requestFA(1)

requestFA FA Data Types
requestFA FADT

Type Code  类型代码	Type Name  类型名称	Description  描述
1	Groups  组别	offer traders a way to create a group of accounts and apply a single allocation method to all accounts in the group.
为交易者提供创建账户组并应用于组内所有账户的单一分配方法的方式。
3	Account Aliases  账户别名	let you easily identify the accounts by meaningful names rather than account numbers.
让你能够通过有意义的名称轻松识别账户，而不是账户号码。
Receiving FA Groups and Profiles
接收 FA 组和配置文件

EWrapper.receiveFA (

faDataType: int. Receive the faDataType value specified in the requestFA. See FA Data Types
faDataType: int. 接收在 requestFA 中指定的 faDataType 值。参见 FA 数据类型

faXmlData: String. The xml-formatted configuration.
faXmlData: String. XML 格式的配置。
)

Receives the Financial Advisor’s configuration available in the TWS.
接收 TWS 中金融顾问的配置。

def receiveFA(self, faData: FaDataType, cxml: str):
  print("Receiving FA: ", faData)
  open('log/fa.xml', 'w').write(cxml)

Replace FA Allocations  替换 FA 分配

EClient.replaceFA (

reqId: int. Request identifier used to track data.
reqId: int. 用于跟踪数据的请求标识符。

faDataType: int. The configuration structure to change. Set to 1 or 3 as defined above.
faDataType: int。要更改的配置结构。设置为上面定义的 1 或 3。

xml: String. XML configuration for allocation profiles or group. See Allocation Method XML Format for more details.
xml: String. XML 配置用于分配配置文件或组别。更多详情请参阅分配方法 XML 格式。
)

self.replaceFa(reqId, 1, xml)

replaceFA FA Data Types
replaceFA FA 数据类型

replaceFA Type Code  replaceFA 类型代码	Type Name  类型名称	Description  描述
1	Groups  组	offer traders a way to create a group of accounts and apply a single allocation method to all accounts in the group.
为交易者提供创建账户组的方法，并将单一分配方法应用于组内所有账户。
2	Account Aliases  账户别名	let you easily identify the accounts by meaningful names rather than account numbers.
让你能够通过有意义的名称轻松识别账户，而不是账户号码。

Note:   注意：

In order to confirm that your FA changes were saved, you may wait for the EWrapper.replaceFAEnd callback, which provides the corresponding reqId. In addition, after saving changes, it is advised to verify the new FA setup via EClient.requestFA. If it is called before changes are fully saved, you may receive an error, such as error 10230. See Message Codes.
为了确认你的 FA 更改已保存，你可以等待 EWrapper.replaceFAEnd 回调，它提供相应的 reqId。此外，保存更改后，建议通过 EClient.requestFA 验证新的 FA 设置。如果更改尚未完全保存就调用它，你可能会收到错误，例如错误代码 10230。参见消息代码。

EClient.replaceFA only accepts faDataType 1 now. Otherwise, it may trigger error 585.
EClient.replaceFA 现在只接受 faDataType 1。否则，可能会触发错误 585。
EWrapper.replaceFAEnd (

reqId: int. Request identifier used to track data.
reqId: int. 用于跟踪数据的请求标识符。

text: String. the message text.
text: String. 消息文本。

)

Marks the ending of the replaceFA reception.
标记 replaceFA 接收的结束。

def replaceFAEnd(self, reqId: int, text: str):
    super().replaceFAEnd(reqId, text)
    print("ReplaceFAEnd.", "ReqId:", reqId, "Text:", text)

Allocation Methods and Groups
分配方法和组

A number of methods for account allocations are available with Financial Advisor and IBroker account structures to specify how trades should be distributed across multiple accounts.
Financial Advisor 和 IBroker 账户结构提供了多种账户分配方法，用于指定交易应如何在多个账户之间分配。

Allocation Groups can be created or modified in the Trader Workstation directly as described in TWS: Allocations and Transfers.
分配组可以直接在交易工作台创建或修改，如 TWS：分配和转账中所述。

Alternatively, allocation groups can be created or modified through the EClient.replaceFA() method in the API.
或者，分配组可以通过 API 中的 EClient.replaceFA()方法创建或修改。

Interactive Brokers supports two forms of allocation methods. Allocation methods that have calculations completed by Interactive Brokers, and a set of allocation methods calculated by the user and then specified.
富途支持两种分配方式。富途计算的分配方式，以及用户计算后指定的分配方式。
IB-computed allocation methods
富途计算的分配方式

    Available Equity  可用股票
    Equal Quantity  等量分配
    Net Liquidation Value  净清算价值

User-specified allocation methods
用户自定义分配方法
Formerly known as Allocation Profiles
曾被称为分配配置文件

    Cash Quantity  现金数量
    Percentages  百分比
    Ratios  比率
    Shares  股票

Allocation Method XML Format
分配方法 XML 格式

Allocation methods for financial advisor’s allocation groups are created using an XML format. The content below signifies the supported allocation groups and how to format them in their respective XML.
财务顾问的分配组使用 XML 格式创建分配方法。下文表示支持的分配组及其在各自 XML 中的格式。
Available Equity  可用股票

Requires you to specify an order size. This method distributes shares based on the amount of available equity in each account. The system calculates ratios based on the Available Equity in each account and allocates shares based on these ratios.
需要您指定订单数量。此方法根据每个账户的可用权益分配股票。系统根据每个账户的可用权益计算比例，并根据这些比例分配股票。

Example: You transmit an order for 700 shares of stock XYZ. The account group includes three accounts, A, B and C with available equity in the amounts of $25,000, $50,000 and $100,000 respectively. The system calculates a ratio of 1:2:4 and allocates 100 shares to Client A, 200 shares to Client B, and 400 shares to Client C.
示例：您传输一个购买 700 股股票 XYZ 的订单。账户组包括三个账户 A、B 和 C，其可用权益分别为$25,000、$50,000 和$100,000。系统计算比例为 1:2:4，并将 100 股分配给客户 A，200 股分配给客户 B，400 股分配给客户 C。
<?xml version="1.0" encoding="UTF-8"?>
<ListOfGroups>
  <Group>
    <name>MyTestProfile2</name>
    <defaultMethod>AvailableEquity</defaultMethod>
    <ListOfAccts varName="list">
      <Account>
        <acct>DU6202167</acct>
      </Account>
      <Account>
        <acct>DU6202168</acct>
      </Account>
    </ListOfAccts>
  </Group>
</ListOfGroups>

Contracts Or Shares  合约或股票

This method allocates the absolute number of shares you enter to each account listed. If you use this method, the order size is calculated by adding together the number of shares allocated to each account in the profile.
此方法将您输入的绝对股份数量分配到每个列出的账户。如果您使用此方法，订单大小是通过将配置中每个账户分配的股份数量相加来计算的。

Example:  示例:

Assume an order for 300 shares of stock ABC is transmitted.
假设发送了 300 股 ABC 股票的订单。

In the example code shown in the right side, you can see that:
在右侧显示的示例代码中，可以看到：

    Account A is set to receive 100.0 shares while Account B is set to receive 200.0 shares. Account A should receive 100 shares and Account B should receive 200 shares.
    账户 A 设置为接收 100.0 股，而账户 B 设置为接收 200.0 股。账户 A 应接收 100 股，账户 B 应接收 200 股。

<?xml version="1.0" encoding="UTF-8"?>
<ListOfGroups>
  <Group>
  <name>MyTestProfile2</name>
  <defaultMethod>ContractsOrShares</defaultMethod>
  
  <ListOfAccts varName="list">
  <Account>
    <acct>DU6202167</acct>
    <amount>100.0</amount>
  </Account>
  <Account>
    <acct>DU6202168</acct>
    <amount>200.0</amount>
  </Account>
  </ListOfAccts>
  </Group>
</ListOfGroups>

Equal Quantity  等量分配

Requires you to specify an order size. This method distributes shares equally between all accounts in the group.
需要您指定订单数量。此方法将股票份额平均分配到组内所有账户中。

Example: You transmit an order for 400 shares of stock ABC. If your Account Group includes four accounts, each account receives 100 shares. If your Account Group includes six accounts, each account receives 66 shares, and then 1 share is allocated to each account until all are distributed.
示例：您传输一个购买 400 股 ABC 股票的订单。如果您的账户组包含四个账户，每个账户将获得 100 股。如果您的账户组包含六个账户，每个账户将获得 66 股，然后剩余的 1 股将分配给每个账户，直到所有份额分配完毕。
<?xml version="1.0" encoding="UTF-8"?>
<ListOfGroups>
  <Group>
    <name>MyTestProfile2</name>
    <defaultMethod>Equal</defaultMethod>
    <ListOfAccts varName="list">
      <Account>
        <acct>DU6202167</acct>
      </Account>
      <Account>
        <acct>DU6202168</acct>
      </Account>
    </ListOfAccts>
  </Group>
</ListOfGroups>
MonetaryAmount

The Monetary Amount method calculates the number of units to be allocated based on the monetary value assigned to each account.
货币金额法根据每个账户分配的货币价值计算应分配的单位数量。
<?xml version="1.0" encoding="UTF-8"?>
<ListOfGroups>
  <Group>
  <name>MyTestProfile2</name>
  <defaultMethod>MonetaryAmount</defaultMethod>
  
  <ListOfAccts varName="list">
  <Account>
    <acct>DU6202167</acct>
    <amount>1000.0</amount>
  </Account>
  <Account>
    <acct>DU6202168</acct>
    <amount>2000.0</amount>
  </Account>
  </ListOfAccts>
  </Group>
</ListOfGroups>

Net Liquidation Value  净清算价值

Requires you to specify an order size. This method distributes shares based on the net liquidation value of each account. The system calculates ratios based on the Net Liquidation value in each account and allocates shares based on these ratios.
需要您指定订单大小。此方法根据每个账户的净清算价值分配股票。系统根据每个账户中的净清算价值计算比例，并根据这些比例分配股票。

Example: You transmit an order for 700 shares of stock XYZ. The account group includes three accounts, A, B and C with Net Liquidation values of $25,000, $50,000 and $100,000 respectively. The system calculates a ratio of 1:2:4 and allocates 100 shares to Client A, 200 shares to Client B, and 400 shares to Client C.
示例：您传输了一个购买 700 股股票 XYZ 的订单。账户组包括三个账户 A、B 和 C，其净清算价值分别为$25,000、$50,000 和$100,000。系统计算比例 1:2:4，并将 100 股分配给客户 A，200 股分配给客户 B，400 股分配给客户 C。
<?xml version="1.0" encoding="UTF-8"?>
<ListOfGroups>
  <Group>
    <name>MyTestProfile2</name>
    <defaultMethod>NetLiq</defaultMethod>
    <ListOfAccts varName="list">
      <Account>
        <acct>DU6202167</acct>
      </Account>
      <Account>
        <acct>DU6202168</acct>
      </Account>
    </ListOfAccts>
  </Group>
</ListOfGroups>
Percentages  百分比

This method will split the total number of shares in the order between listed accounts based on the percentages you indicate.
此方法将根据您指定的百分比，将订单中的总股数在已列出账户之间进行分配。

Example:  示例：

Assume an order for 300 shares of stock ABC is transmitted.
假设发送了一个购买 300 股 ABC 股票的订单。

In the example code shown in the right side, you can see that:
在右侧显示的示例代码中，可以看到：

    Account A is set to have 60.0 percentage while Account B is set to have 40.0 percentage. Account A should receive 180 shares and Account B should receive 120 shares.
    账户 A 设置为 60.0 百分比，而账户 B 设置为 40.0 百分比。账户 A 应获得 180 股，账户 B 应获得 120 股。

While making modifications to allocations for profiles, the method uses an enumerated value. The number shown below demonstrates precisely what profile corresponds to which value.
在修改配置文件的分配时，该方法使用枚举值。下面的数字精确地展示了哪个配置文件对应哪个值。
BUY ORDER  买入订单	Positive Percent  正向百分比	Negative Percent  负向百分比
Long Position  多头仓位	Increases position  仓位增加	No effect  无效
Short Position  空头头寸	No effect  无效	Decreases position  减少头寸
SELL ORDER  卖出订单	Positive Percent  正向百分比	Negative Percent  负向百分比
Long Position  多头仓位	No effect  无效果	Decreases position  减少仓位
Short Position  空头仓位	Increases position  增加仓位	No effect  无效

Note:  注意：
Do not specify an order size. Since the quantity is calculated by the system, the order size is displayed in the Quantity field after the order is acknowledged. This method increases or decreases an already existing position. Positive percents will increase a position, negative percents will decrease a position. For exmaple, to fully close out a position, you just need to specify percentage to be -100.
不要指定订单大小。由于系统会计算数量，订单确认后，订单大小会显示在数量字段中。此方法会增加或减少已存在的头寸。正百分比会增加头寸，负百分比会减少头寸。例如，要完全平掉头寸，只需指定百分比为-100 即可。

<?xml version="1.0" encoding="UTF-8"?>
<ListOfGroups>
  <Group>
  <name>MyTestProfile2</name>
  <defaultMethod>Percent</defaultMethod>
  <ListOfAccts varName="list">
  <Account>
    <acct>DU6202167</acct>
    <amount>60.0</amount>
  </Account>
  <Account>
    <acct>DU6202168</acct>
    <amount>40.0</amount>
  </Account>
  </ListOfAccts>
  </Group>
</ListOfGroups>

Ratios  比率

This method calculates the allocation of shares based on the ratios you enter.
此方法根据您输入的比率计算股票的分配。

Example:  示例：

Assume an order for 300 shares of stock ABC is transmitted.
假设发送了一笔购买 300 股 ABC 股票的订单。

In the example code shown in the right side, you can see that:
在右侧显示的示例代码中，可以看到：

    A ratio of 1.0 and 2.0 is set to Account A and Account B. Account A should receive 100 shares and Account B should receive 200 shares.
    为账户 A 和账户 B 设置了 1.0 和 2.0 的比例。账户 A 应获得 100 股，账户 B 应获得 200 股。

<?xml version="1.0" encoding="UTF-8"?>
<ListOfGroups>
  <Group>
  <name>MyTestProfile2</name>
  <defaultMethod>Ratio</defaultMethod>
  
  <ListOfAccts varName="list">
  <Account>
    <acct>DU6202167</acct>
    <amount>1.0</amount>
  </Account>
  <Account>
    <acct>DU6202168</acct>
    <amount>2.0</amount>
  </Account>
  </ListOfAccts>
  </Group>
</ListOfGroups>

Model Portfolios and the API
模型投资组合与 API

Advisors can use Model Portfolios to easily invest some or all of a client’s assets into one or multiple custom-created portfolios, rather than tediously managing individual investments in single instruments.
顾问可以使用模型投资组合，轻松地将客户的部分或全部资产投资到一个或多个自定义创建的投资组合中，而不是费力地管理单个工具中的个别投资。

More about Model Portfolios
更多关于模型投资组合

The TWS API can access model portfolios in accounts where this functionality is available and a specific model has previously been setup in TWS. API functionality allows the client application to request model position update subscriptions, request model account update subscriptions, or place orders to a specific model.
TWS API 可以访问在启用此功能且已在 TWS 中设置特定模型的账户中的模型投资组合。API 功能允许客户端应用程序请求模型头寸更新订阅、请求模型账户更新订阅或向特定模型下单。

Model Portfolio functionality not available in the TWS API:
TWS API 中不提供的模型投资组合功能：

    Portfolio Model Creation  投资组合模型创建
    Portfolio Model Rebalancing
    投资组合模型再平衡
    Portfolio Model Position or Cash Transfer
    投资组合模型头寸或现金转移

To request position updates from a specific model, the function IBApi::EClient::reqPositionsMulti can be used: Position Update Subscription by Model
若要从特定模型请求头寸更新，可以使用 IBApi::EClient::reqPositionsMulti 函数：按模型进行头寸更新订阅

To request model account updates, there is the function IBApi::EClient::reqAccountUpdatesMulti, see: Account Value Update Subscriptions by Model
若要请求模型账户更新，可以使用 IBApi::EClient::reqAccountUpdatesMulti 函数，参见：按模型进行账户价值更新订阅

To place an order to a model, the IBApi.Order.ModelCode field must be set accordingly, for example:
向模型下单时，必须相应地设置 IBApi.Order.ModelCode 字段，例如：

modelOrder = Order()
modelOrder.account = "DF12345"
modelOrder.modelCode = "Technology" # model for tech stocks first created in TWS
self.placeOrder(self.nextOrderId(), contract, modelOrder)

Unification of Groups and Profiles
组别和配置文件的统一

With TWS/IBGW build 983+, the API settings will have a new flag/checkbox, “Use Account Groups with Allocation Methods” (enabled by default for new users). If not enabled, groups and profiles would behave the same as before. If it is checked, group and profile functionality will be merged.
从 TWS/IBGW 版本 983 开始，API 设置将新增一个标志/复选框，“使用带分配方法的账户组”（新用户默认启用）。如果未启用，组别和配置文件将保持之前的行为。如果勾选，组别和配置文件功能将被合并。

With TWS/IBGW Build 10.20+, this setting is now enabled by default, and moving forward into new versions, the two systems can be deemed as interchangeable for modifying allocation groups, placing orders, requesting account or portfolio summaries, or requesting multiple positions.
从 TWS/IBGW 版本 10.20 开始，此设置默认启用，在未来的版本中，这两个系统可被视为可互换的，用于修改分配组、下单、请求账户或投资组合摘要，或请求多个持仓。
Order Placement  订单下单

For advisors to place orders to their allocation groups users would simply declare their allocation group name in the order object. This would be done with the Order’s faGroup field. The example to the right references a standard market order placed to our allocation group, MyTestProfile.
为了使顾问向其分配组下单，用户只需在订单对象中声明其分配组名称。这会通过订单的 faGroup 字段完成。右侧的示例引用了一个标准市价单，该单被下发给我们的分配组，MyTestProfile。

order = Order()
order.action = "BUY"
order.orderType = "MKT"
order.totalQuantity = 50
order.faGroup = "MyTestProfile"

Market Data: Delayed  市场数据：延迟

Delayed market data can only be used with EClient.reqMktData and EClient.reqHistoricalData. This does not function for tick data.
延迟的市场数据只能与 EClient.reqMktData 和 EClient.reqHistoricalData 一起使用。这不能用于逐笔数据。

The API can request Live, Frozen, Delayed and Delayed Frozen market data from Trader Workstation by switching market data type via the EClient.reqMarketDataType before making a market data request. A successful switch to a different (non-live) market data type for a particular market data request will be indicated by a callback to EWrapper.marketDataType with the ticker ID of the market data request which is returning a different type of data.
API 可以通过在发出市场数据请求之前通过 EClient.reqMarketDataType 切换市场数据类型，从交易工作台请求实时、冻结、延迟和延迟冻结的市场数据。对于特定市场数据请求成功切换到不同（非实时）市场数据类型，将通过回调 EWrapper.marketDataType 并使用市场数据请求的股票代码 ID 来指示，该市场数据请求返回不同类型的数据。

    A EClient.reqMarketDataType callback of 1 will occur automatically after invoking reqMktData if the user has live data permissions for the instrument.
    如果用户对工具具有实时数据权限，调用 reqMktData 后将自动发生 EClient.reqMarketDataType 回调为 1。

Market Data Type  市场数据类型	ID	Description  描述
Live  实时	1	Live market data is streaming data relayed back in real time. Market data subscriptions are required to receive live market data.
实时市场数据是实时传输的行情数据。要接收实时市场数据，需要订阅市场数据。
Frozen  冻结	2	Frozen market data is the last data recorded at market close. In TWS, Frozen data is displayed in gray numbers. When you set the market data type to Frozen, you are asking TWS to send the last available quote when there is not one currently available. For instance, if a market is currently closed and real time data is requested, -1 values will commonly be returned for the bid and ask prices to indicate there is no current bid/ask data available. TWS will often show a ‘frozen’ bid/ask which represents the last value recorded by the system. To receive the last know bid/ask price before the market close, switch to market data type 2 from the API before requesting market data. API frozen data requires TWS/IBG v.962 or higher and the same market data subscriptions necessary for real time streaming data.
冻结市场数据是市场收盘时记录的最后数据。在 TWS 中，冻结数据显示为灰色数字。当您将市场数据类型设置为冻结时，您是在请求 TWS 在当前没有可用报价时发送最后可用的报价。例如，如果市场当前关闭且请求实时数据，通常会返回-1 的买卖价以表示当前没有可用的买卖报价。TWS 通常会显示“冻结”的买卖报价，这代表系统记录的最后值。要在市场收盘前接收最后已知的买卖报价，请在请求市场数据前通过 API 切换到市场数据类型 2。API 冻结数据需要 TWS/IBG v.962 或更高版本，以及与实时流数据相同的必要市场数据订阅。
Delayed  延迟	3

Free, delayed data is 15 – 20 minutes delayed. In TWS, delayed data is displayed in brown background. When you set market data type to delayed, you are telling TWS to automatically switch to delayed market data if the user does not have the necessary real time data subscription. If live data is available a request for delayed data would be ignored by TWS. Delayed market data is returned with delayed Tick Types (Tick ID 66~76).
免费延迟数据延迟 15-20 分钟。在 TWS 中，延迟数据显示为棕色背景。当您将市场数据类型设置为延迟时，您是在告诉 TWS 如果用户没有必要的实时数据订阅，则自动切换到延迟市场数据。如果实时数据可用，TWS 将忽略延迟数据请求。延迟市场数据返回的是延迟的 Tick 类型（Tick ID 66~76）。
Delayed Frozen  延迟冻结	4	Requests delayed “frozen” data for a user without market data subscriptions.
为没有市场数据订阅的用户请求“冻结”数据。
Market Data Type Behavior
市场数据类型行为

1) If user sends reqMarketDataType(1) – TWS will start sending only regular (1) market data.
2) 如果用户发送 reqMarketDataType(1) – TWS 将开始仅发送常规 (1) 市场数据。

3) If user sends reqMarketDataType(2) – frozen, TWS will start sending regular (1) as default and frozen (2) market data. TWS sends marketDataType callback (1 or 2) indicating what market data will be sent after this callback. It can be regular or frozen.
4) 如果用户发送 reqMarketDataType(2) – 冻结，TWS 将开始以常规（1）为默认发送，同时发送冻结（2）的市场数据。TWS 发送 marketDataType 回调（1 或 2），指示在此回调之后将发送哪种类型的市场数据。可以是常规或冻结。

5) If user sends reqMarketDataType(3) – delayed, TWS will start sending regular (1) as default and delayed (3) market data.
6) 如果用户发送 reqMarketDataType(3) – 延迟，TWS 将开始以常规（1）为默认发送，同时发送延迟（3）的市场数据。

7) If user sends reqMarketDataType(4) – delayed-frozen, TWS will start sending regular (1) as default, delayed (3) and delayed-frozen (4) market data.
8) 如果用户发送 reqMarketDataType(4) – 延迟-冻结，TWS 将开始以常规（1）为默认发送，同时发送延迟（3）和延迟-冻结（4）的市场数据。

Interactive Brokers data will always try to provide the most up to date market data possible, but will permit additional delayed or frozen data if available upon request.
Interactive Brokers 数据将始终尝试提供尽可能最新的市场数据，但在请求时，如果可用，将允许提供额外的延迟或冻结数据。
Request Market Data Type  请求市场数据类型

EClient.reqMarketDataType (

marketDataType: int. Type of market data to retrieve.
marketDataType: int. 获取的市场数据类型。
)

Switches data type returned from reqMktData request to Live (1), Frozen (2), Delayed (3), or Frozen-Delayed (4).
将 reqMktData 请求返回的数据类型切换为实时（1）、冻结（2）、延迟（3）或冻结-延迟（4）。

self.reqMarketDataType(3)

Receive Market Data Type  接收市场数据类型

EWrapper.marketDataType (

reqId: int. Request identifier used to track data.
reqId: int. 用于跟踪数据的请求标识符。

marketDataType: int. Type of market data to retrieve.
marketDataType: int. 要检索的市场数据类型。
)

def marketDataType(self, reqId: TickerId, marketDataType: int):
  print("MarketDataType. ReqId:", reqId, "Type:", marketDataType)

Market Data: Historical  Market Data: 历史数据

Historical Market data is available for Interactive Brokers market data subscribers in a range of methods and structures. This includes requests for historical bars, identical to the Trader Workstation, historical Time & Sales, as well as Histogram data.
历史市场数据可供 Interactive Brokers 市场数据订阅者在多种方法和结构中使用。这包括与交易工作站相同的历史 K 线请求、历史交易时间与销售数据，以及直方图数据。
Historical Data Limitations
历史数据限制

Historical market data has it’s own set of market data limitations unique to other requests such as real time market data. This section will cover all limitations that effect historical market data in the Trader Workstation API.
历史市场数据有其独特的市场数据限制，与其他请求（如实时市场数据）不同。本节将涵盖所有影响交易工作站 API 中历史市场数据的限制。
Historical Data Filtering
历史数据过滤

Historical data at IB is filtered for trade types which occur away from the NBBO such as combo legs, block trades, and derivative trades. For that reason the daily volume from the (unfiltered) real time data functionality will generally be larger than the (filtered) historical volume reported by historical data functionality. Also, differences are expected in other fields such as the VWAP between the real time and historical data feeds.
IB 的历史数据会过滤掉远离 NBBO 的交易类型，如组合腿交易、大宗交易和衍生品交易。因此，(未过滤的)实时数据功能的日交易量通常会大于(过滤的)历史数据功能报告的历史交易量。此外，实时数据和历史数据之间的其他字段（如 VWAP）也可能存在差异。

As historical data at IB gets adjusted, compressed and filtered by default, there may be historical data differences if you request historical data at different time points.
由于 IB 的历史数据默认会进行调整、压缩和过滤，如果你在不同的时间点请求历史数据，可能会出现历史数据差异。
Historical Volume Scaling
历史成交量缩放

Volume data returned for historical bars can be modified to return in shares or lots.
历史 K 线图的成交量数据可以调整为以股或手返回。

    Open the Global Configuration window
    打开全局配置窗口
    Navigate to “API” and then “Settings” on the left pane
    导航至左侧面板的“API”然后选择“设置”
    Scroll down to the “Send market data in lots for US Stocks for dual-mode API clients”
    向下滚动至“为双模式 API 客户端发送美国股票的市场数据以整批发送”

If the setting is checked, historical volume data will return as a Round Lot.
如果该设置已勾选，历史成交量数据将返回为整批交易单位。

If the setting is unchecked, historical volume data will return in Shares.
如果该设置未勾选，历史成交量数据将以股数形式返回。

Send market data in lots for US stocks for dual-mode API clients highlighted in API Settings.
Pacing Violations for Small Bars (30 secs or less)
小周期数据（30 秒或更短）的节拍违规

Although Interactive Brokers offers our clients high quality market data, IB is not a specialised market data provider and as such it is forced to put in place restrictions to limit traffic which is not directly associated to trading. A Pacing Violation occurs whenever one or more of the following restrictions is not observed:
尽管 Interactive Brokers 为客户提供高质量的市场数据，但 IB 并非专业的市场数据提供商，因此不得不实施限制以限制与交易不直接相关的流量。当出现以下一项或多项限制未被遵守时，就会发生节拍违规：

Important: these limitations apply to all our clients and it is not possible to overcome them. If your trading strategy’s market data requirements are not met by our market data services please consider contacting a specialized provider.
重要提示：这些限制适用于所有客户，且无法绕过。如果您的交易策略的市场数据需求无法通过我们的市场数据服务满足，请考虑联系专业的提供商。

    Making identical historical data requests within 15 seconds.
    在 15 秒内重复请求相同的历史数据。
    Making six or more historical data requests for the same Contract, Exchange and Tick Type within two seconds.
    在两秒内对同一合约、交易所和报价类型进行六次或更多历史数据请求。
    Making more than 60 requests within any ten minute period.
    在任何十分钟内进行超过 60 次请求。
    Note that when BID_ASK historical data is requested, each request is counted twice. In a nutshell, the information above can simply be put as “do not request too much data too quick”.
    请注意，当请求 BID_ASK 历史数据时，每次请求会被计算两次。简而言之，上述信息可以简单地概括为“不要过快请求过多数据”。

Unavailable Historical Data
无法获取历史数据

The other historical data limitations listed are general limitations for all trading platforms:
其他列出的历史数据限制是所有交易平台的通用限制：

    Bars whose size is 30 seconds or less older than six months
    六个月内 30 秒或更短时间间隔的 K 线数据
    Expired futures data older than two years counting from the future’s expiration date.
    从期货到期日起超过两年的到期期货数据
    Expired options, FOPs, warrants and structured products.
    到期期权、FOPs、认股权证和结构化产品。
    End of Day (EOD) data for options, FOPs, warrants and structured products.
    期权、FOPs、认股权证和结构化产品的日终（EOD）数据。
    Data for expired future spreads
    到期期货价差数据。
    Data for securities which are no longer trading.
    不再交易的证券数据。
    Native historical data for combos. Historical data is not stored in the IB database separately for combos.; combo historical data in TWS or the API is the sum of data from the legs.
    组合的本地历史数据。组合的历史数据不会单独存储在 IB 数据库中；TWS 或 API 中的组合历史数据是各组成部分数据的总和。
    Historical data for securities which move to a new exchange will often not be available prior to the time of the move. For example, SOXX stock moved to NASDAQ exchange on 15 Oct 2010, so no SOXX data before 15 Oct 2010 can be retrieved despite SOXX was listed in 2001. This limitation also applied to contract which specifies SMART as the exchange.
    证券迁移到新交易所的历史数据通常在迁移时间之前不可用。例如，SOXX 股票于 2010 年 10 月 15 日迁移到纳斯达克交易所，因此尽管 SOXX 于 2001 年上市，但无法检索到 2010 年 10 月 15 日之前的数据。此限制也适用于指定 SMART 为交易所的合约。
    Studies and indicators such as Weighted Moving Averages or Bollinger Bands are not available from the API.
    加权移动平均线或布林带等研究和指标无法从 API 获取。

Finding the Earliest Available Data Point
查找最早可用的数据点

For many functions, such as EClient.reqHistoricalData, you will need to request market data for a contract. Given that you may not know how long a symbol has been available, you can use EClient.reqHeadTimestamp to find the first available point of data for a given whatToShow value.
对于许多函数，如 EClient.reqHistoricalData，您需要请求合约的市场数据。考虑到您可能不知道某个符号何时可用，您可以使用 EClient.reqHeadTimestamp 来查找给定 whatToShow 值的第一条可用数据点。

ReqHeadTimeStamp counts as an ongoing historical data request, similar to using EClient.reqHistoricalData’s keepUpToDate=True flag. As a result, users should always:
ReqHeadTimeStamp 计算为持续的历史数据请求，类似于使用 EClient.reqHistoricalData 的 keepUpToDate=True 标志。 因此，用户应该始终：

    Cancel timestamp requests using EClient.cancelHeadTimeStamp.
    使用 EClient.cancelHeadTimeStamp 取消时间戳请求。
    All EClient.reqHeadTimestamp requests follow the 30 second bar limitations, regardless of which bar size value has been requested.
    所有 EClient.reqHeadTimestamp 请求都遵循 30 秒 K 线的限制，无论已请求的 K 线大小值为何。

Requesting the Earliest Data Point
请求最早数据点

EClient.reqHeadTimestamp (

tickerId: int., A unique identifier which will serve to identify the incoming data.
tickerId: int., 一个唯一标识符，用于识别传入的数据。

contract: Contract. The IBApi.Contract you are interested in.
contract: Contract 您感兴趣的兴趣 IBApi.Contract。

whatToShow: String. The type of data to retrieve. See Historical Data Types
whatToShow: String. 要检索的数据类型。参见历史数据类型

useRTH: int. Whether (1) or not (0) to retrieve data generated only within Regular Trading Hours (RTH)
useRTH: int. 是否（1）或否（0）仅检索在常规交易时间（RTH）内生成数据

formatDate: int. Using 1 will return UTC time in YYYYMMDD-hh:mm:ss format. Using 2 will return epoch time.
formatDate: int. 使用 1 将返回 YYYYMMDD-hh:mm:ss 格式的 UTC 时间。使用 2 将返回时间戳。
)

Returns the timestamp of earliest available historical data for a contract and data type.
返回合约和数据类型最早可用的历史数据的时戳。

self.reqHeadTimeStamp(1, ContractSamples.USStockAtSmart(), "TRADES", 1, 1)

Receiving the Earliest Data Point
接收最早的数据点

EWrapper.headTimestamp (

requestId: int. Request identifier used to track data.
requestId: int. 用于跟踪数据的请求标识符。

headTimestamp: String. Value identifying earliest data date
headTimestamp: String. 值标识最早数据日期
)

The data requested will be returned to EWrapper.headTimeStamp.
请求的数据将返回到 EWrapper.headTimeStamp。

def headTimestamp(self, reqId, headTimestamp):
        print(reqId, headTimestamp)

Cancelling Timestamp Requests
取消时间戳请求

EWrapper.cancelHistogramData (

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。
)

A reqHeadTimeStamp request can be cancelled with EClient.cancelHeadTimestamp
一个 reqHeadTimeStamp 请求可以通过 EClient.cancelHeadTimestamp 取消

self.cancelHeadTimeStamp(reqId)

Historical Bars  历史 K 线

Historical Bar data returns a candlestick value based on the requested duration and bar size. This will always return an open, high, low, and close values. Based on which whatToShow value is used, you may also receive volume data. See the whatToShow section for more details.
历史 K 线数据根据请求的持续时间和 K 线大小返回一个 K 线值。这始终会返回开盘价、最高价、最低价和收盘价。根据使用的 whatToShow 值，您可能还会收到成交量数据。有关详细信息，请参阅 whatToShow 部分。
Requesting Historical Bars
请求历史 K 线

EClient.reqHistoricalData(

reqId: int, A unique identifier which will serve to identify the incoming data.
reqId: int, 一个唯一标识符，用于识别传入的数据。

contract: Contract, The IBApi.Contract object you are working with.
contract: Contract, 您正在处理的 IBApi.Contract 对象。

endDateTime: String, The request’s end date and time. This should be formatted as “YYYYMMDD HH:mm:ss TMZ” or an empty string indicates current present moment).
endDateTime: String, 请求的结束日期和时间。应格式化为“YYYYMMDD HH:mm:ss TMZ”，或空字符串表示当前时刻。
Please be aware that endDateTime must be left as an empty string when requesting continuous futures contracts.
请注意，在请求连续期货合约时，endDateTime 必须保持为空字符串。

durationStr: String, The amount of time (or Valid Duration String units) to go back from the request’s given end date and time.
durationStr: String, 从请求给定的结束日期和时间回溯的时间量（或有效持续时间字符串单位）。

barSizeSetting: String, The data’s granularity or Valid Bar Sizes
barSizeSetting: String, 数据的粒度或有效 K 线大小

whatToShow: String, The type of data to retrieve. See Historical Data Types
whatToShow: String, 要检索的数据类型。参见历史数据类型

useRTH: bool, Whether (1) or not (0) to retrieve data generated only within Regular Trading Hours (RTH)
useRTH: bool, 是否（1）或不是（0）仅检索在常规交易时间（RTH）内生成数据

formatDate: bool, The format in which the incoming bars’ date should be presented. Note that for day bars, only yyyyMMdd format is available.
formatDate: bool, 输入数据的时间格式。注意，对于日 K 线，仅支持 yyyyMMdd 格式。

keepUpToDate: bool, Whether a subscription is made to return updates of unfinished real time bars as they are available (True), or all data is returned on a one-time basis (False). If True, and endDateTime cannot be specified.
keepUpToDate: bool, 是否订阅以返回未完成的实时 K 线数据更新（True），或一次性返回所有数据（False）。如果为 True，则不能指定 endDateTime。
Supported whatToShow values: Trades, Midpoint, Bid, Ask.

chartOptions: TagValueList, This is a field used exclusively for internal use.
chartOptions: TagValueList, 该字段专用于内部使用。

)

self.reqHistoricalData(4102, contract, queryTime, "1 M", "1 day", "MIDPOINT", 1, 1, False, [])

Duration  持续时间

The Interactive Brokers Historical Market Data maintains a duration parameter which specifies the overall length of time that data can be collected. The duration specified will derive the bars of data that can then be collected.
富途证券历史市场数据维护一个持续时间参数，该参数指定数据收集的总时长。指定的持续时间将决定可以收集的数据条形图。
Valid Duration String Units:
有效的持续时间字符串单位：
Unit  单位	Description  描述
S	Seconds  秒
D	Day  天
W	Week  周
M	Month  月
Y	Year  年份

Historical Bar Sizes  历史 K 线大小

Bar sizes dictate the data returned by historical bar requests. The bar size will dictate the scale over which the OHLC/V is returned to the API.
K 线大小决定了历史 K 线请求返回的数据。K 线大小将决定 OHLC/V 返回给 API 的时间范围。
Valid Bar Sizes:  有效 K 线大小：
Bar Unit  K 线单位	Bar Sizes  K 线大小
secs  秒	1, 5, 10, 15, 30
mins  分钟	1, 2, 3, 5, 10, 15, 20, 30
hours  小时	1, 2, 3, 4, 8
day  天	1
weeks  周	1
months  月份	1

Step Sizes  步长

The functionality of market data requests are predicated on preset step sizes. As such, not all bar sizes will work with all duration values. The table listed here will discuss the smallest to largest bar size value for each duration string.
市场数据请求的功能基于预设的步长。因此，并非所有 K 线大小都适用于所有持续时间值。此处列出的表格将讨论每种持续时间字符串的最小到最大 K 线大小值。
Duration Unit  持续时间单位	Bar units allowed  允许的条形单位	Bar size Interval (Min/Max)
条形大小间隔（最小/最大）
S	secs | mins  秒 | 分钟	1 secs -> 1mins  1 秒 -> 1 分钟
D	secs | mins | hrs
秒 | 分 | 小时	5 secs -> 1 hours
5 秒 -> 1 小时
W	sec | mins | hrs
秒 | 分 | 小时	10 secs -> 4 hrs
10 秒 -> 4 小时
M	sec | mins | hrs	30 secs -> 8 hrs
Y	mins | hrs   | d	1 mins-> 1 day
Max Duration Per Bar Size
每根 K 线最大持续时间

The table below displays the maximum duration values allowed for a given bar.
下表显示了给定 K 线允许的最大持续时间值。

As an example, the maximum duration for Seconds values supported for 5 seconds bars are 86400 S. This means that if I want to retrieve more than 1 day’s worth of 5 second bars, I will then need to request data in increments of D (days).
例如，对于 5 秒 K 线，支持的秒值最大持续时间为 86400 秒。这意味着如果我想获取超过 1 天的 5 秒 K 线，那么我需要以 D（天）为增量请求数据。
Bar Size  条形大小	Max Second Duration  最大秒级持续时间	Max Day Duration  最大日级持续时间	Max Week Duration  最大周级持续时间	Max Month Duration  最大月持续时间	Max Year Duration  最大年持续时间
1 secs  1 秒	2000 S  2000 秒	{Not Supported}	{Not Supported}	{Not Supported}	{Not Supported}
5 secs  5 秒	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
10 secs  10 秒	86400 S	365 D	52 W	12 M	68 Y
15 secs  15 秒	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
30 secs  30 秒	86400 S	365 D	52 W	12 M	68 Y
1 min  1 分钟	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
2 mins  2 分钟	86400 S	365 D	52 W	12 M	68 Y
3 mins  3 分钟	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
5 mins  5 分钟	86400 S	365 D	52 W	12 M	68 Y
10 mins  10 分钟	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
15 mins  15 分钟	86400 S	365 D	52 W	12 M	68 Y
20 mins  20 分钟	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
30 mins  30 分钟	86400 S	365 D	52 W	12 M	68 Y
1 hour  1 小时	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
2 hours  2 小时	86400 S	365 D	52 W	12 M	68 Y
3 hours  3 小时	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
4 hours  4 小时	86400 S	365 D	52 W	12 M	68 Y
8 hours  8 小时	86400 S  86400 秒	365 D  365 天	52 W  52 周	12 M	68 Y
1 day  1 天	86400 S	365 D	52 W	12 M	68 Y
1M	86400 S	365 D	52 W	12 M	68 Y
1W	86400 S	365 D	52 W	12 M	68 Y
Format Date Received  格式化接收日期

Interactive Brokers will return historical market data based on the format set from the request. The formatDate parameter can be provided an integer value to indicate how data should be returned.
Interactive Brokers 将根据请求中设置的格式返回历史市场数据。formatDate 参数可以提供一个整数值来指示数据应如何返回。

Note: Day bars will only return dates in the yyyyMMdd format. Time data is not available.
注意：日 K 线仅以 yyyyMMdd 格式返回日期。时间数据不可用。
Value  值	Description  描述	Example  示例
1	String Time Zone Date  字符串时区日期	“20231019 16:11:48 America/New_York”
2	Epoch Date  纪元日期	1697746308
3	Day & Time Date  日期和时间	“1019 16:11:48 America/New_York”
Keep Up To Date  保持最新

When using keepUpToDate=True for historical data requests, you will see several bars returned with the same timestamp. This is because data is updated approximately every 4-6 seconds. These updates compound until the end of the specified bar size.
当对历史数据请求使用 keepUpToDate=True 时，您会看到多个具有相同时间戳的 K 线返回。这是因为数据大约每 4-6 秒更新一次。这些更新会累积，直到指定的 K 线大小结束。

In our example to the below, 15 second bars are requested, and we can see the 30 second bar built out incrementally until 20231204 13:30:30 is completed. At which point, we move on to the 45th second bars. This same logic extends into minute, hourly, or daily bars.
在我们的示例中，请求了 15 秒的 K 线图，我们可以看到 30 秒的 K 线图逐步构建，直到 20231204 13:30:30 完成。此时，我们继续构建 45 秒的 K 线图。同样的逻辑适用于分钟、小时或每日的 K 线图。
Note:  注意：

keepUpToDate is only available for whatToShow: Trades, Midpoint, Bid, Ask
keepUpToDate 仅在 whatToShow 参数为 Trades、Midpoint、Bid、Ask 时可用
Date: 20231204 13:30:30 US/Eastern, Open: 188.56, High: 188.56, Low: 188.54, Close: 188.55
Date: 20231204 13:30:30 US/Eastern, Open: 188.56, High: 188.56, Low: 188.54, Close: 188.55
Date: 20231204 13:30:30 US/Eastern, Open: 188.56, High: 188.56, Low: 188.54, Close: 188.55
Date: 20231204 13:30:30 US/Eastern, Open: 188.56, High: 188.56, Low: 188.54, Close: 188.55
Date: 20231204 13:30:30 US/Eastern, Open: 188.56, High: 188.56, Low: 188.54, Close: 188.55
Date: 20231204 13:30:30 US/Eastern, Open: 188.56, High: 188.56, Low: 188.54, Close: 188.56
Date: 20231204 13:30:30 US/Eastern, Open: 188.56, High: 188.56, Low: 188.54, Close: 188.56
Date: 20231204 13:30:30 US/Eastern, Open: 188.56, High: 188.57, Low: 188.54, Close: 188.55
Date: 20231204 13:30:45 US/Eastern, Open: 188.54, High: 188.54, Low: 188.54, Close: 188.54

Receiving Historical Bars
接收历史 K 线图

EWrapper.historicalData (
EWrapper.historicalData

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

bar: Bar. The OHLC historical data Bar. The time zone of the bar is the time zone chosen on the TWS login screen. Smallest bar size is 1 second.
bar: Bar. OHLC 历史数据 Bar。Bar 的时间区域是 TWS 登录屏幕上选择的时间区域。最小的 Bar 大小为 1 秒。
)

The historical data will be delivered via the EWrapper.historicalData method in the form of candlesticks. The time zone of returned bars is the time zone chosen in TWS on the login screen.
历史数据将通过 EWrapper.historicalData 方法以 K 线图的形式发送。返回的 Bar 的时间区域是 TWS 登录屏幕上选择的时间区域。

def historicalData(self, reqId:int, bar: BarData):
  print("HistoricalData. ReqId:", reqId, "BarData.", bar)

Default Return Format  默认返回格式

The text on the right is the default formatting for returning data.
右侧的文字是返回数据的默认格式。

The datetime value here was modified to return UTC datetime formatting.
这里的日期时间值已修改为返回 UTC 日期时间格式。

Note: The datetime value indicates the beginning of the request range rather than the end. The last bar on the right would then indicate data that took place between 20241111-16:53:15 to 20241111-16:53:20.
注意：日期时间值表示请求范围的开始，而不是结束。那么最右侧的最后一根 K 线将表示发生在 20241111-16:53:15 至 20241111-16:53:20 之间的数据。
Date: 20241111-16:53:00, Open: 222.97, High: 222.97, Low: 222.96, Close: 222.97, Volume: 300, WAP: 222.965, BarCount: 2
Date: 20241111-16:53:05, Open: 222.97, High: 223.01, Low: 222.96, Close: 223.01, Volume: 5378, WAP: 222.981, BarCount: 38
Date: 20241111-16:53:10, Open: 223.02, High: 223.02, Low: 222.98, Close: 222.98, Volume: 3659, WAP: 222.997, BarCount: 24
Date: 20241111-16:53:15, Open: 222.98, High: 222.98, Low: 222.96, Close: 222.97, Volume: 2585, WAP: 222.963, BarCount: 24
EWrapper.historicalSchedule (
EWrapper.historicalSchedule

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

startDateTime: String. Returns the start date and time of the historical schedule range.
startDateTime: String. 返回历史时间表范围的开始日期和时间。

endDateTime: String. Returns the end date and time of the historical schedule range.
endDateTime: String. 返回历史时间表范围的结束日期和时间。

timeZone: String. Returns the time zone referenced by the schedule.
timeZone: String. 返回时间表参考的时区。

sessions: HistoricalSession[]. Returns the full block of historical schedule data for the duration.
sessions: HistoricalSession[]。返回指定时间段内的完整历史日程数据块。
)

In the case of whatToShow=”schedule”, you will need to also define the EWrapper.historicalSchedule value. This is a unique method that will only be called in the case of the unique whatToShow value to display calendar information.
当 whatToShow="schedule"时，您还需要定义 EWrapper.historicalSchedule 的值。这是一种唯一的方法，仅在显示日历信息的唯一 whatToShow 值时才会被调用。

def historicalSchedule(self, reqId: int, startDateTime: str, endDateTime: str, timeZone: str, sessions: ListOfHistoricalSessions):
  print("HistoricalSchedule. ReqId:", reqId, "Start:", startDateTime, "End:", endDateTime, "TimeZone:", timeZone)
  for session in sessions:
    print("\tSession. Start:", session.startDateTime, "End:", session.endDateTime, "Ref Date:", session.refDate)

EWrapper.historicalDataUpdate (
EWrapper.historicalDataUpdate

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

bar: Bar. The OHLC historical data Bar. The time zone of the bar is the time zone chosen on the TWS login screen. Smallest bar size is 1 second.
bar: Bar. OHLC 历史数据 Bar。Bar 的时间区域是 TWS 登录屏幕上选择的时间区域。最小的 Bar 大小为 1 秒。
)

Receives bars in real time if keepUpToDate is set as True in reqHistoricalData. Similar to realTimeBars function, except returned data is a composite of historical data and real time data that is equivalent to TWS chart functionality to keep charts up to date. Returned bars are successfully updated using real time data.
如果 reqHistoricalData 中的 keepUpToDate 设置为 True，则实时接收 K 线数据。与 realTimeBars 函数类似，但返回的数据是历史数据和实时数据的组合，相当于 TWS 图表功能以保持图表更新。返回的 K 线数据成功使用实时数据进行了更新。

def historicalDataUpdate(self, reqId: int, bar: BarData):
  print("HistoricalDataUpdate. ReqId:", reqId, "BarData.", bar)

EWrapper.historicalDataEnd (

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

start: String. Returns the starting time of the first historical data bar.
start: String. 返回第一个历史数据条的开始时间。

end: String. Returns the end time of the last historical data bar.
end: String。返回最后一个历史数据条的时间。
)

Marks the ending of the historical bars reception.
标记历史数据接收的结束。

def historicalDataEnd(self, reqId: int, start: str, end: str):
  print("HistoricalDataEnd. ReqId:", reqId, "from", start, "to", end)

Historical Bar whatToShow
历史数据条 whatToShow

The historical bar types listed below can be used as the whatToShow value for historical bars. These values are used to request different data such as Trades, Midpoint, Bid_Ask data and more. Some bar types support more products than others. Please note the Supported Products section for each bar type below.
下面列出的历史数据条类型可以用作历史数据条的 whatToShow 值。这些值用于请求不同类型的数据，如交易、中点、买价卖价数据等。不同类型的数据条支持的产品种类不同。请参考下面每种数据条类型的支持产品部分。
AGGTRADES

Bar Values:  条形图值：
Open  开盘价	High  最高价	Low  最低价	Close  关闭	Volume  成交量
First traded price  首次交易价格	Highest traded price  最高成交价	Lowest traded price  最低成交价	Last traded price  最后成交价	Total traded volume  总成交量

Supported Products: Cryptocurrency
支持的产品：加密货币
ADJUSTED_LAST

Bar Values:  K 线值：
Open  开盘价	High  高	Low  低	Close  关闭	Volume  量
First traded price  首次交易价格	Highest traded price  最高成交价	Lowest traded price  最低成交价	Last traded price  最后成交价	Total traded volume  总成交量

Supported Products: ETFs, Options, Stocks
支持的产品：ETFs、期权、股票

NOTES: ADJUSTED_LAST data is adjusted for splits and dividends.
注意：ADJUSTED_LAST 数据已根据拆分和股息进行调整。
ASK  卖价

Bar Values:  条形图值：
Open  开盘价	High  最高价	Low  最低价	Close  关闭	Volume  成交量
Starting ask price  起始卖价	Highest ask price  最高要价	Lowest ask price  最低要价	Last ask price  最后要价	N/A  不适用

Supported Products: Bonds, CFDs, Commodities, Cryptocurrencies, ETFs, FOPs, Forex, Funds, Futures,  Metals, Options, SSFs, Stocks, Structured Products, Warrants
支持的产品：债券、差价合约、商品、加密货币、交易所交易基金、场外期权、外汇、基金、期货、金属、期权、短期证券、股票、结构性产品、认股权证
BID

Bar Values:
Open	High	Low  最低价	Close  关闭	Volume  成交量
Starting bid price  起拍价	Highest bid price  最高出价	Lowest bid price  最低出价	Last bid price  最后出价	N/A

Supported Products: Bonds, CFDs, Commodities, Cryptocurrencies, ETFs, FOPs, Forex, Funds, Futures,  Metals, Options, SSFs, Stocks, Structured Products, Warrants
支持的产品：债券、差价合约、商品、加密货币、交易所交易基金、场外期权、外汇、基金、期货、金属、期权、证券型开放式基金、股票、结构化产品、认股权证
BID_ASK

Bar Values:  条形图值：
Open  开盘价	High  最高价	Low  最低价	Close  关闭	Volume  成交量
Time average bid  时间平均买价	Max Ask  最大卖价	Min Bid  最小买价	Time average ask  时间平均卖价	N/A  不适用

Supported Products: Bonds, CFDs, Commodities, Cryptocurrencies, ETFs, FOPs, Forex, Funds, Futures, Metals, Options, SSFs, Stocks, Structured Products, Warrants
支持的产品：债券、差价合约、商品、加密货币、交易所交易基金、场外期权、外汇、基金、期货、金属、期权、短期证券、股票、结构性产品、认股权证
FEE_RATE  费率

Bar Values:  条形图值：
Open  打开	High  最高价	Low  最低价	Close  关闭	Volume  成交量
Starting Fee Rate  起始费率	Highest fee rate  最高费率	Lowest fee rate  最低费率	Last fee rate  最后费率	N/A  不适用

Supported Products: Stocks, ETFs,
支持的产品：股票、ETFs、
HISTORICAL_VOLATILITY  历史波动率

Bar Values:  柱状图数值：
Open  开盘价	High  最高价	Low  低	Close  收盘	Volume  成交量
Starting volatility  初始波动率	Highest volatility  最高波动率	Lowest volatility  最低波动率	Last volatility  最后波动率	N/A  不适用

Supported Products: ETFs, Indices, Stocks
支持的产品：ETFs、指数、股票
MIDPOINT  中点

Bar Values:  条形图值：
Open  打开	High  高	Low  最低价	Close  关闭	Volume  成交量
Starting midpoint price  起始中间价	Highest midpoint price  最高中间价	Lowest midpoint price  最低中间价	Last midpoint price  最后中间价	N/A

Supported Products: Bonds, CFDs, Commodities, Cryptocurrencies, ETFs, FOPs, Forex, Funds, Futures,  Metals, Options, SSFs, Stocks, Structured Products, Warrants
支持的产品：债券、差价合约、商品、加密货币、交易所交易基金、场外期权、外汇、基金、期货、金属、期权、证券型开放式基金、股票、结构化产品、认股权证
OPTION_IMPLIED_VOLATILITY

Bar Values:  柱状图数值：
Open  开盘价	High  最高价	Low  低	Close  关闭	Volume  成交量
Starting implied volatility
起始隐含波动率	Highest implied volatility
最高隐含波动率	Lowest implied volatility
最低隐含波动率	Last implied volatility  最后隐含波动率	N/A  不适用

Supported Products: ETFs, Indices, Stocks
支持的产品：ETFs、指数、股票
SCHEDULE  日程

Bar Values:  条形图值：
Open  打开	High  高	Low  低	Close  收盘	Volume  成交量
Starting ask price  起始卖价	Highest ask price  最高要价	Lowest ask price  最低要价	Last ask price  最后要价	N/A  不适用

Supported Products: Bonds, CFDs, Commodities, Cryptocurrencies, ETFs, Forex, Funds, Futures, Indices, Metals,  SSFs, Stocks, Structured Products, Warrants
支持的产品：债券、差价合约、商品、加密货币、交易所交易基金、外汇、基金、期货、指数、金属、短期证券、股票、结构性产品、认股权证

NOTE: SCHEDULE data returns only on 1 day bars but returns historical trading schedule only with no information about OHLCV.
注意：SCHEDULE 数据仅在 1 天 K 线上返回，但返回的历史交易时间表不包含 OHLCV 信息。
TRADES  交易

Bar Values:  K 线值：
Open  打开	High  高	Low  低	Close  关闭	Volume  成交量
First traded price  首次交易价格	Highest traded price  最高成交价	Lowest traded price  最低成交价	Last traded price  最新成交价	Total traded volume  总成交量

Supported Products: Bonds, ETFs, FOPs, Futures, Indices, Metals, Options, SSFs, Stocks, Structured Products, Warrants
支持的产品：债券、ETF、FOP、期货、指数、金属、期权、SSF、股票、结构性产品、认股权证

NOTES: TRADES data is adjusted for splits, but not dividends.
注意：TRADES 数据已调整拆分，但未调整股息。
YIELD_ASK

Bar Values:  条形图值：
Open  开盘价	High  最高价	Low  最低价	Close  关闭	Volume  成交量
Starting ask yield  初始卖方报价收益率	Highest ask yield  最高卖方报价收益率	Lowest ask yield  最低买价收益率	Last ask yield  最新买价收益率	N/A  不适用

Supported Products: Indices
支持的产品：指数

Note: Yield historical data only available for corporate bonds.
注意：仅企业债券提供历史收益率数据。
YIELD_BID

Bar Values:  条形图值：
Open  打开	High  高	Low  低	Close  关闭	Volume  成交量
Starting bid yield  起始买价收益率	Highest bid yield  最高买价收益率	Lowest bid yield  最低买价收益率	Last bid yield  最后报价收益率	N/A  不适用

Supported Products: Indices
支持的产品：指数

Note: Yield historical data only available for corporate bonds.
注意：收益率历史数据仅适用于公司债券。
YIELD_BID_ASK

Bar Values:  柱状图数值：
Open  开盘价	High  高	Low  低	Close  收盘	Volume  成交量
Time average bid yield  时间平均买价收益率	Highest ask yield  最高卖价收益率	Lowest bid yield  最低买价收益率	Time average ask yield  时间平均卖价收益率	N/A

Supported Products: Indices
支持的产品：指数

Note: Yield historical data only available for corporate bonds.
注意：收益率历史数据仅适用于公司债券。
YIELD_LAST

Bar Values:  柱状图值：
Open  开盘价	High  高	Low  低	Close  收盘	Volume  成交量
Starting last yield  开始上一个收益率	Highest last yield  最高上一个收益率	Lowest last yield  最低上一个收益率	Last last yield  上一个上一个收益率	N/A

Supported Products: Indices
支持的产品：指数

Note: Yield historical data only available for corporate bonds.
注意：收益率历史数据仅适用于公司债券。
Histogram Data  直方图数据

Instead of returned data points as a function of time as with the function IBApi::EClient::reqHistoricalData, histograms return data as a function of price level with function IBApi::EClient::reqHistogramData
与函数 IBApi::EClient::reqHistoricalData 返回数据点随时间变化不同，直方图数据通过函数 IBApi::EClient::reqHistogramData 以价格水平为函数返回
Requesting Histogram Data
请求直方图数据

EClient.reqHistogramData (

requestId: int, id of the request
requestId: int, 请求 ID

contract: Contract, Contract object that is subject of query.
合约：合约，查询的主题合约对象。

useRth: bool, Data from regular trading hours (1), or all available hours (0).
useRth：bool，常规交易时段数据（1），或所有可用时段数据（0）。

period: String, string value of requested date range. This will be tied to the same bar size strings as the historical bar sizes
period：String，请求的日期范围字符串值。这将与历史 K 线大小字符串相同。
)

Returns data histogram of specified contract.
返回指定合约的数据直方图。

self.reqHistogramData(4004, contract, false, "3 days")

Receiving Histogram Data  接收直方图数据

EWrapper.histogramData (

requestId: int. Request identifier used to track data.
requestId: int. 用于跟踪数据的请求标识符。

data: HistogramEntry[]. Returned Tuple of histogram data, number of trades at specified price level.
data: HistogramEntry[]. 返回的直方图数据元组，指定价格水平的交易数量。
)

Returns relevant histogram data.
返回相关的直方图数据。

def histogramData(self, reqId:int, items:HistogramDataList):
  print("HistogramData. reqid, items)

Cancelling Histogram Data
取消直方图数据

EClient.cancelHistogramData (
EClient.cancelHistogramData

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。
)

An active histogram request which has not returned data can be cancelled with EClient.cancelHistogramData
一个未返回数据的活跃直方图请求可以通过 EClient.cancelHistogramData 来取消

self.reqHistogramData(4004)

Historical Time & Sales  历史时间与交易

The highest granularity of historical data from IB’s database can be retrieved using the API function EClient.reqHistoricalTicks for historical time and sales values. Historical Time & Sales will return the same data as what is available in Trader Workstation under the Time and Sales window. This is a series of ticks indicating each trade based on the requested values.
使用 API 函数 EClient.reqHistoricalTicks 可以获取 IB 数据库中历史数据的最高粒度，用于获取历史时间和交易值。历史时间和交易将返回与 Trader Workstation 中时间和交易窗口中相同的数据。这是一系列基于请求值的每个交易的点。

    Historical Tick-By-Tick data is not available for combos.
    组合交易不提供逐点历史数据。
    Data will not be returned from multiple trading sessions in a single request; Multiple requests must be used.
    单个请求不会从多个交易会话中返回数据；必须使用多个请求。
    To complete a full second, more ticks may be returned than requested.
    完成一整秒，返回的报文可能比请求的多。
    Time & Sales data requires a Level 1, Top Of Book market data subscription. This would be the same subscription as EClient.reqMktData() or EClient.reqHistoricalData().
    时间与交易数据需要 Level 1、图书顶端市场数据订阅。这将是与 EClient.reqMktData()或 EClient.reqHistoricalData()相同的订阅。

Requesting Time and Sales data
请求时间与交易数据

EClient.reqHistoricalTicks (

requestId: int, id of the request
requestId: int, 请求的 ID

contract: Contract, Contract object that is subject of query.
合约：合约，查询的主题合约对象。

startDateTime: String, i.e. “20170701 12:01:00”. Uses TWS timezone specified at login.
startDateTime: 字符串，例如“20170701 12:01:00”。使用登录时指定的 TWS 时区。

endDateTime: String, i.e. “20170701 13:01:00”. In TWS timezone. Exactly one of startDateTime or endDateTime must be defined.
endDateTime: 字符串，例如“20170701 13:01:00”。在 TWS 时区中。startDateTime 和 endDateTime 必须且仅有一个被定义。

numberOfTicks: int, Number of distinct data points. Max is 1000 per request.
numberOfTicks: int, 不同数据点的数量。每次请求最大为 1000。

whatToShow: String, (Bid_Ask, Midpoint, or Trades) Type of data requested.
whatToShow: String, (Bid_Ask, Midpoint, or Trades) 请求的数据类型。

useRth: bool, Data from regular trading hours (1), or all available hours (0).
useRth: bool, 来自常规交易时段（1），或所有可用时段（0）的数据。

ignoreSize: bool, Omit updates that reflect only changes in size, and not price. Applicable to Bid_Ask data requests.
ignoreSize: bool, 忽略仅反映数量变化而不反映价格变化的更新。适用于 Bid_Ask 数据请求。
Note: Options and Future Options will only display a value of 1, unless to indicate a removed bid/ask, which will instead return a price and size value of 0.
注意：期权和未来期权将仅显示值为 1，除非表示已移除的买卖报价，此时将返回价格为 0 和大小为 0 的值。

miscOptions: list, Should be defined as null; reserved for internal use.
miscOptions: list，应定义为 null；保留用于内部使用。
)

Requests historical Time & Sales data for an instrument.
请求某个金融工具的历史交易时间与成交数据。

self.reqHistoricalTicks(18001, contract, "20170712 21:39:33 US/Eastern", "", 10, "TRADES", 1, True, [])

Receiving Time and Sales data
接收时间与交易数据

Data is returned to unique functions based on what is requested in the whatToShow field.
数据根据 whatToShow 字段中请求的内容返回到不同的函数。

    IBApi.EWrapper.historicalTicks for whatToShow=MIDPOINT
    IBApi.EWrapper.historicalTicksBidAsk for whatToShow=BID_ASK
    IBApi.EWrapper.historicalTicksLast for for whatToShow=TRADES

EWrapper.historicalTicks (

reqId: int, id of the request
reqId: int, 请求的 ID

ticks: ListOfHistoricalTick, object containing a list of tick values for the requested timeframe.
ticks: ListOfHistoricalTick, 包含请求时间段内的 tick 值列表的对象。

done: bool, return whether or not this is the end of the historical ticks requested.
done: bool, 返回是否为请求的历史 K 线数据的末尾。
)

For whatToShow=MIDPOINT

def historicalTicks(self, reqId: int, ticks: ListOfHistoricalTickLast, done: bool):
  for tick in ticks:
    print("historicalTicks. ReqId:", reqId, tick)

EWrapper.historicalTicksBidAsk (

reqId: int, id of the request
reqId: int, 请求的 ID

ticks: ListOfHistoricalTick, object containing a list of tick values for the requested timeframe.
ticks: ListOfHistoricalTick, 包含请求时间段内的 tick 值列表的对象。

done: bool, return whether or not this is the end of the historical ticks requested.
done: bool, 返回是否为请求的历史 K 线数据的末尾。
)

For whatToShow=BidAsk

def historicalTicksBidAsk(self, reqId: int, ticks: ListOfHistoricalTickLast, done: bool):
  for tick in ticks:
    print("historicalTicksBidAsk. ReqId:", reqId, tick)

EWrapper.historicalTicksLast (
EWrapper.historicalTicksLast

reqId: int, id of the request
reqId: int, 请求的 ID

ticks: ListOfHistoricalTick, object containing a list of tick values for the requested timeframe.
ticks: ListOfHistoricalTick, 包含请求时间段内的 tick 值列表的对象。

done: bool, return whether or not this is the end of the historical ticks requested.
done: bool, 返回是否为请求的历史 K 线数据的末尾。
)

For whatToShow=Last & AllLast
对于 whatToShow=Last & AllLast

def historicalTicksLast(self, reqId: int, ticks: ListOfHistoricalTickLast, done: bool):
  for tick in ticks:
    print("HistoricalTickLast. ReqId:", reqId, tick)

Historical Halted and Unhalted ticks
历史暂停和未暂停的报價

The tick attribute pastLimit is also returned with streaming Tick-By-Tick responses. Check Halted and Unhalted ticks section.
流式报價的 Tick-By-Tick 响应中也会返回 pastLimit 属性。查看暂停和未暂停的报價部分。

    If tick has zero price, zero size and pastLimit flag is set – this is “Halted” tick.
    如果报價具有零价格、零数量且 pastLimit 标志被设置——这是“暂停”报價。
    If tick has zero price, zero size and followed immediately after “Halted” tick – this is “Unhalted” tick.
    如果价格为零、数量为零，并且紧随“暂停”报文之后——这是“未暂停”报文。

Historical Date Formatting
历史日期格式

When creating dates in the TWS API, Interactive Brokers typically supports three methods:
在 TWS API 中创建日期时，Interactive Brokers 通常支持三种方法：

    Operator Time Zone  运营商时区
    Exchange Time Zone  交易所时区
    Coordinated Universal Time (UTC)
    协调世界时（UTC）

Operator Time Zone  运营商时区

Operator Time Zone is the local time set by the user in Trader Workstation. The Operator Time Zone typically maintains a unique formatting structure separate from Exchange Time Zones; however, they can match.
操作员时区是用户在交易工作台设置的本地时间。操作员时区通常保持独特的格式结构，与交易所时区分离；但也可以匹配。

A user can confirm their Operator Time Zone by launching Trader Workstation then, before logging in, click “More Options >”.
用户可以通过启动交易工作台，在登录前点击“更多选项 >”来确认其操作员时区。

More Options button on the TWS login window.

Users can then confirm their active Operator Time Zone by referencing the “Time Zone” field.
用户可以通过参考“时区”字段来确认其当前的运营商时区。

For US residents, this will typically appear as “America/New_York”, “America/Chicago”, or “America/Los_Angeles”. It is essential to note the Time Zone value, as this will be the value supplied when making requests with the Operator Time Zone.
对于美国居民，这通常会显示为“America/New_York”、“America/Chicago”或“America/Los_Angeles”。需要注意的是时区值，因为在使用操作时区发起请求时，将使用此值。

More Options settings on the TWS login window.

After logging in to Trader Workstation or IB Gateway, you would be able to submit time stamps in the format of “YYYYMMDD HH:mm:ss Operator/Time_Zone”.
登录 Trader Workstation 或 IB Gateway 后，您将能够提交格式为“YYYYMMDD HH:mm:ss 操作员/时区”的时间戳。

Given our prior example, a historical data endDateTime value would appear as”20250101 23:59:59 America/Chicago”. This would mean the latest value I want is just before midnight in Chicago on January 1st, 2025. Even if I am trading contracts in New York or overseas, all historical data requests would be relative to my own time zone.
根据我们之前的示例，历史数据 endDateTime 值将显示为“20250101 23:59:59 America/Chicago”。这意味着我最想获取的值是 2025 年 1 月 1 日芝加哥时间午夜之前的一个值。即使我正在纽约或海外进行合约交易，所有历史数据请求都将相对于我自己的时区。
Exchange Time Zone  交易所时区

The exchange Time Zone is the value the exchange itself uses to calculate time. This value is typically unique to the Operator Time Zone, but these values can overlap.
交易所的时区是交易所用来计算时间的值。这个值通常与运营商时区唯一，但这些值可能会重叠。

As an example, the New York Stock Exchange operates on “US/Eastern”. However, the CME operates on “US/Central”. This values can be programmatically requested using the EClient.reqContractDetails method, and then received from EWrapper.contractDetails in contractDetails.Time ZoneId.
例如，纽约证券交易所使用“US/Eastern”时区。然而，芝加哥商品交易所使用“US/Central”时区。这些值可以通过 EClient.reqContractDetails 方法程序化地请求，然后从 EWrapper.contractDetails 中的 contractDetails.TimeZoneId 接收。

Note that this will be interpreted differently from “America/Chicago”.
请注意，这与“America/Chicago”的解读方式不同。

Time Zone response from a reqContractDetails request.
Coordinated Universal Time (UTC)
协调世界时 (UTC)

UTC is a time standard centered around Greenwich Mean Time (GMT). UTC historical data can be formatted as “YYYYMMDD-hh:mm:ss”. Please keep in mind this is based on UTC+0, and as a reference, US/Eastern time is approximately UTC-4 or UTC-5 depending on U.S. Daylight savings.
UTC 是一个以格林尼治标准时间（GMT）为中心的时间标准。UTC 历史数据可以格式化为“YYYYMMDD-hh:mm:ss”。请注意这是基于 UTC+0，作为参考，美国东部时间大约是 UTC-4 或 UTC-5，取决于美国的夏令时。

Please note GMT is unaffected by Daylight savings, and so 09:00:00 will be the same time of day year round regardless of the exchange’s or your local daylight savings observation.
请注意 GMT 不受夏令时影响，因此无论交易所或您当地是否实行夏令时，09:00:00 都将是全年相同的时间。
Modifying Returned Date  修改返回的日期

You may also log in to the Trader Workstation and modify this in the Global Configuration under API and then Settings. Here, you will find a modifiable setting labeled “Send instrument-specific attributes for dual-mode API client in” Here you can select one of the following:
您也可以登录交易工作台，在 API 和设置下的全局配置中修改此设置。在这里，您会找到一个可修改的设置，标签为“在双模式 API 客户端中发送特定于仪器的属性”。在这里，您可以选择以下之一：

    operator timezone: refers to the local timezone you have set in the Trader Workstation or IB Gateway
    操作员时区：指你在交易工作台或 IB 网关中设置的本地时区
    instrument timezone: refers to the timezone of the requested exchange. If “SMART” is used, this will use the instrument’s primary exchange.
    合约时区：指请求的交易所的时区。如果使用“SMART”，将使用合约的主要交易所
    UTC format: refers to a standardized return using UTC as the timezone. This will be returned in the format YYYYMMDD-hh:mm:ss
    UTC 格式：指以 UTC 作为时区的标准化返回。这将按照 YYYYMMDD-hh:mm:ss 的格式返回

Market Data: Live  市场数据：实时

Live Data Limitations  实时数据限制

For all data, besides Delayed Watchlist Data, a paid data subscription is required to receive market data through the API. See the Market Data Subscriptions page for more information.
对于所有数据，除了延迟监控列表数据，通过 API 接收市场数据需要付费数据订阅。有关更多信息，请参阅市场数据订阅页面。

    Live market data and historical bars are currently not available from the API for the exchange OSE. Only 15 minute delayed streaming data will be available for this exchange.
    目前，对于交易所 OSE，API 无法提供实时市场数据和历史 K 线数据。该交易所仅提供 15 分钟延迟的流数据。
    Some Available Tick Types may not be provided due to the contract details, the time that you run the code…… ,etc. To verify whether the specific Available Tick Type is provided, it is suggested to manually check the data in TWS.
    由于合约细节、运行代码的时间……等原因，某些可用报类型可能无法提供。建议手动检查 TWS 中的数据，以验证特定可用报类型是否提供。
    Different Available Tick Types have different updating frequency.
    不同的可用价差类型具有不同的更新频率。

The bid, ask, and last size quotes are displayed in shares instead of lots.
报价、询价和最新成交量的报价以股为单位显示，而不是以手为单位。

API users have the option to configure the TWS API to work in compatibility mode for older programs, but we recommend migrating to “quotes in shares” at your earliest convenience.
API 用户可以选择将 TWS API 配置为兼容模式以支持旧程序，但我们建议尽快迁移到“以股为单位显示报价”。

To display quotes as lots, from the Global Configuration > API > Settings page, check “Bypass US Stocks market data in shares warning for API orders.”
要按手显示报价，请从全局配置 > API > 设置页面中，选中“为 API 订单绕过美国股票以股为单位显示的警告”。

Highlights the "Bypass US Stocks market data in shares warning for API Orders" under API Precautions.
5 Second Bars  5 秒 K 线图

Real time and historical data functionality is combined through the EClient.reqRealTimeBars request. reqRealTimeBars will create an active subscription that will return a single bar in real time every five seconds that has the OHLC values over that period. reqRealTimeBars can only be used with a bar size of 5 seconds.
实时和历史数据功能通过 EClient.reqRealTimeBars 请求结合在一起。reqRealTimeBars 将创建一个活跃订阅，每五秒返回一个包含该时间段 OHLC 值的单根 K 线。reqRealTimeBars 只能用于 5 秒的 K 线间隔。

Important: real time bars subscriptions combine the limitations of both, top and historical market data. Make sure you observe Market Data Lines and Pacing Violations for Small Bars (30 secs or less). For example, no more than 60 *new* requests for real time bars can be made in 10 minutes, and the total number of active active subscriptions of all types cannot exceed the maximum allowed market data lines for the user.
重要提示：实时 K 线订阅结合了顶部市场和实时市场数据的限制。请确保遵守市场数据线和小 K 线（30 秒或更短）的节奏违规规则。例如，10 分钟内实时 K 线的新请求不能超过 60 个，所有类型的活跃订阅总数不能超过用户允许的最大市场数据线数。
Request Real Time Bars  请求实时 K 线

EClient.reqRealTimeBars (

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。

contract: Contract. The Contract object for which the depth is being requested
合约：合约。正在请求深度的合约对象

barSize: int. Currently being ignored
barSize：int。当前被忽略

whatToShow: String. The nature of the data being retrieved:
whatToShow：String。正在检索的数据的性质：
Available Values: TRADES, MIDPOINT, BID, ASK
可用值：TRADES，MIDPOINT，BID，ASK

useRTH: int. Set to 0 to obtain the data which was also generated outside of the Regular Trading Hours, set to 1 to obtain only the RTH data
useRTH: int。设置为 0 以获取在常规交易时间之外生成的数据，设置为 1 以仅获取常规交易时间内的数据
)

realTimeBarOptions: List<TagValue>. Internal use only.
realTimeBarOptions: List<TagValue>. 仅内部使用。

Requests real time bars.  请求实时 K 线图。

Only 5 seconds bars are provided. This request is subject to the same pacing as any historical data request: no more than 60 API queries in more than 600 seconds.
仅提供 5 秒 K 线。此请求与任何历史数据请求一样，受相同节奏的限制：超过 600 秒内，API 查询不得超过 60 次。

Real time bars subscriptions are also included in the calculation of the number of Level 1 market data subscriptions allowed in an account.
实时 K 线订阅也包含在账户允许的 Level 1 市场数据订阅数量计算中。

self.reqRealTimeBars(3001, contract, 5, "MIDPOINT", 0, [])

Code example:  代码示例：
from ibapi.client import *
from ibapi.wrapper import *
from ibapi.contract import Contract
import time
class TradeApp(EWrapper, EClient):
    def __init__(self):
        EClient.__init__(self, self)
    def realtimeBar(self, reqId: TickerId, time:int, open_: float, high: float, low: float, close: float, volume: Decimal, wap: Decimal, count: int):
        print("RealTimeBar. TickerId:", reqId, RealTimeBar(time, -1, open_, high, low, close, volume, wap, count))

app = TradeApp()
app.connect("127.0.0.1", 7496, clientId=1)
contract = Contract()
contract.symbol = "AAPL"
contract.secType = "STK"
contract.currency = "USD"
contract.exchange = "SMART"
app.reqRealTimeBars(3001, contract, 5, "TRADES", 0, [])
app.run()

Receive Real Time Bars  接收实时 K 线图

EWrapper.realtimeBar (  EWrapper.realtimeBar

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

time: long. The bar’s start date and time (Epoch/Unix time)
时间：长整型。K 线的开始日期和时间（Epoch/Unix 时间）

open: double. The bar’s open point
开盘价：双精度浮点型。K 线的开盘点

high: double. The bar’s high point
最高价：双精度浮点型。K 线的最高点

low: double. The bar’s low point
最低价：双精度浮点型。K 线的最低点

close: double. The bar’s closing point
close: double. K 线的收盘点

volume: decimal. The bar’s traded volume (only returned for TRADES data)
volume: decimal. K 线的交易量（仅返回 TRADES 数据）

WAP: decimal. The bar’s Weighted Average Price rounded to minimum increment (only available for TRADES).
WAP: decimal. K 线的加权平均价，四舍五入到最小增量（仅适用于 TRADES）。

count: int. The number of trades during the bar’s timespan (only available for TRADES).
count: int. K 线时间段内的交易次数（仅适用于 TRADES）。
)

Receives the real time 5 second bars.
接收实时 5 秒 K 线图。

def realtimeBar(self, reqId: TickerId, time:int, open_: float, high: float, low: float, close: float, volume: Decimal, wap: Decimal, count: int):
  print("RealTimeBar. TickerId:", reqId, RealTimeBar(time, -1, open_, high, low, close, volume, wap, count))

Cancel Real Time Bars  取消实时 K 线图

EClient.cancelRealTimeBars (

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。
)

Cancels Real Time Bars’ subscription.
取消实时 K 线图的订阅。

self.cancelRealTimeBars(3001)

Component Exchanges  组件交易所

A single data request from the API can receive aggregate quotes from multiple exchanges. The tick types ‘bidExch’ (tick type 32), ‘askExch’ (tick type 33), ‘lastExch’ (tick type 84) are used to identify the source of a quote. To preserve bandwidth, the data returned to these tick types consists of a sequence of capital letters rather than a long list of exchange names for every returned exchange name field. To find the full exchange name corresponding to a single letter code returned in tick types 32, 33, or 84, and API function IBApi::EClient::reqSmartComponents is available. Note: This function can only be used when the exchange is open.
单个 API 数据请求可以接收来自多个交易所的聚合报价。用于标识报价来源的报文类型包括‘bidExch’（报文类型 32）、‘askExch’（报文类型 33）、‘lastExch’（报文类型 84）。为节省带宽，返回给这些报文类型的报文内容由一系列大写字母组成，而不是为每个返回的交易所名称字段提供长长的交易所名称列表。要查找与报文类型 32、33 或 84 中返回的单字母代码对应的完整交易所名称，可以使用 API 函数 IBApi::EClient::reqSmartComponents。注意：此函数仅在交易所开盘时可用。

Different IB contracts have a different exchange map containing the set of exchanges on which they trade. Each exchange map has a different code, such as “a6” or “a9”. This exchange mapping code is returned to EWrapper.tickReqParams immediately after a market data request is made by a user with market data subscriptions. To find a particular map of single letter codes to full exchange names, the function reqSmartComponents is invoked with the exchange mapping code returned to tickReqParams.
不同的 IB 合约具有不同的交易所映射，其中包含它们交易的交易所集合。每个交易所映射具有不同的代码，例如“a6”或“a9”。当用户具有市场数据订阅并发出市场数据请求时，此交易所映射代码会立即返回给 EWrapper.tickReqParams。要查找特定单字母代码到完整交易所名称的映射，可以调用 reqSmartComponents 函数，并传入返回给 tickReqParams 的交易所映射代码。

For instance, a market data request for the IBKR US contract may return the exchange mapping identifier “a6” to EWrapper.tickReqParams . Invoking the function EClient.reqSmartComponents with the symbol “a9” will reveal the list of exchanges offering market data for the IBKR US contract, and their single letter codes. The code for “ARCA” may be “P”. In that case if “P” is returned to the exchange tick types, that would indicate the quote was provided by ARCA.
例如，针对 IBKR 美国合约的市场数据请求可能会返回交易所映射标识符“a6”到 EWrapper.tickReqParams 中。调用函数 EClient.reqSmartComponents 并传入符号“a9”将显示提供 IBKR 美国合约市场数据的交易所列表及其单字母代码。ARCA 的代码可能是“P”。在这种情况下，如果交易所类型返回“P”，则表示报价是由 ARCA 提供的。
Request Component Exchanges
请求组件交易所

EClient.reqSmartComponents (

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

bboExchange: String. Mapping identifier received from EWrapper.tickReqParams
bboExchange: String. 从 EWrapper.tickReqParams 接收的映射标识符
)

Returns the mapping of single letter codes to exchange names given the mapping identifier.
返回根据映射标识符提供的单个字母代码到交易所名称的映射。

self.reqSmartComponents(1018, "a6")

Receive Component Exchanges
接收组件交换

EWrapper.smartComponents (

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

smartComponentMap: SmartComponentMap. Unique object containing a map of all key-value pairs
smartComponentMap: SmartComponentMap. 包含所有键值对映射的唯一对象
)

Containing a bit number to exchange + exchange abbreviation dictionary. All IDs can be initially retrieved using reqTickParams.
包含一个用于交换的数字和交换缩写词典。所有 ID 最初都可以使用 reqTickParams 获取。

def smartComponents(self, reqId:int, smartComponentMap:SmartComponentMap):
  print("SmartComponents:")
  for smartComponent in smartComponentMap:
    print("SmartComponent.", smartComponent)

Market Depth Exchanges  市场深度交易所

To check which exchanges offer deep book data, the function EClient.reqMktDepthExchanges can be invoked. It will return a list of exchanges from where market depth is available if the user has the appropriate market data subscription.
要检查哪些交易所提供深度行情数据，可以调用函数 EClient.reqMktDepthExchanges。如果用户具有相应的市场数据订阅，它将返回一个交易所列表，从中可以获取市场深度数据。

API ‘Exchange’ fields for which a market depth request would return market maker information and result in a callback to EWrapper.updateMktDepthL2 will be indicated in the results from the EWrapper.mktDepthExchanges field by a ‘True’ value in the ‘isL2’ field:
API “Exchange” 字段，用于市场深度请求会返回做市商信息并导致 EWrapper.updateMktDepthL2 回调的字段，将在 EWrapper.mktDepthExchanges 字段的返回结果中通过 “isL2” 字段的 “True” 值来指示：
Requesting Market Depth Exchanges
请求市场深度交易所

EClient.reqMktDepthExchanges ()

Requests venues for which market data is returned to updateMktDepthL2 (those with market makers).
请求返回市场深度数据以更新 MktDepthL2（那些有做市商的）。

self.reqMktDepthExchanges()

Receive Market Depth Exchanges
接收市场深度交易所

EWrapper.mktDepthExchanges (

depthMktDataDescriptions: DepthMktDataDescription[]. A list containing all available exchanges offering market depth.
depthMktDataDescriptions: DepthMktDataDescription[]。一个包含所有提供市场深度信息的交易所的列表。
)

Called when receives Depth Market Data Descriptions.
当接收到深度市场数据描述时调用。

def mktDepthExchanges(self, depthMktDataDescriptions:ListOfDepthExchanges):
  print("MktDepthExchanges:")
  for desc in depthMktDataDescriptions:
    print("DepthMktDataDescription.", desc)

Market Depth (L2)  市场深度（L2）

Market depth data, also known as level II, represents an instrument’s order book. Via the TWS API it is possible to obtain this information with the EClient.reqMarketDepth function.
市场深度数据，也称为 L2，代表一个金融工具的订单簿。通过 TWS API，可以使用 EClient.reqMarketDepth 函数获取这些信息。

Unlike Top Market Data (Level I), market depth data is sent without sampling or filtering, however we cannot guarantee that every price quoted for a particular security will be displayed. In particular, odd lot orders are not included.
与顶级市场数据（L1）不同，市场深度数据会直接发送，不进行采样或过滤，但我们无法保证某个特定证券的每个报价都会显示。特别是，零散订单不包括在内。

It is possible to Smart-route a EClient.reqMarketDepth request to receive aggregated data from all available exchanges.
可以通过智能路由将 EClient.reqMarketDepth 请求路由到所有可用交易所，以接收聚合数据。

An integral part of processing the incoming data is monitoring EWrapper.error for message 317 “Market depth data has been RESET. Please empty deep book contents before applying any new entries.” and handling it appropriately, otherwise the update process would be corrupted.
处理传入数据的一个关键部分是监控 EWrapper.error，以获取消息 317“市场深度数据已被重置。在应用任何新条目之前，请清空深度书内容。”并适当处理，否则更新过程将被破坏。

Market Depth is not support for Calendar Spreads or Combos.
市场深度不支持日历价差或组合。
Request Market Depth  请求市场深度

Important: Please note that the languages use different method names for requesting market depth.
重要提示：请注意，不同语言在请求市场深度时使用不同的方法名。

The C# and Visual Basic APIs use reqMarketDepth().
C#和 Visual Basic API 使用 reqMarketDepth()。

The Python, Java, and C++ APIs use reqMktDepth().
Python、Java 和 C++ API 使用 reqMktDepth()。
EClient.reqMarketDepth (

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。

contract: Contract. The Contract for which the depth is being requested.
contract: Contract. 请求深度信息的合约。

numRows: int. The number of rows on each side of the order book.
numRows: int. 每侧订单簿的行数。

isSmartDepth: bool. Flag indicates that this is a Smart-routed market depth request. Supplying true will return data identical to the TWS Book Trader while False returns direct routed data similar to the TWS Market Depth tool.
isSmartDepth: bool. 标志指示这是一个智能路由的市场深度请求。提供 true 将返回与 TWS Book Trader 相同的数据，而 False 将返回类似 TWS Market Depth 工具的直接路由数据。

mktDepthOptions: List. Internal use only. Leave an empty array or None type.
mktDepthOptions: 列表。仅内部使用。留空数组或 None 类型。
)

Requests the contract’s market depth (order book).
请求合约的市场深度（订单簿）。

self.reqMktDepth(2001, contract, 5, False, [])

Receive Market Depth  接收市场深度

EWrapper.updateMktDepth (

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。

position: int. The order book’s row being updated
position: int. 更新的订单簿行

operation: int. Indicates a change in the row’s value.:
operation: int. 指示行值的变化。

    0 = insert (insert new price into the row)·
    0 = 插入（将新价格插入该行）·
    1 = update (update the existing order in the row)·
    1 = 更新（更新该行中的现有订单）·
    2 = delete (delete the existing order at the row).
    2 = 删除（删除该行中的现有订单）.

side: int. 0 for ask, 1 for bid
side: int. 0 为卖价，1 为买价

price: double. The order’s price
价格：double。订单的价格

size: decimal. The order’s size
数量：decimal。订单的数量
)

Returns the order book. Used for direct routed requests only.
返回订单簿。仅用于直接路由请求。

def updateMktDepth(self, reqId: TickerId, position: int, operation: int, side: int, price: float, size: Decimal):
    print("UpdateMarketDepth. ReqId:", reqId, "Position:", position, "Operation:", operation, "Side:", side, "Price:", floatMaxString(price), "Size:", decimalMaxString(size))

EWrapper.updateMktDepthL2 (

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。

position: int. The order book’s row being updated.
position: int. 更新的订单簿行号。

marketMaker: String. The exchange holding the order if isSmartDepth is True, otherwise the MPID of the market maker.
marketMaker: String. 如果 isSmartDepth 为 True，则表示持有订单的交易所；否则表示做市商的 MPID。

operation: int. Indicates a change in the row’s value.:
operation: int. 指示行值的变化。

    0 = insert (insert new price into the row)·
    0 = 插入（将新价格插入该行）·
    1 = update (update the existing order in the row)·
    1 = 更新（更新该行中的现有订单）·
    2 = delete (delete the existing order at the row).
    2 = 删除（删除该行中的现有订单）.

side: int. 0 for ask, 1 for bid
side: int. 0 为卖价，1 为买价

price: double. The order’s price
价格：double。订单的价格

size: decimal. The order’s size
数量：decimal。订单的数量

isSmartDepth: bool. Flag indicating if this is smart depth response (True) or the MPID of the market maker.
isSmartDepth: bool. 指示这是智能深度响应（True）还是做市商的 MPID。
)

Returns the order book.  返回订单簿。

def updateMktDepthL2(self, reqId: TickerId, position: int, marketMaker: str, operation: int, side: int, price: float, size: Decimal, isSmartDepth: bool):
  print("UpdateMarketDepthL2. ReqId:", reqId, "Position:", position, "MarketMaker:", marketMaker, "Operation:", operation, "Side:", side, "Price:", floatMaxString(price), "Size:", decimalMaxString(size), "isSmartDepth:", isSmartDepth)

Cancel Market Depth  取消市场深度

EClient.cancelMarketDepth (

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。

isSmartDepth: bool. Flag indicates that this is smart depth request.
isSmartDepth: bool. 标志指示这是一个智能深度请求。

)

Cancel’s market depth’s request.
取消市场深度请求。

self.cancelMktDepth(2001, False)

Option Greeks  期权希腊字母

The option greek values- delta, gamma, theta, vega- are returned by default following a reqMktData() request for the option. See Available Tick Types
期权希腊值（delta、gamma、theta、vega）在执行 reqMktData()请求后默认返回。参见可用报价类型

Tick types “Bid Option Computation” (#10), “Ask Option Computation” (#11), “Last Option Computation” (#12), and “Model Option Computation” (#13) return all Greeks (delta, gamma, vega, theta), the underlying price and the stock and option reference price when requested.
“买入期权计算”（#10）、“卖出期权计算”（#11）、“最新期权计算”（#12）和“模型期权计算”（#13）这些报价类型在请求时返回所有希腊值（delta、gamma、vega、theta）、标的资产价格以及股票和期权参考价格。

MODEL_OPTION_COMPUTATION also returns model implied volatility.
模型期权计算还返回模型隐含波动率。

Note that to receive live greek values it is necessary to have market data subscriptions for both the option and the underlying contract.
注意，要接收实时希腊值，必须同时订阅期权和标的合约的市场数据。

The implied volatility for an option given its price and the price of the underlying can be calculated with the function EClient.calculateImpliedVolatility.
根据期权价格和标的资产价格，可以使用函数 EClient.calculateImpliedVolatility 计算隐含波动率。

Alternatively, given the price of the underlying and an implied volatility it is possible to calculate the option price using the function EClient.calculateOptionPrice.
或者，根据标的资产价格和隐含波动率，可以使用函数 EClient.calculateOptionPrice 计算期权价格。

After the request, the option specific information will be delivered via the EWrapper.tickOptionComputation method.
请求后，期权特定信息将通过 EWrapper.tickOptionComputation 方法传递。
Request Options Greeks  请求选项希腊字母

EClient.reqMktData (  EClient.reqMktData

reqId: int. Request identifier for tracking data.
reqId: int. 请求标识符，用于跟踪数据。

contract: Contract. Contract object used for specifying an instrument.
合约：合约。用于指定金融工具的合约对象。

genericTickList: String. Comma separated ids of the available generic ticks.
genericTickList: String. 可用通用报价的逗号分隔的 id。

snapshot: bool. Set to True for snapshot data with a relevant subscription or False for live data.
snapshot: bool. 设置为 True 表示相关订阅的快照数据，或设置为 False 表示实时数据。

regulatorySnapshot: bool. Set to True for a paid, regulatory snapshot or False for live data.
regulatorySnapshot: bool. 设置为 True 表示付费监管快照，或设置为 False 表示实时数据。

mktDataOptions: List<TagValue>. Internal use only.
mktDataOptions: List<TagValue>. 仅内部使用。
)

Greeks are requested automatically when pulling market data for an Options contract.
在拉取期权合约的市场数据时，会自动请求希腊字母参数。
Users that do not have a valid Market Data Subscription for the underlying contract will receive an error that Market Data Is Not Subscribed. This error can be ignored if Greeks are not wanted.
如果没有有效的市场数据订阅，用户会收到"市场数据未订阅"的错误。如果不需要希腊字母参数，可以忽略此错误。

self.reqMktData(reqId, OptionContract, "", False, False, [])
Calculating option prices
计算期权价格

EClient.calculateOptionPrice (

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

contract: Contract. The Contract object for which the depth is being requested
合约：合约。正在请求深度的合约对象

volatility: double. Hypothetical volatility.
volatility: double. 假设波动率。

underPrice: double. Hypothetical option’s underlying price.
underPrice: double. 假设期权的标的资产价格。

optionPriceOptions: List<TagValue>. Internal use only. Send an empty tag value list.
optionPriceOptions: List<TagValue>. 仅内部使用。发送一个空的标签值列表。
)

Calculates an option’s price based on the provided volatility and its underlying’s price.
根据提供的波动率和其标的资产的价格计算期权价格。

self.calculateOptionPrice(5002, OptionContract, 0.6, 55, [])

Calculating historical volatility
计算历史波动率

EClient.calculateImpliedVolatility (

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

contract: Contract. The Contract object for which the depth is being requested
合约：合约。正在请求深度的合约对象

optionPrice: double. Hypothetical option price.
optionPrice: double. 假设期权价格。

underPrice: double. Hypothetical option’s underlying price.
underPrice: double. 假设期权的标的物价格。

impliedVolatilityOptions: List<TagValue>. Internal use only. Send an empty tag value list.
impliedVolatilityOptions: List<TagValue>. 仅内部使用。发送一个空的标签值列表。
)

Calculate the volatility for an option. Request the calculation of the implied volatility based on hypothetical option and its underlying prices.
计算期权的波动率。请求根据假设的期权及其标的物价格计算隐含波动率。

self.calculateImpliedVolatility(5001, OptionContract, 0.5, 55, [])

Receiving Options Data  接收期权数据

EWrapper.tickOptionComputation (

tickerId the request’s unique identifier.
tickerId 请求的唯一标识符。

field: int. Specifies the type of option computation.
field: int。指定期权计算的类型。
Pass the field value into
将字段值传递到
TickType.getField(int tickType) to retrieve the field description. For example, a field value of 13 will map to modelOptComp, etc. 10 = Bid 11 = Ask 12 = Last
使用 TickType.getField(int tickType)来获取字段描述。例如，字段值 13 将映射到 modelOptComp 等。10 = Bid 11 = Ask 12 = Last

tickAttrib: int. 0 – return based, 1- price based.
tickAttrib: int。0 – 基于返回值，1 – 基于价格。

impliedVolatility: double. the implied volatility calculated by the TWS option modeler, using the specified tick type value.
impliedVolatility: double。由 TWS 期权模型器使用指定的 tick 类型值计算出的隐含波动率。

delta: double. The option delta value.
delta: double。期权 delta 值。

optPrice: double. The option price.
optPrice: double. 期权价格。

pvDividend: double. The present value of dividends expected on the option’s underlying.
pvDividend: double. 期权标的物预期股息的现值。

gamma: double. The option gamma value.
gamma: double. 期权 gamma 值。

vega: double. The option vega value.
vega: double. 期权 vega 值。

theta: double. The option theta value.
theta: double. 期权 theta 值。

undPrice: double. The price of the underlying.
undPrice: double. 标的资产价格。
)

Receives option specific market data. This method is called when the market in an option or its underlier moves. TWS’s option model volatilities, prices, and deltas, along with the present value of dividends expected on that options underlier are received.
接收期权特定市场数据。当期权或其标的物市场价格变动时，会调用此方法。接收 TWS 期权模型中的波动率、价格和 Delta，以及期权标的物预期股息的现值。

def tickOptionComputation(self, reqId: TickerId, tickType: TickType, tickAttrib: int, impliedVol: float, delta: float, optPrice: float, pvDividend: float, gamma: float, vega: float, theta: float, undPrice: float):
  print("TickOptionComputation. TickerId:", reqId, "TickType:", tickType, "TickAttrib:", intMaxString(tickAttrib), "ImpliedVolatility:", floatMaxString(impliedVol), "Delta:", floatMaxString(delta), "OptionPrice:", floatMaxString(optPrice), "pvDividend:", floatMaxString(pvDividend), "Gamma: ", floatMaxString(gamma), "Vega:", floatMaxString(vega), "Theta:", floatMaxString(theta), "UnderlyingPrice:", floatMaxString(undPrice))

Top of Book (L1)  最优买卖报价（L1）

Streaming market data values corresponding to data shown in TWS watchlists is available via the EClient.reqMktData. This data is not tick-by-tick but consists of aggregate snapshots taken several times per second. A set of ‘default’ tick types are returned by default from a call to EClient.reqMktData, and additional tick types are available by specifying the corresponding generic tick type in the market data request. Including the generic tick types many, but not all, types of data are available that can be displayed in TWS watchlists by adding additional columns.
通过 EClient.reqMktData 可以获取与 TWS 监视列表中显示的数据相对应的市场数据流。这些数据不是逐笔数据，而是由每秒多次捕获的聚合快照组成。默认情况下，从 EClient.reqMktData 调用返回一组“默认”的逐笔类型，可以通过在市场数据请求中指定相应的通用逐笔类型来获取额外的逐笔类型。包括通用逐笔类型，许多（但并非所有）类型的数据都可以通过添加额外的列在 TWS 监视列表中显示。

Using the TWS API, you can request real time market data for trading and analysis. From the API, market data returned from the function IBApi.EClient.reqMktData corresponds to market data displayed in TWS watchlists. This data is not tick-by-tick but consists of aggregated snapshots taken at intra-second intervals which differ depending on the type of instrument:
使用 TWS API，您可以请求用于交易和分析的实时市场数据。从 API 返回的函数 IBApi.EClient.reqMktData 对应于 TWS 监视列表中显示的市场数据。这些数据不是逐笔数据，而是由在秒内间隔捕获的聚合快照组成，具体间隔取决于工具类型：
Product  产品	Frequency  频率
Stocks, Futures and others
股票、期货及其他	250 ms  250 毫秒
US Options  美国期权	100 ms  100 毫秒
FX pairs  货币对	5 ms  5 毫秒
Request Watchlist Data  请求观察列表数据

EClient.reqMktData (  EClient.reqMktData

reqId: int. Request identifier for tracking data.
reqId: int. 请求标识符，用于跟踪数据。

contract: Contract. Contract object used for specifying an instrument.
contract: Contract. 用于指定金融工具的合约对象。

genericTickList: String. Comma separated ids of the available generic ticks.
genericTickList: String. 可用通用报价的 ID，以逗号分隔。

snapshot: bool. Used to retrieve a single snapshot of data for those with an existing market data subscirption.
snapshot: bool. 用于获取现有市场数据订阅者的单次数据快照。

regulatorySnapshot: bool. Used to retrieve a single snapshot of paid data. Each snapshot costs $0.01.
regulatorySnapshot: bool. 用于获取单个付费数据快照。每个快照费用为$0.01。
See here for more information about Regulatory Snapshots and Market Data.
有关监管快照和市场数据的更多信息，请参见此处。

mktDataOptions: List<TagValue>. Internal use only.
mktDataOptions: List<TagValue>. 仅限内部使用。
)

Requests real time market data. Returns market data for an instrument either in real time or 10-15 minutes delayed data.
请求实时市场数据。返回某个工具的实时数据或延迟 10-15 分钟的数据。

self.reqMktData(reqId, contract, "", False, False, [])

Market Data Update Frequency
市场数据更新频率

Watchlist market data at Interactive Brokers is derived from time-based snapshot intervals which vary by product and region. This means that a given tick will only update as frequently as its interval allows. See the table for more details on product specifics.
在 Interactive Brokers 中，观察列表的市场数据来源于基于时间的快照间隔，这些间隔因产品和地区而异。这意味着某个特定数据点的更新频率将受其间隔限制。有关产品具体信息的更多详情，请参阅下表。
Product  产品	Frequency  频率
United States  美国
Stocks, Futures, Bonds, Indices
股票、期货、债券、指数	250ms  250 毫秒
Options  期权	10ms  10 毫秒
Forex  外汇	5ms  5 毫秒
Europe  欧洲
All Products  所有产品	250ms  250 毫秒
Asia  亚洲
All Products  所有产品	250ms  250 毫秒
Generic Tick Types  通用行情类型

The most common tick types are delivered automatically after a successful market data request. There are however other tick types available by explicit request: the generic tick types. When invoking IBApi.EClient.reqMktData, specific generic ticks can be requested via the genericTickList parameter of the function:
最常见的行情类型在成功获取市场数据请求后自动提供。然而，通过显式请求，还有其他行情类型可用：通用行情类型。在调用 IBApi.EClient.reqMktData 时，可以通过函数的 genericTickList 参数请求特定的通用行情类型：

See the Available Tick Types section for more information on generic ticks.
参见可用报文类型部分以获取通用报文的更多信息。
Streaming Data Snapshots  流数据快照

With an exchange market data subscription, such as Network A (NYSE), Network B(ARCA), or Network C(NASDAQ) for US stocks, it is possible to request a snapshot of the current state of the market once instead of requesting a stream of updates continuously as market values change. By invoking the EClient.reqMktData function passing in true for the snapshot parameter, the client application will receive the currently available market data once before a EWrapper.tickSnapshotEnd event is sent 11 seconds later. Snapshot requests can only be made for the default tick types; no generic ticks can be specified. It is important to note that a snapshot request will only return available data over the 11 second span; in some cases values may not be returned for all tick types.
对于交易所市场数据订阅，例如美国股票的网络 A（纽约证券交易所）、网络 B（ARCA）或网络 C（纳斯达克），可以请求一次当前市场的快照，而不是在市场价值变化时持续请求更新流。通过调用 EClient.reqMktData 函数并将快照参数传入 true，客户端应用程序将在 11 秒后接收到 EWrapper.tickSnapshotEnd 事件之前的一次当前可用市场数据。快照请求只能用于默认报文类型；不能指定通用报文。需要注意的是，快照请求仅会返回 11 秒内的可用数据；在某些情况下，某些报文类型可能不会返回值。
EWrapper.tickSnapshotEnd (

tickerId: int. Request identifier used to track data.
tickerId: int. 请求标识符，用于跟踪数据。
)

When requesting market data snapshots, this market will indicate the snapshot reception is finished. Expected to occur 11 seconds after beginning of request.
在请求市场数据快照时，此市场将表示快照接收已完成。预计在请求开始 11 秒后发生。

def tickSnapshotEnd(self, reqId: int):
  print("TickSnapshotEnd. TickerId:", reqId)

Regulatory Snapshots  监管快照

The fifth argument to reqMktData specifies a regulatory snapshot request to US stocks and options.
reqMktData 的第五个参数指定了对美国股票和期权的监管快照请求。

For stocks, there are individual exchange-specific market data subscriptions necessary to receive streaming quotes. For instance, for NYSE stocks this subscription is known as “Network A”, for ARCA/AMEX stocks it is called “Network B” and for NASDAQ stocks it is “Network C”. Each subscription is added a la carte and has a separate market data fee.
对于股票，需要单独的交易所特定市场数据订阅才能接收实时报价。例如，对于纽约证券交易所的股票，这种订阅被称为“网络 A”，对于 ARCA/AMEX 的股票，它被称为“网络 B”，而对于纳斯达克的股票，它是“网络 C”。每个订阅都是单独添加的，并且有单独的市场数据费用。

Alternatively, there is also a “US Securities Snapshot Bundle” subscription which does not provide streaming data but which allows for real time calculated snapshots of US market NBBO prices. By setting the 5th parameter in the function EClient::reqMktData to True, a regulatory snapshot request can be made from the API. The returned value is a calculation of the current market state based on data from all available exchanges.
或者，还有一个“美国证券快照捆绑包”订阅，它不提供实时数据，但允许实时计算的美国市场 NBBO 价格快照。通过将函数 EClient::reqMktData 中的第五个参数设置为 True，可以从 API 发起监管快照请求。返回值是基于所有可用交易所的数据计算出的当前市场状态。

Important: Each regulatory snapshot made will incur a fee of 0.01 USD to the account. This applies to both live and paper accounts.. If the monthly fee for regulatory snapshots reaches the price of a particular ‘Network’ subscription, the user will automatically be subscribed to that Network subscription for continuous streaming quotes and charged the associated fee for that month. At the end of the month the subscription will be terminated. Each listing exchange will be capped independently and will not be combined across listing exchanges.
重要提示：每次生成的监管快照将向账户收取 0.01 美元的费用。这适用于实盘和模拟账户。如果监管快照的月费达到某个特定“网络”订阅的价格，用户将自动订阅该网络订阅以获取持续行情数据，并支付当月相关费用。该月订阅将在月底终止。每个上市交易所将独立设置限额，不会跨交易所合并。

Requesting regulatory snapshots is subject to pacing limitations:
请求监管快照受速率限制：

    No more than one request per second.
    每秒最多一个请求。

The following table lists the cost and maximum allocation for regulatory snapshot quotes:
下表列出了监管快照行情的成本和最大分配量：
Listed Network Feed  上市网络源	Price per reqSnapshot request
每 reqSnapshot 请求价格	Pro or non-Pro  专业版或非专业版	Max reqSnapshot request  最大 reqSnapshot 请求
NYSE (Network A/CTA)  纽约证券交易所（网络 A/CTA）	0.01 USD  0.01 美元	Pro  专业版	4500
NYSE (Network A/CTA)  纽约证券交易所（网络 A/CTA）	0.01 USD  0.01 美元	Non-Pro  非专业	150
AMEX (Network B/CTA)  美国运通（网络 B/CTA）	0.01 USD  0.01 美元	Pro  专业	2300
AMEX (Network B/CTA)  美国运通（网络 B/CTA）	0.01 USD  0.01 美元	Non-Pro  非专业	150
NASDAQ (Network C/UTP)  纳斯达克（网络 C/UTP）	0.01 USD  0.01 美元	Pro  专业版	2300
NASDAQ (Network C/UTP)  纳斯达克（网络 C/UTP）	0.01 USD  0.01 美元	Non-Pro  非专业版	150
Receive Live Data  接收实时数据

Note: Please be aware that in the event subsequent orders are received with the same price value, but different size values, no new tickPrice value should be returned. Only an updated tickSize will denote that a new order was retrieved with the assumption the last tickPrice value will also correlate with the new size.
注意：请留意，如果后续订单具有相同的价格值但不同的大小值，则不应返回新的 tickPrice 值。只有更新的 tickSize 才会表示已检索到新订单，并假设最后的 tickPrice 值也将与新的 size 相关联。
EWrapper.tickGeneric (  EWrapper.tickGeneric

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。

field: int. The type of tick being received.
字段：int。接收的价差类型。

value: double. Return value corresponding to value. See Available Tick Types for more details.
值：double。与值对应的返回值。更多详情请参阅可用价差类型。
)

Returns generic data back to requester. Used for an array of tick types and is used to represent general evaluations.
向请求者返回通用数据。用于多种价差类型，用于表示一般评估。

def tickGeneric(self, reqId: TickerId, tickType: TickType, value: float):
  print("TickGeneric. TickerId:", reqId, "TickType:", tickType, "Value:", floatMaxString(value))

EWrapper.tickPrice (  EWrapper.tickPrice

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。

tickType: int. The type of the price being received (See Tick ID field in Available Tick Types).
tickType: int. 接收的价格类型（参见可用价格类型中的 Tick ID 字段）。

price: double. The monetary value for the given tick type.
价格：double。给定 tick 类型的货币价值。

attribs: TickAttrib. A TickAttrib object that contains price attributes such as TickAttrib::CanAutoExecute, TickAttrib::PastLimit and TickAttrib::PreOpen.
属性：TickAttrib。一个包含价格属性（如 TickAttrib::CanAutoExecute、TickAttrib::PastLimit 和 TickAttrib::PreOpen）的 TickAttrib 对象。
)

Market data tick price callback. Handles all price related ticks. Every tickPrice callback is followed by a tickSize. A tickPrice value of -1 or 0 followed by a tickSize of 0 indicates there is no data for this field currently available, whereas a tickPrice with a positive tickSize indicates an active quote of 0 (typically for a combo contract).
市场数据价格回调。处理所有与价格相关的数据点。每个 tickPrice 回调都会跟随一个 tickSize。如果 tickPrice 值为-1 或 0，且 tickSize 为 0，则表示当前该字段无数据；而一个具有正 tickSize 的 tickPrice 值则表示活跃报价为 0（通常用于组合合约）。

def tickPrice(self, reqId: TickerId, tickType: TickType, price: float, attrib: TickAttrib):
  print(reqId, tickType, price, attrib)

EWrapper.tickSize (  EWrapper.tickSize

tickerId: int. Request identifier used to track data.
tickerId: int. 用于跟踪数据的请求标识符。

field: int. the type of size being received (i.e. bid size)
字段：int。接收的尺寸类型（例如：报价尺寸）

size: int. the actual size. US stocks have a multiplier of 100.
size: int. 实际大小。美国股票的乘数为 100。
)

Market data tick size callback. Handles all size-related ticks.
市场数据价差大小回调。处理所有与大小相关的价差。

def tickSize(self, reqId: TickerId, tickType: TickType, size: Decimal):
  print("TickSize. TickerId:", reqId, "TickType:", tickType, "Size: ", decimalMaxString(size))

EWrapper.tickString (

tickerId: int. Request identifier used to track data.
tickerId: int. 请求标识符，用于跟踪数据。

field: int. The type of the tick being received
字段：int。接收的价差的类型

value: String. Variable containining message response.
值：String。包含消息响应的变量。
)

Market data callback.  市场数据回调。

Note: Every tickPrice is followed by a tickSize. There are also independent tickSize callbacks anytime the tickSize changes, and so there will be duplicate tickSize messages following a tickPrice.
注意：每个 tickPrice 都会跟随一个 tickSize。当 tickSize 变化时，也会有独立的 tickSize 回调，因此 tickPrice 后面会有重复的 tickSize 消息。

def tickString(self, reqId: TickerId, tickType: TickType, value: str):
  print("TickString. TickerId:", reqId, "Type:", tickType, "Value:", value)

Exchange Component Mapping
交易所组件映射

A market data request is able to return data from multiple exchanges. After a market data request is made for an instrument covered by market data subscriptions, a message will be sent to function IBApi::EWrapper::tickReqParams with information about ‘minTick’, BBO exchange mapping, and available snapshot permissions.
一个市场数据请求能够从多个交易所返回数据。当对受市场数据订阅覆盖的合约发起市场数据请求后，会向 IBApi::EWrapper::tickReqParams 函数发送消息，其中包含关于‘minTick’、BBO 交易所映射以及可用的快照权限的信息。

The exchange mapping identifier bboExchange will be a symbol such as “a6” which can be used to decode the single letter exchange abbreviations returned to the bidExch, askExch, and lastExch fields by invoking the function IBApi::EClient::reqSmartComponents. More information about Component Exchanges.
交易所映射标识符 bboExchange 将是一个符号，例如“a6”，它可用于解码通过调用 IBApi::EClient::reqSmartComponents 函数返回给 bidExch、askExch 和 lastExch 字段的单个字母交易所缩写。更多关于组件交易所的信息。

The minTick returned to tickReqParams indicates the minimum increment in market data values returned to the API. It can differ from the minTick value in the ContractDetails class. For instance, combos will often have a minimum increment of 0.01 for market data and a minTick of 0.05 for order placement.
tickReqParams 返回的 minTick 表示 API 返回的市场数据值的最低增量。它可以与 ContractDetails 类中的 minTick 值不同。例如，组合订单通常市场数据的最低增量是 0.01，而订单放置的 minTick 是 0.05。
EWrapper.tickReqParams (

tickerId: int. Request identifier used to track data.
tickerId: int. 请求标识符，用于跟踪数据。

minTick: Minimum tick for the contract on the exchange.
minTick: 交易所合约的最小价格变动单位。

bboExchange: String. Exchange offering the best bid offer.
bboExchange: 字符串。提供最佳买卖报价的交易所。

snapshotPermissions: Based on the snapshot parameter in EClient.reqMktData.
snapshotPermissions: 基于 EClient.reqMktData 中的快照参数。
)

Displays the ticker with BBO exchange.
显示带 BBO 交易所的行情。

def tickReqParams(self, tickerId:int, minTick:float, bboExchange:str, snapshotPermissions:int):
  print("TickReqParams. TickerId:", tickerId, "MinTick:", floatMaxString(minTick), "BboExchange:", bboExchange, "SnapshotPermissions:", intMaxString(snapshotPermissions))

Re-Routing CFDs  重新路由 CFD。

IB does not provide market data for certain types of instruments, such as stock CFDs and forex CFDs. If a stock CFD or forex CFD is entered into a TWS watchlist, TWS will automatically display market data for the underlying ticker and show a ‘U’ icon next to the instrument name to indicate that the data is for the underlying instrument.
IB 不提供某些类型工具的市场数据，例如股票 CFD 和外汇 CFD。如果股票 CFD 或外汇 CFD 被添加到 TWS 监视列表中，TWS 将自动显示基础股票的市场数据，并在工具名称旁边显示一个“U”图标，以指示该数据是基础工具的。

From the API, when level 1 or level 2 market data is requested for a stock CFD or a forex CFD, a callback is made to the functions EWrapper.rerouteMktDataReq or EWrapper.rerouteMktDepthReq respectively with details about the underlying instrument in IB’s database which does have market data.
从 API 请求股票 CFD 或外汇 CFD 的一级或二级市场数据时，会调用 EWrapper.rerouteMktDataReq 或 EWrapper.rerouteMktDepthReq 函数，分别提供 IB 数据库中具有市场数据的基础工具的详细信息。
EWrapper.rerouteMktDataReq (

reqId: int. Request identifier used to track data.
reqId: int. 用于跟踪数据的请求标识符。

conId: int. Contract identifier of the underlying instrument which has market data.
conId: int. 标的合约的市场数据标识符。

exchange: int. Primary exchange of the underlying.
交易所：int。标的物的主要交易所。
)

Returns conid and exchange for CFD market data request re-route.
返回 CFD 市场数据请求重定向的 conid 和交易所。

def rerouteMktDataReq(self, reqId: int, conId: int, exchange: str):
  print("Re-route market data request. ReqId:", reqId, "ConId:", conId, "Exchange:", exchange)

EWrapper.rerouteMktDepthReq (

reqId: int. Request identifier used to track data.
reqId: int. 请求标识符，用于跟踪数据。

conId: int. Contract identifier of the underlying instrument which has market data.
conId：int。具有市场数据的标的物合约标识符。

exchange: int. Primary exchange of the underlying.
交易所：int。标的物的主要交易所。
)

Returns the conId and exchange for an underlying contract when a request is made for level 2 data for an instrument which does not have data in IB’s database. For example stock CFDs and index CFDs.
当对不包含在 IB 数据库中的工具请求二级数据时，返回标的合约的 conId 和交易所。例如股票 CFD 和指数 CFD。

def rerouteMktDepthReq(self, reqId: int, conId: int, exchange: str):
  print("Re-route market depth request. ReqId:", reqId, "ConId:", conId, "Exchange:", exchange)

Cancel Watchlist Data  取消观察列表数据

EClient.cancelMktData(

tickerId: int. Request identifier used to track data.
tickerId: int. 请求标识符，用于跟踪数据。
)

Cancels a watchlist market data request.
取消一个观察列表市场数据请求。

self.cancelMktData(2001)

Available Tick Types  可用行情类型

EClient.reqMktData will return data to various methods such as EWrapper.tickPrice, EWrapper.tickSize, EWrapper.tickString, etc. The values returned are dependent upon the generic tick requested and the type of data returned. The table below references which tick ID will be returned upon requesting a given generic tick.
EClient.reqMktData 将数据返回给 EWrapper.tickPrice、EWrapper.tickSize、EWrapper.tickString 等多种方法。返回的值取决于请求的通用行情类型和返回的数据类型。下表列出了在请求特定通用行情类型时将返回哪个行情 ID。

*RDD: These tick types are provided only when the user makes a request to EClient.reqMarketDataType(3) prior to their market data request.
*RDD：这些行情类型仅在用户在请求市场数据之前调用 EClient.reqMarketDataType(3) 时提供。

– : These ticks are returned by default and do not have any generic tick requirements.
–：这些行情默认返回，且没有通用行情类型要求。
Tick Name  行情名称	Description  描述	Generic tick required  通用报文所需	Delivery Method  交付方式	Tick Id  报文 ID
Disable Default Market Data
禁用默认市场数据	Disables standard market data stream and allows the TWS & API feed to prioritize other listed generic tick types.
禁用标准市场数据流，并允许 TWS & API 数据源优先处理其他上市通用报文类型。	mdoff	–	–
Bid Size  报价量	Number of contracts or lots offered at the bid price.
在报价价格上提供的合约或手数。	–	IBApi.EWrapper.tickSize	0
Bid Price  买价	Highest priced bid for the contract.
合约的最高买价。	–	IBApi.EWrapper.tickPrice	1
Ask Price	Lowest price offer on the contract.
合约的最低报价。	–	IBApi.EWrapper.tickPrice	2
Ask Size  询价量	Number of contracts or lots offered at the ask price.
在询价价格上提供的合约或手数。	–	IBApi.EWrapper.tickSize	3
Last Price  最新价	Last price at which the contract traded (does not include some trades in RTVolume).
最后成交价（不包括 RTVolume 中的部分交易）。	–	IBApi.EWrapper.tickPrice	4
Last Size  最后成交量	Number of contracts or lots traded at the last price.
最后一次成交的价格下成交的合约或手数。	–	IBApi.EWrapper.tickSize	5
High  高	High price for the day.
当日最高价。	–	IBApi.EWrapper.tickPrice	6
Low  低	Low price for the day.
当日最低价。	–	IBApi.EWrapper.tickPrice	7
Volume  成交量	Trading volume for the day for the selected contract (US Stocks volume is display as Round Lots).
所选合约当日的交易量（美国股票成交量以整手显示）。	–	IBApi.EWrapper.tickSize	8
Close Price  收盘价	“The last available closing price for the previous day. For US Equities we use corporate action processing to get the closing price so the close price is adjusted to reflect forward and reverse splits and cash and stock dividends.”
“上一个交易日的最后可用收盘价。对于美国股票，我们使用公司行为处理来获取收盘价，因此收盘价会调整以反映分割和现金及股票股息。”	–	IBApi.EWrapper.tickPrice	9
Bid Option Computation	Computed Greeks and implied volatility based on the underlying stock price and the option bid price. See Option Greeks
基于标的股票价格和期权买入价计算出的希腊字母和隐含波动率。参见 Option Greeks	–	IBApi.EWrapper.tickOptionComputation	10
Ask Option Computation	Computed Greeks and implied volatility based on the underlying stock price and the option ask price. See Option Greeks
基于标的股票价格和期权卖价计算出的希腊字母值和隐含波动率。参见 Option Greeks	–	IBApi.EWrapper.tickOptionComputation	11
Last Option Computation  最后一次期权计算	Computed Greeks and implied volatility based on the underlying stock price and the option last traded price. See Option Greeks
基于标的股票价格和期权最近成交价计算出的希腊字母参数和隐含波动率。参见 期权希腊字母	–	IBApi.EWrapper.tickOptionComputation	12
Model Option Computation  模型期权计算	Computed Greeks and implied volatility based on the underlying stock price and the option model price. Correspond to greeks shown in TWS. See Option Greeks
基于标的股票价格和期权模型价格计算出的希腊字母和隐含波动率。对应于 TWS 中显示的希腊字母。参见期权希腊字母	–	IBApi.EWrapper.tickOptionComputation	13
Open Tick	Current session’s opening price. Before open will refer to previous day. The official opening price requires a market data subscription to the native exchange of the instrument.
当前交易日的开盘价。开盘前将参考前一日。获取官方开盘价需要订阅该工具所属交易所的市场数据。	–	IBApi.EWrapper.tickPrice	14
Low 13 Weeks  近 13 周最低价	Lowest price for the last 13 weeks. For stocks only.
过去 13 周内的最低价格。仅适用于股票。	165	IBApi.EWrapper.tickPrice	15
High 13 Weeks  13 周最高价	Highest price for the last 13 weeks. For stocks only.
过去 13 周的最高价格。仅适用于股票。	165	IBApi.EWrapper.tickPrice	16
Low 26 Weeks  低 26 周	Lowest price for the last 26 weeks. For stocks only.
过去 26 周最低价。仅适用于股票。	165	IBApi.EWrapper.tickPrice	17
High 26 Weeks  高 26 周	Highest price for the last 26 weeks. For stocks only.
过去 26 周的最高价。仅适用于股票。	165	IBApi.EWrapper.tickPrice	18
Low 52 Weeks	Lowest price for the last 52 weeks. For stocks only.
过去 52 周最低价。仅适用于股票。	165	IBApi.EWrapper.tickPrice	19
High 52 Weeks  52 周最高价	Highest price for the last 52 weeks. For stocks only.
过去 52 周的最高价格。仅适用于股票。	165	IBApi.EWrapper.tickPrice	20
Average Volume  平均成交量	The average daily trading volume over 90 days. Multiplier of 100. For stocks only.
90 天的平均日交易量。乘以 100。仅适用于股票。	165	IBApi.EWrapper.tickSize	21
Open Interest  未平仓合约量	“(Deprecated not currently in use) Total number of options that are not closed.”
“(已弃用，当前未使用）未平仓期权总数。”	–	IBApi.EWrapper.tickSize	22
Option Historical Volatility
期权历史波动率	The 30-day historical volatility (currently for stocks).
30 天历史波动率（目前适用于股票）。	104	IBApi.EWrapper.tickGeneric	23
Option Implied Volatility
期权隐含波动率	“A prediction of how volatile an underlying will be in the future. The IB 30-day volatility is the at-market volatility estimated for a maturity thirty calendar days forward of the current trading day and is based on option prices from two consecutive expiration months.”
“对未来标的波动性的预测。IB 30 天波动率是当前交易日 30 个日历日后的到期日市场波动率的估计值，基于两个连续到期月的期权价格。”	106	IBApi.EWrapper.tickGeneric	24
Option Bid Exchange  期权买方出价交易所	Not Used.  未使用。	–	IBApi.EWrapper.tickString	25
Option Ask Exchange  期权卖方交易所	Not Used.  未使用。	–	IBApi.EWrapper.tickString	26
Option Call Open Interest
期权看涨期权未平仓合约量	Call option open interest.
看涨期权未平仓合约量。	101	IBApi.EWrapper.tickSize	27
Option Put Open Interest  期权卖方未平仓合约量	Put option open interest.
期权卖方未平仓合约量。	101	IBApi.EWrapper.tickSize	28
Option Call Volume  期权买方成交量	Call option volume for the trading day.
当日看涨期权成交量。	100	IBApi.EWrapper.tickSize	29
Option Put Volume  期权看跌成交量	Put option volume for the trading day.
当日看跌期权成交量。	100	IBApi.EWrapper.tickSize	30
Index Future Premium	The number of points that the index is over the cash index.
指数高于现金指数的点数。	162	IBApi.EWrapper.tickGeneric	31
Bid Exchange  买方交易所	“For stock and options identifies the exchange(s) posting the bid price. See Component Exchanges”
“对于股票和期权，标识报价显示的交易所。参见 组成交易所”	–	IBApi.EWrapper.tickString	32
Ask Exchange  询价交易所	“For stock and options identifies the exchange(s) posting the ask price. See Component Exchanges”
“对于股票和期权，标识报价的交易所。参见 组成交易所”	–	IBApi.EWrapper.tickString	33
Auction Volume  拍卖量	The number of shares that would trade if no new orders were received and the auction were held now.
若未收到新订单且当前进行拍卖，将交易多少股。	225	IBApi.EWrapper.tickSize	34
Auction Price  拍卖价格	The price at which the auction would occur if no new orders were received and the auction were held now- the indicative price for the auction. Typically received after Auction imbalance (tick type 36)
若未收到新订单且当前进行拍卖，拍卖将发生的价格——拍卖的指示价格。通常在收到拍卖不平衡（类型 36）后接收。	225	IBApi.EWrapper.tickPrice	35
Auction Imbalance  拍卖失衡	The number of unmatched shares for the next auction; returns how many more shares are on one side of the auction than the other. Typically received after Auction Volume (tick type 34)
下一场拍卖中未匹配的股份数量；返回拍卖某一方比另一方多出的股份数量。通常在收到拍卖量（tick 类型 34）后获得	225	IBApi.EWrapper.tickSize	36
Mark Price  标记价格	“The mark price is the current theoretical calculated value of an instrument. Since it is a calculated value it will typically have many digits of precision.”
“市价是工具的当前理论计算值。由于它是计算值，通常会有很多位精度。”	232	IBApi.EWrapper.tickPrice	37
Bid EFP Computation  报价 EFP 计算	Computed EFP bid price  计算出的 EFP 买价	–	IBApi.EWrapper.tickEFP	38
Ask EFP Computation  EFP 卖价计算	Computed EFP ask price  计算出的 EFP 要价	–	IBApi.EWrapper.tickEFP	39
Last EFP Computation  最后一次 EFP 计算	Computed EFP last price  计算得出的 EFP 最后价格	–	IBApi.EWrapper.tickEFP	40
Open EFP Computation  开启 EFP 计算	Computed EFP open price  计算得出的 EFP 开盘价	–	IBApi.EWrapper.tickEFP	41
High EFP Computation  高 EFP 计算	Computed high EFP traded price for the day
当日计算的高 EFP 交易价格	–	IBApi.EWrapper.tickEFP	42
Low EFP Computation  低 EFP 计算	Computed low EFP traded price for the day
计算当日低 EFP 成交价	–	IBApi.EWrapper.tickEFP	43
Close EFP Computation  关闭 EFP 计算	Computed closing EFP price for previous day
计算前一天的收盘 EFP 价格	–	IBApi.EWrapper.tickEFP	44
Last Timestamp  最后时间戳	Time of the last trade (in UNIX time).
最后交易时间（UNIX 时间）。	–	IBApi.EWrapper.tickString	45
Shortable  可卖空	Describes the level of difficulty with which the contract can be sold short. See Shortable
描述合约可以卖空的难易程度。参见 可卖空	236	IBApi.EWrapper.tickGeneric	46
Fundamental_Ratios	Contains an array of fundamental market data that correlate with the Fundamental Data available in the TWS Watchlist. Data returned in the format of, “KEY1=VALUE1;KEY2=VALUE2;…KEY_N=VALUE_N”.
包含与 TWS 监视列表中可用的基本数据相关联的基本市场数据数组。返回的数据格式为，“KEY1=VALUE1;KEY2=VALUE2;…KEY_N=VALUE_N”。	258	IBApi.EWrapper.tickString	47
RT Volume (Time & Sales)
实时交易量（时间与交易）	“Last trade details (Including both “”Last”” and “”Unreportable Last”” trades). See RT Volume”
“最后交易详情（包括“最后”和“不可报告最后”交易）。参见实时交易量”	233	IBApi.EWrapper.tickString	48
Halted  暂停	Indicates if a contract is halted. See Halted
表示合约是否暂停。参见 暂停	–	IBApi.EWrapper.tickGeneric	49
Bid Yield	Implied yield of the bond if it is purchased at the current bid.
债券若以当前要价购买时的隐含收益率。	–	IBApi.EWrapper.tickPrice	50
Ask Yield  要价收益率	Implied yield of the bond if it is purchased at the current ask.
债券若以当前要价购买时的隐含收益率。	–	IBApi.EWrapper.tickPrice	51
Last Yield  最后收益率	Implied yield of the bond if it is purchased at the last price.
债券在最后价格时购买时的隐含收益率。	–	IBApi.EWrapper.tickPrice	52
Custom Option Computation
自定义期权计算	Greek values are based off a user customized price.
希腊值基于用户自定义价格。	–	IBApi.EWrapper.tickOptionComputation	53
Trade Count  交易数量	Trade count for the day.
当日交易量。	293	IBApi.EWrapper.tickGeneric	54
Trade Rate  交易速率	Trade count per minute.  每分钟交易量。	294	IBApi.EWrapper.tickGeneric	55
Volume Rate  成交量速率	Volume per minute.  每分钟成交量。	295	IBApi.EWrapper.tickGeneric	56
Last RTH Trade  最后交易时段交易	Last Regular Trading Hours traded price.
最后常规交易时段交易价格。	318	IBApi.EWrapper.tickPrice	57
RT Historical Volatility  交易时段历史波动率	30-day real time historical volatility.
30 天实时历史波动率。	411	IBApi.EWrapper.tickGeneric	58
IB Dividends  IB 股息	Contract’s dividends. See IB Dividends.
合约的股息。参见 IB 股息。	456	IBApi.EWrapper.tickString	59
Bond Factor Multiplier  债券因子乘数	The bond factor is a number that indicates the ratio of the current bond principal to the original principal
债券因子是一个表示当前债券本金与原始本金比例的数值	460	IBApi.EWrapper.tickGeneric	60
Regulatory Imbalance  监管失衡	The imbalance that is used to determine which at-the-open or at-the-close orders can be entered following the publishing of the regulatory imbalance.
用于确定在监管失衡发布后哪些开盘价或收盘价订单可以提交的不平衡情况	225	IBApi.EWrapper.tickSize	61
News  新闻	Contract’s news feed.  合约的新闻源。	292	IBApi.EWrapper.tickString	62
Short-Term Volume 3 Minutes
短期成交量 3 分钟	The past three minutes volume. Interpolation may be applied. For stocks only.
过去三分钟成交量。可应用插值。仅限股票。	595	IBApi.EWrapper.tickSize	63
Short-Term Volume 5 Minutes
短期成交量 5 分钟	The past five minutes volume. Interpolation may be applied. For stocks only.
过去五分钟成交量。可应用插值。仅限股票。	595	IBApi.EWrapper.tickSize	64
Short-Term Volume 10 Minutes
短期成交量 10 分钟	The past ten minutes volume. Interpolation may be applied. For stocks only.
过去十分钟成交量。可能应用插值。仅适用于股票。	595	IBApi.EWrapper.tickSize	65
Delayed Bid  延迟报价	Delayed bid price. See Market Data Types.
延迟报价价格。参见 市场数据类型。	*RDD	IBApi.EWrapper.tickPrice	66
Delayed Ask  延迟要价	Delayed ask price. See Market Data Types.
延迟要价。参见市场数据类型。	*RDD	IBApi.EWrapper.tickPrice	67
Delayed Last  延迟最后	Delayed last traded price. See Market Data Types.
延迟最后成交价。参见市场数据类型。	*RDD	IBApi.EWrapper.tickPrice	68
Delayed Bid Size  延迟报价大小	Delayed bid size. See Market Data Types.
延迟报价大小。参见市场数据类型。	*RDD	IBApi.EWrapper.tickSize	69
Delayed Ask Size  延迟卖价量	Delayed ask size. See Market Data Types.
延迟的卖价大小。参见市场数据类型。	*RDD	IBApi.EWrapper.tickSize	70
Delayed Last Size  延迟最后成交量	Delayed last size. See Market Data Types.
延迟最后尺寸。参见市场数据类型。	*RDD	IBApi.EWrapper.tickSize	71
Delayed High Price  延迟高价	Delayed highest price of the day. See Market Data Types.
当日最高价（延迟）。参见市场数据类型。	*RDD	IBApi.EWrapper.tickPrice	72
Delayed Low Price  当日最低价（延迟）	Delayed lowest price of the day. See Market Data Types
每日最低延迟价格。参见市场数据类型	*RDD	IBApi.EWrapper.tickPrice	73
Delayed Volume  延迟成交量	Delayed traded volume of the day. See Market Data Types
当日延迟成交量。参见 市场数据类型	*RDD	IBApi.EWrapper.tickSize	74
Delayed Close  延迟收盘价	The prior day’s closing price.
前一天的收盘价。	*RDD	IBApi.EWrapper.tickPrice	75
Delayed Open  延迟开盘价	Displays the current day’s Open price. The price will return 15 minutes after the Open price is made available.
显示当日开盘价。该价格将在开盘价可用后 15 分钟返回。	*RDD	IBApi.EWrapper.tickPrice	76
RT Trade Volume  RT 交易量	“Last trade details that excludes “”Unreportable Trades””. See RT Trade Volume”
“不包括“无法报告的交易”的最后交易详情”。参见实时交易量”	375	IBApi.EWrapper.tickString	77
Creditman mark price  Creditman 标记价格	Not currently available  目前不可用	–	IBApi.EWrapper.tickPrice	78
Creditman slow mark price
信用管理慢速标记价格	Slower mark price update used in system calculations
系统计算中使用的较慢标记价格更新	619	IBApi.EWrapper.tickPrice	79
Delayed Bid Option  延迟报价选项	Computed greeks based on delayed bid price. See Market Data Types and Option Greeks.
基于延迟报价计算的希腊字母。参见市场数据类型和期权希腊字母。	*RDD	IBApi.EWrapper.tickOptionComputation	80
Delayed Ask Option  延迟卖方期权	Computed greeks based on delayed ask price. See Market Data Types and Option Greeks.
基于延迟卖方价格计算的希腊字母。参见市场数据类型和期权希腊字母。	*RDD	IBApi.EWrapper.tickOptionComputation	81
Delayed Last Option  延迟最后期权	Computed greeks based on delayed last price. See Market Data Types and Option Greeks.
基于延迟最后价格计算希腊字母。参见市场数据类型和期权希腊字母。	*RDD	IBApi.EWrapper.tickOptionComputation	82
Delayed Model Option  延迟模型期权	Computed Greeks and model’s implied volatility based on delayed stock and option prices.
基于延迟的股票和期权价格计算出的希腊字母和模型的隐含波动率。	*RDD	IBApi.EWrapper.tickOptionComputation	83
Last Exchange  最后交易市场	Exchange of last traded price
最后交易价格的市场	–	IBApi.EWrapper.tickString	84
Last Regulatory Time  最后监管时间	Timestamp (in Unix ms time) of last trade returned with regulatory snapshot
最后返回的监管快照中的交易时间戳（Unix 毫秒时间）	–	IBApi.EWrapper.tickString	85
Futures Open Interest  期货未平仓合约量	Total number of outstanding futures contracts. *HSI open interest requested with generic tick 101
未平仓期货合约总数。*使用通用行情 101 请求恒生指数未平仓合约量	588	IBApi.EWrapper.tickSize	86
Average Option Volume  平均期权成交量	Average volume of the corresponding option contracts(TWS Build 970+ is required)
相应期权合约的平均成交量（需要 TWS 970+版本）	105	IBApi.EWrapper.tickSize	87
Delayed Last Timestamp  延迟最后时间戳	Delayed time of the last trade (in UNIX time) (TWS Build 970+ is required)
最后交易延迟时间（UNIX 时间）（需要 TWS 构建版本 970 及以上）	*RDD	IBApi.EWrapper.tickString	88
Shortable Shares  可卖空股数	Number of shares available to short (TWS Build 974+ is required)
可卖空股数（需要 TWS 构建版本 974 及以上）	236	IBApi.EWrapper.tickSize	89
ETF Nav Last	The last price of Net Asset Value (NAV). For ETFs: Calculation is based on prices of ETF’s underlying securities. For NextShares: Value is provided by NASDAQ
净资产价值（NAV）的最新价格。对于 ETF：计算基于 ETF 标的证券的价格。对于 NextShares：价值由纳斯达克提供	577	IBApi.EWrapper.tickPrice	96
ETF Nav Frozen Last	ETF Nav Last for Frozen data	623	IBApi.EWrapper.tickPrice	97
ETF Nav High  ETF 净值高	The high price of ETF’s Net Asset Value (NAV)
ETF 净资产价值（NAV）的高价	614	IBApi.EWrapper.tickPrice	98
ETF Nav Low  ETF 净值低	The low price of ETF’s Net Asset Value (NAV)
ETF 净资产价值（NAV）的低价格	614	IBApi.EWrapper.tickPrice	99
Estimated IPO – Midpoint  预估 IPO – 中点	Midpoint is calculated based on IPO price range
中点是根据 IPO 价格区间计算的	586	IBApi.EWrapper.tickGeneric	101
Final IPO Price  最终 IPO 价格	Final price for IPO  IPO 的最终价格	586	IBApi.EWrapper.tickGeneric	102
Delayed Yield Bid  延迟收益率报价	Delayed implied yield of the bond if it is purchased at the current bid.
如果以当前报价购买，则债券的延迟隐含收益率。	*RDD	IBApi.EWrapper.tickPrice	103
Delayed Yield Ask  延迟收益率要价	Delayed implied yield of the bond if it is purchased at the current ask.
如果以当前要价购买，则债券的延迟隐含收益率。	*RDD	IBApi.EWrapper.tickPrice	104
Halted  暂停

The Halted tick type indicates if a contract has been halted for trading. It can have the following values:
暂停的 tick 类型表示合约是否已暂停交易。它可以有以下值：
Value  值	Description  描述
-1	Halted status not available. Usually returned with frozen data.
暂停状态不可用。通常与冻结数据一同返回。
0	Not halted. This value will only be returned if the contract is in a TWS watchlist.
未暂停。只有当合约在 TWS 观察列表中时才会返回此值。
1	General halt. Trading halt is imposed for purely regulatory reasons with/without volatility halt.
一般暂停。由于纯粹监管原因（无论是否伴随波动暂停）而实施的交易暂停。
2	Volatility halt. Trading halt is imposed by the exchange to protect against extreme volatility.
波动性暂停。交易所为防止极端波动而实施交易暂停。
Shortable  可卖空

The shortable tick is an indicative on the amount of shares which can be sold short for the contract:
可卖空数量是指该合约可以卖空多少股的指示：

Receiving the actual number of shares available to short requires TWS 974+. For detailed information about shortability data (shortable shares, fee rate) available outside of TWS, IB also provides an FTP site. For more information on the FTP site, see knowledge base article 2024
获取实际可卖空股数需要 TWS 974+。关于卖空数据（可卖空股、费率）的详细信息，IB 还提供了一个 FTP 站点，供 TWS 外部使用。有关 FTP 站点的更多信息，请参阅知识库文章 2024。
Range  区间	Description  描述
Value higher than 2.5  值大于 2.5	There are at least 1000 shares available for short selling.
如果可以找到股票，则此合约将可用于卖空。
Value higher than 1.5  值大于 1.5	This contract will be available for short selling if shares can be located.
如果可以找到股票，则此合约将可用于卖空。
1.5 or less  1.5 或更低	Contract is not available for short selling.
合约不可用于卖空。
Volume Data  成交量数据

The API reports the current day’s volume in several ways. They are summarized as follows:
API 以多种方式报告当日成交量。总结如下：

    Volume tick type 8: The ‘native volume’. This includes delayed transactions, busted trades, and combos, but will not update with every tick.
    成交量类型 8：'原生成交量'。这包括延迟交易、失败交易和组合交易，但不会随每个 tick 更新。
    RTVolume: highest number, includes non-reportable trades such as odd lots, average price and derivative trades.
    RTVolume：最高数值，包括非报告交易，如零散订单、平均价格和衍生品交易。
    RTTradeVolume: only includes ‘last’ ticks, similar to number also used in charts/historical data.
    RTTradeVolume：仅包括“最新”报单，与图表/历史数据中使用的数值相同。

RT Volume

The RT Volume tick type corresponds to the TWS’ Time & Sales window and contains the last trade’s price, size and time along with current day’s total traded volume, Volume Weighted Average Price (VWAP) and whether or not the trade was filled by a single market maker.
RT Volume 报单类型对应 TWS 的“交易时间与成交”窗口，包含最后一笔交易的价位、数量和时间，以及当日总成交量、成交量加权平均价格（VWAP）以及是否由单一做市商成交。

There is a setting in TWS which displays tick-by-tick data in the TWS Time & Sales Window. If this setting is checked, it will provide a higher granularity of data than RTVolume.
TWS 中有一个设置，可以在 TWS 时间与销售窗口中显示逐笔数据。如果这个设置被勾选，它将提供比 RTVolume 更高的数据粒度。

Example: 701.28;1;1348075471534;67854;701.46918464;true
示例：701.28;1;1348075471534;67854;701.46918464;true

As volume for US stocks is reported in lots, a volume of 0 reported in RTVolume will typically indicate an odd lot data point (less than 100 shares).
由于美国股票的交易量以股数报出，RTVolume 中报告的 0 交易量通常表示一个零股数据点（少于 100 股）。

It is important to note that while the TWS Time & Sales Window also has information about trade conditions available with data points, this data is not available through the API. So for instance, the ‘unreportable’ trade status displayed with points in the Time & Sales Window is not available through the API, and that trade data will appear in the API just as any other data point. As always, an API application needs to exercise caution in responding to single data points.
需要注意的是，虽然 TWS 时间与销售窗口也提供与数据点相关的交易条件信息，但这些数据无法通过 API 获取。例如，时间与销售窗口中显示的“不可报告”交易状态无法通过 API 获取，而该交易数据将像任何其他数据点一样出现在 API 中。一如既往，API 应用程序在响应单个数据点时需要谨慎。

Note: Please be aware that RT Volume is not supported with Cryptocurrencies.
注意：请注意，加密货币不支持实时交易量（RT Volume）。

RT Trade Volume  RT 交易量

The RT Trade Volume is similar to RT Volume, but designed to avoid relaying back “Unreportable Trades” shown in TWS Time&Sales via the API. RT Trade Volume will not contain average price or derivative trades which are included in RTVolume.
实时交易量与实时成交量相似，但设计上避免通过 API 回传 TWS 交易时间与价格中显示的“不可报告交易”。实时交易量不包含平均价格或衍生品交易，这些内容在实时成交量中包含。
IB Dividends  IB 股息

This tick type provides four different comma-separated elements:
这种行情类型提供四个用逗号分隔的元素：

    The sum of dividends for the past 12 months (0.83 in the example below).
    过去 12 个月的股息总额（示例中为 0.83）。
    The sum of dividends for the next 12 months (0.92 from the example below).
    未来 12 个月的股息总额（示例中为 0.92）。
    The next dividend date (20130219 in the example below).
    下一个股息日期（示例中为 20130219）。
    The next single dividend amount (0.23 from the example below).
    下一个单股息金额（示例中为 0.23）。

Example: 0.83,0.92,20130219,0.23
示例：0.83,0.92,20130219,0.23

To receive dividend information it is sometimes necessary to direct-route rather than smart-route market data requests.
要接收股息信息，有时需要直接路由而不是智能路由市场数据请求。
Tick By Tick Data  逐笔数据

In TWS, tick-by-tick data is available in the Time & Sales Window.
在 TWS 中，逐笔数据可以在时间与成交窗口中获取。

The maximum number of simultaneous tick-by-tick subscriptions allowed for a user is 5% of the user’s total market data lines. See Specialized Market Data Lines for more information.
用户允许同时订阅的最大逐笔数据数量为用户总市场数据线路的 5%。有关更多信息，请参阅专业市场数据线路。

    Real time tick-by-tick data is currently not available for options. Historical tick-by-tick data is available.
    目前期权没有实时逐笔数据。可以提供历史逐笔数据。

    The tick type field is case sensitive – it must be BidAsk, Last, AllLast, MidPoint. AllLast has additional trade types such as combos, derivatives, and average price trades which are not included in Last.
    报價类型字段区分大小写——必须为 BidAsk、Last、AllLast 或 MidPoint。AllLast 包含 Last 中未包含的额外交易类型，如组合交易、衍生品和平均价格交易。
    Tick-by-tick data for options is currently only available historically and not in real time.
    期权逐笔数据目前仅提供历史数据，不提供实时数据。
    Tick-by-tick data for indices is only provided for indices which are on CME.
    指数逐笔数据仅提供 CME 上市指数的数据。
    Tick-by-tick data is not available for combos.
    组合交易不提供逐笔数据。
    No more than 1 tick-by-tick request can be made for the same instrument within 15 seconds.
    同一 instruments 在 15 秒内最多只能发送 1 次逐笔 tick-by-tick 请求。
    Time & Sales data requires a Level 1, Top Of Book market data subscription. This would be the same subscription as EClient.reqMktData() or EClient.reqHistoricalData().
    时间与交易数据需要 Level 1、顶部的市场数据订阅。这将是与 EClient.reqMktData() 或 EClient.reqHistoricalData() 相同的订阅。

Request Tick By Tick Data
请求逐笔 tick-by-tick 数据

EClient.reqTickByTickData (
EClient.reqTickByTickData ()

reqId: int. unique identifier of the request.
reqId: int. 请求的唯一标识符。

contract: Contract. the contract for which tick-by-tick data is requested.
contract: Contract. 请求逐笔数据所对应的合约。

tickType: String. tick-by-tick data type: “Last”, “AllLast”, “BidAsk” or “MidPoint”.
tickType: String. 逐笔数据类型："Last", "AllLast", "BidAsk" 或 "MidPoint"。

numberOfTicks: int. If a non-zero value is entered, then historical tick data is first returned via one of the  Historical Time and Sales Ewrapper Methods  respectively. (Max number of historical Ticks is 1000)
numberOfTicks: int. 如果输入非零值，则首先通过相应的 Historical Time and Sales Ewrapper 方法返回历史逐笔数据。（历史逐笔数据的最大数量为 1000）

ignoreSize: bool. Omit updates that reflect only changes in size, and not price. Applicable to Bid_Ask data requests.
ignoreSize: bool. 忽略仅反映尺寸变化而不反映价格更新的信息。适用于 Bid_Ask 数据请求。
)

Requests tick by tick or Time & Sales data.
请求逐笔数据或交易时间与销售数据。

Note:  注意：

The maximum number of simultaneous tick-by-tick subscriptions allowed for a user is 5% of the user’s total market data lines. See Specialized Market Data Lines for more information.
用户允许的最大同时逐笔订阅数是其总市场数据线的 5%。有关更多信息，请参阅专业市场数据线。

self.reqTickByTickData(19001, contract, "Last", 0, True)

Receive Tick By Tick Data
接收逐笔数据

EWrapper.tickByTickAllLast (
EWrapper.tickByTickAllLast

reqId: int. unique identifier of the request.
reqId: int. 请求的唯一标识符。

tickType: int. 0: “Last” or 1: “AllLast”.
tickType: int. 0: “最后”或 1: “所有最后”。

time: long. tick-by-tick real-time tick timestamp.
时间：长整型。逐 tick 实时 tick 时间戳。

price: double. tick-by-tick real-time tick last price.
价格：double。逐 tick 实时最后价格。

size: decimal. tick-by-tick real-time tick last size.
数量：decimal。逐 tick 实时最后数量。

tickAttribLast: TickAttribLast. tick-by-tick real-time last tick attribs (bit 0 – past limit, bit 1 – unreported).
最后 tick 属性：TickAttribLast。逐 tick 实时最后 tick 属性（位 0 – 过去限价，位 1 – 未报告）。

exchange: String. tick-by-tick real-time tick exchange.
交易所：String。逐 tick 实时 tick 交易所。

specialConditions: String. tick-by-tick real-time tick special conditions. Conditions under which the operation took place (Refer to Trade Conditions Page)
)

Returns “Last” or “AllLast” tick-by-tick real-time tick.
返回“Last”或“AllLast”逐 tick 实时 tick。

def tickByTickAllLast(self, reqId: int, tickType: int, time: int, price: float, size: Decimal, tickAtrribLast: TickAttribLast, exchange: str,specialConditions: str):
  print(" ReqId:", reqId, "Time:", time, "Price:", floatMaxString(price), "Size:", size, "Exch:" , exchange, "Spec Cond:", specialConditions, "PastLimit:", tickAtrribLast.pastLimit, "Unreported:", tickAtrribLast.unreported)

EWrapper.tickByTickBidAsk (
EWrapper.tickByTickBidAsk

reqId: int. unique identifier of the request.
reqId: int. 请求的唯一标识符。

time: long. timestamp of the tick.
时间：长整型。该 Tick 的时间戳。

bidPrice: double. bid price of the tick.
bidPrice: double. 该报价的买价。

askPrice: double. ask price of the tick.
askPrice: double. 该报价的卖价。

bidSize: decimal. bid size of the tick.
bidSize: decimal. 该价位上的买单大小。

askSize: decimal. ask size of the tick.
askSize: decimal. 询问价位的刻度大小。

tickAttribBidAsk: TickAttribBidAsk. tick-by-tick real-time bid/ask tick attribs (bit 0 – bid past low, bit 1 – ask past high).
tickAttribBidAsk: TickAttribBidAsk. 每个刻度的实时买/卖价格属性（位 0 表示买价低于最低价，位 1 表示卖价高于最高价）。
)

Returns “BidAsk” tick-by-tick real-time tick.
返回“BidAsk”类型的每个刻度的实时价格。

 def tickByTickBidAsk(self, reqId: int, time: int, bidPrice: float, askPrice: float, bidSize: Decimal, askSize: Decimal, tickAttribBidAsk: TickAttribBidAsk):
  print("BidAsk. ReqId:", reqId, "Time:", time, "BidPrice:", floatMaxString(bidPrice), "AskPrice:", floatMaxString(askPrice), "BidSize:", decimalMaxString(bidSize), "AskSize:", decimalMaxString(askSize), "BidPastLow:", tickAttribBidAsk.bidPastLow, "AskPastHigh:", tickAttribBidAsk.askPastHigh)

EWrapper.tickByTickMidPoint (

reqId: int. Request identifier used to track data.
reqId: int. 用于跟踪数据的请求标识符。

time: long. Timestamp of the tick.
时间：长整型。每个 tick 的时间戳。

midPoint: double. Mid point value of the tick.
中点：双精度浮点型。每个 tick 的中点值。
)

Returns “MidPoint” tick-by-tick real-time tick.
返回“中点”每个 tick 的实时 tick。

def tickByTickMidPoint(self, reqId: int, time: int, midPoint: float):
  print("Midpoint. ReqId:", reqId, "Time:", time, "MidPoint:", floatMaxString(midPoint))

Cancel Tick By Tick Data
取消逐笔数据

EClient.cancelTickByTickData (

requestId: int. Request identifier used to track data.
requestId: int. 用于跟踪数据的请求标识符。
)

Cancels specified tick-by-tick data.
取消指定的逐点数据。

self.cancelTickByTickData(19001)

Halted and Unhalted ticks
暂停和未暂停的报单

The Tick-By-Tick attribute has been introduced. The tick attribute pastLimit is also returned with historical Tick-By-Tick responses.
引入了报单-报单属性。历史报单-报单响应中也返回了报单属性 pastLimit。

    If tick has zero price, zero size and pastLimit flag is set – this is “Halted” tick.
    如果报单价格为零、数量为零且 pastLimit 标志被设置——这是“暂停”报单。
    If tick has zero price, zero size and followed immediately after “Halted” tick – this is “Unhalted” tick.
    如果报单价格为零、数量为零且紧随“暂停”报单之后——这是“未暂停”报单。

Market Scanner  市场扫描器

Some scans in the TWS Advanced Market Scanner can be accessed via the TWS API through the EClient.reqScannerSubscription.
TWS 高级市场扫描器中的某些扫描功能可以通过 TWS API 通过 EClient.reqScannerSubscription 访问。

Results are delivered via EWrapper.scannerData and the EWrapper.scannerDataEnd marker will indicate when all results have been delivered. The returned results to scannerData simply consist of a list of contracts. There are no market data fields (bid, ask, last, volume, …) returned from the scanner, and so if these are desired they have to be requested separately with the reqMktData function. Since the scanner results do not include any market data fields, it is not necessary to have market data subscriptions to use the API scanner. However to use filters, market data subscriptions are generally required.
结果通过 EWrapper.scannerData 传递，而 EWrapper.scannerDataEnd 标记将指示所有结果已传递完毕。返回到 scannerData 的结果仅由合约列表组成。扫描器不返回任何市场数据字段（买价、卖价、最新价、成交量等），因此如果需要这些数据，必须使用 reqMktData 函数单独请求。由于扫描器结果不包含任何市场数据字段，因此使用 API 扫描器不需要市场数据订阅。但是，要使用过滤器，通常需要市场数据订阅。

Since the EClient.reqScannerSubscription request keeps a subscription open you will keep receiving periodic updates until the request is cancelled via EClient.cancelScannerSubscription :
由于 EClient.reqScannerSubscription 请求保持订阅状态，您将不断收到周期性更新，直到通过 EClient.cancelScannerSubscription 取消请求为止。

Scans are limited to a maximum result of 50 results per scan code, and only 10 API scans can be active at a time.
扫描结果最多限制为每个扫描码 50 个结果，同时只能有 10 个 API 扫描处于活动状态。

scannerSubscriptionFilterOptions has been added to the API to allow for generic filters. This field is entered as a list of TagValues which have a tag followed by its value, e.g. TagValue(“usdMarketCapAbove”, “10000”) indicates a market cap above 10000 USD. Available filters can be found using the EClient.reqScannerParameters function.
API 中已添加 scannerSubscriptionFilterOptions 字段，以支持通用过滤器。该字段以 TagValues 列表的形式输入，每个 TagValue 包含一个标签及其值，例如 TagValue("usdMarketCapAbove", "10000")表示市值超过 10000 美元。可用过滤器可通过 EClient.reqScannerParameters 函数获取。

A string containing all available XML-formatted parameters will then be returned via EWrapper.scannerParameters.
随后，EWrapper.scannerParameters 将返回包含所有可用 XML 格式参数的字符串。

Important: remember the TWS API is just an interface to the TWS. If you are having problems defining a scanner, always make sure you can create a similar scanner using the TWS’ Advanced Market Scanner.
重要提示：TWS API 仅是 TWS 的接口。如果您在定义扫描器时遇到问题，请始终确保您可以使用 TWS 的先进市场扫描器创建类似的扫描器。
Market Scanner Parameters
市场扫描参数

A string containing all available XML-formatted parameters will then be returned via EWrapper.scannerParameters.
然后，将通过 EWrapper.scannerParameters 返回一个包含所有可用 XML 格式参数的字符串。
Request Market Scanner Parameters
请求市场扫描器参数

EClient.reqScannerParameters ()

Requests an XML list of scanner parameters valid in TWS.
请求 TWS 中有效的扫描器参数的 XML 列表。

self.reqScannerParameters()

Receive Market Scanner Parameters
接收市场扫描器参数

EWrapper.scannerParameters (

xml: String. The xml-formatted string with the available parameters.
xml: String. 带有可用参数的 XML 格式字符串。
)

Provides the xml-formatted parameters available from TWS market scanners (not all available in API).
提供 TWS 市场扫描器可用的 XML 格式参数（API 中并非所有参数都可用）。

def scannerParameters(self, xml: str):
  open('log/scanner.xml', 'w').write(xml)
  print("ScannerParameters received.")

Market Scanner Subscription
市场扫描器订阅

All values used for the ScannerSubscription object are pulled from EClient.scannerParams response. The XML tree will relay a tree containing a corresponding code to each ScannerSubscription field as documented below.
用于 ScannerSubscription 对象的所有值均从 EClient.scannerParams 响应中获取。XML 树将传递一个包含每个 ScannerSubscription 字段相应代码的树，如下文所述。

instrument: <ScanParameterResponse> <InstrumentList> <Instrument> <type>

Location Code: <ScanParameterResponse> <LocationTree> <Location> <LocationTree> <Location> <locationCode>

Scan Code: <ScanParameterResponse> <ScanTypeList> <ScanType> <scanCode>

扫描码：<ScanParameterResponse> <ScanTypeList> <ScanType> <scanCode>

Subscription Options should be an empty array of TagValues.
订阅选项应为空的 TagValues 数组。

Filter Options: <ScanParameterResponse> <FilterList> <RangeFilter> <AbstractField> <code>

筛选选项：<ScanParameterResponse> <FilterList> <RangeFilter> <AbstractField> <code>

ScannerSubscription()

Instrument: String. Instrument Type to use.
乐器：字符串。用于扫描的乐器类型。

Location Code: String. Country or region for scanner to search.
位置代码：字符串。扫描器搜索的国家或地区。

Scan Code: String. Value for scanner to sort by.
扫描代码：字符串。扫描器排序的值。

Subscription Options: Array of TagValues. For internal use only.
订阅选项：TagValues 数组。仅供内部使用。

Filter Options: Array of TagValues. Contains an array of TagValue objects which filters the scanner subscription.
筛选选项：TagValues 数组。包含用于筛选扫描订阅的 TagValue 对象数组。
Request Market Scanner Subscription
请求市场扫描订阅

EClient.reqScannerSubscription (

reqId: int. Request identifier used for tracking data.
reqId: int. 用于跟踪数据的请求标识符。

subscription: ScannerSubscription. Object containing details on what values should be used to construct and sort the list.
订阅：ScannerSubscription。包含有关用于构建和排序列表的值的详细信息的对象。

scannerSubscriptionOptions: List. Internal use only.
scannerSubscriptionOptions: 列表。仅供内部使用。

scannerSubscriptionFilterOptions: List. List of values used to filter the results of the scanner subscription. May result in an empty scanner response from over-filtering.
scannerSubscriptionFilterOptions: 列表。用于过滤扫描订阅结果的值列表。可能导致过度过滤而返回空的扫描响应。
)

Starts a subscription to market scan results based on the provided parameters.
根据提供的参数开始订阅市场扫描结果。

self.reqScannerSubscription(7002, scannerSubscription, [], filterTagvalues)

Receive Market Scanner Subscription
接收市场扫描器订阅

EWrapper.scannerData (

reqid: int. Request identifier used to track data.
reqid: int. 用于跟踪数据的请求标识符。

rank: int. The ranking position of the contract in the scanner sort.
rank: int. 合约在扫描排序中的排名位置。

contractDetails: ContractDetails. Contract object of the resulting object.
contractDetails: ContractDetails. 结果对象中的合约对象。

distance: String. Internal use only.
距离：String。仅供内部使用。

benchmark: String. Internal use only.
基准：字符串。仅供内部使用。

projection: String. Internal use only.
预测：字符串。仅供内部使用。

legStr: String. Describes the combo legs when the scanner is returning EFP
legStr：字符串。描述扫描器返回 EFP 时的组合腿
)

Provides the data resulting from the market scanner request.
提供市场扫描器请求产生的数据。

def scannerData(self, reqId: int, rank: int, contractDetails: ContractDetails, distance: str, benchmark: str, projection: str, legsStr: str):
  print("ScannerData. ReqId:", reqId, ScanData(contractDetails.contract, rank, distance, benchmark, projection, legsStr))

Cancel Market Scanner Subscription
取消市场扫描器订阅

EClient.cancelScannerSubscription (

tickerId: int. Request identifier used to track data.
tickerId: int. 请求标识符，用于跟踪数据。
)

Cancels the specified scanner subscription using the tickerId.
使用 tickerId 取消指定的扫描订阅。

self.cancelScannerSubscription(7003)

News  新闻

API news requires news subscriptions that are specific to the API; most news services in TWS are not also available in the API. There are three API news services enabled in accounts by default and available from the API. They are:
API 新闻需要专门的 API 新闻订阅；TWS 中的大多数新闻服务在 API 中也不可用。默认情况下，账户中有三个 API 新闻服务可用，可通过 API 访问。它们是：

    Briefing.com General Market Columns (BRFG)
    Briefing.com 通用市场专栏（BRFG）
    Briefing.com Analyst Actions (BRFUPDN)
    Briefing.com 分析师行动（BRFUPDN）
    Dow Jones Newsletters (DJNL)
    道琼斯新闻简报（DJNL）

There are also four additional news services available with all TWS versions which require API-specific subscriptions to first be made in Account Management. They have different data fees than the subscription for the same news in TWS-only. As with all subscriptions, they only apply to the specific TWS username under which they were made:
所有 TWS 版本还提供四种额外的新闻服务，这些服务需要先在账户管理中进行 API 特定的订阅。它们与仅在 TWS 中订阅相同新闻的数据费用不同。和所有订阅一样，它们仅适用于创建订阅时使用的特定 TWS 用户名：

    Briefing Trader (BRF)
    Benzinga Pro (BZ)
    Fly on the Wall (FLY)

The API functions which handle news are able to query available news provides, subscribe to news in real time to receive headlines as they are released, request specific news articles, and return a historical list of news stories that are cached in the system.
处理新闻的 API 功能能够查询可用的新闻提供者，实时订阅新闻以接收发布的头条，请求特定的新闻文章，并返回系统中缓存的新闻故事的历史列表。
News Providers  新闻提供者

Adding or removing API news subscriptions from an account is accomplished through Account Management. From the API, currently subscribed news sources can be retrieved using the function IBApi::EClient::reqNewsProviders. A list of available subscribed news sources is returned to the function IBApi::EWrapper::newsProviders
在账户中添加或删除 API 新闻订阅是通过账户管理完成的。从 API 中，可以使用函数 IBApi::EClient::reqNewsProviders 检索当前订阅的新闻源。可用的订阅新闻源列表将返回给函数 IBApi::EWrapper::newsProviders。
Request News Providers  请求新闻提供者

EClient.reqNewsProviders()

Requests news providers which the user has subscribed to.
请求用户已订阅的新闻提供者。

self.reqNewsProviders()

Receive News Providers  接收新闻提供者

EWrapper.newsProviders (

newsProviders: NewsProviders[]. Unique array containing all available news sources.
newsProviders: NewsProviders[]. 包含所有可用新闻源的唯一数组。
)

Returns array of subscribed API news providers for this user
返回此用户订阅的 API 新闻提供者数组

def newsProviders(self, newsProviders: ListOfNewsProviders):
  print("NewsProviders: ", newsProviders)

Live News Headlines  实时新闻头条

Important: in order to obtain news feeds via the TWS API you need to acquire the relevant API-specific subscriptions via your Account Management.
重要提示：要通过 TWS API 获取新闻源，您需要通过账户管理获取相应的 API 特定订阅。

News articles provided through the API may not correspond to what is available directly through the Trader Workstation. Off-platform distribution of data is at the discretion of the news source provider, not by Interactive Brokers.
通过 API 提供的新闻文章可能与直接通过交易工作站可用的内容不一致。数据离平台分发由新闻源提供者决定，而非由 Interactive Brokers 决定。

When invoking IBApi.EClient.reqMktData, for a specific IBApi.Contract you will follow the same format convention as any other basic contracts. The News Source is identified by the genericTickList argument.
在调用 IBApi.EClient.reqMktData 时，对于特定的 IBApi.Contract，您将遵循与其他基本合约相同的格式约定。新闻源由 genericTickList 参数标识。

Note: The error message “invalid tick type” will be returned if the username has not added the appropriate API news subscription.
注意：如果用户未添加适当的 API 新闻订阅，将返回错误消息“无效的 tick 类型”。

Note: For Briefing Trader live head lines via the API is only offered on a case-by-case basis directly from Briefing.com offers Briefing Trader subscribers access to the subscription live head lines via the API. For more information and to submit an API entitlement application, please contact Briefing.com directly at dbeasley@briefing.com.
注意：通过 API 获取 Briefing Trader 实时头条信息仅提供个案服务，由 Briefing.com 直接为 Briefing Trader 订阅者提供 API 访问订阅实时头条信息的权限。如需更多信息或提交 API 权限申请，请直接联系 Briefing.com，邮箱地址为 dbeasley@briefing.com。
Request Contract Specific News
请求合约特定新闻

EClient.reqMktData (  EClient.reqMktData

reqId: int. Request identifier for tracking data.
reqId: int. 请求标识符，用于跟踪数据。

contract: Contract. Contract object used for specifying an instrument.
contract: Contract. 用于指定金融工具的合约对象。

genericTickList: String. Comma separated ids of the available generic ticks.
genericTickList: String. 可用通用 tick 的 id，以逗号分隔。

snapshot: bool. Always set to false for news data.
snapshot: bool. 对于新闻数据，始终设置为 false。

regulatorySnapshot: bool. Always set to false for news data.
regulatorySnapshot: bool. 总是为新闻数据设置为 false。

mktDataOptions: List<TagValue>. Internal use only.
mktDataOptions: List<TagValue>. 仅限内部使用。
)

Used to request market data typically, but can also be used to retrieve news. “mdoff” can be specified to disable standard market data while retrieving news.
通常用于请求市场数据，但也可以用于获取新闻。"mdoff"可以指定以禁用标准市场数据，同时获取新闻。
For news sources, genericTick 292 needs to be specified followed by a colon and the news provider’s code.
对于新闻源，需要指定通用 Tick 292，后面跟一个冒号和新闻提供者的代码。

self.reqMktData(reqId, contract, "mdoff,292:BRFG", False, False, [])

Request BroadTape News  请求 BroadTape 新闻

BroadTape News Contracts  BroadTape 新闻合约

For BroadTape News you specify the contract for the specific news source. This is uniquely identified by the symbol and exchange. The symbol of an instrument can easily be obtained via the EClientSocket.reqContractDetails request.
对于 BroadTape 新闻，您需要指定特定新闻来源的合约。这通过符号和交易所唯一标识。可以通过 EClientSocket.reqContractDetails 请求轻松获取一个工具的符号。

The symbol is typically the provider code, a colon, then the news provider codes appended with “_ALL”
符号通常是提供者代码，后跟一个冒号，然后是新闻提供者代码，并附加“_ALL”

Example news contract  示例新闻合约
contract = Contract()
contract.symbol  = "BRF:BRF_ALL"
contract.secType = "NEWS"
contract.exchange = "BRF"

EClient.reqMktData (  EClient.reqMktData

reqId: int. Request identifier for tracking data.
reqId: int. 请求标识符，用于跟踪数据。

contract: Contract. Contract object used for specifying an instrument.
contract: Contract. 用于指定金融工具的合约对象。

genericTickList: String. Comma separated ids of the available generic ticks.
genericTickList: String. 可用通用报价的逗号分隔的 id。

snapshot: bool. Always set to false for news data.

regulatorySnapshot: bool. Always set to false for news data.

mktDataOptions: List<TagValue>. Internal use only.
mktDataOptions: List<TagValue>. 仅限内部使用。
)

Used to request market data typically, but can also be used to retrieve news. “mdoff” can be specified to disable standard market data while retrieving news.
用于请求市场数据，但也可以用于获取新闻。"mdoff"可以指定以在获取新闻时禁用标准市场数据。

For news sources, genericTick 292 needs to be specified.
对于新闻来源，需要指定 genericTick 292。

self.reqMktData(reqId, contract, "mdoff,292", False, False, [])

Receive Live News Headlines
接收实时新闻标题

EWrapper.tickNews (

tickerId: int. Request identifier used to track data.
tickerId: int. 请求标识符，用于跟踪数据。

timeStamp: int. Epoch time of the article’s published time.
timeStamp: int. 文章发布时间的 Epoch 时间。

providerCode: String. News provider code based on requested data.
providerCode: String. 基于请求的数据的新闻提供者代码。

articleId: String. Identifier used to track the particular article. See News Article for more.
articleId: String. 用于跟踪特定文章的标识符。参见新闻文章以获取更多信息。

headline: String. Headline of the provided news article.
headline: String. 提供的新闻文章标题。

extraData: String. Returns any additional data available about the article.
extraData: String. 返回关于文章的任何附加数据。
)

Returns news headlines for requested contracts.
返回请求合约的新闻标题。

def tickNews(self, tickerId: int, timeStamp: int, providerCode: str, articleId: str, headline: str, extraData: str):
  print("TickNews. TickerId:", tickerId, "TimeStamp:", timeStamp, "ProviderCode:", providerCode, "ArticleId:", articleId, "Headline:", headline, "ExtraData:", extraData)

Historical News Headlines
历史新闻标题

With the appropriate API news subscription, historical news headlines can be requested from the API using the function EClient::reqHistoricalNews. The resulting headlines are returned to EWrapper::historicalNews.
通过适当的 API 新闻订阅，可以使用 EClient::reqHistoricalNews 函数从 API 请求历史新闻标题。结果标题会返回到 EWrapper::historicalNews。
Requesting Historical News
请求历史新闻

EClient.reqHistoricalNews (
EClient.reqHistoricalNews

requestId: int. Request identifier used to track data.
requestId: int. 请求标识符，用于跟踪数据。

conId: int. Contract id of ticker. See Contract Details for how to retrieve conId.
conId: int. 股票代码的合约 ID。参见合约详情了解如何获取 conId。

providerCodes: String. A ‘+’-separated list of provider codes.
providerCodes: String. 由‘+’分隔的提供者代码列表。

startDateTime: String. Marks the (exclusive) start of the date range. The format is yyyy-MM-dd HH:mm:ss.
startDateTime: String. 标记日期范围的（排他性）开始。格式为 yyyy-MM-dd HH:mm:ss。

endDateTime: String. Marks the (inclusive) end of the date range. The format is yyyy-MM-dd HH:mm:ss.
endDateTime: String. 标记日期范围的（包含性）结束。格式为 yyyy-MM-dd HH:mm:ss。

totalResults: int. The maximum number of headlines to fetch (1 – 300)
totalResults: int. 要获取的最大新闻标题数量（1 – 300）。

historicalNewsOptions: Null. Reserved for internal use. Should be defined as null.
historicalNewsOptions: 空值。保留用于内部使用。应定义为 null。
)

Requests historical news headlines.
请求历史新闻标题。

self.reqHistoricalNews(reqId, 8314, "BRFG", "", "", 10, [])

Receive Historical News  接收历史新闻

EWrapper.historicalNews (

requestId: int. Request identifier used to track data.
requestId: int. 请求标识符，用于跟踪数据。

time: int. Epoch time of the article’s published time.
时间：int。文章发布时间的 Unix 时间戳。

providerCode: String. News provider code based on requested data.
providerCode: String. 基于请求的数据的新闻提供者代码。

articleId: String. Identifier used to track the particular article. See News Article for more.
文章 ID：String。用于跟踪特定文章的标识符。参见新闻文章以获取更多信息。

headline: String. Headline of the provided news article.
标题：String。所提供新闻文章的标题。
)

Returns news headlines for requested contracts.
返回请求合约的新闻标题。

def historicalNews(self, requestId: int, time: int, providerCode: str, articleId: str, headline: str):
  print("historicalNews. RequestId:", requestId, "Time:", time, "ProviderCode:", providerCode, "ArticleId:", articleId, "Headline:", headline)

EWrapper.historicalNewsEnd (

requestId: int. Request identifier used to track data.
requestId: int. 请求标识符，用于跟踪数据。

hasMore: bool. Returns whether there is more data (true) or not (false).
hasMore: bool. 返回是否有更多数据（true）或没有（false）。
)

Returns news headlines end marker
返回新闻标题结束标记

def historicalDataEnd(self, reqId: int, hasMore: bool):
    print("historicalDataEnd. ReqId:", reqId, "Has More:", hasMore)

News Articles  新闻文章

After requesting news headlines using one of the above functions, the body of a news article can be requested with the article ID returned by invoking the function IBApi::EClient::reqNewsArticle. The body of the news article is returned to the function IBApi::EWrapper::newsArticle.
使用上述任一函数请求新闻标题后，可通过调用 IBApi::EClient::reqNewsArticle 函数，使用该函数返回的文章 ID 请求新闻文章正文。新闻文章正文将返回到 IBApi::EWrapper::newsArticle 函数。
Request News Articles  请求新闻文章

EClient.reqNewsArticle (  EClient.reqNewsArticle

requestId: int. id of the request.
requestId: int. 请求的 id。

providerCode: String. Short code indicating news provider, e.g. FLY.
providerCode: String. 表示新闻提供者的简短代码，例如 FLY。

articleId: String. Id of the specific article.
articleId: String. 具体文章的 id。

newsArticleOptions: List. Reserved for internal use. Should be defined as null.
newsArticleOptions: List. 保留供内部使用。应定义为 null。
)

Requests news article body given articleId.
根据文章 ID 请求新闻文章正文。

self.reqNewsArticle(10002,"BRFG", "BRFG$04fb9da2", [])

Receive News Articles  接收新闻文章

EWrapper.newsArticle (

requestId: int. Request identifier used to track data.
requestId: int. 用于跟踪数据的请求标识符。

articleType: int. The type of news article (0 – plain text or html, 1 – binary data / pdf).
articleType: int. 新闻文章的类型（0 – 纯文本或 html，1 – 二进制数据 / pdf）。

articleText: String. The body of article (if articleType == 1, the binary data is encoded using the Base64 scheme).
articleText: String. 文章正文（如果 articleType == 1，二进制数据使用 Base64 方案编码）。
)

Called when receiving a News Article in response to reqNewsArticle().
当接收到对 reqNewsArticle() 的响应时调用。

def newsArticle(self, requestId: int, articleType: int, articleText: str):
  print("requestId: ", requestId, "articleType: ", articleType, "articleText: ", articleText)

Next Valid ID  下一个有效 ID

The nextValidId event provides the next valid identifier needed to place an order. It is necessary to use an order ID with new orders which is greater than all previous order IDs used to place an order. While requests such as EClient.reqMktData will not increment the minimum request ID value, more than one market data request cannot use the same request ID at the same time.
nextValidId 事件提供下单所需的下一个有效标识符。新订单必须使用一个比之前所有用于下单的订单 ID 都大的订单 ID。虽然像 EClient.reqMktData 这样的请求不会增加最小请求 ID 值，但多个市场数据请求不能同时使用相同的请求 ID。

The nextValidId value may be queried on each request. However, it is often recommended to make a request once at the beginning of the session, and then locally increment the value for each request.
nextValidId 值可以在每个请求中查询。但是，通常建议在会话开始时请求一次，然后对每个请求本地递增值。
Request Next Valid ID  请求下一个有效 ID

EClient.reqIds (

numIds: int. This parameter will not affect the value returned to nextValidId but is required.
numIds: int. 此参数不会影响返回给 nextValidId 的值，但需要此参数。
)

Requests the next valid order ID at the current moment be returned to the EWrapper.nextValidId function.
请求在当前时刻返回有效的订单 ID 给 EWrapper.nextValidId 函数。

self.reqIds(-1)

Receive Next Valid ID  接收下一个有效 ID

EWrapper.nextValidId (

orderId: int. Receives next valid order id.
orderId: int. 接收下一个有效订单 ID。
)

Will be invoked automatically upon successful API client connection, or after call to EClient.reqIds.
在 API 客户端连接成功时，或调用 EClient.reqIds 后自动被调用。

def nextValidId(self, orderId: int):
    print("NextValidId:", orderId)

Reset Order ID Sequence  重置订单 ID 序列

The next valid identifier is persistent between TWS sessions.
下一个有效标识符在 TWS 会话之间是持久的。

If necessary, you can reset the order ID sequence within the API Settings dialogue. Note however that the order sequence Id can only be reset if there are no active API orders.
如有必要，您可以在 API 设置对话框中重置订单 ID 序列。但是请注意，只有在没有活动 API 订单的情况下，才能重置订单序列 ID。

"Reset API order ID sequence" button in the API Settings.
Order Management  订单管理

ClientId 0 and the Master Client ID
客户端 ID 0 和主客户端 ID

Each TWS API connection maintains its own ClientID to the host through the EClient.connect function. There are two unique client ID behaviors developers must be aware of:
每个 TWS API 连接通过 EClient.connect 函数维护其到主机的客户端 ID。开发者必须了解两种独特的客户端 ID 行为：

    Master ClientID: The Master Client ID is set in the Global Configuration and is used to distinguish the connecting Client ID used to pull order and trades data even from other API connections. Connecting without using the Master Client ID will mean only trades on the connected Client ID will be returning from calls to the openOrder or execDetails functions.
    主客户端 ID：主客户端 ID 在全局配置中设置，用于区分用于拉取订单和交易数据的连接客户端 ID，即使是从其他 API 连接中。不使用主客户端 ID 连接意味着只有连接客户端 ID 上的交易将从 openOrder 或 execDetails 函数的调用中返回。
    ClientID 0: ClientID 0 is unique from the rest of the client IDs in that users can receive trades made through Trader Workstation or through FIX in addition to trades that take place on the current client ID.
    客户端 ID 0：客户端 ID 0 与其他客户端 ID 不同之处在于，用户除了可以接收当前客户端 ID 上的交易外，还可以接收通过 Trader Workstation 或通过 FIX 进行的交易。

The Master ClientID value can be assigned to 0 so that a connection can retrieve orders placed from TWS, FIX sessions, and all API connections on the account.
主客户端 ID 的值可以分配为 0，以便连接可以检索从 TWS、FIX 会话以及账户上所有 API 连接中放置的订单。

Highlights the "Master API client ID" setting under API Settings.
Commission And Fees Report
佣金与费用报告

When an order is filled either fully or partially, the IBApi.EWrapper.execDetails and IBApi.EWrapper.commissionReport events will deliver IBApi.Execution and IBApi.CommissionAndFeesReport objects. This allows to obtain the full picture of the order’s execution and the resulting commissions.
当订单全部或部分成交时，IBApi.EWrapper.execDetails 和 IBApi.EWrapper.commissionReport 事件将提供 IBApi.Execution 和 IBApi.CommissionAndFeesReport 对象。这允许获取订单成交的完整情况以及产生的佣金。