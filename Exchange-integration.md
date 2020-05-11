# Setting up the node

It is currently recommended that the node has at least 4GB of ram and at least 20 gb of disk space.

The installation of the node is explained [here](https://github.com/UBIC-repo/core#installation-on-linux).

# Exchange integration
# configuring the node
By default the node is configured to generate 100 addresses from the ```wallet.dat``` seed. This number can be increased in the ```~/ubic/config.ini``` file.

```
# The number of addresses to generate from the wallet.dat seed
numberOfAdresses = 100
```
The number of addresses generated from the ```wallet.dat``` seed can be increased indefinitively but note that this will slow down node.

The node only accepts the connection from one IP address that is specified also specified in the ```config.ini``` file. By default this ip address is set to ```127.0.0.1```

# Querying the node API
UBIC doesn't use RPC but a REST API that is listening on port 12303.
To use it, it is necessary to provide an ```apiKey``` field in the header of each request.
The apiKey is found in the ```~/ubic/config.ini``` file.
Example:
```
#password that needs to be send with each API request
apiKey = fe20aa8c5bfe3d7d74100264
```
### Getting all addresses
Query:
```
GET http://127.0.0.1:12303/wallet
```
Response:
```
{
    "addresses": [
        {
            "readable": "qYyVStModu24XihWZEp2t7iUin4Vnyd92",
            "addressLink": "be97078a75326d437f5e586cd9cefdb193b6fad4",
            "hexscript": "92169a581212821d38d13c76d27941fb13515367",
            "pubKey": "020a706ff5f3c75858c6ca1f156bc37cf91a3d6e3ce1179f40ae174fb1fda1972e",
            "amount": {
                "8": "1000000"
            }
        },
        .......
    ],
    "total": {
        "8": "1000000"
    }
}
```
The ```addressLink``` is used in transaction inputs to reference an address. The ```hexscript``` is used in transaction outputs. Both fields are important when handling a trasaction.

The ```total``` field shows the total balance of the node, ```8``` is the currency ID for ```UCN``` and ```1000000``` represents one coin. (Every coin has 1,000,000 sub units)

### Getting all supported currencies
Query:
```
GET http://127.0.0.1:12303/currencies/
```
Response:
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

### Getting the balance of a specific addresses
Query:
```
GET http://127.0.0.1:12303/address/{addressLink}
```
Response:
```
{
    "address": {
        "nonce": "0",
        "scriptType": "2",
        "script": "92169a581212821d38d13c76d27941fb13515367",
        "amountWithUBI": {
            "8": "1000000"
        },
        "amountWithoutUBI": {
            "8": "1000000"
        },
        "UBIdebit": "",
        "dsc": ""
    }
}
```

### Getting the transactions contained within the transaction pool
Query:
```
GET http://127.0.0.1:12303/txpool
```
Response:
```
{
    "transactions": [
        {
            "txId": "ea271ccd67ad67567408288b885c118246c6d5b3ecd7b9d787903e5ed0753e04",
            "txHash": "f4f07eb2d7b2654ddc73f868554d9775286922c7d3e5087f81998551fa0cadbb",
            "network": "1",
            "fee": {
                "8": "15513700"
            },
            "txIn": [
                {
                    "inAddress": "a70d9481b5eb44ee8b812921fb9fdaa46f5b9bdc",
                    "scriptType": "2",
                    "script": "002103ef8a116365f9f185036da1fca0e5df6a55fe0b883796cf9a66246fdb6ef7f91a47304502207aafc7486212af3c06dfc0be8351a1a21868f01048d3769369078a291ce5dee502210098ed0d040c19474b8282159d6274f99c6cc9b797179b4a02f238285614660113",
                    "amount": {
                        "8": "25513700"
                    },
                    "nonce": "60",
                    "isMine": "false"
                }
            ],
            "txOut": [
                {
                    "scriptType": "2",
                    "script": "fe55ea22d8fb83d673e11af3b8e454709c3105f2",
                    "amount": {
                        "8": "10000000"
                    },
                    "isMine": "false"
                }
            ],
            "timestamp": "1589161264"
        }
    ]
}
```
In order to identify which inputs and outputs of the transaction match to a specific address in the wallet it is necessary to use the ```hexscript``` and ```addressLink``` fields from the wallet API call as already specified.

### Getting all transactions for a wallet
Query:
```
GET http://127.0.0.1:12303/wallet
```
Response:
```
similar to the one with the transaction pool just above with the exception thatit contains 2 new fields:
"isInMainChain": "true",
"confirmations": "336"
```

Note that if you import a ```wallet.dat``` seed it is necessary to reindex the the node in order to get the transaction history of that wallet.

### Sending a transaction
Step 1, create the transaction:
Query:
```
POST http://127.0.0.1:12303/wallet/createTransaction
json: {"qb67wUFHwwf8JE3Fet7XuzdqMQZL1NLmh":{"2":100000000},"qZjgXyYzcTkaAvFxvEb64kRHQLknnyVrq":{"2":50000000}}
```
Response:
```{
    "success": "true",
    "transaction": {
        "txId": "4242f382a6b25a48649150a94d4703ddb46b78065e01f9b511cad93fc0dcf67e",
        "txHash": "0dc845eb0b2130c8a3eacf107bbbb68bc45e80087be80e647f2d101cf9d52f3f",
        "network": "1",
        "fee": {
            "2": "11665830"
        },
        "txIn": [
            {
                "inAddress": "c6372b915334df02de5891cd6f3ead8dc9de9858",
                "scriptType": "2",
                "script": "002103a67d080bcb585822fb9238db48b59a9115953f36afbc1b585dc29b338b60a8c9473045022100bdfe02d3cdb136325ce16710d201cc08445fd79e3c3b32da04e08e389b8870ec0220042de42ea203b0a60cd672121d9ed31c84af0a25a256578bb33760bdf608f2ed",
                "amount": {
                    "2": "161665830"
                },
                "nonce": "0"
            }
        ],
        "txOut": [
            {
                "scriptType": "2",
                "script": "f87438345af081c0bd0ae3e3b1ef2006ade520ab",
                "amount": {
                    "2": "100000000"
                }
            },
            {
                "scriptType": "2",
                "script": "b6fa2b3915ae6ef84797ceec74f0181f2b34141f",
                "amount": {
                    "2": "50000000"
                }
            }
        ]
    },
    "base64": "AQEBAgAAAAAJotMmFMY3K5FTNN8C3liRzW8+rY3J3phYAAAAAAJrACEDpn0IC8tYWCL7kjjbSLWakRWVPzavvBtYXcKbM4tgqMlHMEUCIQC9\/gLTzbE2MlzhZxDSAcwIRF\/Xnjw7MtoE4I44m4hw7AIgBC3kLqIDsKYM1nISHZ7THISvCiWiVleLszdgvfYI8u0CAQIAAAAABfXhAAIU+HQ4NFrwgcC9CuPjse8gBq3lIKsBAgAAAAAC+vCAAhS2+is5Fa5u+EeXzux08BgfKzQUHwAA"
}
```
In this case we created a transaction with 2 outputs.

Step 2 is to send the transaction:

Query:
```
POST http://127.0.0.1:12303/wallet/send
json: {"base64":"AQEBAgAAAAAJotMmFMY3K5FTNN8C3liRzW8+rY3J3phYAAAAAAJrACEDpn0IC8tYWCL7kjjbSLWakRWVPzavvBtYXcKbM4tgqMlHMEUCIQC9/gLTzbE2MlzhZxDSAcwIRF/Xnjw7MtoE4I44m4hw7AIgBC3kLqIDsKYM1nISHZ7THISvCiWiVleLszdgvfYI8u0CAQIAAAAABfXhAAIU+HQ4NFrwgcC9CuPjse8gBq3lIKsBAgAAAAAC+vCAAhS2+is5Fa5u+EeXzux08BgfKzQUHwAA"}
```
Response:
```
{"success": true}
```

### Getting current transaction fee estimates
Query:
```
GET http://127.0.0.1:12303/fees
```
Response:
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
        "8": "75685000",
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

# General recommendations

### Protection against dust attacks
The node wallet is protected against dust attacks, it will not include a transaction input that is not "profitable".

### Bundling transaction
As of now an address can not send a transaction if it has another transaction that is still unconfirmed.
It is recommended to bundle withdrawal requests together in a single transaction with multiple transaction outputs.
The current block time is between 3 and 5 minutes, so transactions should idealy be send in intervals of 10 minutes or more to avoid the issue mentionned above.

### Stay informed and be cautiousness
UBIC is an experimental cryptocurrency that is still in it's early stages, this means that bugs are likely to occur.
Many new releases with new protocol features are also sheduled, so it is important to stay informed in order to keep the node running.

### Use the web interface
Use the web interface of the node to play around, using the browser inspection tools you will be able to see and analyze the API calls that are made.

# Questions
If you have questions don't hesitate to open a github issue.
