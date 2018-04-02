# ODEM Airdropper

Airdropper contract for token bounty distribution

## Why an airdropper?

As part of ODEM's bounty and rewards campaign, ODEM tokens need to be
transferred to thousands of users. Transferring the tokens one-by-one is
tedious if done by hand, and incurs very high cumulative fees.

Both of these problems can be mitigated by a contract which rolls a lot of
transfers into one transaction, such as this airdropper. This way, about a
hundred times fewer transactions are needed, which means less work to check
and send them, and lower total transaction fees.

## Technical details

This airdropper differs from others in that it never holds tokens. Instead it
calls ERC20's `transferFrom` to transfer tokens directly from a given source
address to the recipients. This has two distinct advantages:

- ODEM has already published the address which holds the tokens to be
  distributed as bounty. The recipients will see that their tokens came
  directly from this address, so they can verify that they received their
  rewards in good order. The address is recorded in the [ODEM ICO] repository.

- The contract can be tested on the main network without interfering with the
  allocation of bounty tokens, and without the need to deploy a new instance
  after testing.

This airdropper also has a method to self-destruct, which is called as soon as
tokens have been distributed to all recipients. This prevents it from being
used as an attack vector in any future zero-day exploits, and also takes some
load off the Ethereum network—it's important to be a good neighbour.

Parts of this implementation (`SafeMath` and `Ownable`) are taken from
[OpenZeppelin] v1.8.0, with modification to support Solidity v0.4.21.

[ODEM ICO]: https://github.com/odemio/ico/
[OpenZeppelin]: https://github.com/OpenZeppelin/zeppelin-solidity

