# 📃 Contract implementation

Double dice game example [https://github.com/noislabs/double-dice-demo](https://github.com/noislabs/double-dice-demo)​

#### Import the nois packages <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

First thing is to import the packages. Add this to your cargo.toml under dependencies.

<pre class="language-rust" data-title="cargo.toml"><code class="lang-rust"><strong>[dependencies]
</strong>nois = "0.5.0"</code></pre>

#### Configure the proxy address <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

We need to add the address of the nois-proxy. One common way to do it is during the instantiation of the contract.

{% code title="state.rs" %}
```rust
pub const NOIS_PROXY: Item<Addr> = Item::new("nois_proxy");
```
{% endcode %}

import the **nois-proxy**.

{% code title="contract.rs" %}
```rust
use crate::state::{NOIS_PROXY, DOUBLE_DICE_OUCOME};
```
{% endcode %}

Still in the contract.rs add the instantiation msg which validates the nois-proxy address and stores it

{% code title="contract.rs" %}
```rust
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

```
{% endcode %}

Declare the instantiation msg

{% code title="msg.rs" %}
```rust
pub struct InstantiateMsg {
   pub nois_proxy: String,
}
```
{% endcode %}

Add the InvalidProxyError in error.rs

{% code title="error.rs" %}
```rust
#[error("Proxy address is not valid")]
InvalidProxyAddress,T
```
{% endcode %}

#### Triggering the randomness request <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

In order to request the randomness we need a function whether internal or publicly called. For this tutorial we can make a roll dice msg that will in turn call the getNextRandomness(id) handler.\
So the RollDice message gets the id as a parameter

{% code title="contract.rs" %}
```rust
ExecuteMsg::RollDice { job_id} => execute_roll_dice(deps, env, info, job_id),ExecuteMsg::RollDice { job_id} => execute_roll_dice(deps, env, info, job_id),
```
{% endcode %}

#### Triggering the randomness request <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

In order to request the randomness we need a function whether internal or publicly called. For this tutorial we can make a roll dice msg that will in turn call the getNextRandomness(id) handler.\
So the RollDice message gets the id as a parameter

{% code title="contract.rs" %}
```rust
ExecuteMsg::RollDice { job_id} => execute_roll_dice(deps, env, info, job_id),ExecuteMsg::RollDice { job_id} => execute_roll_dice(deps, env, info, job_id),
```
{% endcode %}

{% code title="msg.rs" %}
```rust
pub enum ExecuteMsg {
   RollDice {
       /// An ID for this job which allows for gathering the results.
       job_id: String,
   },
}

```
{% endcode %}

Call the GetNextRandomness(id) from the triggering function

{% code title="contract.rs" %}
```rust
pub fn execute_roll_dice(
    deps: DepsMut,
    _env: Env,
    _info: MessageInfo,
    job_id: String,
) -> Result<Response, ContractError> {
    let nois_proxy = NOIS_PROXY.load(deps.storage)?;

    let res = Response::new().add_message(WasmMsg::Execute {
        contract_addr: nois_proxy.into(),
        msg: to_binary(&nois::ProxyExecuteMsg::GetNextRandomness {
            callback_id: Some(job_id),
        })?,
        funds: vec![],
    });
    Ok(res)
}
```
{% endcode %}

#### Receiving the randomness <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

The nois-proxy contract sends the callback on the Receive entrypoint. Therefore, you should add the Receive handler on your ExecuteMsg.

{% code title="contract.rs" %}
```rust
ExecuteMsg::Receive(NoisCallbackMsg {
           id: callback_id,
           randomness,
       }) => execute_receive(deps, env, info, callback_id, randomness),
```
{% endcode %}

and in msg.rs import NoisCallbackMsg from the nois-proxy package that we included at the beginning and update the Execute msg.

{% code title="msg.ts" %}
```rust
use nois::NoisCallbackMsg;

pub enum ExecuteMsg {
   Receive(NoisCallbackMsg),
   RollDice {..},//defined earlier
}
```
{% endcode %}

In the receive function you can implement whatever you would like to do with the randomness. You directly use the randomness as a raw hexadecimal or you can apply some common randomness functionalities that you get from the Nois toolbox crate.

{% code title="contract.rs" %}
```rust
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
{% endcode %}

