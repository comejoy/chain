# KuCoin 库币API文档

## 接口API
###List coins(获取交易币种信息)
````获取交易列表信息
Request:
    https://api.kucoin.com/v1/market/open/coins
Response:
    {
      "success": true,
      "code": "OK",
      "msg": "Operation succeeded.",
      "timestamp": 1546495724158,
      "data": [
        {
          "withdrawMinFee": 5,
          "coinType": "NEP5",
          "withdrawMinAmount": 100,
          "withdrawRemark": {},
          "orgAddress": {},
          "txUrl": "https://neotracker.io/tx/{txId}",
          "userAddressName": {},
          "withdrawFeeRate": 0.001,
          "confirmationCount": 12,
          "infoUrl": {},
          "enable": true,
          "name": "Travala",
          "tradePrecision": 4,
          "depositRemark": {},
          "enableWithdraw": true,
          "enableDeposit": true,
          "coin": "AVA"
        },
        {
          "withdrawMinFee": 0.5,
          "coinType": "ERC20",
          "withdrawMinAmount": 10,
          "withdrawRemark": {},
          "orgAddress": {},
          "txUrl": "https://etherscan.io/tx/{txId}",
          "userAddressName": {},
          "withdrawFeeRate": 0.001,
          "confirmationCount": 12,
          "infoUrl": {},
          "enable": true,
          "name": "Dai",
          "tradePrecision": 4,
          "depositRemark": {},
          "enableWithdraw": true,
          "enableDeposit": true,
          "coin": "DAI"
        },
        ......
      ]
    }
````

###Get coin info(获取币种信息)
````angular2
Request:
    https://api.kucoin.com/v1/market/open/coin-info?coin=GGC
  Params:
    coin    String
Response:
    {
      "success": true,
      "code": "OK",
      "msg": "Operation succeeded.",
      "timestamp": 1546496712337,
      "data": {
        "withdrawMinFee": 0.01,
        "coinType": "ERC20",
        "withdrawMinAmount": 0.2,
        "withdrawRemark": null,
        "orgAddress": null,
        "txUrl": "https://etherscan.io/tx/{txId}",
        "userAddressName": null,
        "withdrawFeeRate": 0.001,
        "confirmationCount": 12,
        "infoUrl": null,
        "enable": true,
        "name": "GramGold Coin",
        "tradePrecision": 4,
        "depositRemark": null,
        "enableWithdraw": true,
        "enableDeposit": true,
        "coin": "GGC"
      }
    }
````

###Tick(获取行情信息)
````angular2
Request:
    https://api.kucoin.com/v1/open/tick?symbol=GGC-BTC
  Params:
    symbol    enum   eg:GCC-BTC
Response:
    {
      "success": true,
      "code": "OK",
      "msg": "Operation succeeded.",
      "timestamp": 1546494590490,
      "data": {
        "coinType": "GGC",
        "trading": true,
        "symbol": "GGC-BTC",
        "lastDealPrice": 0.01176558,
        "buy": 0.01171421,
        "sell": 0.01177567,
        "change": -7.798E-5,
        "coinTypePair": "BTC",
        "sort": 20,
        "feeRate": 0.0025,
        "volValue": 65.40304974,
        "high": 0.01192763,
        "datetime": 1546494590000,
        "vol": 5580.0279,
        "low": 0.0114969,
        "changeRate": -0.0066
      }
    }
````

## REFERENCE
[KUCOIN官网](https://www.kucoin.com/)

[KUCOIN API链接](https://kucoinapidocs.docs.apiary.io/#)