
## Description：
- parameters format：
    - symbol：str, base_quote.  ps: btc_usdt
    - amount: float
    - time:  int, timestamp. unit in seconds
    - side: buy, sell
    - status: canceled, filled, failed
    - client_name: str, given client name
- private endpoint
    /orders, /position, /price, /deal requires authentication. Please refer to the below example.
[auth example(in Python)](https://github.com/onebit-quant/dealerMaker-API/blob/master/signature_example.py)

## Endpoints

### /symbols

get support trading symbols

- request_params: None
- response:
    - result : 1（return success), 0（return failure)
    - symbols : dict
            key： symbol name
            value: list, [quote precision，base precision，min order vol，min order amount]
```
    {
            "result": 1,
            "symbols": {
                "btc_usdt": [0.01, 0.0001, 0.0001, 1],
                "eth_usdt": [0.01, 0.0001, 0.0001, 1],
                "usdt_btc": [1e-08, 0.01, 1, 0.0001],
                "usdt_eth": [1e-08, 0.01, 1, 0.0001]
            }
    }
```


### /indicativePrice

get the reference price for a given symbol with given vol

- request_params :
    - symbol
    - vol
- response :
    - result : 1（return success), 0（return failure)
    - buy_price: maker’s buy price
    - sell_price: maker’s sell price
    - symbol
    - time
```
    {
            "result": 1,
            "buy_price": 10000.00,
            "sell_price": 10600.00,
            "symbol": "btc_usdt",
            "time": 1546358400
    }
```


### /indicativePriceBatch

get the reference price for all symbols with default vol.
- request_params: None
- response :
    - result : 1（return success), 0（return failure)
    - data: list of dict
    - time
```
    {
        'result': 1,
        'data': [
            {'buy_price': 7331.99, 'sell_price': 7348.8, 'symbol': 'btc_usdt'},
            {'buy_price': 149.84, 'sell_price': 150.13, 'symbol': 'eth_usdt'},
            {'buy_price': 0.00013607, 'sell_price': 0.00013639, 'symbol': 'usdt_btc'},
            {'buy_price': 0.00666116, 'sell_price': 0.00667361, 'symbol': 'usdt_eth'}
        ],
        'time': 1575282550.8962257
    }
```

### /orders

obtain a list of filled orders during a certain period in order to facilitate the settlement of transactions within this time period


- request_params :
    - start : timestamp ,start time
    - end : timestamp , end time
- response :
    - result : 1（return success), 0（return failure)
    - data : list of dict
        - order_id
        - symbol
        - side : buy, sell
        - vol
        - price, transaction price
        - dealt_time


### /orderInfo

get information of given filled order


- request_params :
    - order_id
- response :
    - result : 1（return success), 0（return failure)
    - data : dict
        - order_id
        - symbol
        - side : buy, sell
        - vol
        - price, transaction price
        - dealt_time

### /position

get current total position

- request_params: None
- response:
    - result
    - position : dict
```
{
    "result": 1,
    "position": {
        'btc': -0.002154,
        'usdt': 158.5082533848014
    }
}
```


### /price

get trading price endpoint

- request_params :
    - symbol
    - side
    - vol

NOTE: To sell btc with equivalent value of 100 usdt, the order symbol should be usdt_btc, the side is “buy”, and the vol is 100.

- response :
    - result
    - exchange_able : boolean, false means that market is close
    - price: transaction price
    - symbol
    - vol: the required vol
    - amount: the exchangeable amount of quote coin
    - dealt_time, Transaction confirmation time (generally the time when the transaction server receives the request)
    - order_id

```
    {
            "result": 1,
            "exchange_able": true,
            "symbol": "btc_usdt",
            "vol": "0.001",
            "amount": "10",
            "price": 10000,
            "dealt_time":1546358400,
            "order_id": "dafepofaoifi09aewfdfae"
    }
```


### /deal

confirm the given order


- request_params :
    - order_id
- response :
    - result
    - order_id

```
    {
            "result": 1,
            "order_id": dafepofaoifi09aewfdfae
    }
```
