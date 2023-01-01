# 📃 Contract implementation

Double dice game example [https://github.com/noislabs/nois-dapp-examples](https://github.com/noislabs/nois-dapp-examples)​

#### Import the nois packages <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

First thing is to import the packages. Add this to your cargo.toml under dependencies.

{% code title="# in cargo.toml" %}
```rust
[dependencies]
nois = "0.6.0"
```
{% endcode %}

#### Configure the proxy address <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

We need to add the address of the nois-proxy. One common way to do it is during the instantiation of the contract.

{% code title="# in state.rs" %}
```rust
pub const NOIS_PROXY: Item<Addr> = Item::new("nois_proxy");
```
{% endcode %}

import the **nois-proxy**.

{% code title="# in contract.rs" %}
```rust
use crate::state::{NOIS_PROXY};
```
{% endcode %}

Still in the contract.rs add the instantiation msg which validates the nois-proxy address and stores it

{% code title="# in contract.rs" %}
```rust
pub fn instantiate(
    deps: DepsMut,
    _env: Env,
    info: MessageInfo,
    msg: InstantiateMsg,
) -> Result<Response, ContractError> {
    // The nois-proxy abstracts the IBC and nois chain away from this application
    let nois_proxy_addr = deps
        .api
        .addr_validate(&msg.nois_proxy)
        .map_err(|_| ContractError::InvalidProxyAddress)?;
    set_contract_version(deps.storage, CONTRACT_NAME, CONTRACT_VERSION)?;
    NOIS_PROXY.save(deps.storage, &nois_proxy_addr)?;

    Ok(Response::new()
        .add_attribute("method", "instantiate")
        .add_attribute("owner", info.sender))
}
```
{% endcode %}

Declare the instantiation msg

{% code title="# in msg.rs" %}
```rust
pub struct InstantiateMsg {
   pub nois_proxy: String,
}
```
{% endcode %}

Add the InvalidProxyError in error.rs

{% code title="# in error.rs" %}
```rust
#[error("Proxy address is not valid")]
InvalidProxyAddress,T
```
{% endcode %}

#### Triggering the randomness request <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

In order to request the randomness we need a function whether internal or publicly called. For this tutorial we can make a roll dice msg that will in turn call the getNextRandomness(id) handler.\
So the RollDice message gets the id as a parameter

{% code title="# in contract.rs" %}
```rust
match msg {
        //RollDice should be called by a player who wants to roll the dice
        ExecuteMsg::RollDice { job_id } => execute_roll_dice(deps, env, info, job_id),
    }
```
{% endcode %}

#### Triggering the randomness request <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

In order to request the randomness we need a function whether internal or publicly called. For this tutorial we can make a roll dice msg that will in turn call the getNextRandomness(id) handler.\
So the RollDice message gets the id as a parameter

{% code title="# in contract.rs" %}
```rust
ExecuteMsg::RollDice { job_id} => execute_roll_dice(deps, env, info, job_id),ExecuteMsg::RollDice { job_id} => execute_roll_dice(deps, env, info, job_id),
```
{% endcode %}

{% code title="# in msg.rs" %}
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

{% code title="# in contract.rs" %}
```rust
//execute_roll_dice is the function that will trigger the process of requesting randomness.
//The request from randomness happens by calling the nois-proxy contract
pub fn execute_roll_dice(
    deps: DepsMut,
    _env: Env,
    _info: MessageInfo,
    job_id: String,
) -> Result<Response, ContractError> {
    let nois_proxy = NOIS_PROXY.load(deps.storage)?;

    let response = Response::new().add_message(WasmMsg::Execute {
        contract_addr: nois_proxy.into(),
        //GetNextRandomness requests the randomness from the proxy
        //The job id is needed to know what randomness we are referring to upon reception in the callback
        //In this example, the job_id represents one round of dice rolling.
        msg: to_binary(&ProxyExecuteMsg::GetNextRandomness { job_id })?,
        //In this example the randomness is sent from the gambler, but you may also send the funds from the contract balance
        funds: info.funds, // Just pass on all funds we got
    });
    Ok(response)
}
```
{% endcode %}

#### Receiving the randomness <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

The nois-proxy contract sends the callback on the NoisReceive entrypoint. Therefore, you should add the Receive handler on your ExecuteMsg.

{% code title="# in contract.rs" %}
```rust
//NoisReceive should be called by the proxy contract. The proxy is forwarding the randomness from the nois chain to this contract.
ExecuteMsg::NoisReceive { callback } => execute_nois_receive(deps, env, info, callback),
```
{% endcode %}

and in msg.rs import NoisCallbackMsg from the nois-proxy package that we included at the beginning and update the Execute msg.

{% code title="# in msg.ts" %}
```rust
use nois::NoisCallback;

pub enum ExecuteMsg {
    // job_id for this job which allows for gathering the results.
    RollDice { job_id: String },
    //callback contains the randomness from drand (HexBinary) and job_id
    //callback should only be allowed to be called by the proxy contract
    Receive { callback: NoisCallback },
}
```
{% endcode %}

In the receive function you can implement whatever you would like to do with the randomness. You directly use the randomness as a raw hexadecimal or you can apply some common randomness functionalities that you get from the Nois toolbox crate.

{% code title="# in contract.rs" %}
```rust
pub fn execute_nois_receive(
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
//The execute_receive function is triggered upon reception of the randomness from the proxy contract
//The callback contains the randomness from drand (HexBinary) and the job_id
pub fn execute_receive(
    deps: DepsMut,
    _env: Env,
    info: MessageInfo,
    callback: NoisCallback,
) -> Result<Response, ContractError> {
    //load proxy address from store
    let proxy = NOIS_PROXY.load(deps.storage)?;
    //callback should only be allowed to be called by the proxy contract
    //otherwise anyone can cut the randomness workflow and cheat the randomness by sending the randomness directly to this contract
    ensure_eq!(info.sender, proxy, ContractError::UnauthorizedReceive);
    let randomness: [u8; 32] = callback
        .randomness
        .to_array()
        .map_err(|_| ContractError::InvalidRandomness)?;
    //ints_in_range provides a list of random numbers following a uniform distribution within a range.
    //in this case it will provide 2 uniformly randomized numbers between 1 and 6
    let double_dice_outcome = ints_in_range(randomness, 2, 1, 6);
    //summing the dice to fit the real double dice probability distribution from 2 to 12
    let double_dice_outcome = double_dice_outcome.iter().sum();

    //Congrats, you have access to
    // a publicly-verifiable, unbiasable and decentralised randomness

    Ok(Response::default())
}
```
{% endcode %}
