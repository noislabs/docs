# 🚰 Using the faucet

The faucet can be used via an HTTP POST request.

### With curl

From command line you can use curl:

```shell
curl -X POST \
  -d '{"address": "nois1......","denom": "unois"}' \
  -H "Content-type: application/json" \
  https://faucet.noislabs.com/credit
```

### With Postman

See the following screenshot:

<figure><img src="../.gitbook/assets/postman_faucet.png" alt=""><figcaption></figcaption></figure>
