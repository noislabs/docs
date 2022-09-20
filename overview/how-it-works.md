# 💡 How it works

<figure><img src="https://1923222875-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FsvlZuSMSUX5tUL5iML2q%2Fuploads%2FH8ofEjHeanKzyNfktImh%2FTwitter%20Header.png?alt=media&#x26;token=19725dc2-8d86-45ae-8ef0-acd027086764" alt=""><figcaption></figcaption></figure>

The following steps are taken to get the randomness:

1. Contract on a CosmWasm-enabled chain (such as Juno or Tgrade) sends a message to a Nois proxy contract on the same chain. A reply with further information regarding the job is sent to the original contract.
2. The proxy contract sends an IBC message to its couter-part on the Nois Network where the job is put in the queue.
3. Once the drand beacon of the correct round is released, a network of bots sends it to the Nois Network for verification.
4. After successful verfication, the pending jobs for the round are processed. For every matching job, an IBC response with the beacon is sent.
5. The proxy contract receives the beacon and sends a callback to the original contract.
