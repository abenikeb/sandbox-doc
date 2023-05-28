---
sidebar_position: 1
---

# applyFabricToken

### Request Parameters
#### HEADER PARAMETERS
|Parameter|  Data Type|  M/O|Description|
|:----|:----|:----|:----|
|X-APP-Key|  String|  M|FabricAppID, provided by fabric portal of Ethio telecom|

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