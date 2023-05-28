---
sidebar_position: 3
---

# RequestCreateOrder

To create an order, you must first obtain a token from applyFabricToken method and set it in the header before you can place a request.

- `API\service\requestCreateOrderervice.js`

## Create apply Request Create Order service

Create a file at `API\service\requestCreateOrderService.js`:

```jsx title="API\service\requestCreateOrder.js"
const applyFabricToken = require("./applyFabricTokenService");
const tools = require("../utils/tools");
const config = require("../config/config");
const https = require("http");
var request = require("request");

exports.createOrder = async (req, res) => {
  let title = req.body.title;
  let amount = req.body.amount;
  let applyFabricTokenResult = await applyFabricToken();
  let fabricToken = applyFabricTokenResult.token;
  let createOrderResult = await exports.requestCreateOrder(
    fabricToken,
    title,
    amount
  );
  console.log(createOrderResult);
  let prepayId = createOrderResult.biz_content.prepay_id;
  let rawRequest = createRawRequest(prepayId);
  res.send(rawRequest);
};

exports.requestCreateOrder = async (fabricToken, title, amount) => {
  return new Promise((resolve) => {
    let reqObject = createRequestObject(title, amount);

    var options = {
      method: "POST",
      url: config.baseUrl + "/payment/v1/merchant/preOrder",
      headers: {
        "Content-Type": "application/json",
        "X-APP-Key": config.fabricAppId,
        Authorization: fabricToken,
      },
      rejectUnauthorized: false, //add when working with https sites
      requestCert: false, //add when working with https sites
      agent: false, //add when working with https sites
      body: JSON.stringify(reqObject),
    };

    request(options, (error, response) => {
      if (error) throw new Error(error);
      let result = JSON.parse(response.body);
      resolve(result);
    });
  });
};

function createRequestObject(title, amount) {
  let req = {
    timestamp: tools.createTimeStamp(),
    nonce_str: tools.createNonceStr(),
    method: "payment.preorder",
    version: "1.0",
  };
  let biz = {
    notify_url: "https://www.google.com",
    trade_type: "InApp",
    appid: config.merchantAppId,
    merch_code: config.merchantCode,
    merch_order_id: createMerchantOrderId(),
    title: title,
    total_amount: amount,
    trans_currency: "ETB",
    timeout_express: "120m",
    payee_identifier: config.merchantCode,
    payee_identifier_type: "04",
    payee_type: "3000",
    redirect_url: "https://www.bing.com",
  };
  req.biz_content = biz;
  req.sign = tools.signRequestObject(req);
  req.sign_type = "SHA256WithRSA";
  console.log(req);
  return req;
}

function createMerchantOrderId() {
  return new Date().getTime() + "";
}

function createRawRequest(prepayId) {
  let map = {
    appid: config.merchantAppId,
    merch_code: config.merchantCode,
    nonce_str: tools.createNonceStr(),
    prepay_id: prepayId,
    timestamp: tools.createTimeStamp(),
  };
  let sign = tools.signRequestObject(map);
  let rawRequest = [
    "appid=" + map.appid,
    "merch_code=" + map.merch_code,
    "nonce_str=" + map.nonce_str,
    "prepay_id=" + map.prepay_id,
    "timestamp=" + map.timestamp,
    "sign=" + sign,
    "sign_type=SHA256WithRSA",
  ].join("&");
  return rawRequest;
}

module.exports = createOrder;
```
### Request Parameters
#### HEADER PARAMETERS
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|X-APP-Key|  String|  M|Fabric App ID, provided by fabric portal of Ethio telecom|
|Authorization |  String|  M|App Token for authentication|


#### REQUEST BODY SCHEMA
|Parameter|Data Type|  M/O|Description|
|:----|:----|:----|:----|
|timestamp|string(13)|M|<= 13 characters ^[0-9]*[1-9][0-9]*$|
| | | |Time when a request is sent. UTC timestamp. The unit is second.|
|method |string|M|Value: "payment.preorder"|
| | | |Set to 'payment.preorder', fixed for this interface|
|nonce_str |string(32)|M|<= 32 characters \S+|
| | | |Random character string containing a maximum of 32 characters, including uppercase letters, lowercase letters, digits, but not special characters.|
|sign_type|string|M|Value = "SHA256WithRSA"|
| | | |Signature type.|
|sign|String(512)|M|<= 512 characters \S+|
| | | |This signature is the sign of all the request parameters except the sign and sign_type. First ordered in alphabetical order and joined in a key=value format and joined together with '&' and are signed with the SHA256RSA algorithm.|
|version|String(4)|M|<= 4 characters \S+|
| | | |Interface version number. Only support 1.0 now|
|biz_content| |M|object (CreateOrderBizContent)|
|notify_url|String(512)|M|<= 512 characters \S+|
| | | |Specifies the callback address for receiving payment notifications if payment is successful.|
|redirect_url|String(512)|O|<= 512 characters \S+|
| | | |Indicates the callback address returned to the merchant after the payment is complete.|
|appid|String(32)|M|Length <= 32 characters ^[A-Za-z0-9]* $|
| | | |Application ID allocated to a merchant by Mobile Payment system.|
|merch_code|String(16)|M|Length <= 16 characters ^[1-9][0-9]+$|
| | | |Short code registered by a merchant with the Mobile Money.|
|merch_order_id|String(64)|M|<= 64 characters ^[A-Za-z0-9]+$|
| | | |The order number generated by the merchant side. It must be in the form of letters, numbers, and underscores. Other special characters are not allowed.|
|trade_type|string|M|The B2B business trade type is “Checkout”，|
| | | |Checkout: Payment initiated from a merchant webpage in browser, then redirect to checkout webpage of mobile payment system to pay the order.|
| | | | |
|title|String(512)|M|Length <= 512 characters [^~`!#$%^*()\\-+=|/<>?;:\"\\[\\]{}\\\\&]*Offering name.|
| | | | |
|total_amount|String(20)|M|<= 20 characters ^((0{1}\.\d{1,2})|([1-9]\d*\.{1}\d{1,2})|([1-9]+\d*))$|
| | | |Total order amount. The value can contain two decimal places at most.|
|trans_currency|String(3)|M|<= 3 characters \S+|
| | | |Three-letter code complying with international standards, for example, USD.|
|timeout_express|String(10)|M|Length <= 7 characters ^[1-9]\d{0,5}m$|
| | | |Latest payment time allowed for an order. The transaction will be closed after the deadline. The value ranges from 1 minute to 120 minutes. The value of this parameter cannot contain dots. For example, the value 1.5 hours must be converted to 90 minutes. If this parameter is not set, 120 minutes is used by default.|
| | | | |
|business_type|String(32)|M|<= 32 characters \S+|
| | | |The enumeration values that can be used are related to service scenarios. You need to consult the platform.|
| | | |The value is “TransferToOtherOrg”.|
|callback_info|String |O|<= 512 characters \S+|
| | | |Additional information that the merchant wants to see when the callback is returned.|


### Response Parameters
|Parameter|Description|
|:----|:----|
|result|SUCCESS or FAIL.|When the code field is a specific business error code.|
|code|Return code.|0 is successful, the rest is the business error code|
|msg|Return information, simple error description.|
|sign|<= 512 characters|
| |Response signature. Signed by the privatekey of the SP.|
|nonce_str|<= 32 characters|
| |Random character string. 32 characters or fewer.|
|sign_type|Value = "SHA256WithRSA"|
| |  Signature type.|
| | |
|biz_content|object (AuthTokenResponseBizContent)|
|merch_order_id|<= 64 characters ^[A-Za-z0-9]+$|
| |Order ID on the merchant side. When return_code is SUCCESS, this value will return|
|prepay_id|<= 128 characters ^[A-Za-z0-9]+$|
| |ID of the customer payment process. When return_code is SUCCESS, this value will return|
