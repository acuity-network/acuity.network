+++
title = "Using HD Bitcoin wallets with Drupal Coin Tools"
description = 'Originally published on jonathanpatrick.me.'
date = 2015-01-07
draft = false
template = "blog/page.html"

[taxonomies]
authors = ["Jonathan Brown"]

[extra]
+++

Retrieved from the [Wayback Machine](https://web.archive.org/web/20181114005052/http://jonathanpatrick.me/blog/hd-wallets-drupal).

Each <a href="https://web.archive.org/web/20181114005052/https://www.drupal.org/project/cointools" target="_blank">Coin Tools</a> payment needs its own Bitcoin address. This is necessary so that it is clear whether or not the payment has been completed. It is also important for preserving anonymity.</p>
<p>In order to participate in the Bitcoin network, a <a href="https://web.archive.org/web/20181114005052/http://drupal.com/" target="_blank">Drupal</a> website must talk to a Bitcoin node. Currently Coin Tools utilises the reference implementation, <a href="https://web.archive.org/web/20181114005052/https://bitcoin.org/en/download" target="_blank">bitcoind</a>.</p>
<p>bitcoind has wallet functionality built in. In fact, it was originally released as a desktop wallet for Microsoft Windows. By default, bitcoind will pre-generate a pool of 100 pairs of addresses and corresponding private keys. This pool will be increased as necessary.</p>
<p>This presents a number of problems. If data-loss were to occur on the server, the private keys could be unrecoverable and therefore the funds stored on the addresses would be unspendable. If a hacker gains access to the server they could copy the keys and steal the funds. The private keys can be encrypted, but the password is exposed on the server when generating new keys and spending funds.</p>
<p>To solve these problems, key pairs could be pre-generated in a secure environment and then the public addresses uploaded to the server.</p>
<p>Logistically this is challenging. A much more robust solution to this problem is to use Hierarchical Deterministic Wallets as described in <a href="https://web.archive.org/web/20181114005052/https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki" target="_blank">BIP 32</a> (with draft extensions in BIPs <a href="https://web.archive.org/web/20181114005052/https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki" target="_blank">43</a>, <a href="https://web.archive.org/web/20181114005052/https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki" target="_blank">44</a> &amp; <a href="https://web.archive.org/web/20181114005052/https://github.com/bitcoin/bips/blob/master/bip-0045.mediawiki" target="_blank">45</a>).</p>
<p>HD wallets are composed of a tree of pairs of <b>extended</b> public (xpub) and <b>extended</b> private (xprv) keys derived from a single seed or mnemonic sentence. An xprv can generate its child xpubs and child xprvs. An xpub can only generate its child xpubs. Any <b>extended</b> key can be converted into it's <b>non-extended</b> variant that cannot generate children.</p>
<p>A <b>non-extended</b> public key can be converted into a payment address. A <b>non-extended</b> private key can be used to spend funds that are held on the payment address it is associated with.</p>
<p>An example <b>extended</b> public key is:</p>

```
xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8
```

<p>An example <b>extended</b> private key is:</p>

```
xprv9s21ZrQH143K3QTDL4LXw2F7HEK3wJUD2nW2nRk4stbPy6cq3jPPqjiChkVvvNKmPGJxWUtg6LnF5kejMRNNU3TGtRBeJgk33yuGBxrMPHi
```

<p>Extended key pairs are also considered to be either <b>hardened</b> or <b>non-hardened</b>. One of the properties of <b>extended</b> keys is that if an attacker knows a <b>non-hardened</b> private key and the parent xpub, they are able to determine the parent xprv.</p>
<p>In situations where private keys are to be distributed, for example within a company, <b>hardened</b> derivation must be used to prevent other private keys at the same level from being determined.</p>
<p>A further property of <b>extended</b> keys is that xpubs are not capable of generating <b>hardened</b> child public keys at all. This is fine because in an untrusted environment (with only a <b>non-hardened</b> xpub) no private keys will be present.</p>
<p>Payment addresses in an HD wallet can be considered to be either <b>internal</b> or <b>external</b>. <b>External</b> addresses are used when funds are being paid into an account from outside the wallet. <b>Internal</b> addresses are used as <a href="https://web.archive.org/web/20181114005052/https://bitcoin.stackexchange.com/questions/1629/why-does-bitcoin-send-the-change-to-a-different-address" target="_blank">change addresses</a>.</p>
<p>The default wallet layout is shown below:</p>
<p><img src="/blog/derivation.png" width="764" height="467" style="width: 100%; height: auto;"></p>
<p>HD wallets have many use-cases and BIP 32 identifies <a href="https://web.archive.org/web/20181114005052/https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#use-cases" target="_blank">several</a>.</p>
<p>"Unsecure money receiver" is the use-case to solve the problem described in this blog post.</p>
<p>The idea is to maintain an HD wallet in a secure environment. An account would be created in this wallet for the purpose of receiving payments in a specific Coin Tools payment type. The xpub for <b>external</b> addresses from this account would then be exported and added to the configuration of the payment type within Drupal.</p>
<p>Despite the obvious complexity of HD wallets, the concept of creating an account for a specific person, organisation or reason and exporting the xpub is actually very simple. The key point is that only one authority should be making payments into an specific xpub, otherwise addresses would be used multiple times. Scanning for unused addresses would not be an effective strategy to prevent this. <a href="https://web.archive.org/web/20181114005052/https://bitcoin.stackexchange.com/questions/20701/what-is-a-stealth-address" target="_blank">Stealth addresses</a> could become a solution for allocation of payment addresses without an authority.</p>
<p>Coin Tools can "interrogate" the provided xpub. The results of this process will be displayed, including the first four addresses that can be generated from the xpub and the <b>relative</b> path of the next address Coin Tools will generate:</p>
<p><img src="/blog/xpub-ui.png" width="1155" height="1187" style="width: 100%; height: auto;"></p>
<p>Every key pair in the wallet has a path specifying the indexes at each level in the hierarchy, for example M/44'/0'/0'/0/3. <b>Absolute</b> paths have either an m or M as their first component. <b>Relative</b> paths have an index as their first component. In the example above we can see that the xpub has a depth of 3, so <b>relative</b> paths start with describing the index at depth 4.</p>
<p>A <b>'</b> or <b><sub>H</sub></b> character after an index in the path indicates that the index is actually i+2<sup>31</sup>. This means that keys at this level have <b>hardened</b> derivation.</p>
<p>According to <a href="https://web.archive.org/web/20181114005052/https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki" target="_blank">BIP 43</a> (draft), the index at level 1 should be the <b>hardened</b> index of the BIP that describes the layout of the hierarchy beneath it. In the example above it is 44, meaning that it is using the layout from <a href="https://web.archive.org/web/20181114005052/https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki" target="_blank">BIP 44</a> instead of the default one from <a href="https://web.archive.org/web/20181114005052/https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki" target="_blank">BIP 32</a>.</p>
<p>Despite the fact that many wallets are now HD, support for exporting account xpubs is currently quite low. However, some HD wallets that do not allow xpubs to be exported for regular accounts will allow xpubs to be exported from <a href="https://web.archive.org/web/20181114005052/http://bitcoinmagazine.com/11108/multisig-future-bitcoin/" target="_blank">multisig</a> accounts, for example <a href="https://web.archive.org/web/20181114005052/https://coinkite.com/fr/fce4276q" target="_blank">Coinkite</a>. In the future Coin Tools will support generation of addresses from multisig xpubs.</p>
<p>The only wallets that I know of that will export an xpub from a non-multisig account are <a href="https://web.archive.org/web/20181114005052/https://play.google.com/store/apps/details?id=com.bonsai.wallet32" target="_blank">Wallet32</a> and <a href="https://web.archive.org/web/20181114005052/https://electrum.org/" target="_blank">Electrum</a>.</p>
<p>Both these wallets export xpubs that allow derivation of both the <b>external</b> (0/i) and <b>internal</b> (1/i) addresses. This is useful for watching an account balance but means that an entity making the payments into the account has greater ability to spy on subsequent transactions than would otherwise be possible. Coin Tools is currently hard coded to use the relative path 0/i. This will need to be made configurable as Coinkite xpubs do not need any prefix on the index.</p>
<p>It is essential that the addresses generated by Coin Tools match those generated by the wallet otherwise the account will not receive the payments. The xpub in the previous screenshot was exported from a Wallet32 account. In the following screenshot we can see that the addresses displayed in Wallet32 are the same as those generated by Coin Tools:</p>
<p><img src="/blog/wallet32.png" width="270" height="244"></p>
<p>In theory when an xpub is imported the addresses should be scanned to make sure the xpub has not been used before. However, bitcoind does not maintain the correct indexes to be able to quickly list transactions for arbitrary addresses. When Coin Tools needs to receive a payment on an address from an xpub, it adds it as a watch-only address using the "importaddress" bitcoind command. The "rescan" parameter is set to false which means that transactions that happened before the address was added are not detectable. If this parameter is set to true it can take many minutes even on an SSD to import each address.</p>
<p>Coin Tools uses the Drupal <a href="https://web.archive.org/web/20181114005052/https://www.drupal.org/node/1787278" target="_blank">State API</a> to maintain the next index for each xpub. If there are more unreceived payments in a row than the <a href="https://web.archive.org/web/20181114005052/https://electrum.org/faq.html#gap-limit" target="_blank">gap limit</a> of the wallet software, the wallet will loose track of later payments. To avoid this happening, payments that expire should maybe have their addresses put in a pool for re-use. However this may cause a problem if someone records the payment address and then satisfies the payment at a later time after it has expired.</p>
<p>Of course, if a hacker gained access to the web server they could change an xpub to their own. This would mean that until the problem was detected and the service shut down the hacker would be receiving the funds instead of the intended recipient. While damaging, this would be nowhere near as bad as the total loss of a hot wallet.</p>
<p>In order to facilitate handling of HD wallets, Coin Tools was converted to use the <a href="https://web.archive.org/web/20181114005052/https://github.com/Bit-Wasp/bitcoin-lib-php" target="_blank">BitWasp PHP Bitcoin library</a> instead of <a href="https://web.archive.org/web/20181114005052/https://github.com/mikegogulski/bitcoin-php" target="_blank">Gogulski</a>.</p>
