# Setting up the node

It is currently recommended that the node has at least 4GB of ram and at least 20 gb of disk space.

The installation of the node is explained [here](https://github.com/UBIC-repo/core#installation-on-linux).

#Exchange integration
# configuring the node
By default the node is configured to generate 100 addresses from the ```wallet.dat``` seed. This number can be increased in the ```~/ubic/config.ini``` file.

```
# The number of addresses to generate from the wallet.dat seed
numberOfAdresses = 100
```
This number can be increased indefinitively but note that this will slow down node.

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
            "amount": ""
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

### Getting the balance of a specific addresses
//@todo documentation

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
similar to the one with the transaction pool just above.
```

Note that if you import a ```wallet.dat``` seed it is necessary to reindex the the node in order to get the transaction history of that wallet.

### Getting current transaction fee estimates
//@todo

Not copleted yet
