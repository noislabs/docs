# 🪛 JS

The Nois Toolbox can be compiled to JavaScript via WebAssembly. This way you can simulate\
the outputs for every randomness value. The results match exactly those of CosmWasm contracts\
using the same tools.

In order to keep the JS/Wasm interface simple, there is a wrapper in the module `lib/js` which takes\
randomness inputs in hex format and uses types and error handling that plays well with JS.\
JS/Wasm bindings are created using wasm-bindgen.

The JS does not match 100% the contract implementation. The differences are documented here.

| Contract function                                                                  | JS function      | Status     | Note                                                                 |
| ---------------------------------------------------------------------------------- | ---------------- | ---------- | -------------------------------------------------------------------- |
| [`nois::coinflip`](https://docs.rs/nois/latest/nois/fn.coinflip.html)              | `coinflip`       | ✅ Ready    | Returns string instead of enum                                       |
| [`nois::roll_dice`](https://docs.rs/nois/latest/nois/fn.roll\_dice.html)           | `roll_dice`      | ✅ Ready    |                                                                      |
| [`nois::int_in_range`](https://docs.rs/nois/latest/nois/fn.int\_in\_range.html)    | `int_in_range`   | ✅ Ready    | Only supports half-oen range, i.e. the end value is always exluded   |
| [`nois::ints_in_range`](https://docs.rs/nois/latest/nois/fn.ints\_in\_range.html)  | `ints_in_range`  | 🚫 Missing |                                                                      |
| [`nois::random_decimal`](https://docs.rs/nois/latest/nois/fn.random\_decimal.html) | `random_decimal` | ✅ Ready    | Encodes result Decimal as string                                     |
| [`nois::sub_randomness`](https://docs.rs/nois/latest/nois/fn.sub\_randomness.html) | `sub_randomness` | ✅ Ready    | Takes a `count` argument and returns an Array instead of an iterator |
| [`nois::shuffle`](https://docs.rs/nois/latest/nois/fn.shuffle.html)                |                  | 🚫 Missing |                                                                      |

\
