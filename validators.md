# Becoming A Validator

In order to become an **active** validator, you must have more stake than the bottom validator. You may still execute the following steps, but you will not be active and therefore won't receive staking rewards.

### [​**Run a Full Node​**](https://docs.nois.network/running-a-fullnode) <a href="#run-a-full-node" id="run-a-full-node"></a>

In order to become a validator, you node must be fully synced with the network. You can check this by doing:

`noisd status | jq .SyncInfo.catching_up`

When the value of `catching_up` is _false_, your node is fully sync'd with the network. You can speed up syncing time by State Syncing to the current block.



```
"sync_info": {
      "latest_block_hash": "A53BA4C8CEF9399A4CE8E49C7FC46264B9EB9440C61819E43F67BD401CE6BCEF",
      "latest_app_hash": "145FB9AFBAF01B78C5B1F052E55EC6D181E4E8AE67E3ED68F9587755D4797370",
      "latest_block_height": "72509",
      "latest_block_time": "2022-08-21T15:31:28.068263737Z",
      "earliest_block_hash": "D6E6967C67A28DC1A9510C26A9E57D0AF3328F1CBC1B29546D830B389EB216BC",
      "earliest_app_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "earliest_block_height": "1",
      "earliest_block_time": "2022-08-17T10:05:19.9269454Z",
      "catching_up": false
    },
```

### **Confirm Wallet is Funded:** <a href="#confirm-wallet-is-funded" id="confirm-wallet-is-funded"></a>

This is the `nois` wallet which you used **** to create your full node, and will use to delegate your funds to you own validator. You must delegate at least 1 NOIS (1000000unois) from this wallet to your validator. `noisd q bank balances $(noisd keys show -a <key-alias>)`

If you get the following message, it means that you have no tokens, or your node is not yet synced:ERROR: unknown address: account nois1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx does not exist

### **Backup Validator Key** <a href="#backup-validator-key" id="backup-validator-key"></a>

Before **** creating your validator, [backup your validator key.](https://docs.scrt.network/secret-network-documentation/node-runners/node-setup/best-practices/validator-backup)WARNING: if you don't backup your key and your node goes down, you will lose your validator and have to start a new one.

### **Create Validator** <a href="#create-validator" id="create-validator"></a>

Remember 1 NOIS = 1,000,000 uNOIS, and so the command below stakes 100 NOIS

`noisd tx staking create-validator \`\
`--amount=100000000unois \`\
`--pubkey=$(noisd tendermint show-validator) \`\
`--identity={KEYBASE_IDENTITY} \`\
`--details="To infinity and beyond!" \`\
`--commission-rate="0.10" \--commission-max-rate="0.20" \`\
`--commission-max-change-rate="0.01" \`\
`--min-self-delegation="1" \`\
`--moniker=<MONIKER> \`\
`--from=<key-alias>`

### **Confirm Validator is Created** <a href="#confirm-validator-is-created" id="confirm-validator-is-created"></a>

You should see your moniker listed.

`noisd q staking validators | grep moniker`

## Important CLI Commands for Validators <a href="#dangers-in-running-a-validator" id="dangers-in-running-a-validator"></a>

### Staking More Tokens <a href="#staking-more-tokens" id="staking-more-tokens"></a>

(remember 1 `NOIS` = 1,000,000 uNOIS)In order to stake more tokens beyond those in the initial transaction, run:\
nois`d tx staking delegate $(noiscli keys show <key-alias> --bech=val -a) <amount>unois --from <key-alias>`

### Editing Your Validator <a href="#editing-your-validator" id="editing-your-validator"></a>

`noisd tx staking edit-validator --moniker "<new-moniker>" --node "https://rpc.nois.network" --identity 6A0D65E29A4CBC8E --details "To infinity and beyond!" --chain-id <chain_id> --from <key_name> --commission-rate "0.10"`

### Seeing Your Rewards From Being A Validator <a href="#seeing-your-rewards-from-being-a-validator" id="seeing-your-rewards-from-being-a-validator"></a>

noisd q distribution rewards $(noiscli keys show -a \<key-alias>)

### Seeing Your Commissions From Your Delegators <a href="#seeing-your-commissions-from-your-delegators" id="seeing-your-commissions-from-your-delegators"></a>

noisd q distribution commission $(noiscli keys show -a \<key-alias> --bech=val)

### Withdrawing Rewards <a href="#withdrawing-rewards" id="withdrawing-rewards"></a>

noisd tx distribution withdraw-rewards $(noiscli keys show --bech=val -a \<key-alias>) --from \<key-alias>

### Withdrawing Rewards+Commissions <a href="#withdrawing-rewards-commissions" id="withdrawing-rewards-commissions"></a>

noisd tx distribution withdraw-rewards $(noiscli keys show --bech=val -a \<key-alias>) --from \<key-alias> --commission

### Removing Your Validator <a href="#removing-your-validator" id="removing-your-validator"></a>

Currently deleting a validator is not possible. If you redelegate or unbond your self-delegations then your validator will become offline and all your delegators will start to unbond.

### Changing Your Validator's Commission-Rate <a href="#changing-your-validator-s-commission-rate" id="changing-your-validator-s-commission-rate"></a>

You are currently unable to modify the `--commission-max-rate` and `--commission-max-change-rate"` parameters.Modifying the commision-rate can be done using this:

`noisd tx staking edit-validator --commission-rate="0.05" --from <key-alias>`

## Slashing <a href="#slashing" id="slashing"></a>

### **Unjailing** <a href="#unjailing" id="unjailing"></a>

To unjail your jailed validator

`noisd tx slashing unjail --from <key-alias>`

## **Signing Info** <a href="#signing-info" id="signing-info"></a>

To retrieve a validator's signing info:

`noisd q slashing signing-info <validator-conspub-key>`

## **Query Parameters** <a href="#query-parameters" id="query-parameters"></a>

You can get the current slashing parameters via:

`noisd q slashing params`
