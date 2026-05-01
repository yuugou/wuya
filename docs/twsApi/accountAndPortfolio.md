# TWS API Documentation / TWS API 文档

## Account & Portfolio Data / 账户与投资组合数据

The IBApi.EClient.reqAccountSummary method creates a subscription for the account data displayed in the TWS Account Summary window. It is commonly used with multiple-account structures. Introducing broker (IBroker) accounts with more than 50 subaccounts or configured for on-demand account lookup cannot use reqAccountSummary with group=”All”. A profile name can be accepted in place of group. See Unification of Groups and Profiles.

IBApi.EClient.reqAccountSummary 方法为 TWS 账户摘要窗口中显示的账户数据创建订阅。它通常用于多账户结构。引入经纪人（IBroker）账户有超过 50 个子账户或配置为按需账户查找的账户不能通过 group=”All” 来使用 reqAccountSummary。可以用配置文件名代替组名。参见组和配置文件的统一。

The TWS offers a comprehensive overview of your account and portfolio through its Account and Portfolio windows. This information can be obtained via the TWS API through three different kind of requests/operations.

TWS 通过其账户和投资组合窗口为您提供账户和投资组合综合概览。这些信息可以使用 TWS API 通过三种不同类型的请求/操作获得。

### Account Summary / 账户摘要

The initial invocation of reqAccountSummary will result in a list of all requested values being returned, and then every three minutes those values which have changed will be returned. The update frequency of 3 minutes is the same as the TWS Account Window and cannot be changed.

首次调用 reqAccountSummary 将返回所有请求值的列表，然后每三分钟返回已更改的值。3 分钟更新频率与 TWS 账户窗口相同，无法更改。

#### Requesting Account Summary / 请求账户摘要

Requests a specific account’s summary. This method will subscribe to the account summary as presented in the TWS’ Account Summary tab. Customers can specify the data received by using a specific tags value. See the Account Summary Tags section for available options.

请求特定账户的摘要。此方法将订阅 TWS 账户摘要标签中显示的账户摘要。客户可以使用特定的标签值来指定接收的数据。有关可用选项，请参阅账户摘要标签部分。

Alternatively, many languages offer the import of AccountSummaryTags with a method to retrieve all tag values.

或者，许多语言提供了导入 AccountSummaryTags 的方法，可以检索所有标签值。

    EClient.reqAccountSummary (

        reqId: int. The unique request identifier.
        reqId: int. 唯一请求标识符。

        group: String. set to “All” to return account summary data for all accounts, or set to a specific Advisor Account Group name that has already been created in TWS Global Configuration.
        group: String. 设置为“所有”以返回所有账户的账户摘要数据，或设置为已在 TWS 全局配置中创建的特定顾问账户组名称。

        tags: String. A comma separated list with the desired tags
        tags: String. 以逗号分隔的所需标签列表

    )

> [desired tags](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#account-summary-tags)

Important: only two active summary subscriptions are allowed at a time!

重要提示：同一时间只允许有两个活跃的摘要订阅！

=== "Python"

    ``` python
    self.reqAccountSummary(9001, "All", AccountSummaryTags.AllTags)
    ```

    Code example:  
    代码示例：

    ``` python
    from ibapi.client import *
    from ibapi.wrapper import *
    from ibapi.contract import Contract
    import time
    class TradeApp(EWrapper, EClient): 
        def __init__(self): 
            EClient.__init__(self, self) 
        def accountSummary(self, reqId: int, account: str, tag: str, value: str,currency: str):
            print("AccountSummary. ReqId:", reqId, "Account:", account,"Tag: ", tag, "Value:", value, "Currency:", currency)
        
        def accountSummaryEnd(self, reqId: int):
            print("AccountSummaryEnd. ReqId:", reqId)
        
    app = TradeApp()      
    app.connect("127.0.0.1", 7496, clientId=1)
    time.sleep(1)
    app.reqAccountSummary(9001, "All", 'NetLiquidation')
    app.run()
    ```

#### Account Summary Tags / 账户摘要标签

| Tag                                | Info                                                                                                                                                              |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AccountType                        | Identifies the IB account structure                                                                                                                               |
| 账户类型                           | 识别 IB 账户结构                                                                                                                                                  |
| NetLiquidation                     | The basis for determining the price of the assets in your account. Total cash value + stock value + options value + bond value                                    |
| 净清算                             | 确定您账户中资产价格的基础。总现金价值 + 股票价值 + 期权价值 + 债券价值                                                                                           |
| TotalCashValue                     | Total cash balance recognized at the time of trade + futures PNL                                                                                                  |
| 总现金价值                         | 交易时确认的现金余额 + 期货盈亏                                                                                                                                   |
| SettledCash                        | Cash recognized at the time of settlement – purchases at the time of trade – commissions – taxes – fees                                                           |
| 结算现金                           | 结算时确认的现金 - 交易时购买 - 手续费 - 税费 - 费用                                                                                                              |
| AccruedCash                        | Total accrued cash value of stock, commodities and securities                                                                                                     |
| 应计现金                           | 股票、商品和证券的应计现金总额                                                                                                                                    |
| BuyingPower                        | Buying power serves as a measurement of the dollar value of securities that one may purchase in a securities account without depositing additional funds          |
| 购买力                             | 购买力是衡量在证券账户中无需追加资金即可购买证券的美元价值的一种指标                                                                                              |
| EquityWithLoanValue                | Forms the basis for determining whether a client has the necessary assets to either initiate or maintain security positions. Cash + stocks + bonds + mutual funds |
| 可交易股本与贷款价值               | 作为判断客户是否拥有必要资产以启动或维持担保头寸的基础。现金 + 股票 + 债券 + 共同基金                                                                             |
| PreviousEquityWithLoanValue        | Marginable Equity with Loan value as of 16:00 ET the previous day                                                                                                 |
|                                    | 前一日东部时间 16:00 的具有贷款价值的可交易股本                                                                                                                   |
| GrossPositionValue                 | The sum of the absolute value of all stock and equity option positions                                                                                            |
|                                    | 所有股票和股票期权头寸的绝对值之和                                                                                                                                |
| RegTEquity                         | Regulation T equity for universal account                                                                                                                         |
|                                    | 通用账户的 T 法规股权                                                                                                                                             |
| RegTMargin                         | Regulation T margin for universal account                                                                                                                         |
|                                    | 通用账户的 T 法规保证金                                                                                                                                           |
| SMA                                | Special Memorandum Account: Line of credit created when the market value of securities in a Regulation T account increase in value                                |
| 特殊备忘账户                       | 特殊备忘账户：当 T 法规账户中证券的市场价值增加时创建的信贷额度                                                                                                   |
| InitMarginReq                      | Initial Margin requirement of whole portfolio                                                                                                                     |
| 初始保证金要求                     | 整个投资组合的初始保证金要求                                                                                                                                      |
| MaintMarginReq                     | Maintenance Margin requirement of whole portfolio                                                                                                                 |
| 维持保证金要求                     | 整个投资组合的维持保证金要求                                                                                                                                      |
| AvailableFunds                     | This value tells what you have available for trading                                                                                                              |
| 可用资金                           | 这个值告诉你可用于交易的金额                                                                                                                                      |
| ExcessLiquidity                    | This value shows your margin cushion, before liquidation                                                                                                          |
| 超额流动性                         | 这个值显示你在被清算前的保证金缓冲                                                                                                                                |
| Cushion                            | Excess liquidity as a percentage of net liquidation value                                                                                                         |
| 超额流动性占净资产清算价值的百分比 | 超额流动性占净资产清算价值的百分比                                                                                                                                |
| FullInitMarginReq                  | Initial Margin of whole portfolio with no discounts or intraday credits                                                                                           |
|                                    | 无折扣或日内信贷的整个投资组合初始保证金                                                                                                                          |
| FullMaintMarginReq                 | Maintenance Margin of whole portfolio with no discounts or intraday credits                                                                                       |
|                                    | 全组合无折扣或日内信贷的维持保证金                                                                                                                                |
| FullAvailableFunds                 | Available funds of whole portfolio with no discounts or intraday credits                                                                                          |
| 全可用资金                         | 全组合无折扣或日内信贷的可用资金                                                                                                                                  |
| FullExcessLiquidity                | Excess liquidity of whole portfolio with no discounts or intraday credits                                                                                         |
| 全超额流动性                       | 全组合多余流动性，无折扣或日内信用                                                                                                                                |
| LookAheadNextChange                | Time when look-ahead values take effect                                                                                                                           |
|                                    | LookAhead 值生效的时间                                                                                                                                            |
| LookAheadInitMarginReq             | Initial Margin requirement of whole portfolio as of next period’s margin change                                                                                   |
|                                    | 下一周期保证金变更时的整个投资组合初始保证金要求                                                                                                                  |
| LookAheadMaintMarginReq            | Maintenance Margin requirement of whole portfolio as of next period’s margin change                                                                               |
|                                    | 下一周期保证金变更时的整个投资组合维持保证金要求                                                                                                                  |
| LookAheadAvailableFunds            | This value reflects your available funds at the next margin change                                                                                                |
|                                    | 这个值反映了你在下次保证金变动时的可用资金                                                                                                                        |
| LookAheadExcessLiquidity           | This value reflects your excess liquidity at the next margin change                                                                                               |
|                                    | 这个值反映了你在下次保证金变动时的超额流动性                                                                                                                      |
| HighestSeverity                    | A measure of how close the account is to liquidation                                                                                                              |
|                                    | 衡量账户接近清算的程度                                                                                                                                            |
| DayTradesRemaining                 | The Number of Open/Close trades a user could put on before Pattern Day Trading is detected. A value of “-1” means that the user can put on unlimited day trades.  |
| 剩余日内交易次数                   | 用户在检测到模式日内交易前可以进行的开/平仓交易数量。-1 表示用户可以进行无限次的日内交易。                                                                        |
| Leverage                           | GrossPositionValue / NetLiquidation                                                                                                                               |
| 杠杆                               | 总持仓价值 / 净清算价值                                                                                                                                           |
| $LEDGER                            | Single flag to relay all cash balance tags*, only in base currency.                                                                                               |
|                                    | 单一标志位用于传递所有现金余额标签*，仅为基础货币。                                                                                                               |
| $LEDGER:CURRENCY                   | Single flag to relay all cash balance tags*, only in the specified currency.                                                                                      |
|                                    | 单个标志用于转发所有现金余额标签*，仅限于指定货币。                                                                                                               |
| $LEDGER:ALL                        | Single flag to relay all cash balance tags* in all currencies.                                                                                                    |
|                                    | 单个标志用于转发所有货币中的所有现金余额标签*。                                                                                                                   |

#### Receiving Account Summary / 接收账户摘要

    EWrapper.accountSummary (

        reqId: int. the request’s unique identifier.
        reqId: int. 请求的唯一标识符。

        account: String. the account id
        account: String. 账户 ID

        tag: String. the account’s attribute being received.
        tag: String. 接收到的账户属性。

        value: String. the account’s attribute’s value.
        value: String. 账户属性的值。

        currency: String. the currency on which the value is expressed.
        currency: String. 表示值的货币。

    )

Receives the account information. This method will receive the account information just as it appears in the TWS’ Account Summary Window.

接收账户信息。此方法将接收与 TWS 账户摘要窗口中显示的账户信息完全相同的账户信息。

=== "Python"

    ``` python
    def accountSummary(self, reqId: int, account: str, tag: str, value: str,currency: str):
        print("AccountSummary. ReqId:", reqId, "Account:", account,"Tag: ", tag, "Value:", value, "Currency:", currency)
    ```

    EWrapper.accountSummaryEnd(

        reqId: String. The request’s identifier.
        reqId: String. 请求的标识符。

    )

Notifies when all the accounts’ information has ben received. Requires TWS 967+ to receive accountSummaryEnd in linked account structures.

当所有账户信息已接收时通知。需要 TWS 967+ 才能在关联账户结构中接收 accountSummaryEnd。

=== "Python"
    ``` python
    def accountSummaryEnd(self, reqId: int):
        print("AccountSummaryEnd. ReqId:", reqId)
    ```

#### Cancel Account Summary / 取消账户摘要

Once the subscription to account summary is no longer needed, it can be cancelled via the IBApi::EClient::cancelAccountSummary method:

一旦不再需要订阅账户摘要，可以通过 IBApi::EClient::cancelAccountSummary 方法取消：

    EClient.cancelAccountSummary (

        reqId: int. The identifier of the previously performed account request
        reqId: int. 之前执行过的账户请求的标识符

    )

=== "Python"
    ``` python
    self.cancelAccountSummary(9001)
    ```

### Account Updates / 账户更新

The IBApi.EClient.reqAccountUpdates function creates a subscription to the TWS through which account and portfolio information is delivered. This information is the exact same as the one displayed within the TWS’ Account Window. Just as with the TWS’ Account Window, unless there is a position change this information is updated at a fixed interval of three minutes.

IBApi.EClient.reqAccountUpdates 函数通过创建对 TWS 的订阅来获取账户和投资组合信息。这些信息与 TWS 账户窗口中显示的信息完全相同。与 TWS 账户窗口一样，除非出现头寸变动，否则这些信息会以固定的三分钟间隔更新。

Unrealized and Realized P&L is sent to the API function IBApi.EWrapper.updateAccountValue function after a subscription request is made with IBApi.EClient.reqAccountUpdates. This information corresponds to the data in the TWS Account Window, and has a different source of information, a different update frequency, and different reset schedule than PnL data in the TWS Portfolio Window and associated API functions (below). In particular, the unrealized P&L information shown in the TWS Account Window which is sent to EWrapper.updatePortfolio will update either (1) when a trade for that particular instrument occurs or (2) every 3 minutes. The realized P&L data in the TWS Account Window is reset to 0 once per day.

在通过 IBApi.EClient.reqAccountUpdates 发出订阅请求后，未实现盈亏和已实现盈亏会发送到 API 函数 IBApi.EWrapper.updateAccountValue。这些信息与 TWS 账户窗口中的数据相对应，但信息来源、更新频率和重置计划与 TWS 投资组合窗口和关联 API 函数（下文）中的盈亏数据不同。特别是，TWS 账户窗口中发送到 EWrapper.updatePortfolio 的未实现盈亏信息会在 (1) 该特定工具发生交易时，或 (2) 每 3 分钟更新。TWS 账户窗口中的已实现盈亏数据每天重置为 0。

It is important to keep in mind that the P&L data shown in the Account Window and Portfolio Window will sometimes differ because there is a different source of information and a different reset schedule.

要记住的是，账户窗口和投资组合窗口中显示的盈亏数据有时会因信息来源不同和重置时间表不同而有所差异。

See [Profit & Loss](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#pnl) for alternative PnL data

另请参阅盈亏以获取替代盈亏数据

#### Requesting Account Updates / 请求账户更新

Subscribes to a specific account’s information and portfolio. Through this method, a single account’s subscription can be started/stopped. As a result from the subscription, the account’s information, portfolio and last update time will be received at EWrapper.updateAccountValue, EWrapper.updatePortfolio, EWrapper.updateAccountTime respectively. All account values and positions will be returned initially, and then there will only be updates when there is a change in a position, or to an account value every 3 minutes if it has changed. Only one account can be subscribed at a time. A second subscription request for another account when the previous one is still active will cause the first one to be canceled in favor of the second one.

订阅特定账户的信息和投资组合。通过此方法，可以开始/停止单个账户的订阅。订阅的结果将在 EWrapper.updateAccountValue、EWrapper.updatePortfolio、EWrapper.updateAccountTime 中分别接收账户信息、投资组合和最后更新时间。所有账户价值和头寸将最初返回，然后只有在头寸发生变化时，或账户价值每 3 分钟变化时才会更新。一次只能订阅一个账户。当之前的订阅仍然活跃时，对另一个账户的第二个订阅请求将导致第一个订阅被取消，以支持第二个订阅。

    EClient.reqAccountUpdates (

        subscribe: bool. Set to true to start the subscription and to false to stop it.
        subscribe: bool. 设置为 true 以开始订阅，设置为 false 以停止订阅。

        acctCode: String. The account id (i.e. U123456) for which the information is requested.
        acctCode: String. 请求信息的账户 id（例如 U123456）。

    )

=== "Python"
    ``` python
    self.reqAccountUpdates(True, self.account)
    ```

    Code example:  代码示例：

    ```python
    from ibapi.client import *
    from ibapi.wrapper import *
    from ibapi.contract import Contract
    import time
    class TradeApp(EWrapper, EClient): 
        def __init__(self): 
            EClient.__init__(self, self) 
        def updateAccountValue(self, key: str, val: str, currency: str,accountName: str):
            print("UpdateAccountValue. Key:", key, "Value:", val, "Currency:", currency, "AccountName:", accountName)
        
        def updatePortfolio(self, contract: Contract, position: Decimal,marketPrice: float, marketValue: float, averageCost: float, unrealizedPNL: float, realizedPNL: float, accountName: str):
            print("UpdatePortfolio.", "Symbol:", contract.symbol, "SecType:", contract.secType, "Exchange:",contract.exchange, "Position:", decimalMaxString(position), "MarketPrice:", floatMaxString(marketPrice),"MarketValue:", floatMaxString(marketValue), "AverageCost:", floatMaxString(averageCost), "UnrealizedPNL:", floatMaxString(unrealizedPNL), "RealizedPNL:", floatMaxString(realizedPNL), "AccountName:", accountName)
        def updateAccountTime(self, timeStamp: str):
            print("UpdateAccountTime. Time:", timeStamp)
        def accountDownloadEnd(self, accountName: str):
            print("AccountDownloadEnd. Account:", accountName)
        
    app = TradeApp()      
    app.connect("127.0.0.1", 7496, clientId=1)
    time.sleep(1)
    app.reqAccountUpdates(True, 'U123456')
    app.run()
    ```

#### Receiving Account Updates / 接收账户更新

Resulting account and portfolio information will be delivered via the IBApi.EWrapper.updateAccountValue, IBApi.EWrapper.updatePortfolio, IBApi.EWrapper.updateAccountTime and IBApi.EWrapper.accountDownloadEnd

结果账户和投资组合信息将通过 IBApi.EWrapper.updateAccountValue、IBApi.EWrapper.updatePortfolio、IBApi.EWrapper.updateAccountTime 和 IBApi.EWrapper.accountDownloadEnd 传递

    EWrapper.updateAccountValue (

        key: String. The value being updated.
        key: String. 要更新的值。

        value: String. up-to-date value
        value: String. 最新值

        currency: String. The currency on which the value is expressed.
        currency: String. 表示该值的货币。

        accountName: String. The account identifier.
        accountName: String. 账户标识符。
    )

Receives the subscribed account’s information. Only one account can be subscribed at a time. After the initial callback to updateAccountValue, callbacks only occur for values which have changed. This occurs at the time of a position change, or every 3 minutes at most. This frequency cannot be adjusted.

接收订阅账户的信息。一次只能订阅一个账户。在首次调用 updateAccountValue 进行回调后，只有值发生变化时才会进行回调。这发生在仓位变化时，或最多每 3 分钟一次。此频率不可调整。

Note: An important key passed back in EWrapper.updateAccountValue after a call to EClient.reqAccountUpdates is a boolean value ‘accountReady’. If an accountReady value of false is returned that means that the IB server is in the process of resetting at that moment, i.e. the account is ‘not ready’. When this occurs subsequent key values returned to EWrapper.updateAccountValue in the current update can be out of date or incorrect.

注意：在调用 EClient.reqAccountUpdates 后，EWrapper.updateAccountValue 返回的一个重要键值是一个布尔值“accountReady”。如果返回的 accountReady 值为 false，则表示 IB 服务器正在此时重置，即账户“未就绪”。在这种情况下，当前更新中返回到 EWrapper.updateAccountValue 的后续键值可能已过时或不正确。

=== "Python"
    ``` python
    def updateAccountValue(self, key: str, val: str, currency: str,accountName: str):
        print("UpdateAccountValue. Key:", key, "Value:", val, "Currency:", currency, "AccountName:", accountName)
    ```

``` python
EWrapper.updatePortfolio (

    contract: Contract. The Contract for which a position is held.
    合约：合约。指持有的合约。

    position: Decimal. The number of positions held.
    仓位：十进制数。持有的仓位数量。

    marketPrice: Double. The instrument’s unitary price
    市场价格：双精度浮点数。该工具的单位价格。

    marketValue: Double. Total market value of the instrument.
    市场价值：双精度浮点数。该工具的总市场价值。

    averageCost: Double. Average cost of the overall position.
    averageCost: Double. 平均持仓成本。

    unrealizedPNL: Double. Daily unrealized profit and loss on the position.
    unrealizedPNL: Double. 持仓每日未实现盈亏。

    realizedPNL: Double. Daily realized profit and loss on the position.
    realizedPNL: Double. 持仓每日已实现盈亏。

    accountName: String. Account ID for the update.
    accountName: String. 更新的账户 ID。

)
```

Receives the subscribed account’s portfolio. This function will receive only the portfolio of the subscribed account. After the initial callback to updatePortfolio, callbacks only occur for positions which have changed.

接收订阅账户的投资组合。此函数仅接收订阅账户的投资组合。在首次调用 updatePortfolio 后，回调仅发生在发生变化的持仓上。

=== "Python"
    ``` python
    def updatePortfolio(self, contract: Contract, position: Decimal,marketPrice: float, marketValue: float, averageCost: float, unrealizedPNL: float, realizedPNL: float, accountName: str):
        print("UpdatePortfolio.", "Symbol:", contract.symbol, "SecType:", contract.secType, "Exchange:",contract.exchange, "Position:", decimalMaxString(position), "MarketPrice:", floatMaxString(marketPrice),"MarketValue:", floatMaxString(marketValue), "AverageCost:", floatMaxString(averageCost), "UnrealizedPNL:", floatMaxString(unrealizedPNL), "RealizedPNL:", floatMaxString(realizedPNL), "AccountName:", accountName)
    ```

``` python
EWrapper.updateAccountTime (

    timestamp: String. the last update system time.
    timestamp: String. 最后一次更新系统时间。

)
```

Receives the last time on which the account was updated.

接收账户最后更新的时间。

=== "Python"
    ``` python
    def updateAccountTime(self, timeStamp: str):
        print("UpdateAccountTime. Time:", timeStamp)
    ```

=== "Python"

``` python
EWrapper.accountDownloadEnd (

    account: String. The account identifier.
    account: String. 账户标识符。

)
```

Notifies when all the account’s information has finished.

当所有账户信息下载完成后通知。

=== "Python"
    ``` python
    def accountDownloadEnd(self, accountName: str):
        print("AccountDownloadEnd. Account:", accountName)
    ```

#### Account Value Keys / 账户值的键

When requesting [reqAccountUpdates](https://www.interactivebrokers.com/campus/ibkr-api-page/trader-workstation-api/#request-account-updates) customers will receive values corresponding to various account key/value pairs. The table below documents potential responses and what they mean.

当请求 reqAccountUpdates 时，客户将收到与各种账户键/值对相对应的值。下表记录了潜在的响应及其含义。

Account values delivered via [IBApi.EWrapper.updateAccountValue](https://www.interactivebrokers.com/campus/ibkr-api-page/trader-workstation-api/#receive-account-updates) can be classified in the following way:

通过 IBApi.EWrapper.updateAccountValue 传递的账户价值可以按以下方式分类：

- Commodities: suffixed by a “-C”

    商品：以“-C”为后缀

- Securities: suffixed by a “-S”

    证券：以“-S”为后缀

- Totals: no suffix

    总计：无后缀

| Key                              | 键                 | Description                                                                                                                                                                                                                                                                                       | 描述                                                                                                                                                                              |
| -------------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AccountCode                      | 账户代码           | The account ID number                                                                                                                                                                                                                                                                             | 账户 ID 号码                                                                                                                                                                      |
| AccountOrGroup                   | 账户或组           | “All” to return account summary data for all accounts, or set to a specific Advisor Account Group name that has already been created in TWS Global Configuration                                                                                                                                  | “全部”以返回所有账户的账户摘要数据，或设置为 TWS 全局配置中已创建的特定顾问账户组名称                                                                                             |
| AccountReady                     | 账户就绪           | If an accountReady value of false is returned that means that the IB server is in the process of resetting at that moment, i.e. the account is ‘not ready’. When this occurs subsequent key values returned to EWrapper.updateAccountValue in the current update can be out of date or incorrect. | 如果返回的 accountReady 值为 false，则表示 IB 服务器正在此时重置中，即账户“未就绪”。当这种情况发生时，当前更新中返回到 EWrapper.updateAccountValue 的后续键值可能已过时或不正确。 |
| AccountType                      | 账户类型           | Identifies the IB account structure                                                                                                                                                                                                                                                               | 标识 IB 账户结构                                                                                                                                                                  |
| AccruedCash                      |                    | Total accrued cash value of stock, commodities and securities                                                                                                                                                                                                                                     | 股票、商品和证券的累计现金总额                                                                                                                                                    |
| AccruedCash-C                    | 应计现金-C         | Reflects the current’s month accrued debit and credit interest to date, updated daily in commodity segment                                                                                                                                                                                        | 反映当前月份累计的借方和贷方利息至当前日期，在商品板块每日更新                                                                                                                    |
| AccruedCash-S                    | 累计现金-S         | Reflects the current’s month accrued debit and credit interest to date, updated daily in security segment                                                                                                                                                                                         | 反映当前月份累计的借方和贷方利息至当前日期，在证券板块每日更新                                                                                                                    |
| AccruedDividend                  | 应计股息           | Total portfolio value of dividends accrued                                                                                                                                                                                                                                                        | 应计股息的总投资组合价值                                                                                                                                                          |
| AccruedDividend-C                | 应计股息-C         | Dividends accrued but not paid in commodity segment                                                                                                                                                                                                                                               | 商品板块中已应计但未支付的股息                                                                                                                                                    |
| AccruedDividend-S                | 应计股息-S         | Dividends accrued but not paid in security segment                                                                                                                                                                                                                                                | 已计但未支付的证券部分股息                                                                                                                                                        |
| AvailableFunds                   | 可用资金           | This value tells what you have available for trading                                                                                                                                                                                                                                              | 此值显示您可用于交易的金额                                                                                                                                                        |
| AvailableFunds-C                 | 可用资金-C         | Net Liquidation Value – Initial Margin                                                                                                                                                                                                                                                            | 净清算价值 - 初始保证金                                                                                                                                                           |
| AvailableFunds-S                 | 可用资金-S         | Equity with Loan Value – Initial Margin                                                                                                                                                                                                                                                           | 含贷款价值的股东权益 - 初始保证金                                                                                                                                                 |
| Billable                         | 可计费             | Total portfolio value of treasury bills                                                                                                                                                                                                                                                           | 国库券总投资组合价值                                                                                                                                                              |
| Billable-C                       | 可收费-C           | Value of treasury bills in commodity segment                                                                                                                                                                                                                                                      | 商品板块的国债价值                                                                                                                                                                |
| Billable-S                       | 可收费-S           | Value of treasury bills in security segment                                                                                                                                                                                                                                                       | 证券部门国库券的价值                                                                                                                                                              |
| BuyingPower                      | 买入资金           | Cash Account: Minimum (Equity with Loan Value, Previous Day Equity with Loan Value)-Initial Margin, Standard Margin Account: Minimum (Equity with Loan Value, Previous Day Equity with Loan Value) – Initial Margin *4                                                                            | 现金账户：最低（含贷款价值的权益，前一日含贷款价值的权益）-初始保证金，标准保证金账户：最低（含贷款价值的权益，前一日含贷款价值的权益）-初始保证金*4                              |
| CashBalance                      | 现金余额           | Cash recognized at the time of trade + futures PNL                                                                                                                                                                                                                                                | 交易时确认的现金+期货盈亏                                                                                                                                                         |
| CorporateBondValue               | 公司债券价值       | Value of non-Government bonds such as corporate bonds and municipal bonds                                                                                                                                                                                                                         | 公司债券和市政债券等非政府债券的价值                                                                                                                                              |
| Currency                         | 货币               | Open positions are grouped by currency                                                                                                                                                                                                                                                            | 持仓按货币分组                                                                                                                                                                    |
| Cushion                          | 缓冲垫             | Excess liquidity as a percentage of net liquidation value                                                                                                                                                                                                                                         | 净清算价值中过剩流动性的百分比                                                                                                                                                    |
| DayTradesRemaining               |                    | Number of Open/Close trades one could do before Pattern Day Trading is detected                                                                                                                                                                                                                   | 在检测到交易模式前，用户可以进行的开/平仓交易次数                                                                                                                                 |
| DayTradesRemainingT+1            |                    | Number of Open/Close trades one could do tomorrow before Pattern Day Trading is detected                                                                                                                                                                                                          | 在检测到交易模式前，用户明天可以进行的开/平仓交易次数                                                                                                                             |
| DayTradesRemainingT+2            |                    | Number of Open/Close trades one could do two days from today before Pattern Day Trading is detected                                                                                                                                                                                               | 距离今天两天内可以进行的开/平仓交易数量，在此期间未检测到交易模式日交易                                                                                                           |
| DayTradesRemainingT+3            |                    | Number of Open/Close trades one could do three days from today before Pattern Day Trading is detected                                                                                                                                                                                             | 距离今天三天内可以进行的开/平仓交易数量，在此期间未检测到交易模式日交易                                                                                                           |
| DayTradesRemainingT+4            |                    | Number of Open/Close trades one could do four days from today before Pattern Day Trading is detected                                                                                                                                                                                              | 从今天起的四天内，一个人可以进行的开仓/平仓交易数量，直到检测到模式日交易                                                                                                         |
| EquityWithLoanValue              |                    | Forms the basis for determining whether a client has the necessary assets to either initiate or maintain security positions                                                                                                                                                                       | 用于确定客户是否拥有必要的资产来启动或维持证券头寸的基础                                                                                                                          |
| EquityWithLoanValue-C            | 带贷款价值的股票-C | Cash account: Total cash value + commodities option value – futures maintenance margin requirement + minimum (0, futures PNL) Margin account: Total cash value + commodities option value – futures maintenance margin requirement                                                                | 现金账户：总现金价值 + 商品期权价值 - 期货维持保证金要求 + 最小值（0，期货 PNL） 保证金账户：总现金价值 + 商品期权价值 - 期货维持保证金要求                                       |
| EquityWithLoanValue-S            | 权益加贷款价值-S   | Cash account: Settled Cash Margin Account: Total cash value + stock value + bond value + (non-U.S. & Canada securities options value)                                                                                                                                                             | 现金账户：结算现金保证金账户：总现金价值 + 股票价值 + 债券价值 + （非美国和加拿大证券期权价值）                                                                                   |
| ExcessLiquidity                  | 超额流动性         | This value shows your margin cushion, before liquidation                                                                                                                                                                                                                                          | 此值显示您的保证金缓冲，在清算之前                                                                                                                                                |
| ExcessLiquidity-C                | 超额流动性-C       | Equity with Loan Value – Maintenance Margin                                                                                                                                                                                                                                                       | 带贷款价值的股票 – 维持保证金                                                                                                                                                     |
| ExcessLiquidity-S                | 超额流动性-S       | Net Liquidation Value – Maintenance Margin                                                                                                                                                                                                                                                        | 净清算价值 – 维持保证金                                                                                                                                                           |
| ExchangeRate                     | 汇率               | The exchange rate of the currency to your base currency                                                                                                                                                                                                                                           | 该货币兑您基准货币的汇率                                                                                                                                                          |
| FullAvailableFunds               | 全部可用资金       | Available funds of whole portfolio with no discounts or intraday credits                                                                                                                                                                                                                          | 整个投资组合的可用资金，无折扣或日内信用                                                                                                                                          |
| FullAvailableFunds-C             | 全可用资金-C       | Net Liquidation Value – Full Initial Margin                                                                                                                                                                                                                                                       | 净清算价值——全额初始保证金                                                                                                                                                        |
| FullAvailableFunds-S             | 全可用资金-S       | Equity with Loan Value – Full Initial Margin                                                                                                                                                                                                                                                      | 有贷款价值的股票——全额初始保证金                                                                                                                                                  |
| FullExcessLiquidity              |                    | Excess liquidity of whole portfolio with no discounts or intraday credits                                                                                                                                                                                                                         | 全组合超额流动性（无折扣或日内信用）                                                                                                                                              |
| FullExcessLiquidity-C            |                    | Net Liquidation Value – Full Maintenance Margin                                                                                                                                                                                                                                                   | 净清算价值——全额维持保证金                                                                                                                                                        |
| FullExcessLiquidity-S            |                    | Equity with Loan Value – Full Maintenance Margin                                                                                                                                                                                                                                                  | 带贷款价值的股票 - 完整维持保证金                                                                                                                                                 |
| FullInitMarginReq                |                    | Initial Margin of whole portfolio with no discounts or intraday credits                                                                                                                                                                                                                           | 无折扣或日内信贷的整个投资组合初始保证金                                                                                                                                          |
| FullInitMarginReq-C              |                    | Initial Margin of commodity segment’s portfolio with no discounts or intraday credits                                                                                                                                                                                                             | 无折扣或日内信用商品板块投资组合的初始保证金                                                                                                                                      |
| FullInitMarginReq-S              |                    | Initial Margin of security segment’s portfolio with no discounts or intraday credits                                                                                                                                                                                                              | 无折扣或日内信用证券板块投资组合的初始保证金                                                                                                                                      |
| FullMaintMarginReq               |                    | Maintenance Margin of whole portfolio with no discounts or intraday credits                                                                                                                                                                                                                       | 无折扣或日内信用额度时的整个投资组合维持保证金                                                                                                                                    |
| FullMaintMarginReq-C             |                    | Maintenance Margin of commodity segment’s portfolio with no discounts or intraday credits                                                                                                                                                                                                         | 无折扣或日内信用额度时的商品板块投资组合维持保证金                                                                                                                                |
| FullMaintMarginReq-S             |                    | Maintenance Margin of security segment’s portfolio with no discounts or intraday credits                                                                                                                                                                                                          | 无折扣或日内信贷的安全部分投资组合的维持保证金                                                                                                                                    |
| FundValue                        |                    | Value of funds value (money market funds + mutual funds)                                                                                                                                                                                                                                          | 资金价值（货币市场基金+共同基金）                                                                                                                                                 |
| FutureOptionValue                |                    | Real-time market-to-market value of futures options                                                                                                                                                                                                                                               | 期货期权实时市值                                                                                                                                                                  |
| FuturesPNL                       | 期货 PNL           | Real-time changes in futures value since last settlement                                                                                                                                                                                                                                          | 自上次结算以来期货价值的实时变化                                                                                                                                                  |
| FxCashBalance                    |                    | Cash balance in related IB-UKL account                                                                                                                                                                                                                                                            | 相关 IB-UKL 账户的现金余额                                                                                                                                                        |
| GrossPositionValue               |                    | Gross Position Value in securities segment                                                                                                                                                                                                                                                        | 证券部门的毛头寸价值                                                                                                                                                              |
| GrossPositionValue-S             | 总持仓价值-S       | Long Stock Value + Short Stock Value + Long Option Value + Short Option Value                                                                                                                                                                                                                     | 多头股票价值 + 空头股票价值 + 多头期权价值 + 空头期权价值                                                                                                                         |
| IndianStockHaircut               |                    | Margin rule for IB-IN accounts                                                                                                                                                                                                                                                                    | IB-IN 账户的保证金规则                                                                                                                                                            |
| InitMarginReq                    | 初始保证金要求     | Initial Margin requirement of whole portfolio                                                                                                                                                                                                                                                     | 全组合初始保证金要求                                                                                                                                                              |
| InitMarginReq-C                  |                    | Initial Margin of the commodity segment in base currency                                                                                                                                                                                                                                          | 商品板块在基准货币中的初始保证金                                                                                                                                                  |
| InitMarginReq-S                  |                    | Initial Margin of the security segment in base currency                                                                                                                                                                                                                                           | 证券板块的基础货币初始保证金                                                                                                                                                      |
| IssuerOptionValue                |                    | Real-time mark-to-market value of Issued Option                                                                                                                                                                                                                                                   | 已发行期权实时盯市价值                                                                                                                                                            |
| Leverage-S                       | 杠杆-S             | GrossPositionValue / NetLiquidation in security segment                                                                                                                                                                                                                                           | 证券板块总头寸价值 / 净清算                                                                                                                                                       |
| LookAheadNextChange              |                    | Time when look-ahead values take effect                                                                                                                                                                                                                                                           | 提前值生效时间                                                                                                                                                                    |
| LookAheadAvailableFunds          |                    | This value reflects your available funds at the next margin change                                                                                                                                                                                                                                | 此值反映您在下次保证金变动时的可用资金                                                                                                                                            |
| LookAheadAvailableFunds-C        |                    | Net Liquidation Value – look ahead Initial Margin                                                                                                                                                                                                                                                 | 净清算价值——前瞻初始保证金                                                                                                                                                        |
| LookAheadAvailableFunds-S        | 前瞻可用资金-S     | Equity with Loan Value – look ahead Initial Margin                                                                                                                                                                                                                                                | 带贷款价值的股权——前瞻初始保证金                                                                                                                                                  |
| LookAheadExcessLiquidity         | 前瞻超额流动性     | This value reflects your excess liquidity at the next margin change                                                                                                                                                                                                                               | 这个值反映了您在下次保证金变动时的超额流动性                                                                                                                                      |
| LookAheadExcessLiquidity-C       |                    | Net Liquidation Value – look ahead Maintenance Margin                                                                                                                                                                                                                                             | 净资产清算价值——前瞻性维持保证金                                                                                                                                                  |
| LookAheadExcessLiquidity-S       |                    | Equity with Loan Value – look ahead Maintenance Margin                                                                                                                                                                                                                                            | 市值与贷款价值——前瞻性维持保证金                                                                                                                                                  |
| LookAheadInitMarginReq           |                    | Initial margin requirement of whole portfolio as of next period’s margin change                                                                                                                                                                                                                   | 下一周期保证金变更时的整个投资组合初始保证金要求                                                                                                                                  |
| LookAheadInitMarginReq-C         |                    | Initial margin requirement as of next period’s margin change in the base currency of the account                                                                                                                                                                                                  | 截至下一周期保证金变更时账户基准货币的初始保证金要求                                                                                                                              |
| LookAheadInitMarginReq-S         |                    | Initial margin requirement as of next period’s margin change in the base currency of the account                                                                                                                                                                                                  | 截至下一周期保证金变更时账户基准货币的初始保证金要求                                                                                                                              |
| LookAheadMaintMarginReq          | 前瞻维持保证金需求 | Maintenance margin requirement of whole portfolio as of next period’s margin change                                                                                                                                                                                                               | 下一期保证金变更时的整个投资组合维持保证金要求                                                                                                                                    |
| LookAheadMaintMarginReq-C        |                    | Maintenance margin requirement as of next period’s margin change in the base currency of the account                                                                                                                                                                                              | 下一期保证金变更时账户基准货币的维持保证金要求                                                                                                                                    |
| LookAheadMaintMarginReq-S        |                    | Maintenance margin requirement as of next period’s margin change in the base currency of the account                                                                                                                                                                                              | 维持保证金要求，自下一期保证金变更时以账户基准货币计算                                                                                                                            |
| MaintMarginReq                   |                    | Maintenance Margin requirement of whole portfolio                                                                                                                                                                                                                                                 | 整个投资组合的维持保证金要求                                                                                                                                                      |
| MaintMarginReq-C                 |                    | Maintenance Margin for the commodity segment                                                                                                                                                                                                                                                      | 商品板块维持保证金                                                                                                                                                                |
| MaintMarginReq-S                 |                    | Maintenance Margin for the security segment                                                                                                                                                                                                                                                       | 证券板块维持保证金                                                                                                                                                                |
| MoneyMarketFundValue             | 货币市场基金价值   | Market value of money market funds excluding mutual funds                                                                                                                                                                                                                                         | 货币市场基金（不包括共同基金）的市场价值                                                                                                                                          |
| MutualFundValue                  | 共同基金价值       | Market value of mutual funds excluding money market funds                                                                                                                                                                                                                                         | 共同基金（不包括货币市场基金）的市场价值                                                                                                                                          |
| NetDividend                      | 净红利             | The sum of the Dividend Payable/Receivable Values for the securities and commodities segments of the account                                                                                                                                                                                      | 证券和商品账户的股息应付/应收值的总和                                                                                                                                             |
| NetLiquidation                   | 净清算             | The basis for determining the price of the assets in your account                                                                                                                                                                                                                                 | 确定您账户中资产价格的基础                                                                                                                                                        |
| NetLiquidation-C                 | 净清算-C           | Total cash value + futures PNL + commodities options value                                                                                                                                                                                                                                        | 总现金价值 + 期货盈亏 + 商品期权价值                                                                                                                                              |
| NetLiquidation-S                 | 净清算-S           | Total cash value + stock value + securities options value + bond value                                                                                                                                                                                                                            | 总现金价值 + 股票价值 + 证券期权价值 + 债券价值                                                                                                                                   |
| NetLiquidationByCurrency         | 按货币净清算       | Net liquidation for individual currencies                                                                                                                                                                                                                                                         | 单个货币的净清算                                                                                                                                                                  |
| OptionMarketValue                | 期权市场价值       | Real-time mark-to-market value of options                                                                                                                                                                                                                                                         | 期权实时盯市价值                                                                                                                                                                  |
| PASharesValue                    |                    | Personal Account shares value of whole portfolio                                                                                                                                                                                                                                                  | 个人账户整个投资组合的股票价值                                                                                                                                                    |
| PASharesValue-C                  |                    | Personal Account shares value in commodity segment                                                                                                                                                                                                                                                | 个人账户商品板块的股份价值                                                                                                                                                        |
| PASharesValue-S                  |                    | Personal Account shares value in security segment                                                                                                                                                                                                                                                 | 个人账户在证券部分的股份价值                                                                                                                                                      |
| PostExpirationExcess             |                    | Total projected “at expiration” excess liquidity                                                                                                                                                                                                                                                  | 总预计“到期时”超额流动性                                                                                                                                                          |
| PostExpirationExcess-C           |                    | Provides a projected “at expiration” excess liquidity based on the soon-to expire contracts in your portfolio in commodity segment                                                                                                                                                                | 基于您投资组合中即将到期的商品板块合约，提供“到期时”超额流动性的预计                                                                                                              |
| PostExpirationExcess-S           |                    | Provides a projected “at expiration” excess liquidity based on the soon-to expire contracts in your portfolio in security segment                                                                                                                                                                 | 根据您投资组合中即将到期的合约，在证券板块提供预计的“到期时”超额流动性                                                                                                            |
| PostExpirationMargin             |                    | Total projected “at expiration” margin                                                                                                                                                                                                                                                            | 预计的“到期时”总保证金                                                                                                                                                            |
| PostExpirationMargin-C           |                    | Provides a projected “at expiration” margin value based on the soon-to expire contracts in your portfolio in commodity segment                                                                                                                                                                    | 根据您投资组合中即将到期的商品板块合约，提供预计的“到期时”保证金价值                                                                                                              |
| PostExpirationMargin-S           |                    | Provides a projected “at expiration” margin value based on the soon-to expire contracts in your portfolio in security segment                                                                                                                                                                     | 根据您投资组合中即将到期的证券板块合约，提供预计的“到期时”保证金价值                                                                                                              |
| PreviousDayEquityWithLoanValue   |                    | Marginable Equity with Loan value as of 16:00 ET the previous day in securities segment                                                                                                                                                                                                           | 前一日证券部门截至东部时间 16:00 的含贷款价值的可交易资本                                                                                                                         |
| PreviousDayEquityWithLoanValue-S |                    | IMarginable Equity with Loan value as of 16:00 ET the previous day                                                                                                                                                                                                                                | 前一日东部时间 16:00 的含贷款价值可交易资本                                                                                                                                       |
| RealCurrency                     | 真实货币           | Open positions are grouped by currency                                                                                                                                                                                                                                                            | 持仓按货币分组                                                                                                                                                                    |
| RealizedPnL                      | 已实现盈亏         | Shows your profit on closed positions, which is the difference between your entry execution cost and exit execution costs, or (execution price + commissions to open the positions) – (execution price + commissions to close the position)                                                       | 显示已关闭持仓的利润，即入场成交成本与出场成交成本之间的差额，或（成交价+开仓佣金）–（成交价+平仓佣金）                                                                           |
| RegTEquity                       |                    | Regulation T equity for universal account                                                                                                                                                                                                                                                         | 通用账户的 Regulation T 股票权益                                                                                                                                                  |
| RegTEquity-S                     |                    | Regulation T equity for security segment                                                                                                                                                                                                                                                          | 证券部分的 Regulation T 股票权益                                                                                                                                                  |
| RegTMargin                       |                    | Regulation T margin for universal account                                                                                                                                                                                                                                                         | 通用账户的 T 法规保证金                                                                                                                                                           |
| RegTMargin-S                     |                    | Regulation T margin for security segment                                                                                                                                                                                                                                                          | 证券部分的 T 法规保证金                                                                                                                                                           |
| SMA                              |                    | Line of credit created when the market value of securities in a Regulation T account increase in value                                                                                                                                                                                            | 当监管 T 账户中的证券市值增加时产生的信贷额度                                                                                                                                     |
| SMA-S                            |                    | Regulation T Special Memorandum Account balance for security segment                                                                                                                                                                                                                              | 监管 T 特别备忘录账户证券板块的余额                                                                                                                                               |
| SegmentTitle                     | 分段标题           | Account segment name                                                                                                                                                                                                                                                                              | 账户分段名称                                                                                                                                                                      |
| StockMarketValue                 | 股票市场价值       | Real-time mark-to-market value of stock                                                                                                                                                                                                                                                           | 股票实时盯市价值                                                                                                                                                                  |
| TBondValue                       |                    | Value of treasury bonds                                                                                                                                                                                                                                                                           | 国债价值                                                                                                                                                                          |
| TBillValue                       |                    | Value of treasury bills                                                                                                                                                                                                                                                                           | 国库券价值                                                                                                                                                                        |
| TotalCashBalance                 | 总现金余额         | Total Cash Balance including Future PNL                                                                                                                                                                                                                                                           | 包含未来盈亏的总现金余额                                                                                                                                                          |
| TotalCashValue                   | 总现金价值         | Total cash value of stock, commodities and securities                                                                                                                                                                                                                                             | 股票、商品和证券的总现金价值                                                                                                                                                      |
| TotalCashValue-C                 | 总现金价值-C       | CashBalance in commodity segment                                                                                                                                                                                                                                                                  | 商品板块现金余额                                                                                                                                                                  |
| TotalCashValue-S                 | 总现金价值-S       | CashBalance in security segment                                                                                                                                                                                                                                                                   | 证券部分的现金余额                                                                                                                                                                |
| TradingType-S                    | 交易类型-S         | Account Type                                                                                                                                                                                                                                                                                      | 账户类型                                                                                                                                                                          |
| UnrealizedPnL                    | 未实现盈亏         | The difference between the current market value of your open positions and the average cost, or Value – Average Cost                                                                                                                                                                              | 您当前持仓的市场价值与平均成本之间的差额，或价值减去平均成本                                                                                                                      |
| WarrantValue                     |                    | Value of warrants                                                                                                                                                                                                                                                                                 | 认股权证的价值                                                                                                                                                                    |
| WhatIfPMEnabled                  |                    | To check projected margin requirements under Portfolio Margin model                                                                                                                                                                                                                               | 在投资组合保证金模型下检查预计保证金要求                                                                                                                                          |

#### Cancel Account Updates / 取消账户更新

Once the subscription to account updates is no longer needed, it can be cancelled by invoking the IBApi.EClient.reqAccountUpdates method while specifying the subscription flag to be False.

一旦不再需要订阅账户更新，可以通过调用 IBApi.EClient.reqAccountUpdates 方法并指定订阅标志为 False 来取消订阅。

Important: only one account at a time can be subscribed at a time. Attempting a second subscription without previously cancelling an active one will not yield any error message although it will override the already subscribed account with the new one. With Financial Advisory (FA) account structures there is an alternative way of specifying the account code such that information is returned for ‘All’ sub accounts- this is done by appending the letter ‘A’ to the end of the account number, i.e. reqAccountUpdates(true, “F123456A”)

重要提示：一次只能订阅一个账户。如果在取消当前活跃订阅之前尝试进行第二次订阅，虽然不会显示错误消息，但新订阅将覆盖已订阅的账户。对于金融顾问（FA）账户结构，可以通过在账户号码末尾附加字母“A”来指定账户代码，以便返回所有子账户的信息，即 reqAccountUpdates(true, “F123456A”)

``` python
EClient.reqAccountUpdates (

subscribe: bool. Set to true to start the subscription and to false to stop it.
订阅：布尔值。设置为 true 以开始订阅，设置为 false 以停止。

acctCode: String. The account id (i.e. U123456) for which the information is requested.
acctCode：字符串。请求信息的账户 id（例如 U123456）。

)
```

=== "Python"
    ``` python
    self.reqAccountUpdates(False, self.account)
    ```

### Account Update by Model / 通过模型更新账户

#### Requesting Account Update by Model / 通过模型请求账户更新

The IBApi.EClient.reqAccountUpdatesMulti can be used in any account structure to create simultaneous account value subscriptions from one or multiple accounts and/or models. As with IBApi.EClient.reqAccountUpdates the data returned will match that displayed within the TWS Account Window.

IBApi.EClient.reqAccountUpdatesMulti 可用于任何账户结构中，从一个或多个账户和/或模型创建同时的账户价值订阅。与 IBApi.EClient.reqAccountUpdates 类似，返回的数据将与 TWS 账户窗口中显示的数据相匹配。

    EClient.reqAccountUpdatesMulti (

        reqId: int. Identifier to label the request
        reqId: int. 用于标记请求的标识符

        account: String. Account values can be requested for a particular account
        account: String. 可请求特定账户的账户值

        modelCode: String. Values can also be requested for a model
        modelCode: String. 也可以请求模型的值

        ledgerAndNLV: bool. returns light-weight request; only currency positions as opposed to account values and currency positions
        ledgerAndNLV: bool. 返回轻量级请求；仅返回货币头寸，而不是账户值和货币头寸

    )

Requests account updates for account and/or model.

请求账户和/或模型的更新。

IBApi.EClient.reqAccountUpdatesMulti cannot be used with Account=”All” in IBroker accounts with more than 50 subaccounts.

IBApi.EClient.reqAccountUpdatesMulti 不能在拥有超过 50 个子账户的 IBroker 账户中使用 Account=”All”。

A profile name can be accepted in place of group in the account parameter for [Financial Advisors](https://www.interactivebrokers.com/campus/ibkr-api-page/trader-workstation-api/#financial-advisors)

在账户参数中，可以为财务顾问使用个人资料名称代替组名称

=== "Python"

    ``` python
    self.reqAccountUpdatesMulti(reqId, self.account, "", True)
    ```

    Code example:  
    代码示例：

    ``` python
    from ibapi.client import *
    from ibapi.wrapper import *
    import time

    class TradeApp(EWrapper, EClient): 
        def __init__(self): 
            EClient.__init__(self, self) 

        def accountUpdateMulti(self, reqId: int, account: str, modelCode: str, key: str, value: str, currency: str):
            print("AccountUpdateMulti. RequestId:", reqId, "Account:", account, "ModelCode:", modelCode, "Key:", key, "Value:", value, "Currency:", currency)
        
        def accountUpdateMultiEnd(self, reqId: int):
            print("AccountUpdateMultiEnd. RequestId:", reqId)
        
    app = TradeApp()      
    app.connect("127.0.0.1", 7496, clientId=1)

    time.sleep(1)

    app.reqAccountUpdatesMulti(103, 'U123456', "", True)

    app.run()
    ```

#### Receiving Account Updates by Model / 通过模型接收账户更新

The resulting account and portfolio information will be delivered via the IBApi.EWrapper.accountUpdateMulti and IBApi.EWrapper.accountUpdateMultiEnd

生成的账户和投资组合信息将通过 IBApi.EWrapper.accountUpdateMulti 和 IBApi.EWrapper.accountUpdateMultiEnd 交付

    EWrapper.accountUpdateMulti (

        requestId: int. The id of request.
        requestId: int. 请求的 ID。

        account: String. The account with updates.
        account: String. 带有更新的账户。

        modelCode: String. The model code with updates.
        modelCode: String. 带有更新的模型代码。

        key: String. The name of parameter.
        key: String. 参数的名称。

        value: String. The value of parameter.
        value: String. 参数的值。

        currency: String. The currency of parameter.
        currency: String. 参数的货币。
    )

Provides the account updates.

提供账户更新。

=== "Python"
    ``` python
    def accountUpdateMulti(self, reqId: int, account: str, modelCode: str, key: str, value: str, currency: str):
        print("AccountUpdateMulti. RequestId:", reqId, "Account:", account, "ModelCode:", modelCode, "Key:", key, "Value:", value, "Currency:", currency)
    ```

EWrapper.accountUpdateMultiEnd (

    requestId: int. The id of request
    requestId: int. 请求的 id

)

Indicates all the account updates have been transmitted.

表示所有账户更新信息已传输完毕。

=== "Python"
    ``` python
    def accountUpdateMultiEnd(self, reqId: int):
        print("AccountUpdateMultiEnd. RequestId:", reqId)
    ```

#### Cancelling Account Updates by Model / 通过模型取消账户更新

    EClient.reqAccountUpdatesMulti (

        reqId: int. Identifier to label the request
        reqId: int. 标识请求的标识符

        account: String. Account values can be requested for a particular account
        account: String. 可为特定账户请求账户值

        modelCode: String. Values can also be requested for a model
        modelCode: String. 也可以为模型请求值

        ledgerAndNLV: bool. Specify false to cancel your subscription.
        ledgerAndNLV: bool. 指定为 false 以取消您的订阅。

    )
=== "Python"
    ``` python
    self.reqAccountUpdatesMulti(reqId, self.account, "", False)
    ```

### Family Codes / 家族代码

It is possible to determine from the API whether an account exists under an account family, and find the family code using the function reqFamilyCodes.

可以通过 API 判断一个账户是否属于某个账户家族，并使用 reqFamilyCodes 函数找到家族代码。

For instance, if individual account U112233 is under a financial advisor with account number F445566, if the function reqFamilyCodes is invoked for the user of account U112233, the family code “F445566A” will be returned, indicating that it belongs within that account family.

例如，如果个人账户 U112233 属于一位财务顾问，其账户号为 F445566，当为账户 U112233 的用户调用 reqFamilyCodes 函数时，将返回家族代码“F445566A”，表明该账户属于该账户家族。

#### Request Family Codes / 请求家族代码

    EClient.reqFamilyCodes()

Requests family codes for an account, for instance if it is a FA, IBroker, or associated account.

请求账户的家庭代码，例如如果是 FA、IBroker 或关联账户。

=== "Python"
    ``` python
    self.reqFamilyCodes()
    ```

#### Receive Family Codes / 接收家族代码

    EWrapper.familyCodes(

        familyCodes: FamilyCodes[]. Unique family codes array of accountIds.
        familyCodes: FamilyCodes[]. 账户 ID 的唯一家族代码数组。

    )

Returns array of family codes.

返回家族代码数组。

=== "Python"
    ``` python
    def familyCodes(self, familyCodes: ListOfFamilyCode):
        print("Family Codes:", familyCodes)
    ```

### Managed Accounts / 管理账户

A single user name can handle more than one account. As mentioned in the Connectivity section, the TWS will automatically send a list of managed accounts once the connection is established. The list can also be fetched via the IBApi.EClient.reqManagedAccts method.

一个用户名可以管理多个账户。正如连接部分所述，一旦建立连接，TWS 将自动发送管理账户列表。该列表也可以通过 IBApi.EClient.reqManagedAccts 方法获取。

#### Request Managed Accounts / 请求管理账户

    EClient.reqManagedAccts()

Requests the accounts to which the logged user has access to.
请求登录用户可访问的账户。

self.reqManagedAccts()

#### Receive Managed Accounts / 接收管理账户

    EWrapper.managedAccounts (

        accountsList: String. A comma-separated string with the managed account ids.
        accountsList: String. 以逗号分隔的受管理账户 ID 字符串。

    )

Returns a string of all available accounts for the logged in user. Occurs automatically on initial API client connection.

返回登录用户所有可用账户的字符串。在初始 API 客户端连接时自动发生。

=== "Python"
    ``` python
    def managedAccounts(self, accountsList: str):
        print("Account list:", accountsList)
    ```

### Positions / 持仓

A limitation of the function ``IBApi.EClient.reqAccountUpdates`` is that it can only be used with a single account at a time. To create a subscription for position updates from multiple accounts, the function ``IBApi.EClient.reqPositions`` is available.

``IBApi.EClient.reqAccountUpdates`` 函数的一个限制是它一次只能用于一个账户。要为多个账户创建位置更新订阅，可以使用 ``IBApi.EClient.reqPositions`` 函数。

Note: The reqPositions function is not available in Introducing Broker or Financial Advisor master accounts that have very large numbers of subaccounts (> 50) to optimize the performance of TWS/IB Gateway. Instead the function reqPositionsMulti can be used to subscribe to updates from individual subaccounts. Also not available with IBroker accounts configured for on-demand account lookup.

注意：reqPositions 函数在拥有大量子账户（>50）的介绍经纪人或财务顾问主账户中不可用，以优化 TWS/IB Gateway 的性能。相反，可以使用 reqPositionsMulti 函数来订阅单个子账户的更新。配置为按需账户查找的 IBroker 账户也不可用。

After initially invoking reqPositions, information about all positions in all associated accounts will be returned, followed by the ``IBApi::EWrapper::positionEnd`` callback. Thereafter, when a position has changed an update will be returned to the ``IBApi::EWrapper::position`` function. To cancel a reqPositions subscription, invoke ``IBApi::EClient::cancelPositions``.

调用 reqPositions 后，会返回所有关联账户中所有持仓的信息，然后触发 ``IBApi::EWrapper::positionEnd`` 回调。之后，当持仓发生变化时，会通过 ``IBApi::EWrapper::position`` 函数返回更新。要取消 reqPositions 订阅，请调用 ``IBApi::EClient::cancelPositions``。

#### Request Positions / 请求持仓

EClient.reqPositions()

Subscribes to position updates for all accessible accounts. All positions sent initially, and then only updates as positions change.

订阅所有可访问账户的持仓更新。初始时发送所有持仓，之后仅发送持仓变更的更新。

=== "Python"
    ``` python
    self.reqPositions()
    ```
    Code example:  
    代码示例：
    ``` python
    from ibapi.client import *
    <!-- markdownlint-disable MD037 -->
    from ibapi.wrapper import *
    import threading
    import time

    class TradingApp(EWrapper, EClient):
        def __init__(self):
            EClient.__init__(self,self)

        def position(self, account: str, contract: Contract, position: Decimal, avgCost: float):
            print("Position.", "Account:", account, "Contract:", contract, "Position:", position, "Avg cost:", avgCost)
        
        def positionEnd(self):
            print("PositionEnd")
       
    def websocket_con():
        app.run()
    
    app = TradingApp()
    app.connect("127.0.0.1", 7496, clientId=1)

    con_thread = threading.Thread(target=websocket_con, daemon=True)
    con_thread.start()
    time.sleep(1)

    app.reqPositions()
    time.sleep(1)
    ```

#### Receive Positions / 接收持仓

    EWrapper.position(

        account: String. The account holding the position.
        账户：String。持有该持仓的账户。

        contract: Contract. The position’s Contract
        合约：Contract。该持仓的合约

        pos: decimal. The number of positions held. avgCost the average cost of the position.
        pos：decimal。持有的持仓数量。avgCost 该持仓的平均成本。

        avgCost: double. The total average cost of all trades for the currently held position.
        avgCost：double。当前持有的持仓所有交易的总体平均成本。
    )

Provides the portfolio’s open positions. After the initial callback (only) of all positions, the ``IBApi.EWrapper.positionEnd`` function will be triggered.

提供投资组合的未结仓位。在所有仓位完成初始回调（仅此一次）后，``IBApi.EWrapper.positionEnd`` 函数将被触发。

For futures, the exchange field will not be populated in the position callback as some futures trade on multiple exchanges

对于期货，由于部分期货在多个交易所交易，因此仓位回调中不会填充交易所字段。

=== "Python"
    ``` python
    def position(self, account: str, contract: Contract, position: Decimal, avgCost: float):
        print("Position.", "Account:", account, "Contract:", contract, "Position:", position, "Avg cost:", avgCost)
    ```

EWrapper.positionEnd()

Indicates all the positions have been transmitted. Only returned after the initial callback of EWrapper.position.

表示所有仓位信息已传输完成。仅在 EWrapper.position 的初始回调后返回。

=== "Python"
    ``` python
    def positionEnd(self):
        print("PositionEnd")
    ```

#### Cancel Positions Request / 取消持仓请求

EClient.cancelPositions()

Cancels a previous position subscription request made with ``EClient.reqPositions()``.

取消之前使用 ``EClient.reqPositions()`` 发起的仓位订阅请求。

=== "Python"
    ``` python
    self.cancelPositions()
    ```

### Positions By Model / 按模型查询仓位

The function ``IBApi.EClient.reqPositionsMulti`` can be used with any account structure to subscribe to positions updates for multiple accounts and/or models. The account and model parameters are optional if there are not multiple accounts or models available. It is more efficient to use this function for a specific subset of accounts than using ``IBApi.EClient.reqPositions``. A profile name can be accepted in place of group in the account parameter.

可以使用 ``IBApi.EClient.reqPositionsMulti`` 函数与任何账户结构一起订阅多个账户和/或模型的仓位更新。如果不存在多个账户或模型，则账户和模型参数是可选的。对于特定的账户子集，使用此函数比使用 ``IBApi.EClient.reqPositions`` 更高效。在账户参数中，可以使用配置文件名代替组名。

#### Request Positions By Model / 按模型请求持仓

    EClient.reqPositionsMulti(

        requestId: int. Request’s identifier.
        requestId: int. 请求的标识符。

        account: String. If an account Id is provided, only the account’s positions belonging to the specified model will be delivered.
        account: String. 如果提供了账户 ID，则仅会返回该账户属于指定模型的持仓。

        modelCode: String. The code of the model’s positions we are interested in.
        modelCode: String. 我们感兴趣的模型持仓的代码。
    )

Requests position subscription for account and/or model Initially all positions are returned, and then updates are returned for any position changes in real time.

为账户和/或模型请求持仓订阅。最初返回所有持仓，然后实时返回任何持仓变化的更新。

=== "Python"
    ``` python
    self.reqPositionsMulti(requestid, "U1234567", "")
    ```

    Code example:  
    代码示例：
    ``` python
    from ibapi.client import *
    from ibapi.wrapper import *
    import threading
    import time

    class TradingApp(EWrapper, EClient):
        def __init__(self):
            EClient.__init__(self,self)
                
        def positionMulti(self, reqId: int, account: str, modelCode: str, contract: Contract, pos: Decimal, avgCost: float):
        print("PositionMulti. RequestId:", reqId, "Account:", account, "ModelCode:", modelCode, "Contract:", contract, ",Position:", pos, "AvgCost:", avgCost)         
            
        def positionMultiEnd(self, reqId: int):
            print("")
            print("PositionMultiEnd. RequestId:", reqId)      

    def websocket_con():
        app.run()
        
    app = TradingApp()
    app.connect("127.0.0.1", 7497, clientId=1)

    con_thread = threading.Thread(target=websocket_con, daemon=True)
    con_thread.start()
    time.sleep(1) 

    app.reqPositionsMulti(2, "DU1234567", "")  #To specify a U-account number
    time.sleep(1)

    app.reqPositionsMulti(3, "Group1", "")     #To specify a Financial Advisor Group / Profile 
    time.sleep(1)
    ```

#### Receive Positions By Model / 按模型接收持仓

    EWrapper.positionMulti(

        requestId: int. The id of request
        requestId: int. 请求的 id

        account: String. The account holding the position.
        account: String. 持有该持仓的账户。

        modelCode: String. The model code holding the position.
        modelCode: String. 持有该持仓的模型代码。

        contract: Contract. The position’s Contract
        contract: Contract. 该持仓的合约

        pos: decimal. The number of positions held.
        pos: decimal. 持有的头寸数量。

        avgCost: double. The average cost of the position.
        avgCost: double. 头寸的平均成本。
    )

    Provides the portfolio’s open positions.
    
    提供投资组合中的未平仓头寸。

=== "Python"
    ``` python
    def positionMulti(self, reqId: int, account: str, modelCode: str, contract: Contract, pos: Decimal, avgCost: float):
        print("PositionMulti. RequestId:", reqId, "Account:", account, "ModelCode:", modelCode, "Contract:", contract, ",Position:", pos, "AvgCost:", avgCost)
    ```

EWrapper.positionMultiEnd(

    requestId: int. The id of request
    requestId: int. 请求的 id
)

Indicates all the positions have been transmitted.

表示所有仓位已传输。

=== "Python"
    ``` python
    def positionMultiEnd(self, reqId: int):
        print("PositionMultiEnd. RequestId:", reqId)
    ```

#### Cancel Positions By Model / 按模型取消仓位

    EClient.cancelPositionsMulti (

        requestId: int. The identifier of the request to be canceled.
        requestId: int. 要取消的请求的标识符。

    )

Cancels positions request for account and/or model.

取消账户和/或模型的头寸请求。

=== "Python"
    ``` python
    self.cancelPositionsMulti(requestid)
    ```

### Profit & Loss (PnL) / 损益 (PnL)

Requests can be made to receive real time updates about the daily P&L and unrealized P&L for an account, or for individual positions. Financial Advisors can also request P&L figures for ‘All’ subaccounts, or for a portfolio model. This is further extended to include realized P&L information at the account or individual position level.

可以请求接收有关账户每日损益和未实现损益的实时更新，或针对单个头寸。财务顾问还可以请求“所有”子账户的损益数据，或针对投资组合模型。这进一步扩展到包括账户或单个头寸的已实现损益信息。

The P&L API functions demonstrated below return the data which is displayed in the TWS Portfolio Window in current versions of TWS. As such, the P&L values are calculated based on the reset schedule specified in TWS Global Configuration (by default an instrument-specific reset schedule) and this setting affects values sent to the associated API functions as well. Also in TWS, P&L data from virtual forex positions will be included in the account P&L if and only if the Virtual Fx section of the Account Window is expanded.

下面展示的 P&L API 函数返回的数据，在 TWS 当前版本中会显示在 TWS 投资组合窗口中。因此，P&L 值是根据 TWS 全局配置中指定的重置计划（默认为特定工具的重置计划）计算的，并且此设置会影响发送到相关 API 函数的值。此外，在 TWS 中，只有当账户窗口的虚拟外汇部分展开时，虚拟外汇头寸的 P&L 数据才会包含在账户 P&L 中。

See [Account Updates](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#account-updates) for alternative PnL data.

另请参阅账户更新，以获取替代的 PnL 数据。

#### Request P&L for individual positions / 为单个头寸请求 P&L

Subscribe using the IBApi::EClient::reqPnLSingle function Cannot be used with IBroker accounts configured for on-demand lookup with account = ‘All’. Currently updates are returned to IBApi.EWrapper.pnlSingle approximately once per second*.

使用 IBApi::EClient::reqPnLSingle 函数订阅, 不能与配置为按需查找（账户='All'）的 IBroker 账户一起使用。目前更新大约每秒返回一次到 IBApi.EWrapper.pnlSingle*。

- If a P&L subscription request is made for an invalid conId or contract not in the account, there will not be a response.

    如果对无效的 conId 或账户中不存在的合约发起 P&L 订阅请求，则不会有响应。

- As elsewhere in the API, a max double value will indicate an ‘unset’ value. This corresponds to an empty cell in TWS.

    与 API 的其他部分一样，一个最大双精度值将表示一个“未设置”的值。这对应于 TWS 中的空白单元格。

- Introducing broker accounts without a large number of subaccounts (<50) can receive aggregate data by specifying the account as “All”.

    引入没有大量子账户（<50）的经纪账户可以通过指定账户为“所有”来接收聚合数据。

- *Cannot be used with IBroker accounts configured for on-demand lookup with account = ‘All’

    *不能与配置为按需查找且账户为‘所有’的 IBroker 账户一起使用

*subject to change in the future.

*未来可能变更。

    EClient.reqPnLSingle (

        reqId: int. Request identifier for to track the data.
        reqId: int. 用于跟踪数据的请求标识符。

        account: String. Account in which position exists
        account: String. 存在持仓的账户。

        modelCode: String. Model in which position exists
        modelCode: String. 存在持仓的模型

        conId: int. Contract ID (conId) of contract to receive daily PnL updates for. Note: does not return message if invalid conId is entered
        conId: int. 接收每日盈亏更新的合约 ID（conId）。注意：如果输入无效的 conId，则不会返回消息

    )

Requests real time updates for daily PnL of individual positions.

请求单个持仓的实时每日盈亏更新。

=== "Python"
    ``` python
    self.reqPnLSingle(requestId, "U1234567", "", 265598)
    ```
    Code example:  
    代码示例：
    ``` python
    from ibapi.client import *
    from ibapi.wrapper import *
    import time

    class TradeApp(EWrapper, EClient): 
        def __init__(self): 
            EClient.__init__(self, self) 

        def pnlSingle(self, reqId: int, pos: Decimal, dailyPnL: float, unrealizedPnL: float, realizedPnL: float, value: float):
            print("Daily PnL Single. ReqId:", reqId, "Position:", pos, "DailyPnL:", dailyPnL, "UnrealizedPnL:", unrealizedPnL, "RealizedPnL:", realizedPnL, "Value:", value)
        
    app = TradeApp()
    app.connect("127.0.0.1", 7496, clientId=1)

    time.sleep(1)
    app.reqPnLSingle(101, "U123456", "", 8314) #IBM conId: 8314

    app.run()
    ```

#### Receive P&L for individual positions / 接收单个持仓的盈亏

    EWrapper.pnlSingle (

        reqId: int. Request identifier used for tracking.
        reqId: int. 请求标识符，用于跟踪。

        pos: decimal. Current size of the position
        pos: decimal. 当前持仓的规模

        dailyPnL: double. DailyPnL for the position
        dailyPnL: double. 仓位每日盈亏

        unrealizedPnL: double. Total unrealized PnL for the position (since inception) updating in real time
        unrealizedPnL: double. 仓位自开仓以来的总未实现盈亏（实时更新）

        realizedPnL: double. Total realized PnL for the position (since inception) updating in real time
        realizedPnL: double. 仓位自开仓以来的总已实现盈亏（实时更新）

        value: double. Current market value of the position.
        value: double. 仓位当前市场价值
    )

Receives real time updates for single position daily PnL values

接收单个持仓每日盈亏的实时更新

=== "Python"
    ``` python
    def pnlSingle(self, reqId: int, pos: Decimal, dailyPnL: float, unrealizedPnL: float, realizedPnL: float, value: float):
        print("Daily PnL Single. ReqId:", reqId, "Position:", pos, "DailyPnL:", dailyPnL, "UnrealizedPnL:", unrealizedPnL, "RealizedPnL:", realizedPnL, "Value:", value)
    ```

#### Cancel P&L request for individual positions / 取消单个持仓的盈亏请求

    EClient.cancelPnLSingle (

        reqId: int. Request identifier to cancel the P&L subscription for.
        reqId: int. 用于取消 P&L 订阅的请求标识符。
    )

    Cancels real time subscription for a positions daily PnL information.

    取消订阅实时获取持仓每日 P&L 信息。

    === "Python"
    ``` python
    self.cancelPnLSingle(requestId);
    ```

#### Request P&L for accounts / 请求账户的 P&L 信息

Subscribe using the IBApi::EClient::reqPnL function. Updates are sent to IBApi.EWrapper.pnl.

使用 IBApi::EClient::reqPnL 函数订阅。更新信息将发送至 IBApi.EWrapper.pnl。

- Introducing broker accounts with less than 50 subaccounts can receive aggregate PnL for all subaccounts by specifying ‘All’ as the account code.

    介绍：拥有少于 50 个子账户的经纪账户可以通过将账户代码指定为“全部”来接收所有子账户的汇总盈亏。

- With requests for advisor accounts with many subaccounts and/or positions can take several seconds for aggregated P&L to be computed and returned.

    对于拥有许多子账户和/或头寸的建议账户，计算并返回汇总盈亏的请求可能需要几秒钟的时间。

- For account P&L data the TWS setting “Prepare portfolio PnL data when downloading positions” must be checked.

    对于账户盈亏数据，TWS 设置“下载头寸时准备投资组合盈亏数据”必须被勾选。

    EClient.reqPnL (

        reqId: int. Request ID to track the data.
        reqId: int. 用于跟踪数据的请求 ID。

        account: String. Account for which to receive PnL updates
        account: String. 接收盈亏更新（PnL）的账户。

        modelCode: String. Specify to request PnL updates for a specific model.
        modelCode: String. 指定请求特定模型的盈亏更新（PnL）。
    )

Creates subscription for real time daily PnL and unrealized PnL updates.

创建订阅以接收实时每日盈亏（PnL）和未实现盈亏（PnL）的更新。

=== "Python"
    ``` python
    self.reqPnL(reqId, "U1234567", "")
    ```
    Code example:  
    代码示例：
    ``` python
    from ibapi.client import *
    <!-- markdownlint-disable MD037 -->
    from ibapi.wrapper import *
    import time

    class TradeApp(EWrapper, EClient): 
        def __init__(self): 
            EClient.__init__(self, self) 

        def pnl(self, reqId: int, dailyPnL: float, unrealizedPnL: float, realizedPnL: float):
            print("Daily PnL. ReqId:", reqId, "DailyPnL:", dailyPnL, "UnrealizedPnL:", unrealizedPnL, "RealizedPnL:", realizedPnL)
        
    app = TradeApp()      
    app.connect("127.0.0.1", 7496, clientId=1)

    time.sleep(1)
    app.reqPnL(102, "U123456", "")

    app.run()
    ```

#### Receive P&L for accounts / 接收账户盈亏

    EWrapper.pnl (

        reqId: int. Request identifier for tracking data.
        reqId: int. 用于跟踪数据的请求标识符。

        dailyPnL: double. DailyPnL updates for the account in real time
        dailyPnL: double. 实时更新账户的每日盈亏

        unrealizedPnL: double. Total Unrealized PnL updates for the account in real time
        unrealizedPnL: double. 实时更新账户的总未实现盈亏

        realizedPnL: double. Total Realized PnL updates for the account in real time
        realizedPnL: double. 实时更新账户的总已实现盈亏

    )

=== "Python"
    ``` python
    def pnl(self, reqId: int, dailyPnL: float, unrealizedPnL: float, realizedPnL: float):
        print("Daily PnL. ReqId:", reqId, "DailyPnL:", dailyPnL, "UnrealizedPnL:", unrealizedPnL, "RealizedPnL:", realizedPnL)
    ```

#### Cancel P&L subscription requests for accounts / 取消账户的盈亏订阅请求

    EClient.cancelPnL (

        reqId: int. Request identifier for tracking data.
        reqId: int. 请求标识符，用于跟踪数据。
    )

Cancels subscription for real time updated daily PnL params reqId

取消订阅实时更新的每日 PnL 参数的 reqId

    === "Python"
    ``` python
    self.cancelPnL(reqId)
    ```

### White Branding User Info / 白标用户信息

This function will return [White Branding ID](https://www.interactivebrokers.com/en/trading/white-branding.php) associated with the user.

这个函数将返回与用户关联的白色品牌 ID。

Please note, that nothing will be returned if requesting username is not associated with any White Branding entity.

请注意，如果请求的用户名不与任何白色品牌实体相关联，则不会返回任何内容。

#### Requesting White Branding Info / 请求白色品牌信息

    EClient.reqUserInfo(

        reqId: int. Request ID  
        reqId: int. 请求 ID

    )

    === "Python"
    ``` python
    self.reqUserInfo(reqId)
    ```

#### Receiving White Branding Info / 接收白标品牌信息

    EWrapper.userInfo (

        reqId: int. Identifier for the given request.
        reqId: int. 给定请求的标识符。

        whiteBrandingId: String. Identifier for the white branded entity.
        whiteBrandingId: String. 白色品牌实体的标识符。
    )

    === "Python"
    ``` python
    def userInfo(self, reqId: int, whiteBrandingId: str):
        print("UserInfo.", "ReqId:", reqId, "WhiteBrandingId:", whiteBrandingId)
    ```
