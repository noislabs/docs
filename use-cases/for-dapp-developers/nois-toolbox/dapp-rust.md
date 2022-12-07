# 📃 DAPP - Rust



#### Import the nois packages <a href="#import-the-nois-packages" id="import-the-nois-packages"></a>

First thing is to import the packages. Add this to your cargo.toml under dependencies.

{% code title="# in cargo.toml" %}
```rust
[dependencies]
nois = "0.5.1"
```
{% endcode %}

#### Coinflip

```rust
let side = nois::coinflip(randomness);
if side.is_heads(){
    println!("heads")
}
if side.is_tails(){
    println!("tails")
}
```

#### int\_in\_range

```rust
//dice_result takes the value of 
let dice_result = nois::int_in_range(randomness, 1..7);
```
