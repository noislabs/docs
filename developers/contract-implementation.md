# Contract implementation

#### Import the nois packages

First thing is to import the packages. \
Add this to your cargo.toml under dependencies.

```
nois-proxy = { path = "../../contracts/nois-proxy", features = ["library"]}
nois-toolbox = { path = "../nois-contracts/packages/nois-toolbox" }
```

#### Configure the proxy-address

We need to add the address of the nois-proxy. One common way to do it is during the instantiation of the contract. To do this, add this to state.rs

```
pub const NOIS_PROXY: Item<Addr> = Item::new("nois_proxy");
```

import the **NOIS\_PROXY** in the contract.rs

```
use crate::state::{NOIS_PROXY, DOUBLE_DICE_OUCOME};
```

Still in the contract.rs add the instatiation msg which validates the nois-proxy address and stores it

```
pub struct InstantiateMsg {
#[cfg_attr(not(feature = "library"), entry_point)]
pub fn instantiate(
...
) -> Result<Response, ContractError> {
    let nois_proxy_addr = deps
        .api
        .addr_validate(&msg.nois_proxy)
        .map_err(|_| ContractError::InvalidProxyAddress)?;
    set_contract_version(deps.storage, CONTRACT_NAME, CONTRACT_VERSION)?;
    NOIS_PROXY.save(deps.storage, &nois_proxy_addr)?;
    ...
}

```

Declare the instantiation msg

```
pub struct InstantiateMsg {
   pub nois_proxy: String,
}
```

add the InvalidProxyError in error.rs

```
  #[error("Proxy address is not valid")]
   InvalidProxyAddress,
```

#### Triggering the randomness request

In order to request the randomness we need a function whether internal or publicly called. For this tutorial we can make a roll dice msg that will in turn call the getNextRandomness(id) handler.\
So the RollDice message gets the id as a parameter

```
ExecuteMsg::RollDice { job_id} => execute_roll_dice(deps, env, info, job_id),
```

```
pub enum ExecuteMsg {
   RollDice {
       /// An ID for this job which allows for gathering the results.
       job_id: String,
   },
}

```

Call the GetNextRandomness(id) from the triggering function

```
pub fn execute_roll_dice(
    deps: DepsMut,
    _env: Env,
    _info: MessageInfo,
    job_id: String,
) -> Result<Response, ContractError> {
    let nois_proxy = NOIS_PROXY.load(deps.storage)?;

    let res = Response::new().add_message(WasmMsg::Execute {
        contract_addr: nois_proxy.into(),
        msg: to_binary(&nois_proxy::ExecuteMsg::GetNextRandomness {
            callback_id: Some(job_id),
        })?,
        funds: vec![],
    });
    Ok(res)
}
```

#### Receiving the randomness

The nois-proxy contract sends the callback on the Receive entrypoint. Therefore, you should add the Receive handler on your ExecuteMsg. In contract.rs add

```
ExecuteMsg::Receive(NoisCallbackMsg {
           id: callback_id,
           randomness,
       }) => execute_receive(deps, env, info, callback_id, randomness),
```

and in msg.rs import NoisCallbackMsg from the nois-proxy package that we included at the beginning and update the Execute msg&#x20;

<pre><code>use nois_proxy::NoisCallbackMsg;
pub enum ExecuteMsg {
<strong>   Receive(NoisCallbackMsg),
</strong>   RollDice {..},
}
</code></pre>

In the receive function you can implement whatever you would like to do with the randomness. You directly use the randomness as a raw hexadecimal or you can apply some common randomness functionalities that you get from the Nois toolbox crate.

```
pub fn execute_receive(
    deps: DepsMut,
    _env: Env,
    _info: MessageInfo,
    callback_id: String,
    randomness: Data,
) -> Result<Response, ContractError> {
    let randomness: [u8; 32] = randomness
        .to_array()
        .map_err(|_| ContractError::InvalidRandomness)?;

   //Congrats, you have access to
   // a publicly-verifiable, unbiasable and decentralised randomness

    Ok(Response::default())
}
```



