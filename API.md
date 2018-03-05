By default the API is reachable under the port 12303. It is a REST API using JSON encoding.
For security reasons every request is required to send a header parameter "apiKey" that contains your API KEY that you can find in ```~/ubic/config.ini```

Endpoints
===

/
===
returns basic information regarding the node status.

Example response:
c
{
    "bestBlock": {
        "hash": "c7135c059bcd1fcab4f76095110c76f815f712021fbd43c7f2df223515d50d75",
        "height": "1280"
    },
    "synced": "true"
}
```

/wallet
===
returns all addresses contained in the wallet.

Example response:
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
