# HD Wallet

A basic toolkit for Cryptocurrency wallet in Dart. This package is used to generate and manage the Hierarchical Deterministic (HD) Wallet application. Forked from https://github.com/CrystalNetwork/hd_wallet

## Features

You can easily use this package to develop crypto wallets through the Dart language.
The goal of this fork is to support a growing list of coin types. Please submit a pull request if you would like to contribute.

## Getting started

### Use this package as a library

Depend on it

Run this command:

With Dart:

```sh
dart pub add flutter_hd_wallet
```

With Flutter:

```sh
flutter pub add flutter_hd_wallet
```

This will add a line like this to your package's pubspec.yaml (and run an implicit `dart pub get`):

```sh
dependencies:
  flutter_hd_wallet: ^latest
```

Alternatively, your editor might support dart pub get or flutter pub get. Check the docs for your editor to learn more.

### Import it

Now in your Dart code, you can use:

```dart
import 'package:flutter_hd_wallet/flutter_hd_wallet.dart';
```

## Usage

Short and useful examples for package users. Longer examples
is in `/example` folder.

### Bip39

Generate mnemonice:

```dart
// default mnemonic is 12 english words,
final m = BIP39();
debugPrint("words: ${m.mnemonic}");
debugPrint("seed: ${m.seed}");
```

### Bip32

Forked from [bip32-dart](https://github.com/dart-bitcoin/bip32-dart) and improve some flaws.

```dart
BIP32 node = BIP32.fromBase58(
        'xprv9s21ZrQH143K3QTDL4LXw2F7HEK3wJUD2nW2nRk4stbPy6cq3jPPqjiChkVvvNKmPGJxWUtg6LnF5kejMRNNU3TGtRBeJgk33yuGBxrMPHi');
final nodePrvk = HEX.encode(node.privateKey as List<int>);
debugPrint("nodePrvk: $nodePrvk");
// => e8f32e723decf4051aefac8e2c93c9c5b214313817cdb01a1494b917c8436b35

BIP32 nodeNeutered = node.neutered();
debugPrint("isNeutered: ${nodeNeutered.isNeutered()}");
// => true

debugPrint(HEX.encode(nodeNeutered.publicKey));
// => 0339a36013301597daef41fbe593a02cc513d0b55527ec2df1050e2e8ff49c85c2

debugPrint(nodeNeutered.toBase58());
// => xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8

BIP32 child = node.derivePath('m/0/0');
debugPrint(child.toBase58());
// => xprv9ww7sMFLzJMzur2oEQDB642fbsMS4q6JRraMVTrM9bTWBq7NDS8ZpmsKVB4YF3mZecqax1fjnsPF19xnsJNfRp4RSyexacULXMKowSACTRc

debugPrint(HEX.encode(child.privateKey as List<int>));
// => f26cf12f89ab91aeeb8d7324a22e8ba080829db15c9245414b073a8c342322aa

BIP32 childNeutered = child.neutered();
debugPrint("childNeutered isNeutered: ${childNeutered.isNeutered()}");
// => true

debugPrint(HEX.encode(childNeutered.publicKey));
// => 02756de182c5dd4b717ea87e693006da62dbb3cddaa4a5cad2ed1f5bbab755f0f5

BIP32 nodeFromSeed = BIP32.fromSeed(
    Uint8List.fromList(HEX.decode("000102030405060708090a0b0c0d0e0f")));
debugPrint(nodeFromSeed.toBase58());
// => xprv9s21ZrQH143K3QTDL4LXw2F7HEK3wJUD2nW2nRk4stbPy6cq3jPPqjiChkVvvNKmPGJxWUtg6LnF5kejMRNNU3TGtRBeJgk33yuGBxrMPHi

BIP32 nodeFromPub = BIP32.fromBase58(
    "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8");
debugPrint(nodeFromPub.toBase58());
// => xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8

```

### Bip44

```dart
const coin = 60;
const account = 0;
const change = 0;
const index = 1024;
final node = BIP44.fromSeed(
    "87fbb2c47e6cb3a8c5dc0eb9c6b86ac6d78c19a3a62ee6e3b59cb92fda2c9a58df77a691f450645f78ec98584edf9c7b4c25f2e414fc9f39e97ba6788f5311a0",
    coinType: coin);

debugPrint(node.privateKeyHex());
// => 126b43a322b80d0f18bbdef0fd455fe680d78b7c038b243a5b5d40e331b032d7

debugPrint(node.privateKeyHex());
// => 02b6f32dde6031f55787fbf9d861c233526b53c56e30c854ebd61fc6ed681fc91d

debugPrint(node.privateKeyHex(hardened: true));
// => 07590f06719e5b561b2323568a668ac21c4ca327a28960534ef0b392148fa180

debugPrint(node.publicKeyHex(hardened: true));
// => 03e6dd25138127138140a0af34d214c9f006e2d6d475eed3fac0c2659ff2c1eacf

debugPrint(node.privateKeyHex(account: account, change: change, index: index));
// => f033d0f338838555d2842a33e21cbf59a49a3537cd494bff7ada534cbc118243

debugPrint(node.publicKeyHex(account: account, change: change, index: index));
// => 02d3464e4d0504a6eed97299e85975fdf5be29cb1d1c49f6d217113bc7d77181b2

debugPrint(node.privateKeyHex(
          account: account, change: change, index: index, hardened: true));
// => 155105f81999676268d15ba007aff2f8f391d7f21df2c9a50b39fd58f62de546

debugPrint(node.publicKeyHex(
          account: account, change: change, index: index, hardened: true));
// => 03e6859749a3b614748046b820fb3f3496d1116734e3e2acb67bf25f6a91fb6cc9

debugPrint(node.address(index: 1));
// => 0x2071fC489F9998d8C6674176655F18D73AFa5Aaa

debugPrint(node.address(index: 2, hardened: true));
// => 0x487d87C2aCC6272Aad9CdF2cCb98cC06a4e989ae
```


## Additional information

If you have any problems or bugs in use, please contact us.
