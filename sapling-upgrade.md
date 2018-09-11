# Sapling Upgrade

This document serves to outline an upgrade path for Glyff to the ZCash
Sapling protocol. Sapling changes the way private transactions work in ZCash,
and brings with it a number of new features, increased speed, and a new set
of cryptographic dependencies. In Glyff, we currently have a rough and
incomplete implementation of ZCash's Sprout cryptography over Ethereum.
Although ZCash has described developing a ledger-agnostic "ZCash Security
Layer" (ZSL), there is no formal specification for ZSL, and the only
implementation is for Sprout cryptography, and is incomplete in the sense
that it suffers from transaction malleability (JPMorgan's Quorum, built on
Ethereum).

It is then our goal to work out the specifics of how Sapling, which was
written for ZCash, a fork of Bitcoin, can be applied to Glyff, a fork of
Ethereum. Differences such as the account model in Bitcoin and Ethereum, will
preclude an unmodified application of the ideas in Sapling to Glyff.

## New Cryptographic Primitives

Here we describe new cryptographic primitives we'll need to use, where and
how they're used in Sapling, and where we can find how quality
implementations (or if we should write our own). It goes without saying that
we could use the ZCash implementation of any of these primitives via the
Golang ffi interface. We should expect their implementations to be fast,
high-quality, and heavily reviewed. However, when there are similarly fast
and high-quality alternatives, especially when written in safe languages
unlike C++, we should consider using them. Additionally, ZCash also uses
third-party libraries, and it would make more sense to directly hook into
those libraries than effectively use them through ZCash.

### JubJub

JubJub is an elliptic curve designed to be efficiently implementable in
zk-SNARK circuits.

JubJub is used in the windowed and homomorphic Pedersen commitments used in the Sapling note and value commitments, respectively.

Promising implementations include:
* https://github.com/barryWhiteHat/baby_jubjub_ecc is a standalone implementation written in C++.
* https://github.com/zkcrypto/jubjub is a standalone implementation written in Rust by Sean Bowe (@ebfull), a ZCash employee.