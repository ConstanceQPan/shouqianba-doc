# Refund

### API Target

    {api_domain}/upay/v2/refund

### Request Paramters

Parameter | Description | Data Type | Required | Note | Example
--------- | ------ | ----- | ------- | --- | ----
terminal_sn | Terminal serial number | String(32) | Y | Represents a unique identifier of a Upay terminal; returned in responses for requesting activation or check-in; number string no longer than 32 characters. | "00101010029201012912"
sn | Upay order serial number | String(16) | N | Unique order serial number in Upay system | "7892259488292938"
client_sn | Order serial number in client system | String(32) | N | Must be unique in client system; no longer than 32 characters. | "18348290098298292838"
refund_request_no | Refund request number | String(20) | Y | Used to prevent duplicate refund requests | "23030349"
operator | Operator of the transaction | String(32) | Y | No longer than 32 characters | "Obama"
refund_amount | Refund amount | String(10) | Y | Should be less or equal to the total amount of the order | "100"

<font color="red"><b>Note: Either <code>sn</code> or <code>client_sn</code> must be presented in the request, otherwise the request is invalid; if both are presented, <code>sn</code> will be used to identify the order.</b></font>

### Response Paramters

All following parameters refer to the fields in `biz_response`

Parameter | Description | Data Type | Required | Note | Example
--------- | ------ | ----- | -------| --- | ----
result_code | Request result code | String | Y | Result code of business response | "REFUND_SUCCESS"
error_code | Error code of business response | String | N | See "Business Response Error Codes and Messages" | "ACCOUNT_BALANCE_NOT_ENOUGH"
error_message | Error message of business response | String | N | See "Business Response Error Codes and Messages" | "商户余额不足"
terminal_sn | Terminal serial number of the transaction | String(32) | Y | Used by Upay to identify a unique terminal | "01939202039923029"
sn | Upay order serial number | String(16) | Y | Unique order serial number in Upay system | "7892259488292938"
client_sn | Order serial number in client system | String(32) | Y | Used by client to identify its own order | "7654321132"
client_tsn | Refund transaction serial number | String(53) | Y | Indicates the most current transaction of this order. It is basically client_sn + '-' + refund_request_no | "7654321132-123"
trade_no | Order number in payment service provider system | String(64) | Y | Used by payment service provider to identify its own order | "2013112011001004330000121536"
status | The latest transaction status | String(32) | Y |  | "SUCCESS"
order_status | The latest order status | String(32) | Y |  | "PAID"
total_amount | Total amount of the order in <font color="red" style="font-weight: bold;">cents</font> | String(10) | Y |  | "10000"
net_amount | Net amount of the order (the actual amount seller receives) in <font color="red" style="font-weight: bold;">cents</font> | String(10) | Y | Equals to total amount if the order has not been refunded, otherwise equals to `total amount - refund amount` | "0"
subject | Subject or brief summary of the transaction | String(64) | Y |  | "Pizza"
finish_time | Transaction finish time in Upay system | String(13) | Y | Unix Timestamp in milliseconds | "1449646835244"
channel_finish_time | Transaction finish time in payment service provider's system | String(13) | Y | Unix Timestamp in milliseconds | "1449646835244"
operator | Operator of the transaction | String(32) | Y |  | "Peter"

### Response Example - Failed

```json
{
    "result_code": "200",
    "biz_response": {
        "result_code": "FAIL",
        "error_code": "UPAY_REFUND_INVALID_ORDER_STATE",
        "error_message": "订单已全额退款,可退金额不足"
    }
}
```
