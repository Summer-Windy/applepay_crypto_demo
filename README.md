applepay_crypto_demo
====================
APPLE PAY
 apple pay支付是ios开发 一部分。java后台研发只需要提供订单的详情接口，和支付的回调接口就可以了。

苹果支付加密demo
完美实现苹果支付加密的基本例子。 就像在苹果网站描述的一样
Basic demo-quality implementation of Apple Pay In-App payment crypto, as described at https://developer.apple.com/library/ios/Documentation/PassKit/Reference/PaymentTokenJSON/PaymentTokenJSON.html
如果你喜欢这份工作，来clover工作吧。fork这个项目做一些有用的事情
_If you like this kind of stuff, come work at Clover (https://www.clover.com/). Fork this repo and do something useful._
如何玩这个项目：
-创建自己的ECDSA加密键值对  用用苹果的钥匙串生成CSR
-发送这个CSR到苹果平台，获取apple开发者证书
-用PassKit记录Apple Pay 账单 并且保存到json文件的APIs写一个苹果应用  相关的连接
-从Keychain导出你的私钥到.p12文件
- 将.json文件，开发者证书，.p12导入这个程序
Here's the process to play:
 - Create your own ECDSA keypair and generate the CSR (using Keychain)
 - Send the CSR to Apple and get save the resulting .cer file
 - Write an iOS app that uses PassKit APIs that logs the result of putting up the Apple Pay sheet and save this in a .json file. Recommended: https://github.com/michael-quinlan/swift_basic_apple_pay 
 - Export your private key from Keychain as a .p12 file protected with the password "test" 
 - Feed the .json file, the .cer file, and the .p12 file into this program

重要的部分：
- 做这件事， 你应该正确的使用一个网络（会翻墙等等）
- 当你做好了，不要在你的项目使用这个代码。  私钥应该生成并永久保存在告诉存储器
- 不要继承 苹果的签名认证
IMPORTANT:
 - You should probably use a processor/gateway that does this for you.
 - If you do it yourself, you should never use this code in production! Merchant EC private keys should be generated and remain permanently in an HSM.
 - Didn't implement Apple signature validation yet.

这个程序用PassKitAPI将数据转化为下面的json
This program turns the 'data' element from this JSON (which comes from the PassKit API): 

```
{
    "data": "2DzU9u6byIY4qCs3lW4KgK3JWC6Ac+x28Ck5PLCjQPJ+y6vCrEXqmBfdEm8uWT02lpGtYeo51WVOevuyX6cFguHIUzsCrhdvfSCV456G768lzbH6SwEk5ST/qiKI/rTQbeDAle7l5Njlil50hmVUTLqhmhS3ouC43+rf2NDR7y7Fr+JVkkHBqdEcONJnqFms+SfEPdNXNVccITdO/dkw3FAkXIy1lro1upZkjZSFdm5HCApRkDiTv6FLiUz/osKZsYKWQV+IEZdXjZZ3WF7Zmn8tOvwZdZy4NMq39oQFVt7VA7VRWs/RgPl0BK2xiGqTz1YFW+J6XE62MfW7yc8tFsJlIwTW7uCHY2ENwTFn11flN+7R64PSfPobUWlMjI3jiY+hMtynSkuSUImxXV0J76N4ItX60ce4E8o3ipZe0v6hLjNapr4Y6OcmTKnG0hy0X3f/cczN1K/YXLWkFco=",
    "header": {
        "ephemeralPublicKey": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEtxcxQw0rS30y28P45MB/owA1H9OSeTIkiiuACxEpY7usak/He4suC446HPrPimw4+vZKO2nx+Ntyu13uALT3bA==",
        "publicKeyHash": "spzGX6upCJhx5UD8vCo1+LcIi7+fkxEUaVmhbX18cJM=",
        "transactionId": "79ccd07eb432f80067d8e5bbc4c38ee1def7fcc1827f6ba5b63bf47b283ebf89"
    },
    "signature": "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",
    "version": "EC_v1"
}
```

这里  编辑我的安全信息（支付信息）
Into this (redacted for my security):
```
{
    "applicationExpirationDate": "190131",
    "applicationPrimaryAccountNumber": "370295XXXXX5435",
    "currencyCode": "840",
    "deviceManufacturerIdentifier": "XXXXXXXXXX",
    "paymentData": {
        "emvData": "nycBgJ82AgDCnyYIG2vuQydGkMafEAcGhgEDoLABXzQBAJUFgAABAACCAhzAnwMGAAAAAAAAnxoCCECaAxQQBJwBAJ83BLnvab4="
    },
    "paymentDataType": "EMV",
    "transactionAmount": 100
}
```

