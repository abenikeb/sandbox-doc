---
sidebar_position: 6
---

# RequestSignatureProcess
On the above requests we are using request signature to make sure the authenticity of the data being sent throught a network. 
For this purpose we require a RSA key pairs to sign and verify a request body. Here is a step by step guide on how to do a request body signature when sending a request to the payment server.

1.	Generate RSA key pairs(Public and Private key) shown on 
2.	Keep the private key to your self and share the public to the SP
3.	Prepare you request accordingly  
    (1)	exclude the sign and signType of the request body parameter  
    (2)	pair the request body parameters in a key=value format. e.g short_code=1234  
    (3)	order/sort the request body parameters in alphabetical order(A-z)  
    (4)	join the alphabeticaly ordered key=value with '&'. eg short_code=1234&timestamp=1684481139  
    (5)	then sign the above prepared string with your private key with SHA256RSA algorithm,  
    (6)	then add it to you request body parameter as a value to the sign parameter.
4.	send the request to the payment server
5.	the SP server will verify you request body by the public key you provided on step 1.
6.	if there is any sign related issue the server will respons a "veriy sign failed message" in this case you should check  
    (1)	the way you prepared the request  
    (2)	the signature algorithm you are using currently.
7.	Here is a sample example of how to sign

#### Here is a sample request body
```json
{
  timestamp: '1684481139',
  nonce_str: 'XG9C5S6R0NLEYF1AGYW5BT237SMDYCUH',
  method: 'payment.preorder',
  version: '1.0',
  biz_content: {
    notify_url: 'https://www.google.com',
    appid: '930231098961202',
    merch_code: '123456',
    merch_order_id: '1684481138534',
    trade_type: 'Checkout',
    title: 'diamond_1.5',
    total_amount: '1.5',
    trans_currency: 'ETB',
    timeout_express: '120m',
    business_type: 'BuyGoods',
    redirect_url: 'https://www.bing.com/',
    callback_info: 'From web'
  },
  sign: 'bu5eJJqmlZW4mMBon9J4dS57BgbHgTMyrlJBEfQSuwJYGCACAS245unOutu324XHfalQ4E0yyvSzndIEDiibSDZmoJbPKb+WUdSXfMlZmo1hPJcf4EXpe+4WRR3nE5ckcpIrrziyOSqkfnaypfB3M01sNSq4bA9oegmf2SLokYo4B/4js6cmXQMZHR0zh89Vxvr3Rh9Jh3lww7uecWHn4af8ouXS3aBWK305H5eZsQ2XVQxVK3hJuT/WnXzKfXCoSsc1jjzT2p7pReQVEwu+LP3i/jmJloCUe8GtEjdOUEWgKBmkJnzIiMENIGYSixHjlgSYVKbSGrfgZN+Sjq2eOQ==',
  sign_type: 'SHA256WithRSA'
}
```

#### Prepare the request to be signed
```
appid=930231098009602&business_type=BuyGoods&callback_info=From web&merch_code=101011&merch_order_id=1684481138534&method=payment.preorder&nonce_str=XG9C5S6R0NLEYF1AGYW5BT237SMDYCUH&notify_url=https://www.google.com&redirect_url=https://www.bing.com/&timeout_express=120m&timestamp=1684481139&title=diamond_1.5&total_amount=1.5&trade_type=Checkout&trans_currency=ETB&version=1.0
```

Sign the the request parameter with a custom sign function with your private key and with SHA256RSA algorithm.
Then place the correct sign on the body request and send it to the payment server.
For more information check the demo provided on the document on Quick Access


