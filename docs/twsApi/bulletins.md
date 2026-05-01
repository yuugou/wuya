# TWS API DocumentationTWS API 文档

## Bulletins / 公告

<details open>
<summary>From time to time, IB sends out important News Bulletins, which can be accessed via the TWS API through the EClient.reqNewsBulletins. Bulletins are delivered via IBApi.EWrapper.updateNewsBulletin whenever there is a new bulletin. In order to stop receiving bulletins you need to cancel the subscription.</summary>
IB 会不时发送重要新闻公告，可通过 TWS API 通过 EClient.reqNewsBulletins 访问。每当有新公告时，会通过 IBApi.EWrapper.updateNewsBulletin 发送公告。为了停止接收公告，需要取消订阅。
</details>

### Request IB Bulletins / 请求 IB 公告

<details open>
<summary>
```Python
    EClient.reqNewsBulletins (

        allMessages: bool. If set to true, will return all the existing bulletins for the current day, set to false to receive only the new bulletins.

    )
```
</summary>

```python
EClient.reqNewsBulletins(

    allMessages: bool. 如果设置为 true，将返回当天所有现有的公告，设置为 false 则仅接收新公告。

)
```

</details>

<details open>
<summary>
Subscribes to IB’s News Bulletins.
</summary>
订阅 IB 新闻快讯。
</details>

=== "Python"
``` python
def userInfo(self, reqId: int, whiteBrandingId: str):
    print("UserInfo.", "ReqId:", reqId, "WhiteBrandingId:", whiteBrandingId)
```

### Receive IB Bulletins / 接收 IB 公告

<details open>

<summary>
    EWrapper.updateNewsBulletin (

        ```python
        msgId: int. The bulletin’s identifier.
        msgId: int. 布告的标识符。

        msgType: int. 1: Regular news bulletin; 2: Exchange no longer available for trading; 3: Exchange is available for trading.
        msgType: int. 1: 普通新闻布告；2: 交易所不再可供交易；3: 交易所可供交易。

        message: String. The news bulletin context.
        消息：字符串。新闻公告的上下文。

        origExchange: String. The exchange where the message comes from.
        原始交易所：字符串。消息来源的交易所。
        ```
    )

</details>

Provides IB’s bulletins.

提供 IB 的公告。

```Python
def updateNewsBulletin(self, msgId: int, msgType: int, newsMessage: str, originExch: str):
    print("News Bulletins. MsgId:", msgId, "Type:", msgType, "Message:", newsMessage, "Exchange of Origin: ", originExch)
```

### Cancel Bulletin Request / 取消公告请求

```Python
EClient.cancelNewsBulletin ()
```

Cancels IB’s news bulletin subscription.

取消 IB 新闻简报订阅。
