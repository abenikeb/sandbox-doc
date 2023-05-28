---
sidebar_position: 2
---

# AuthToken

This is for system's that use auto Login functionality. The request for this need to originate from within the super app as it requires the access_token which is generated from with the super app via function call from the H5. For more information check the demo provided

- `API\service\applyFabricTokenService.js`


## Create auth token service

Create a file at `API\service\authTokenService.js`:

```jsx title="API\service\authTokenService.js"
const applyFabricToken = require("./applyFabricTokenService");
const tools = require("../utils/tools");
const config = require("../config/config");
const https = require("http");

exports.authToken = async (req, res) => {
  let appToken = req.body.authToken;
  console.log("token = ", appToken);
  let applyFabricTokenResult = await applyFabricToken();
  console.log("applyFabricTokenResult", applyFabricTokenResult);
  let fabricToken = applyFabricTokenResult.token;
  let result = await exports.requestAuthToken(fabricToken, appToken);
  res.send(result);
};

exports.requestAuthToken = async (fabricToken, appToken) => {
  return new Promise((resolve) => {
    let reqObject = createRequestObject(appToken);
    var options = {
      method: "POST",
      url: config.baseUrl + "/payment/v1/auth/authToken",
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
    request(options, function (error, response) {
      if (error) throw new Error(error);
      console.log(response.body);
      let result = JSON.parse(response.body);
      console.log(result);
      resolve(result);
    });
  });
};

function createRequestObject(appToken) {
  let req = {
    timestamp: tools.createTimeStamp(),
    nonce_str: tools.createNonceStr(),
    method: "payment.authtoken",
    version: "1.0",
  };
  let biz = {
    access_token: appToken,
    trade_type: "InApp",
    appid: config.merchantAppId,
    resource_type: "OpenId",
  };
  req.biz_content = biz;
  req.sign = tools.signRequestObject(req);
  req.sign_type = "SHA256WithRSA";
  console.log(req);
  return req;
}

// module.exports = authToken;

```
### Request Parameters
#### HEADER PARAMETERS
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|X-APP-Key|  String|  M|Fabric App ID, provided by fabric portal of Ethio telecom|
|Authorization |  String|  M|App Token for authentication|


#### REQUEST BODY SCHEMA
|Parameter|Data Type|M/O|Description|
|:----|:----|:----|:----|
|timestamp|string|M|<= 13 characters ^[0-9]*[1-9][0-9]*$|
| | | |Time when a request is sent. UTC timestamp. The unit is second.|
|method|string|M| |
| | | |Value: "payment.applyh5token"|
|nonce_str|string|M|<= 32 characters \S+|
| | | |Random character string containing a maximum of 32 characters, including uppercase letters, lowercase letters, digits, but not special characters.|
|sign_type|string|M|Value = "SHA256WithRSA"|
| | | |Signature type.|
|sign|String|M|<= 512 characters \S+|
| | | |This signature is the sign of all the request parameters except the sign and sign_type. First ordered in alphabetical order and joined in a key=value format and joined together with '&' and are signed with the SHA256RSA algorithm.|
|version|String|M|<= 4 characters \S+|
| | | |Interface version number. Only support 1.0 now|
|biz_content|object | | |
| | | |object (CreateOrderBizContent)|
|appid|String|M|<= 32 characters ^[A-Za-z0-9]+$|
| | | |Application ID allocated to a merchant by Mobile Payment system. Is also known as Merchant app id.|
|access_token|String|M|A token that allow the merchant to access the user information. This is provided from the interface of the superApp.|
| | | |string <= 256 characters [\w-:]+|
|trade_type|String|M| |
| | | | |
| | | |     Value: "InApp"|
| | | | |
|resource_type|String|M|     Value: "OpenId"|
| | | | |
| | | | |


### Response Parameters
|Parameter|Data Type|Description|
|:----|:----|:----|
|result|String|SUCCESS or FAIL. When this field is FAIL, the code field is a specific business error code.|
|code|String|Return code. 0 is successful, the rest is the business error code|
|msg|String|Return information, simple error description.|
|sign| |string <= 512 characters|
| |string|Response signature.|
|nonce_str|string| |
|sign_type|string|Signature type. Currently, only SHA256RSA is supported.|
|biz_content|object (AuthToken| |
| |ResponseBizContent)| |
|open_id|string| |
|identityId|string|Consumer id in mobile payment system|
|identityType|string|Organization or Customer|
|walletIdentityId|string|Wallet identity id|
|identifier|string|msisdn or shortcode. Only authorized partner will get this param returned|
|nickName|string |nickName is the first name of the user.|
| | |Only authorized partner will get this param returned|
| | | |
|status|string|status. Only authorized partner will get this param returned|
|shortcode|string|shortCode. Only authorized partner will get this param returned|
|walletOrgOperator|string|walletOrgOperatorIdentityId. Only authorized partner will get this param returned|
|IdentityId| | |

