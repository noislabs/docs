# Using the faucet

The faucet can be used via an HTTP POST request. From command line you can use curl:

```shell
curl -X POST \
  -d '{"address": "nois1......","denom": "unois"}' \
  -H "Content-type: application/json" \
  http://faucet.noislabs.com/credit
```
