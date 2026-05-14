# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Added
- Forked from [wavesplatform/WavesJ](https://github.com/wavesplatform/WavesJ) v1.6.4
- Rebranded to **DecentralChain Java SDK** (`io.decentralchain:java-sdk`)
- JaCoCo code coverage enforcement (70% line/branch minimum)
- SpotBugs static analysis (High threshold, Max effort)
- Checkstyle style enforcement (Google Java Style)
- GitHub Actions CI matrix: Java 11, 17, 21
- GitHub Actions publish workflow with dry-run option
- Maven wrapper (`mvnw`) for reproducible builds
- `.editorconfig` for consistent editor settings

### Changed
- `groupId`: `com.wavesplatform` → `io.decentralchain`
- `artifactId`: `wavesj` → `java-sdk`
- `central-publishing-maven-plugin`: 0.9.0 → 0.10.0
- `testcontainers`: 2.0.3 → 2.0.5
- GPG loopback pinentry mode declared in pom.xml
- SCM block updated to Decentral-America/java-sdk

### Removed
- Dead dependency `org.testcontainers:junit-jupiter:1.21.4` (unused — no @Testcontainers annotations in test sources)

---

## 1.0.0
- significantly redesigned interface
- based on [Waves Crypto](https://github.com/wavesplatform/waves-crypto-java) and [Waves transactions](https://github.com/wavesplatform/waves-transactions-java) libraries
- supported most of Waves Node API
- feature #15 of Waves Node 1.2 Malibu release is now supported

## 0.17.0
- new InvokeScriptTransactionStCh with stateChanges attribute was added. It was design to provide an additional information about Invocation transaction and couldn't be used to post invokes into blockchain. That why constructor was marked as package-private
- new AllTxIterator class to navigate over all account transactions. It has a generic semantic and can be used for other endpoints and transaction.
- new methods to retrieve information about state changes were added into Node:
  - Node#getStateChanges
  - Node#getAddressStateChanges
  - Node#getAllAddressStateChanges
- applicationStatus supported to see if any transaction has failed in blockchain


## 0.16.0
- Support Order version 3
- Support blockchain rewards information


## 0.14.1

- Support for InvokeScript transaction
- Support for Exchange transaction version 2
- Separated getBodyBytes() (tx bytes without signature) and getBytes() (whole tx bytes) methods.
- Crypto hash methods are public now
- Bug fixing


## 0.13.2

- Asset distribution method was added
- Burn chain id serialization was fixed

## 0.13.1

- Address transaction method was added

## 0.13

- Batch cancel method was added
- Transaction ids calculation was fixed

## 0.10
- All existed transactions was realized as objects
- Block now contains parsed transactions objects

## 0.9
- Added network timeouts to Node so that requests do not hang
- Support for aliases in transfers and leases

## 0.8
- Support for transactions version 2 (compatible with Waves 0.13)
- `Transaction.setProof()` was renamed `withProof()` to better reflect the fact that it doesn't modify the object but rather returns new one
- `char PublicKeyAccount.scheme` was replaced with `byte chainId`
- Introduced `Transaction.getBytes()`

## 0.7
- Support for account scripts
- Added `getBlock()` and `getTransaction()` to `Node`
- Transaction factory methods got overrides that accept `timestamp` parameter
- String entries in Data transactions

## 0.6

Reworked signing and proofs:
- Removed `signers` parameter from factory methods
- Moved `sign` method from `Transaction` to `PrivateKeyAccount`
- In `Transaction`, `addProof(String)` becomes `setProof(int, String)` so that it's possible to create non-contiguous lists of proofs, e.g. define proofs 1 and 3 but leave proof 2 out.

This is a non-compatible change, but hopefully not many people have started using features from version 0.5 given that they are three days old now.

## 0.5

Added multisig support in Transaction. Factory methods now accept PublicKeyAccount for `sender` rather than PrivateKeyAccount, and a separate `signers` array. This is a source-compatible change, however, existing code may need to be recompiled.

Also replaced `signature` with `proofs`, and added methods to add proofs to a transaction.
