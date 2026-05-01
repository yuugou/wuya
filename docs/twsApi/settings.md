# TWS API DocumentationTWS API 文档

## TWS Settings / TWS 设置

Some TWS Settings affect API.

某些 TWS 设置会影响 API。

### TWS Configuration For API Use / TWS API 使用配置

The settings required to use the TWS API with the Trader Workstation are managed in the Global Configuration under “API” -> “Settings”

使用交易工作站中 TWS API 所需的设置在全局配置中的 “API” -> “设置” 下进行管理

In this section, only the most important API settings for API connection are covered.

在本节中，仅涵盖 API 连接的最重要设置。

Please:

请：

- Enable “ActiveX and Socket Clients”
- 启用“ActiveX 和 Socket 客户端”
- Disable “Read-Only API”
- 禁用“只读 API”
- Verify the “Socket Port” value
- 验证“Socket 端口”值

![TWS Global Configuration window displaying API Settings and the required API configuration.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/API-settings-1100x768.png)

### Best Practice: Configure TWS / IB Gateway / 最佳实践：配置 TWS / IB Gateway

The information listed below are not required or necessary in order to operate the TWS API. However, these steps include many references which can help improve the day to day usage of the TWS API that is not explicitly offered as a callable method within the API itself.

下面列出的信息并非使用 TWS API 所必需的。然而，这些步骤包含许多参考内容，可以帮助提高 TWS API 的日常使用效率，而这些效率在 API 本身提供的可调用方法中并未明确说明。

#### "Never Lock Trader Workstation" Setting / "永远不要锁定交易工作台"设置

Note: For IBHK API users, it is commended to use IB Gateway instead of TWS. It is because all IBHK users cannot choose “Never Lock Trader Workstation” in TWS – Global Configuration – Lock and Exit. If there is inactivity, TWS will be locked and there will be API disconnection.

注意：对于 IBHK API 用户，推荐使用 IB Gateway 代替 TWS。这是因为所有 IBHK 用户在 TWS 的“全局配置”->“锁定和退出”中都无法选择“永不锁定交易工作台”。如果出现不活动状态，TWS 将被锁定，导致 API 断开连接。

#### Memory Allocation / 内存分配

In TWS/ IB Gateway – “Global Configuration” – “General”, you can adjust the Memory Allocation (in MB)*.

在 TWS/ IB Gateway – “Global Configuration” – “General” 中，您可以调整内存分配（以 MB 为单位）*。

This feature is to control how much memory your computer can assign to the TWS/ IB Gateway application. Usually, higher value allows users to have faster data returning speed.

此功能用于控制您的计算机可以分配给 TWS/ IB Gateway 应用程序的内存量。通常，较高的值可以让用户获得更快的返回速度。

Normally, it is recommended for API users to set 4000. However, it depends on your computer memory size because setting too high may cause High Memory Usage and application not responding.

通常建议 API 用户设置为 4000。但是，这取决于您的计算机内存大小，因为设置过高可能会导致内存使用过高和应用程序无响应。

![TWS Global Configuration window displaying General Settings and the Memory Allocation section.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/%E6%93%B7%E5%8F%962-1100x691.png)

For details, please visit: <https://www.ibkrguides.com/traderworkstation/increase-tws-memory-size.htm>

详细信息，请访问：<https://www.ibkrguides.com/traderworkstation/increase-tws-memory-size.htm>

Note:

注意：

> In IB Gateway Global Configuration – API – settings, there is no “Compatibility Mode: Send ISLAND for US stocks trading on NASDAQ”. Specifying NASDAQ exchange in contract details may cause error if connecting to IB Gateway. For this error, please specify ISLAND exchange.
>
> 在 IB Gateway 全局配置—API—设置中，没有“兼容模式：为在纳斯达克交易的美国股票发送 ISLAND”选项。如果在连接 IB Gateway 时在合约详细信息中指定纳斯达克交易所，可能会导致错误。对于此错误，请指定 ISLAND 交易所。

#### Daily & Weekly Reauthentication / 每日和每周重新认证

##### Daily Reauthentication / 每日重新认证

In TWS/ IB Gateway – “Global Configuration” – “Lock and Exit”, you can choose the time of your TWS being shut down.

在 TWS/ IB Gateway – “全局配置” – “锁定和退出”中，您可以选择 TWS 关闭的时间。

For API users, it is recommended to choose “Never lock Trader Workstation” and “Auto restart”.

对于 API 用户，建议选择“永不锁定交易工作台”和“自动重启”。

![TWS Global Configuration window displaying Lock and Exit Settings.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/Image-3-1.png)

Note:

注意：

> IBHK users do not have “Never lock Trader Workstation” and “Auto restart” in TWS. It is suggested for IBHK users to use IB Gateway in order to have stable API connection because IB Gateway won’t be locked due to inactivity. Also, IBHK users can choose “Auto restart” in IB Gateway.
>
> IBHK 用户在 TWS 中没有“永不锁定 Trader Workstation”和“自动重启”功能。建议 IBHK 用户使用 IB Gateway 以获得稳定的 API 连接，因为 IB Gateway 不会因不活动而锁定。此外，IBHK 用户可以在 IB Gateway 中选择“自动重启”。

##### Weekly Reauthentication / 每周重新认证

The weekly authentication cycle starts on every Monday. If you receive ``Login failed = Soft token=0 received instead of expected permanent for zdc1.ibllc.com:4001 (SSL)``,  this means you need to manually login again to complete the weekly reauthentication task.

每周认证周期从每个星期一开始。如果您收到 ``Login failed = Soft token=0 received instead of expected permanent for zdc1.ibllc.com:4001 (SSL)``，这意味着您需要手动登录以完成每周重新认证任务。

#### Order Precautions / 订单注意事项

In TWS – “Global Configuration” – “API” – “Precautions”, you can enable the following items to stop receiving the order submission messages.

在 TWS 的“全局配置”–“API”–“注意事项”中，您可以启用以下项目以停止接收订单提交消息。

- Enable “Bypass Order Precautions for API orders”.
- 启用“API 订单绕过订单注意事项”。
- Enable “Bypass Bond warning for API orders”.
- 启用“API 订单绕过债券警告”。
- Enable “Bypass negative yield to worst confirmation for API orders”.
- 启用“API 订单绕过负收益率到最差确认”。
- Enable “Bypass Called Bond warning for API orders”.
- 启用“API 订单绕过已成交债券警告”。
- Enable “Bypass “same action pair trade” warning for API orders”.
- 启用“API 订单绕过‘同行动作对冲交易’警告”。
- Enable “Bypass price-based volatility risk warning for API orders”.
- 启用“API 订单绕过基于价格的波动风险警告”。
- Enable “Bypass US Stocks market data in shares warning for API orders”.
- 启用“API 订单绕过美国股票市场数据基于股票数量的警告”。
- Enable “Bypass Redirect Order warning for Stock API orders”.
- 启用“绕过股票 API 订单的重定向警告”。
- Enable “Bypass No Overfill Protection precaution for destinations where implied natively”.
- 启用“绕过目的地原生隐含的未满额保护预防措施”。

![TWS Global Configuration window displaying API Precautions.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/%E6%93%B7%E5%8F%964-1100x699.png)

#### Connected IB Server Location in TWS / TWS 中的 IB 服务器连接位置

Each IB account has a pre-decided IB server. You can visit this link to know our IB servers’ locations: <https://www.interactivebrokers.com/download/IB-Host-and-Ports.pdf>

每个 IB 账户都有一个预先确定的 IB 服务器。您可以访问此链接了解我们的 IB 服务器位置：<https://www.interactivebrokers.com/download/IB-Host-and-Ports.pdf>

Yet, all IB paper accounts are connected to US server by default and its location cannot be changed.

然而，所有 IB 模拟账户默认都连接到美国服务器，其位置无法更改。

As IB servers in different regions have different scheduled server maintenance time ( <https://www.interactivebrokers.com/en/software/systemStatus.php> ), you may need to change the IB server location in order to avoid service downtime.

由于不同地区的 IB 服务器有不同的计划维护时间（ <https://www.interactivebrokers.com/en/software/systemStatus.php> ），您可能需要更改 IB 服务器位置以避免服务中断。

For checking your connected IB server location, you can go to TWS and click “Data” to see your Primary server. In the below image, the pre-decided IB server location is: cdc1.ibllc.com

要检查您连接的 IB 服务器位置，您可以进入 TWS 并点击“数据”查看您的主服务器。在下图中，预先确定的 IB 服务器位置是：cdc1.ibllc.com

![TWS Connections Window.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/%E6%93%B7%E5%8F%965.png)

If you want to change your live IB account server location in TWS, please submit a web ticket to “Technical Assistance” – “Connectivity” in order to request changing the IB server location.

如果您想在 TWS 中更改您的实时 IB 账户服务器位置，请向“技术支持”下的“连接性”提交网页工单，以请求更改 IB 服务器位置。

In the web ticket, you need to provide:

在网页工单中，您需要提供：

1. Which account do you want to have IB server location change?
1. 您希望更改 IB 服务器位置的账户是哪一个？
1. Which IB server location would you like to connect to?
1. 您希望连接到哪个 IB 服务器位置？
    - TWS AMERICA – EAST (New York)
    - TWS 美国 – 东部（纽约）
    - TWS AMERICA – CENTRAL (Chicago)
    - TWS 美国 – 中部（芝加哥）
    - TWS Europe (Zurich)
    - TWS 欧洲（苏黎世）
    - TWS Asia (Hong Kong)
    - TWS 亚洲（香港）
    - TWS Asia – CHINA (For mainland China users, if the account server is hosted in Hong Kong, they will automatically connect with the Shenzhen Gateway mcgw1.ibllc.com.cn)
    - TWS 亚洲 – 中国（对于中国大陆用户，如果账户服务器位于香港，将自动连接至深圳网关 mcgw1.ibllc.com.cn）
1. Which IB scheduled maintenance time do you choose? (Recommended to choose the default schedule maintenance time of its own IB server location)
1. 您选择哪个 IB 计划维护时间？（建议选择其 IB 服务器所在地的默认计划维护时间）
    - North America
    - 北美洲
    - Europe
    - 欧洲
    - Asia
    - 亚洲

After you submit the ticket, you will receive a web ticket reply which require you to confirm and understand the migration request.

提交工单后，您将收到一个网页工单回复，该回复需要您确认和理解迁移请求。

Note:

注意：

1. For Internet users, as the connection between IB server and Exchange goes through a dedicated line, it is commonly recommended to choose a IB server location which is closer to your TWS location. For IB connection types, please visit: <https://www.interactivebrokers.co.uk/en/software/connectionInterface.php>
1. 对于互联网用户，由于 IB 服务器与交易所之间的连接通过专用线路，通常建议选择一个距离你的 TWS 位置更近的 IB 服务器位置。关于 IB 连接类型，请访问：<https://www.interactivebrokers.co.uk/en/software/connectionInterface.php>
1. The pre-decided IB server location connected from TWS is different from the IB Server location connected from IB Client Portal and IBKR Mobile.
1. 从 TWS 连接的预选 IB 服务器位置与从 IB 客户门户和 IBKR 移动连接的 IB 服务器位置不同。
    - IB server location connected from TWS is pre-decided. You can submit a web ticket to request the IB server relocation for the TWS connection.
    - 从 TWS 连接的 IB 服务器位置是预选的。您可以提交一个网页工单来请求为 TWS 连接重新分配 IB 服务器位置。
    - IB server location connected from Client Portal or IBKR Mobile is based on your nearest IB server location. You cannot request the IB server relocation for Client Portal and IBKR Mobile connections. OAuth CP API users now cannot specify which server they want to connect to by themselves.
    - 从客户端门户或 IBKR 移动端连接的 IB 服务器位置基于离您最近的 IB 服务器位置。您无法请求重新分配客户端门户和 IBKR 移动端连接的 IB 服务器。OAuth CP API 用户现在无法自行指定要连接的服务器。

#### SMART Algorithm / SMART 算法

In TWS Global Configuration – Orders – Smart Routing, you can set your SMART order routing algorithm. For available SMART Routing via TWS API, please visit: <https://www.interactivebrokers.com/campus/ibkr-api-page/contracts/#smart-routing>

在 TWS 全局配置—订单—智能路由中，您可以设置您的 SMART 订单路由算法。有关通过 TWS API 的 SMART 路由，请访问：<https://www.interactivebrokers.com/campus/ibkr-api-page/contracts/#smart-routing>

![TWS Global Configuration window displaying Smart Routing.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/%E6%93%B7%E5%8F%96-1-1100x692.png)

#### Allocation Setup (For Financial Advisors) / 分配设置（针对财务顾问）

In TWS Global Configuration – Advisor Setup – Presets, you can need to choose Allocation Preference in order to avoid wrong allocation result.

在 TWS 全局配置——顾问设置——预设中，您需要选择分配偏好以避免错误的分配结果。

![TWS Global Configuration window displaying Presets for Advisors.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/Advisor-setup.png)

### Intelligent Order Resubmission / 智能订单重提交

The TWS Setting listed in the Global Configuration under API -> Setting for Maintain and resubmit orders when connection is restored, is enabled by default in TWS 10.28 and above. When this setting is checked, all orders received while connectivity is lost will be saved and automatically resubmitted when connectivity is restored. Please note, if the Trader Workstation is closed during this time, the orders are deleted regardless of the setting.

在 API -> 设置中的全局配置下，用于在连接恢复时维持和重新提交订单的 TWS 设置，在 TWS 10.28 及以上版本中默认启用。当此设置被勾选时，在连接中断期间收到的所有订单将被保存，并在连接恢复时自动重新提交。请注意，如果在此时关闭交易工作台，无论设置如何，订单都将被删除。

Beginning with Trader Workstation and IB Gateway 10.40, the Global Configuration -> API -> Settings will provide a new setting for “Maintain and resubmit orders when connection is restored.” This setting will automatically maintain or submit any orders on the platform after a network disconnect or the auto-restart behavior.

从交易工作台和 IB 网关 10.40 开始，全局配置 -> API -> 设置，将提供一个新的设置项“在连接恢复时维持和重新提交订单”。此设置将在网络断开或自动重启行为后，自动维持或提交平台上的任何订单。

### Disconnect on Invalid Format / 无效格式断开连接

The TWS Setting listed in the Global Configuration under API -> Setting for Maintain connection upon receiving incorrectly formatted fields, is enabled by default in TWS 10.28 and above. For clients operating on Client Version 100 and above, users will not disconnect from fields with invalid value submissions when the setting is enabled.

在 API -> 设置下的全局配置中列出的 TWS 设置，在 TWS 10.28 及以上版本中默认启用。对于使用客户端版本 100 及以上的客户，当该设置启用时，不会在与无效值提交相关的字段中断开连接。
