---
description: Tools to safely transform and manipulate your randomness, onchain and offchain
---

# 📦 Nois Toolbox

Next to providing the randomness, Nois offers a set of tooling to help developers reshape the received randomness in order to apply it to their custom use case.

#### Why use the nois-toolbox instead of transforming the randomness yourself?&#x20;

\
Sometimes it can be tricky to manipulate randomness and you can end up with a randomness that follows a diffirent distribution fromwhat is intended or you can also unintentionally lower the entropy of the original randomness. This is why Nois provides this well tested, standard and opensource toolbox that everyone can contribute to and maintain. This way it is in the interest of all that any bug is quickly fixed and the efforts of 1 project contribute to the common good.

The Nois Toolbox can be compiled to JavaScript via WebAssembly. This way you can simulate the outputs for every randomness value. The results match exactly those of CosmWasm contracts using the same tools. The toolbox is available in this github repository [https://github.com/noislabs/nois](https://github.com/noislabs/nois) and two artifacts are published. \
\- The rust crate: [https://crates.io/crates/nois](https://crates.io/crates/nois)\
\- The npm package: [https://www.npmjs.com/package/nois](https://www.npmjs.com/package/nois)&#x20;

If you want to use the nois-toolbox in your contract all you have to do is import the nois crate. If you want to verify, play or simulate the randomness offchain on a script or a UI on a browser instead you can use the nois npm package.



This table provides an overview of the functions that the toolbox provides

| Function            | Description                                                                                                                                                                                                                 |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| coinflip            | Returns heads or tails.                                                                                                                                                                                                     |
| roll\_dice          | <p>returns a random int between 1-6.<br>For a double(or multi) dice you can use ints_in_range or sub_randomness instead.</p>                                                                                                |
| decimal             | Returns a random decimal between 0 and 1 (uniform distribution).                                                                                                                                                            |
| ints\_\_in\_\_range | <p>Provides a list of integers between two numbers.<br>Can be used for a double dice through</p>                                                                                                                            |
| int\_\_in\_\_range  | Provides one integer between two numbers. Can be used for a single dice                                                                                                                                                     |
| shuffle             | <p>Shuffles a vector using the Fisher-Yates algorithm.<br>Can be used for an NFT hero creation with many objects/items/skills to randomly shuffle</p>                                                                       |
| sub\_randomness     | Generates more more random numbers out of a single round of drand round. This can be needed if for example you want to run many lotteries in parallel using the same randomness round without sacrificing the high entropy. |

