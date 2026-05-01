# TWS API Documentation / TWS API 文档

## Synchronous API / 同步 API

With the release of TWS API 10.40, Interactive Brokers has introduced the Synchronous API Wrapper class. This class provides a synchronous API structure, combining the functionality of EClient and EWrapper into a beginner-friendly interface.

随着 TWS API 10.40 的发布，Interactive Brokers 推出了同步 API 包装类。该类提供了一个同步 API 结构，将 EClient 和 EWrapper 的功能结合到一个对初学者友好的界面中。

The current release is still in a Beta state, slowly rolling out only a portion of what is available in the larger Trader Workstation API configuration. The interface is exclusively available through the Python programming language.

当前版本仍处于 Beta 状态，仅逐步推出 Trader Workstation API 配置中可用功能的一部分。该界面仅通过 Python 编程语言提供。

The content shown here is an example of what the Sync Wrapper structure looks like. A larger example of all current functionality is available in the 10.40 release of the TWS API under {TWS API}/samples/Python/Testbed/sync_test.py .

这里展示的内容是 Sync Wrapper 结构的示例。所有当前功能的更大示例可在 TWS API 的 10.40 版本下的 {TWS API}/samples/Python/Testbed/sync_test.py 中找到。

Request sample

请求示例

```python
# Import our Sync Wrapper and Contract objects
from ibapi.sync_wrapper_alt import *
from datetime import datetime
# Instantiate the reference for our sync class
app = TWSSyncWrapper(timeout=30)
# make a connection to Trader Workstation
# In this case, we're connecting on Localhost with port 7496 and Client ID 0.
if not app.connect_and_start(host="127.0.0.1", port=7496, client_id=8675309):
    print("Failed to connect to TWS")
    exit(1)
else:
    print("Connected to TWS")
 
# Create a contract class reference.
# In our case, we'll be testing with AAPL.
contract = Contract()
contract.symbol = "AAPL"
contract.secType = "STK"
contract.exchange = "SMART"
contract.primaryExchange = "ISLAND"
contract.currency = "USD"
'''
Contract details requests will return all contracts the match the details
of our contract object in a list. Because a list is returned, we are 
taking the first (or 0 index) contract returned. 
'''
aapl_contract = app.get_contract_details(contract)[0].contract
print(aapl_contract)
market_data = app.get_market_data_snapshot(aapl_contract)
order = Order()
order.action = "BUY"
order.orderType = "LMT"
order.totalQuantity = 100
order.lmtPrice = 258
order_status = app.place_order_sync(contract, order)
oid = order_status["orderId"]
print(app.get_open_orders()[oid]['orderState'].status)
print(app.cancel_order_sync(oid, OrderCancel()))
app.disconnect_and_stop()
exit()
```

Response Sample

响应示例

```powershell
ERROR -1 1761170335710 2104 Market data farm connection is OK:usbond
ERROR -1 1761170335711 2104 Market data farm connection is OK:usfarm.nj
ERROR -1 1761170335712 2104 Market data farm connection is OK:eufarm
ERROR -1 1761170335712 2104 Market data farm connection is OK:usfarm
ERROR -1 1761170335712 2106 HMDS data farm connection is OK:ushmds
ERROR -1 1761170335713 2158 Sec-def data farm connection is OK:secdefil
Connected to TWS
ConId: 265598, Symbol: AAPL, SecType: STK, LastTradeDateOrContractMonth: , Strike: 0, Right: , Multiplier: , Exchange: SMART, PrimaryExchange: ISLAND, Currency: USD, LocalSymbol: AAPL, TradingClass: NMS, IncludeExpired: False, SecIdType: , SecId: , Description: , IssuerId: Combo:
{'price': {1: {'price': 258.5, 'attrib': 2076793531408: CanAutoExecute: 1, PastLimit: 0, PreOpen: 0}, 2: {'price': 258.65, 'attrib': 2076793531536: CanAutoExecute: 1, PastLimit: 0, PreOpen: 0}, 4: {'price': 258.62, 'attrib': 2076793531600: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}, 6: {'price': 262.85, 'attrib': 2076793531856: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}, 7: {'price': 255.43, 'attrib': 2076793531920: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}, 9: {'price': 262.77, 'attrib': 2076793531984: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}, 14: {'price': 262.74, 'attrib': 2076793532048: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}}, 'size': {0: Decimal('1'), 3: Decimal('5'), 5: Decimal('3'), 8: Decimal('449348')}}
PreSubmitted
{'orderId': 358, 'status': 'PreSubmitted', 'filled': Decimal('0'), 'remaining': Decimal('100'), 'avgFillPrice': 0.0, 'permId': 1054257323, 'parentId': 0, 'lastFillPrice': 0.0, 'clientId': 8675309, 'whyHeld': '', 'mktCapPrice': 0.0}
```

### TWSSyncWrapper Class / TWSSyncWrapper 类

The TWSSyncWrapper class is produced from the ibapi/sync_wrapper file. Clients looking to utilize the class may seek to replace their typical imports for ibapi/client and ibapi/wrapper with an import for “from ibapi.sync_wrapper import TWSSyncWrapper”.

TWSSyncWrapper 类由 ibapi/sync_wrapper 文件生成。希望利用该类的客户端可以考虑将通常对 ibapi/client 和 ibapi/wrapper 的导入替换为对 “from ibapi.sync_wrapper import TWSSyncWrapper” 的导入。

The TWSSyncWrapper class accepts a single argument, timeout. This will provide a default timeout integer in seconds for all connected functions to work with. If no timeout is specified, a default value of 30 seconds is passed instead.

TWSSyncWrapper 类接受一个参数，即 timeout。这将提供一个默认的秒数超时整数，供所有连接的函数使用。如果没有指定超时，则会传递一个 30 秒的默认值。

Each function supports a timeout argument for unique endpoint timeout behavior.

每个函数都支持一个 timeout 参数，用于实现独特的端点超时行为。

=== "python"

    ``` python
    from ibapi.sync_wrapper import TWSSyncWrapper

    app = TWSSyncWrapper(timeout=30)
    ```

### Connect & Start Connection / 连接并启动连接

After creating the class object reference with sync wrapper, connect_and_start() must be used to connect the Python program with the active Trader Workstation implementation. Identical to EClient’s connect() function, connect_and_start() supports arguments for host, port, and client_id.

在创建同步包装器类对象引用后，必须使用 connect_and_start()将 Python 程序与活动的 TWS 连接起来。与 EClient 的 connect()函数相同，connect_and_start()支持主机、端口和 client_id 的参数。

```python
connect_and_start(

    host: String. Determine the connecting host IP for the API to connect to. Connections on the same computer should use “localhost” or “127.0.0.1”.
    host: String. 确定 API 连接的主机 IP 地址。同一台计算机上的连接应使用“localhost”或“127.0.0.1”。

    port: Integer. Determine the connecting port number configured in the Global Configuration in the “Socket Port” field.
    port: Integer. 确定在全局配置中“Socket Port”字段中配置的连接端口编号。

        Defaults: {TWS Live: 7496; TWS Paper: 7497; IBG Live: 4001; IBG Paper: 4002′}
        默认值：{TWS 实盘：7496；TWS 模拟：7497；IBG 实盘：4001；IBG 模拟：4002}

    client_id: Integer. Determine the connecting client ID. TWS Supports up to 32 simultaneous API connections.
    client_id: 整数。确定连接的客户端 ID。TWS 支持最多 32 个同时的 API 连接。

        Users should connect with a client_id of 0 for [optimal order management functionality](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#master-client-id).
        用户应使用 client_id 为 0 进行连接，以获得[最佳订单管理功能](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#master-client-id)。
)
```

=== "Python"

    ```python
    app.connect_and_start(host="127.0.0.1", port=7496, client_id=0)
    ```

#### Response Object / 响应对象

While it is not necessary to handle the response from connect_and_start(), the function will return the result of EClient.isConnected() to help with connection validation.

虽然处理 connect_and_start() 的响应不是必需的，但该函数会返回 EClient.isConnected() 的结果，以帮助进行连接验证。

The function call will return a single Boolean value, True or False, in reference to the connection status at the time of reference.

函数调用将返回一个布尔值，True 或 False，指示参考时的连接状态。

Developers may look to implement code such as this that will gracefully handle the connection procedure should it fail to connect rather than proceeding with the rest of the code implementation.

开发者可以参考实现类似代码，以便在连接失败时优雅地处理连接过程，而不是继续执行其余的代码实现。

=== "Python"

    ``` python
    # Connect to TWS
    # If the connection succeeded, notify the user
    # If the connection fails and False is returned, notify the user and gracefully exit the application
    if not app.connect_and_start(host="127.0.0.1", port=7496, client_id=0):
        print("Failed to connect to TWS")
        exit(1)
    else:
        print("Connected to TWS")
    ```

### Disconnect & Stop Connection / 断开连接 & 停止连接

Once a connection is no longer needed, developers should disconnect the session. This will terminate all ongoing requests through the class’s client_id. Connections through any other client ID or port will be unaffected.

一旦不再需要连接，开发者应断开会话。这将终止通过该类的 client_id 进行的所有进行中的请求。通过任何其他 client ID 或端口的连接将不受影响。

``` python
disconnect_and_stop()
```

=== "Python"

    ``` python
    app.disconnect_and_stop()
    ```

The function call does not return after calling. As a result, None is automatically passed in the event the function is referenced.

调用该函数后不会返回。因此，如果引用该函数，将自动传递 None。

### Current Time / 当前时间

Whenever a user would need to verify the current time used within Trader Workstation or to verify the connection with the application, users may call the get_current_time() function.

每当用户需要验证 Trader Workstation 中使用的当前时间，或验证与应用程序的连接时，用户可以调用 get_current_time()函数。

``` python
get_current_time(
    timeout: Integer. Timeout before the request disconnects. Function-specific timeout default of 1 second.
    timeout: Integer. 请求断开前的超时时间。函数特定的超时默认值为 1 秒。
)
```

=== "Python"

    ``` python
    app.get_current_time()
    ```

#### Response Object / 响应对象

get_current_time() will return the current timestamp as an integer representing an epoch timestamp.

get_current_time() 将返回当前时间戳，以表示为 UNIX 时间戳的整数形式。

```powershell
1760478515
```

### Next Valid ID  下一个有效 ID

Requests should utilize an unique identifier after each request is submitted.

请求在提交后应使用一个唯一标识符。

The same order identifier cannot be reused except to modify an existing order.

相同的订单标识符不能被重复使用，除非用于修改现有订单。

``` python
get_next_valid_id(

    timeout: Integer. Uses default timeout value passed to TWSSyncClass.
    timeout: Integer. 使用传递给 TWSSyncClass 的默认超时值。

)
```

=== "Python"

    ``` python
    app.get_next_valid_id()
    ```

#### Response Object / 响应对象

Requests to the get_next_valid_id() function will return the next valid order ID, which may be used in order submission.

对 get_next_valid_id() 函数的请求将返回下一个有效订单 ID，该 ID 可用于订单提交。

```powershell
123456789
```

### Account Summary / 账户摘要

The get_account_summary() function returns all relevant account details identical to Trader Workstation’s “Account” window. Users may query to receive all available data or a narrow window based on the [Account Summary Tag](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#account-summary-tags).

get_account_summary() 函数返回与 Trader Workstation 的“账户”窗口完全相同的所有相关账户详细信息。用户可以查询以接收所有可用数据，或基于[账户摘要标签](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#account-summary-tags)的(较少的)窗口数据。

``` python
get_account_summary(

    tags: String. Account summary key value to receive data for. See Account Summary Tags for details.
    tags: String. 账户摘要键值，用于接收数据。详情请参阅账户摘要标签。

    group: String. Indicates a Financial Advisor’s allocation group to reference account details for. Non-advisor account structures should always pass “All”.
    group: String。表示财务顾问参考账户详情的分配组。非顾问账户结构应始终传递 “All”。

        Default value passed, “All”.
        默认值，“All”。

    timeout: Integer. Timeout before the request disconnects. Function-specific timeout default of 5 second.
    超时：整数。请求断开连接前的超时时间。特定函数的超时默认为 5 秒。
)
```

=== "Python"

    ``` python
    from ibapi.account_summary_tags import AccountSummaryTags
    app.get_account_summary(AccountSummaryTags.AllTags, "All")
    ```

Total size of the request may vary depending on number of accounts held in the account, and the number of tags requested.

请求的总大小可能会根据账户中持有的账户数量以及请求的标签数量而变化。

#### Response Object / 响应对象

{AccountId}: Dictionary. Contains all tag value pairs for the designated accountId.
{AccountId}: 字典。包含指定 accountId 的所有标签值对。

{

    {Tag}: Dictionary. Contains the value of the affiliated tag along with the relevant currency.
    {Tag}: 字典。包含相关标签的值以及相关货币。

    value: String. Contains the alphanumeric value affiliated with the designated tag.
    value: String。包含与指定标签关联的字母数字值。

    currency: String. Returns the currency used to denote the value. May return an empty string if returning value does not contain a price.
    currency: String。返回表示值的货币。如果返回的值不包含价格，则可能返回空字符串。

}

``` powershell
{'U1234567': {'AccountType': {'value': 'LLC', 'currency': ''}, 'Cushion': {'value': '0.993764', 'currency': ''}, 'DayTradesRemaining': {'value': '-1', 'currency': ''}, 'LookAheadNextChange': {'value': '1760558400', 'currency': ''}, 'AccruedCash': {'value': '262079.00', 'currency': 'USD'}, 'AvailableFunds': {'value': '219944453.18', 'currency': 'USD'}, 'BuyingPower': {'value': '1466299088.69', 'currency': 'USD'}, 'EquityWithLoanValue': {'value': '221042710.95', 'currency': 'USD'}, 'ExcessLiquidity': {'value': '220044618.70', 'currency': 'USD'}, 'FullAvailableFunds': {'value': '219944453.18', 'currency': 'USD'}, 'FullExcessLiquidity': {'value': '220044618.70', 'currency': 'USD'}, 'FullInitMarginReq': {'value': '1101020.27', 'currency': 'USD'}, 'FullMaintMarginReq': {'value': '1000859.00', 'currency': 'USD'}, 'GrossPositionValue': {'value': '2982965.22', 'currency': 'USD'}, 'InitMarginReq': {'value': '1101020.27', 'currency': 'USD'}, 'LookAheadAvailableFunds': {'value': '219944453.18', 'currency': 'USD'}, 'LookAheadExcessLiquidity': {'value': '220044618.70', 'currency': 'USD'}, 'LookAheadInitMarginReq': {'value': '1101020.27', 'currency': 'USD'}, 'LookAheadMaintMarginReq': {'value': '1000859.00', 'currency': 'USD'}, 'MaintMarginReq': {'value': '1000859.00', 'currency': 'USD'}, 'NetLiquidation': {'value': '221425500.56', 'currency': 'USD'}, 'PreviousDayEquityWithLoanValue': {'value': '205659145.23', 'currency': 'USD'}, 'TotalCashValue': {'value': '218181198.71', 'currency': 'USD'}}}
```

### Contract Details / 合约详情

Interactive Brokers trading is centered around [Contract Objects](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object). This is used when submitting requests for market data, retrieving position information, and placing orders. The Synchronous Wrapper utilizes the same [Contract Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object) as the standard TWS API.

Interactive Brokers 的交易围绕着[合约对象](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object)进行。这用于提交市场数据请求、检索持仓信息和下单。Synchronous Wrapper 使用与标准 TWS API 相同的[合约对象](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object)。

Passing as much known information through a Contract Details will return all contracts that match the requesting information. At a minimum, the Contract ID, or Symbol and Security Type must be passed for contract discovery.

通过合约详情传递尽可能多的已知信息将返回所有匹配请求信息的合约。至少需要传递合约 ID，或者证券代码和证券类型来获取合约。

``` python
get_contract_details(

    contract: Contract Object. Contract details you are searching for.
    合约：合约对象。您正在搜索的合约详情。

    timeout: Integer. Timeout before the request disconnects. Function-specific timeout default of 5 second.
    超时：整数。请求断开连接前的超时时间。特定函数的超时默认为 5 秒。
)
```

=== "Python"
    ``` python
    contract = Contract()
    contract.symbol = "AAPL"
    contract.secType = "STK"

    app.get_contract_details(contract=contract)
    ```

#### Response Object / 响应对象

The get_contract_details() function will return a list of Contract objects.

get_contract_details() 函数将返回一个 Contract 对象的列表。

Unless a relatively narrow scope is provided during the initial contract details request, multiple contract objects may be returned within the list. Please be aware that directly printing this information may result in the memory address being displayed.

除非在初始合同详情请求时提供了一个相对较窄的范围，否则列表中可能会返回多个合同对象。请注意，直接打印这些信息可能会显示内存地址。

``` powershell
[3039334541648: ConId: 265598, Symbol: AAPL, SecType: STK, LastTradeDateOrContractMonth: , Strike: 0, Right: , Multiplier: , Exchange: SMART, PrimaryExchange: ISLAND, Currency: USD, LocalSymbol: AAPL, TradingClass: NMS, IncludeExpired: False, SecIdType: , SecId: , Description: , IssuerId: Combo:,NMS,0.01,ACTIVETIM,AD,ADDONT,ADJUST,ALERT,ALGO,ALLOC,AON,AVGCOST,BASKET,BENCHPX,CASHQTY,COND,CONDORDER,DARKONLY,DARKPOLL,DAY,DEACT,DEACTDIS,DEACTEOD,DIS,DUR,GAT,GTC,GTD,GTT,HID,IBKRATS,ICE,IMB,IOC,LIT,LMT,LOC,MIDPX,MIT,MKT,MOC,MTL,NGCOMB,NODARK,NONALGO,OCA,OPG,OPGREROUT,PEGBENCH,PEGMID,POSTATS,POSTONLY,PREOPGRTH,PRICECHK,REL,REL2MID,RELPCTOFS,RPI,RTH,SCALE,SCALEODD,SCALERST,SIZECHK,SMARTSTG,SNAPMID,SNAPMKT,SNAPREL,STP,STPLMT,SWEEP,TRAIL,TRAILLIT,TRAILLMT,TRAILMIT,WHATIF,SMART,AMEX,NYSE,CBOE,PHLX,ISE,CHX,ARCA,ISLAND,DRCTEDGE,BEX,BATS,EDGEA,BYX,IEX,EDGX,FOXRIVER,PEARL,NYSENAT,LTSE,MEMX,IBEOS,OVERNIGHT,TPLUS0,PSX,T24X,1,0,APPLE INC,,Technology,Computers,Computers,US/Eastern,20251015:0400-20251015:2000;20251016:0400-20251016:2000;20251017:0400-20251017:2000;20251018:CLOSED;20251019:CLOSED;20251020:0400-20251020:2000,20251015:0930-20251015:1600;20251016:0930-20251016:1600;20251017:0930-20251017:1600;20251018:CLOSED;20251019:CLOSED;20251020:0930-20251020:1600,,0,,,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,1,[3039334542544: ISIN=US0378331005;],,COMMON,,,,,,False,False,0,False,,,,,False,,0.0001,0.0001,100,None,,,, 3039334543504: ConId: 273982664,...]
```

### Live Market Data / 实时市场数据

Users may request market data using get_market_data_snapshot() to retrieve available market data.

用户可以使用 get_market_data_snapshot() 请求市场数据以获取可用市场数据。

The request currently only supports [tickPrice and tickSize](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#receive-live-data) data.

当前请求仅支持 [tickPrice 和 tickSize](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#receive-live-data) 数据。

``` python
get_market_data_snapshot(

    contract: [Contract Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object). Contract to retrieve market data for.
    contract: Contract 对象。要获取市场数据的合约。

    generic_tick_list: String. String containing comma-separate values to determine addition data to retrieve.
    generic_tick_list: String。包含逗号分隔值的字符串，用于确定要检索的附加数据。

    Default: Automatically sends an empty string, returning only the basic data such as Last, Bid, and Ask. See Available Tick Types for more details.
    默认值：自动发送空字符串，仅返回基本数据，如 Last、Bid 和 Ask。有关可用 K 线类型的详细信息，请参阅 Available Tick Types。

    snapshot: Boolean. Determine if a single snapshot should be returned or if data should be continuously updated until the timeout threshold has been reached.
    snapshot: Boolean。确定是否返回单个快照，或者是否应持续更新数据，直到达到超时阈值。

    Default: Set to True, returning a snapshot of data as soon as possible.
    默认值：设置为 True，尽快返回数据快照。

    timeout: Integer. Uses default timeout value passed to TWSSyncClass.
    超时时间：整数。使用传递给 TWSSyncClass 的默认超时值。
)
```

=== "Python"

    ``` python
    contract = Contract()
    contract.symbol = "AAPL"
    contract.secType = "STK"
    contract.exchange = "SMART"
    contract.primaryExchange = "NASDAQ"
    contract.currency = "USD"
    market_data = app.get_market_data_snapshot(contract, "225,232", False)
    ```

Data returned by get_market_data_snapshot() is delivered as a json dictionary object, separating data into “price” and “size” tags. Values are then returned as the affiliated tick types alongside any price or attribute data.

get_market_data_snapshot() 返回的数据以 json 字典对象形式提供，将数据分为“价格”和“数量”标签。值随后以相关的点类型 alongside 任何价格或属性数据返回。

#### Response Object / 响应对象

``` json
{

    price: Dictionary. Contains all price-based ticks.
    价格：字典。包含所有基于价格的点。

    {

        {TickType}: Dictionary. Contains price related to the given tick alongside any related attributes for the price.
        {TickType}: 字典。包含与给定报文相关的价格以及任何相关的价格属性。

        {

            price: Float. The value of the tag.
            price: 浮点数。标签的值。

            tickAttrib: TickAttrib. The Tick Attributes of the price, including AutoExecute, PastLimit, and PreOpen details.
            tickAttrib: TickAttrib。价格报文属性，包括自动执行、突破限制和开盘前详细信息。
        }

    }

    size: Dictionary  
    size: 字典

    {

        {TickType}: Decimal. Contains size related to the given tick
        {TickType}: 十进制数。包含与给定价格变动相关的尺寸
    }

}
```

``` json
{'price': {1: {'price': 252.27, 'attrib': 1813315433616: CanAutoExecute: 1, PastLimit: 0, PreOpen: 0}, 2: {'price': 252.29, 'attrib': 1813315433744: CanAutoExecute: 1, PastLimit: 0, PreOpen: 0}, 4: {'price': 252.28, 'attrib': 1813315433808: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}, 6: {'price': 252.54, 'attrib': 1813315434064: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}, 7: {'price': 247.27, 'attrib': 1813312493200: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}, 9: {'price': 247.45, 'attrib': 1813315434192: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}, 14: {'price': 248.0, 'attrib': 1813315434320: CanAutoExecute: 0, PastLimit: 0, PreOpen: 0}}, 'size': {0: Decimal('2'), 3: Decimal('1'), 5: Decimal('3'), 8: Decimal('292425')}}
```

### Historical Market Data / 历史市场数据

``` python
get_historical_data(  get_historical_data()

    contract: Contract Object. Contract to retrieve market data for.
    合约：合约对象。用于获取市场数据的合约。

    end_date_time: String. The request’s end date and time. This should be formatted as “YYYYMMDD HH:mm:ss TMZ”. You may also pass an empty string to indicate the current moment
    end_date_time: String. 请求的结束日期和时间。此格式应为“YYYYMMDD HH:mm:ss TMZ”。您也可以传递一个空字符串来表示当前时刻
    Please be aware that endDateTime must be left as an empty string when requesting continuous futures contracts or certain whatToShow values like ADJUSTED_LAST.
    请注意，在请求连续期货合约或某些 whatToShow 值（如 ADJUSTED_LAST）时，endDateTime 必须保持为空字符串。

    duration_str: String. The total timespan the bars should cover. See Duration for details.
    duration_str: String。条形图应覆盖的总时间段。详情请参见 Duration。

    bar_size_setting: String. The time span covered by each bar. See Bar Sizes for details.
    bar_size_setting: String。每个条形图覆盖的时间段。详情请参见 Bar Sizes。

    what_to_show: String. Determines what kind of data should be returned. See whatToShow for more details.
    what_to_show: String。决定返回何种类型的数据。详情请参见 whatToShow。

    use_rth: Boolean. Define if data should only be returned from the regular trading session or if extended trading hours should be included.
    use_rth: Boolean。定义数据是否仅返回常规交易时段，或是否包含扩展交易时段。

        Default: True is passed by default, only returning data from the regular trading sessions.
        默认情况下传递 True，仅返回常规交易时段的数据。

    format_date: Integer. Determine the return structure of the date. Supports (1) to return a datetime formatting string or (2) to return a epoch Unix timestamp.
    format_date: 整数。确定日期的返回结构。支持 (1) 返回 datetime 格式字符串或 (2) 返回 epoch Unix 时间戳。

        Default: Set to 1, returning a datetime string.
        默认：设置为 1，返回 datetime 字符串。

    timeout: Integer. A default value of 30 is supplied.
    timeout: 整数。默认值为 30。
)
```

[Contract Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object)

[Duration](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#hist-duration)

[Bar Sizes](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#hist-bar-size)

[whatToShow](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#historical-whattoshow)

=== "Python"

    ``` python
    contract = Contract()
    contract.symbol = "AAPL"
    contract.secType = "STK"
    contract.exchange = "SMART"
    contract.primaryExchange = "NASDAQ"
    contract.currency = "USD"
    app.get_historical_data(contract=contract, end_date_time="", duration_str="1 W", bar_size_setting="1 day", what_to_show="TRADES", use_rth=True)
    ```

#### Response Object / 响应对象

Requesting historical bars will return return a list containing all Bar objects for the duration. Please be aware that directly printing this information may result in the memory address being displayed.

请求历史 K 线将返回包含该时间段内所有 K 线对象的列表。请注意，直接打印这些信息可能会显示内存地址。

```python
[2524872613328: Date: 20251013, Open: 249.31, High: 249.69, Low: 245.56, Close: 247.66, Volume: 187465.43, WAP: 247.952, BarCount: 105768, 2524872614864: Date: 20251014, Open: 246.6, High: 248.85, Low: 244.7, Close: 247.77, Volume: 176034.99, WAP: 247.21, BarCount: 100507, 2524872615120: Date: 20251015, Open: 249.49, High: 251.82, Low: 247.47, Close: 249.34, Volume: 172136.46, WAP: 249.754, BarCount: 96331, 2524872615248: Date: 20251016, Open: 248.28, High: 249.04, Low: 245.13, Close: 247.45, Volume: 235179.94, WAP: 247.351, BarCount: 132811, 2524872615376: Date: 20251017, Open: 248.08, High: 253.38, Low: 247.27, Close: 252.29, Volume: 260673.48, WAP: 250.408, BarCount: 125863]
```

### Place Order / 下单

``` python
place_order_sync(

    contract: Contract Object. Contract to trade.
    合约：合约对象。用于交易的合约。

    order: Order Object. Order parameters to be traded.
    order: 订单对象。用于交易的订单参数。

    timeout: Integer. Uses default timeout value passed to TWSSyncClass. Please be aware the timeout is only relevant for the response details. The order will submit in accordance with the order object’s details.
    超时：整数。使用传递给 TWSSyncClass 的默认超时值。请注意，超时值仅与响应详情相关。订单将根据订单对象的详情提交。
)
```

[Contract Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object)

[Order Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#order-object)

=== "Python"

    ``` python
    contract = Contract()
    contract.symbol = "AAPL"
    contract.secType = "STK"
    contract.exchange = "SMART"
    contract.primaryExchange = "NASDAQ"
    contract.currency = "USD"

    order = Order()
    order.action = "BUY"
    order.orderType = "LMT"
    order.totalQuantity = 100
    order.lmtPrice = 250

    app.place_order_sync(contract, order)
    ```

Upon placing an order, a dictionary containing all of the order status’s information will be returned. As the response is static, refer to the [get_open_orders](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#sync-open-orders) function more more details on the current order status.

下单后，将返回一个包含所有订单状态信息的字典。由于响应是静态的，请参考 get_open_orders 函数以获取更多关于当前订单状态的信息。

#### Response Object / 响应对象

{

    orderId: Integer. The identifier for the order. Relevant for order tracking, modification, and cancellation.
    orderId: 整数。订单的标识符。用于订单跟踪、修改和取消。

    status: String. The current status of the order. See [Order Status](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#order-status-message) for more details.
    status: 字符串。订单的当前状态。详情请参阅[订单状态](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#order-status-message)。

    filled: Decimal. The total quantity of executed shares for the order.
    filled: 小数。该订单已执行的股份数量总和。

    remaining: Decimal. The total quantity of shares that have yet to execute for the order.
    remaining: 小数。该订单尚未执行的股份数量总和。

    avgFillPrice: Float. The average execution price across fills.
    avgFillPrice: 浮点数。各次成交的平均执行价格。

    permId: Integer. The permanent identifier for the order. This is calculated based on orderId and client ID for internal order tracking.
    permId: 整数。订单的永久标识符。这是基于 orderId 和客户 ID 计算得出的，用于内部订单跟踪。

    parentId: Integer. The orderId for the parent of this contract. Will return 0 unless trading a [bracket or OCA](https://www.interactivebrokers.com/campus/ibkr-api-page/order-types/#one-cancels-all-orders) order.
    parentId: Integer. 此合约的父订单 orderId。除非交易[括号订单或 OCA 订单](https://www.interactivebrokers.com/campus/ibkr-api-page/order-types/#one-cancels-all-orders)，否则将返回 0。

    lastFillPrice: Float. The price of the most recent execution for the order.
    lastFillPrice: Float. 该订单最近一次成交的价格。

    clientId: Integer. The identifier for which client ID the order was placed through. Orders can only be cancelled or modified by their on the [clientId they are bound to](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#modify-order).
    clientId: Integer. 订单通过哪个客户端 ID 提交的标识符。订单只能通过其[绑定的客户端 ID](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#modify-order) 取消或修改。

    whyHeld: String. In the event an order is held instead of being transmitted, the reason will be documented here.
    whyHeld: String. 如果订单未发送而被持有，原因将记录在此处。

    mktCapPrice: Float. If an order is capped due to it exceeding the market price and the price is automatically modified, the modified price will be returned. Otherwise 0.0 is displayed.
    mktCapPrice: Float. 如果由于订单超出市场价而被限制，且价格被自动修改，则返回修改后的价格。否则显示 0.0。
}

``` python
{'orderId': 347, 'status': 'PreSubmitted', 'filled': Decimal('0'), 'remaining': Decimal('100'), 'avgFillPrice': 0.0, 'permId': 979867961, 'parentId': 0, 'lastFillPrice': 0.0, 'clientId': 8675309, 'whyHeld': '', 'mktCapPrice': 0.0}
```

### Cancel Order / 取消订单

``` python
cancel_order_sync(

    order_id: Integer. Identifier for the order to cancel. Retrieved from the original Order Placement or get_open_orders().
    order_id: 整数。用于取消订单的标识符。从原始订单下单或 get_open_orders()中获取。

    order: OrderCancel Object. Order cancellation parameters.
    order: OrderCancel 对象。订单取消参数。

    timeout: Integer. A default value of 3 seconds is supplied.
    超时时间：整数。默认值为 3 秒。
)
```

[Order Placement](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#sync-place-order)

[get_open_orders()](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#sync-open-orders)

[OrderCancel Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#ordercancel-ref)

=== "Python"

    ``` python
    app.cancel_order_sync(347, OrderCancel())
    ```

Upon cancelling an order, a dictionary containing all of the order status’s information will be returned. As the response is static, refer to the [get_open_orders](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#sync-open-orders) function more more details on the current order status.

取消订单时，将返回一个包含所有订单状态信息的字典。由于响应是静态的，请参考 get_open_orders 函数以获取更多关于当前订单状态的信息。

#### Response Object  响应对象

{

    orderId: Integer. The identifier for the order. Relevant for order tracking, modification, and cancellation.
    orderId: 整数。订单的标识符。用于订单跟踪、修改和取消。

    status: String. The current status of the order. See Order Status for more details.
    status: 字符串。订单的当前状态。详情请参阅订单状态。

    filled: Decimal. The total quantity of executed shares for the order.
    filled: Decimal. 订单已执行的总股数。

    remaining: Decimal. The total quantity of shares that have yet to execute for the order.
    remaining: Decimal. 订单尚未执行的股数总和。

    avgFillPrice: Float. The average execution price across fills.
    avgFillPrice: Float. 执行的平均价格。

    permId: Integer. The permanent identifier for the order. This is calculated based on orderId and client ID for internal order tracking.
    permId: Integer. 订单的永久标识符。这是基于 orderId 和客户 ID 计算得出的，用于内部订单跟踪。

    parentId: Integer. The orderId for the parent of this contract. Will return 0 unless trading a bracket or OCA order.
    parentId: Integer。此合约父订单的 orderId。除非交易括号订单或 OCA 订单，否则将返回 0。

    lastFillPrice: Float. The price of the most recent execution for the order.
    lastFillPrice: Float。订单最近一次成交的价格。

    clientId: Integer. The identifier for which client ID the order was placed through. Orders can only be cancelled or modified by their on the clientId they are bound to.
    clientId: Integer。订单通过哪个客户端 ID 提交的标识符。订单只能通过其绑定的客户端 ID 取消或修改。

    whyHeld: String. In the event an order is held instead of being transmitted, the reason will be documented here.
    whyHeld: String。如果订单未发送而被持有，原因将记录在此处。

    mktCapPrice: Float. If an order is capped due to it exceeding the market price and the price is automatically modified, the modified price will be returned. Otherwise 0.0 is displayed.
    mktCapPrice: 浮点数。如果由于订单超过市场价格而被限制，并且价格被自动修改，将返回修改后的价格。否则显示 0.0。

}

``` python
{'orderId': 347, 'status': 'PendingCancel', 'filled': Decimal('0'), 'remaining': Decimal('100'), 'avgFillPrice': 0.0, 'permId': 1395073938, 'parentId': 0, 'lastFillPrice': 0.0, 'clientId': 8675309, 'whyHeld': '', 'mktCapPrice': 0.0}
```

### Open Orders / 未成交订单

``` python
get_open_orders(

    timeout: Integer. A default value of 3 seconds is supplied.
    timeout: Integer. 默认值为 3 秒。
)
```

=== "Python"

``` python
app.get_open_orders()
```

All orders from the current day’s trading session are returned in a dictionary, using the orderId as the key to discover the specific order.

当前交易日内的所有订单以字典形式返回，使用 orderId 作为键来查找特定订单。

#### Response Object / 响应对象

{

    {Order ID}: Dictionary. Returns the Contract, Order, and OrderState objects of the affiliated orderId.
    {订单 ID}: 字典。返回关联 orderId 的合约、订单和订单状态对象。

    {
        orderId: Integer. The identifier for the order. Relevant for order tracking, modification, and cancellation.
        orderId: 整数。订单的标识符。用于订单跟踪、修改和取消。

        contract: [Contract Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object). Contract to trade.
        contract: 合约对象。要交易的合约。

        order: [Order Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#order-object). Parameters for the given order to execute.
        order: 订单对象。执行给定订单的参数。

        orderState: [OrderState Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-ref/#orderstate-ref). Current state of the order. Contains margin impact and status details.
        orderState: 订单状态对象。订单的当前状态。包含保证金影响和状态详细信息。
    }   
}

``` json
{351: {'orderId': 351, 'contract': 2172957720272: ConId: 265598, Symbol: AAPL, SecType: STK, LastTradeDateOrContractMonth: , Strike: 0, Right: , Multiplier: , Exchange: SMART, PrimaryExchange: , Currency: USD, LocalSymbol: AAPL, TradingClass: NMS, IncludeExpired: False, SecIdType: , SecId: , Description: , IssuerId: Combo:, 'order': 2172957719120: 351,8675309,979867965: LMT BUY 100@800 GTC, 'orderState': }}
```

### Executions / 执行记录

Request all executions following the Execution Filter’s restrictions.

获取所有符合执行过滤器限制的执行记录。

``` python
get_executions(

    exec_filter: [ExecutionFilter Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-ref/#execfilter-ref). Parameters to restrict the Execution data to be returned.
    exec_filter: ExecutionFilter 对象。用于限制返回的执行数据的参数。

    timeout: Integer. A default value of 10 seconds is supplied.
    超时时间：整数。默认值为 10 秒。
)
```

=== "Python"

    ``` python
    app.get_executions(exec_filter)
    ```

All executions passed in the context of the ExecutionFilter are returned in a list.

在 ExecutionFilter 上下文中传递的所有执行操作都会以列表形式返回。

#### Response Object / 响应对象

    [

        {

            contract: [Contract Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object). Contract to trade.
            合约：合约对象。要交易的合约。

            execution: [Execution Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-ref/#exec-ref). Execution details regarding the recent trade.
            执行：执行对象。关于最近交易的执行详情。

        }
    ]

```python
[{'contract': 1530250139984: ConId: 265598, Symbol: AAPL, SecType: STK, LastTradeDateOrContractMonth: , Strike: 0, Right: , Multiplier: , Exchange: IEX, PrimaryExchange: , Currency: USD, LocalSymbol: AAPL, TradingClass: NMS, IncludeExpired: False, SecIdType: , SecId: , Description: , IssuerId: Combo:, 'execution': 1530250140432: ExecId: 0000e0d5.68fa9014.01.01, Time: 20251022 14:56:24 US/Eastern, Account: U1234567, Exchange: IEX, Side: BOT, Shares: 100, Price: 256.62, PermId: 1395073936, ClientId: 8675309, OrderId: 355, Liquidation: 0, CumQty: 100, AvgPrice: 256.62, OrderRef: , EvRule: , EvMultiplier: 0, ModelCode: , LastLiquidity: 2, PendingPriceRevision: False, Submitter: csdem9545, OptExerciseOrLapseType: None}]
```

### Positions / 仓位

Request positions for all accounts available to the user.

请求用户所有可用账户的持仓。

``` python
get_positions(

    timeout: Integer. A default value of 10 seconds is supplied.
    timeout: Integer. 默认值为 10 秒。
)
```

=== "Python"

    ``` python
    app.get_positions()
    ```

All orders from the current day’s trading session are returned in a dictionary, using the orderId as the key to discover the specific order.

返回当天交易会话中的所有订单，以 orderId 作为键来查找特定订单。

#### Response Object / 响应对象

    {
        {Account ID}: List. List of all contracts
        {账户 ID}: 列表。所有合约的列表
        [
            {

                contract: [Contract Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object). Contract to trade.
                contract: 合约对象。用于交易的合约。

                position: Decimal. The total number of shares held in the account.
                position: 十进制数。账户中持有的总股数。

                avgCost: Float. The average price across executions for the position.
                avgCost: Float. 该持仓在执行过程中的平均价格。

            }
        ]
    }

```json
{'U1234567': [{'contract': 2333839861008: ConId: 340216238, Symbol: COIL, SecType: FUT, LastTradeDateOrContractMonth: 20251031, Strike: 0, Right: , Multiplier: 1000, Exchange: IPE, PrimaryExchange: , Currency: , LocalSymbol: COILZ5, TradingClass: COIL, IncludeExpired: False, SecIdType: , SecId: , Description: , IssuerId: Combo:, 'position': Decimal('4'), 'avgCost': 61359.9}]}
```

### Portfolio / 投资组合

Request portfolio details for the selected account or accounts available to the user.

获取选定账户或用户可用的账户的投资组合详细信息。

``` python
get_portfolio(

    account_code: String. The accountID to pull portfolio information for. If an empty string is passed, all accounts are requested.
    account_code: String. 要获取投资组合信息的账户 ID。如果传递空字符串，则请求所有账户。

    timeout: Integer. A default value of 10 seconds is supplied.
    超时时间：整数。默认值为 10 秒。
)
```

=== "Python"

    ``` python
    app.get_portfolio("")
    ```

#### Response Object / 响应对象

    {
        {Account ID}: List. List of all contracts
        {账户 ID}：列表。所有合约的列表
        [
            {

                contract: [Contract Object](https://www.interactivebrokers.com/campus/ibkr-api-page/twsapi-doc/#contract-object). Contract to trade.
                合约：合约对象。用于交易的合约。

                position: Decimal. The total number of shares held in the account.
                position: Decimal. 账户中持有的总股份数量。

                marketPrice: Float. The current market price of the instrument.
                marketPrice: Float. 仪器当前的市价。

                marketValue: Float. The current value of the total position.
                marketValue: Float. 总持仓的当前价值。

                averageCost: Float. The average price across executions for the position.
                averageCost: Float. 持仓的执行均价。

                unrealizedPNL: Float. The unrealized profit and loss for the instrument.
                unrealizedPNL: Float. 该工具的未实现盈亏。

                realizedPNL: Float. The realized profit and loss for the instrument.
                realizedPNL: Float. 该工具的已实现盈亏。

                accountName: String. The account identifier that holds the given position.
                accountName: String. 持有该头寸的账户标识符。

            }
        ]
    }

```python
[{'contract': 1957652380880: ConId: 265598, Symbol: AAPL, SecType: STK, LastTradeDateOrContractMonth: , Strike: 0, Right: , Multiplier: , Exchange: ISLAND, PrimaryExchange: , Currency: USD, LocalSymbol: AAPL, TradingClass: NMS, IncludeExpired: False, SecIdType: , SecId: , Description: , IssuerId: Combo:, 'position': Decimal('202635'), 'marketPrice': 258.57998655, 'marketValue': 52397355.58, 'averageCost': 263.3360764, 'unrealizedPNL': -963750.26, 'realizedPNL': 0.0, 'accountName': 'DU5240685'}]
```
