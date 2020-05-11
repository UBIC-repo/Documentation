默认情况下，API端口12303,JSON编码的REST API。

出于安全考虑，每个请求都需要发送一个头参数``` apiKey ```，该参数包含您可以在``` ~/ubic/config.ini```中找到的API密匙

接口
===

```/``` or ```status```
===
返回关于节点状态的基本信息。

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/status/
```

返回:
```
{
    "bestBlock": {
        "hash": "2467bb2e591cff6eda451f03eca10cf771ee5a04be031ec403c0c5dbdd74eb5c",
        "height": "274453"
    },
    "synced": "true",
    "peersCount": "11"
}
```
/currencies
===
币种

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/currencies/
```

返回:
```
{
    "currencyCodes": {
        "1": "UCH",
        "2": "UDE",
        "3": "UAT",
        "4": "UUK",
        "5": "UIR",
        "6": "UUS",
        "7": "UAU",
        "8": "UCN",
        "9": "USE",
        "10": "UFR",
        "11": "UCA",
        "12": "UJP",
        "13": "UTH",
        "14": "UNZ",
        "15": "UAE",
        "16": "UFI",
        "17": "ULU",
        "18": "USG",
        "19": "UHU",
        "20": "UCZ",
        "21": "UMY",
        "22": "UUA",
        "23": "UES",
        "24": "UMC",
        "25": "ULI",
        "26": "UIS",
        "27": "UHK",
        "28": "UES"
    }
}
```

/incoming
===
正在转入的交易

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/wallet/
```

返回:
```
{
    "transactions": [
        {
            "txId": "a3f339fd676848f6d2bfb2ba7ee6bf55155a8e2e1d27582e9a0299cdb778e208",
            "txHash": "564b0ab6abc690f7a3556987badd9392e5e865e3cade479bff8445b1ee4151c1",
            "network": "1",
            "fee": {
                "2": "9570000"
            },
            "txIn": [
                {
                    "inAddress": "62a95d5f3ba8a551384aa85f14d6c4a1d1821770",
                    "scriptType": "2",
                    "script": "0021020637a211d682d1402557be5407336c9958cf11809952835d7ce91eef8976b848473045022100c3394ac5369f2cedc31eeac8d83c083c4306759acec402e62ea4a8566ada753902207de5eb584543b1eba6a678d614bf5280272cb80cbd613de0b95e31df57e367e3",
                    "amount": {
                        "2": "1009570000"
                    },
                    "nonce": "34",
                    "isMine": "false"
                }
            ],
            "txOut": [
                {
                    "scriptType": "2",
                    "script": "fc5892311282ed287f50a2553936824bacbac30e",
                    "amount": {
                        "2": "1000000000"
                    },
                    "isMine": "true"
                }
            ],
            "timestamp": "1589167942"
        }
    ]
}
```

/wallet
===
返回钱包中包含的所有地址。

请求
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/wallet/
```

返回:
```
{
    "addresses": [
        {
            "readable": "qXPeVk5oQgnXzN2WF1bzejtYf9VcpXQxi",
            "hex": "456bb9959c8d3d3b2433674e81e41567822ee5d2",
            "pubKey": "02fdcff0506a3569aabefd49d4be88386a7eb44815d363490e792ad23029c5c49c",
            "amount": ""
        },
        {
            "readable": "qYxo1v3DRrpqsxmePKTXHvA44sGreWFRU",
            "hex": "9181a37f79bdfa3d2ae07f70fb429a55f2ddab85",
            "pubKey": "029ecab6b53e32fe2f599874b25c6640431bf80264feffb2c159a85c962fceedf8",
            "amount": ""
        },
        {
            "readable": "qWusuMDj8Vojt1kXM69GXgMDTqEPRU2iE",
            "hex": "2e3d93c77ed8b3e9aa2923c1eeb87529bb20d897",
            "pubKey": "022789585774ccb185e583eac6c3ad326776b87988afac324c4b0982ce99276760",
            "amount": ""
        },
    ],
    "total": ""
}
```

/wallet/createTransaction
===
创建交易

请求:
```
curl 'http://127.0.0.1:12303/wallet/createTransaction' \
  -H 'apiKey: xxxxxx' \
  --data 'json={"qYqo3LsVmyusiZHSqAEndcLnNSgEqNUah":{"2":100000000}}' \
  --compressed \
  --insecure
```
```
{"qYqo3LsVmyusiZHSqAEndcLnNSgEqNUah":{"2":100000000}}
地址：qYqo3LsVmyusiZHSqAEndcLnNSgEqNUah
币种：2
数量：100000000/1000000=100个
```

返回
```
{
    "success": "true",
    "transaction": {
        "txId": "e3fca4300a58e04406858a4140f54344e26a367c7e9f762ca9f1834117f013c5",
        "txHash": "0d26ea9953bbf95dcfb626669525e513f1925f69b7cce3578c61e7b352c9a355",
        "network": "1",
        "fee": {
            "2": "9849922"
        },
        "txIn": [
            {
                "inAddress": "7ce3318301bad5fbdb58f8d034bac2d8453ffc46",
                "scriptType": "2",
                "script": "002102bf863cb4e2493773d957587d019bbfe4730ec351a371962db752c9a08cafa464473045022021fc32ae9ea8c8206663cd305dfc392a09c9337da6ce9db7221c3907b4730a3a022100b6611d94dfed8083651495ac618a05b0f64b27b1d3674f3a25627c4192096adf",
                "amount": {
                    "2": "109849922"
                },
                "nonce": "0"
            }
        ],
        "txOut": [
            {
                "scriptType": "2",
                "script": "8ba9d7d6a74303a79548500aca0423968431a030",
                "amount": {
                    "2": "100000000"
                }
            }
        ]
    },
    "base64": "AQEBAgAAAAAGjC1CFHzjMYMButX721j40DS6wthFP\/xGAAAAAAJrACECv4Y8tOJJN3PZV1h9AZu\/5HMOw1GjcZYtt1LJoIyvpGRHMEUCICH8Mq6eqMggZmPNMF38OSoJyTN9ps6dtyIcOQe0cwo6AiEAtmEdlN\/tgINlFJWsYYoFsPZLJ7HTZ086JWJ8QZIJat8BAQIAAAAABfXhAAIUi6nX1qdDA6eVSFAKygQjloQxoDAAAA=="
}
```

/wallet/send
===
发送交易

请求:
```
curl 'http://127.0.0.1:12303/wallet/send' \
  -H 'apiKey: xxxxxx' \
  --data 'json={"base64":"AQEBAgAAAAAGjC1CFHzjMYMButX721j40DS6wthFP/xGAAAAAAJrACECv4Y8tOJJN3PZV1h9AZu/5HMOw1GjcZYtt1LJoIyvpGRHMEUCICH8Mq6eqMggZmPNMF38OSoJyTN9ps6dtyIcOQe0cwo6AiEAtmEdlN/tgINlFJWsYYoFsPZLJ7HTZ086JWJ8QZIJat8BAQIAAAAABfXhAAIUi6nX1qdDA6eVSFAKygQjloQxoDAAAA=="}' \
  --compressed \
  --insecure
```
json数据为创建交易后返回的base64数据

返回：
```
{"success": true}
```

/wallet/transactions
===
本地发生的转账

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/wallet/transactions/
```

返回:
```
{
    "transactions": [
        {
            "txId": "e3fca4300a58e04406858a4140f54344e26a367c7e9f762ca9f1834117f013c5",
            "txHash": "0d26ea9953bbf95dcfb626669525e513f1925f69b7cce3578c61e7b352c9a355",
            "network": "1",
            "fee": {
                "2": "9849922"
            },
            "txIn": [
                {
                    "inAddress": "7ce3318301bad5fbdb58f8d034bac2d8453ffc46",
                    "scriptType": "2",
                    "script": "002102bf863cb4e2493773d957587d019bbfe4730ec351a371962db752c9a08cafa464473045022021fc32ae9ea8c8206663cd305dfc392a09c9337da6ce9db7221c3907b4730a3a022100b6611d94dfed8083651495ac618a05b0f64b27b1d3674f3a25627c4192096adf",
                    "amount": {
                        "2": "109849922"
                    },
                    "nonce": "0",
                    "isMine": "true"
                }
            ],
            "txOut": [
                {
                    "scriptType": "2",
                    "script": "8ba9d7d6a74303a79548500aca0423968431a030",
                    "amount": {
                        "2": "100000000"
                    },
                    "isMine": "false"
                }
            ],
            "isInMainChain": "true",
            "confirmations": "3"
        },
        {
            "txId": "a3f339fd676848f6d2bfb2ba7ee6bf55155a8e2e1d27582e9a0299cdb778e208",
            "txHash": "564b0ab6abc690f7a3556987badd9392e5e865e3cade479bff8445b1ee4151c1",
            "network": "1",
            "fee": {
                "2": "9570000"
            },
            "txIn": [
                {
                    "inAddress": "62a95d5f3ba8a551384aa85f14d6c4a1d1821770",
                    "scriptType": "2",
                    "script": "0021020637a211d682d1402557be5407336c9958cf11809952835d7ce91eef8976b848473045022100c3394ac5369f2cedc31eeac8d83c083c4306759acec402e62ea4a8566ada753902207de5eb584543b1eba6a678d614bf5280272cb80cbd613de0b95e31df57e367e3",
                    "amount": {
                        "2": "1009570000"
                    },
                    "nonce": "34",
                    "isMine": "false"
                }
            ],
            "txOut": [
                {
                    "scriptType": "2",
                    "script": "fc5892311282ed287f50a2553936824bacbac30e",
                    "amount": {
                        "2": "1000000000"
                    },
                    "isMine": "true"
                }
            ],
            "isInMainChain": "true",
            "confirmations": "4"
        }
    ]
}
```
/wallet/generate-key-pair
===
生成一个地址，包含私钥

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/wallet/generate-key-pair
```

返回:

```
{
    "warning": "this key is not stored in the UBIC wallet",
    "privateKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "readableAddress": "qXvua5m8hJ5JDbtyym5edyrL2CDYxxxxx"
}

```
/address/{addressLink}
===
查询地址交易
请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/address/{addressLink}
```

返回:
```
{
    "address": {
        "nonce": "0",
        "scriptType": "2",
        "script": "00de042c56f08b77f4304d3374cce334fa3c9069",
        "amountWithUBI": {
            "8": "500000000"
        },
        "amountWithoutUBI": {
            "8": "500000000"
        },
        "UBIdebit": "",
        "dsc": ""
    }
}
```

/peers
===
返回所有已连接的节点。

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/peers/
```

返回:
```
{
    "peers": [
        {
            "ip": "101.133.143.192",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "163.172.182.123",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "192.161.49.152",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "195.154.106.30",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "219.152.54.247",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "27.155.101.114",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "47.240.160.37",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "47.57.122.73",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "47.75.13.236",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "51.15.99.155",
            "blockHeight": "274454",
            "donationAddress": ""
        },
        {
            "ip": "91.200.242.115",
            "blockHeight": "274454",
            "donationAddress": ""
        }
    ]
}

```
/peers/add
===
增加节点

```
curl 'http://127.0.0.1:12303/wallet/add' \
  -H 'apiKey: xxxxxx' \
  --data 'json={"ip": "47.240.160.37"}' \
  --compressed \
  --insecure
```

返回:
```
{"success": true}
```

/peers/remove
===
删除节点

```
curl 'http://127.0.0.1:12303/wallet/send' \
  -H 'apiKey: xxxxxx' \
  --data 'json={"ip": "47.240.160.37"}' \
  --compressed \
  --insecure
```

返回:
```
{"success": true}
```

/txpool
===
从事务池返回挂起的事务。

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/peers/
```

返回
```
{
    "transactions": ""
}
```

/delegates
===
返回所有已知的委托。

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/peers/
```

示例:
```
{
    "delegates": [
        {
            "pubKey": "026b84273f8a385c0858995daae91002cbcc341381cbeb5ff0cd66e29f9ae751ed",
            "nonce": "0",
            "totalVote": "14",
            "votes": "14",
            "unVotes": "0",
            "lastVotedInBlock": "",
            "isActive": "true",
            "isCurrent": "false",
            "isMe": "false",
            "votes": ""
        }
    ]
}
```

/blocks/{height or hash}
===
返回一个块的内容

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/blocks/{height or hash}
```

返回:
```
{
    "blockHeader": {
        "headerHash": "0af7d525a40b93cdb9873ff953731b09e341056981ea1689b890fb745bdd0a92",
        "previousHeaderHash": "nullptr",
        "merkleRootHash": "nullptr",
        "blockHeight": "1",
        "timestamp": "1519606896",
        "issuerPubKey": "03193ad92191b376ddfb70d5a6ab5f41a5197644fdd39d1d9dffcbc6cc18dbec19",
        "issuerSignature": "304502202608ec61f10341af5ffe94a7087bfb34a78bebcc340a85328051119aee5bcbf4022100c5bc34dd1f629ea960916a09eb4b849f39010c8f74efa2897220130054fb476a",
        "payout": "",
        "payoutRemainder": "",
        "ubiReceiverCount": {
            "1": "0",
            "2": "0",
            "3": "0",
            "4": "0",
            "5": "0",
            "6": "0",
            "7": "0",
            "8": "0",
            "9": "0",
            "10": "0",
            "11": "0",
            "12": "0",
            "13": "0",
            "14": "0",
            "15": "0",
            "16": "0",
            "17": "0",
            "18": "0",
            "19": "0",
            "20": "0",
            "21": "0",
            "22": "0",
            "23": "0",
            "24": "0",
            "25": "0"
        },
        "votes": ""
    },
    "transactions": ""
}
```

/bans
===
节点的分数

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/bans
```

返回:
```
{
    "bans": [
        {
            "ip": "195.154.106.30",
            "score": "0"
        }
    ]
}
```

fees
===
转账手续费率

请求:
```
curl -s -H "apiKey:xxxxxxx" http://127.0.0.1:12303/fees
```

返回:
```
{
    "description": "Fees for 1MB (1000 bytes)",
    "fees": {
        "1": "4850000",
        "2": "47850000",
        "3": "5050000",
        "4": "37975000",
        "5": "2750000",
        "6": "186975000",
        "7": "13900000",
        "8": "77478000",
        "9": "5725000",
        "10": "38875000",
        "11": "21000000",
        "12": "73500000",
        "13": "38775000",
        "14": "2650000",
        "15": "5375000",
        "16": "3175000",
        "17": "250000",
        "18": "3250000",
        "19": "5675000",
        "20": "6125000",
        "21": "18400000",
        "22": "26050000",
        "23": "800000",
        "24": "25000",
        "25": "25000",
        "26": "200000",
        "27": "4275000",
        "28": "27000000"
    }
}

```