---
sidebar_position: 1
---

# ApplyFabricToken

### Request Parameters

#### HEADER PARAMETERS
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|X-APP-Key|  String|  M|Fabric App ID, provided by fabric portal of Ethio telecom|

#### REQUEST BODY SCHEMA
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|appSecret|  String|  M|App Secret, provided by fabric portal of Ethio telecom|

#### Example Http Request
-- Table here --

### Response Parameters
|Parameter|Data Type|Description|
|:----|:----|:----|
|token|String|ApiFabric Token|
|effectiveDate|String|Effective Date of App Token, the Format is yyyyMMddHHmmss|
|expirationDate|String|Expiration Date of App Token, the Format is yyyyMMddHHmmss|

#### Example Http Response (200: Processed Successful)
-- Table here --

**Add description here**

- `API\service\applyFabricTokenService.js`

## Create apply token service

Create a file at `API\service\applyFabricTokenService.js`:

```jsx title="API\service\applyFabricTokenService.js"
const https = require("http");
const config = require("../config/config");
var request = require("request");

function applyFabricToken() {
  return new Promise((resolve, reject) => {
    var options = {
      method: "POST",
      url: config.baseUrl + "/payment/v1/token",
      headers: {
        "Content-Type": "application/json",
        "X-APP-Key": config.fabricAppId,
      },
      rejectUnauthorized: false, //add when working with https sites
      requestCert: false, //add when working with https sites
      agent: false, //add when working with https sites
      body: JSON.stringify({
        appSecret: config.appSecret,
      }),
    };
    console.log(options);
    request(options, function (error, response) {
      let result = JSON.parse(response.body);
      resolve(result);
    });
  });
}

module.exports = applyFabricToken;
```
### Request Parameters
#### HEADER PARAMETERS
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|X-APP-Key|  String|  M|Fabric App ID, provided by fabric portal of Ethio telecom|

#### REQUEST BODY SCHEMA
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|appSecret|  String|  M|App Secret, provided by fabric portal of Ethio telecom|

### Response Parameters
|Parameter|Data Type|Description|
|:----|:----|:----|
|token|String|ApiFabric Token|
|effectiveDate|String|Effective Date of App Token, the Format is yyyyMMddHHmmss|
|expirationDate|String|Expiration Date of App Token, the Format is yyyyMMddHHmmss|

### Example Http Request
-- Table here --