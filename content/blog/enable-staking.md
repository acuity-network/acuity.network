+++
title = "Let's enable staking on the new Acuity Polkadot blockchain"
description = 'The Acuity blockchain uses standard Polkadot consensus and governance.'
date = 2020-09-25
draft = false
template = "blog/page.html"

[taxonomies]
authors = ["Jonathan Brown"]

[extra]
+++

<p>The Acuity blockchain uses standard Polkadot <a target="_blank" href="https://wiki.polkadot.network/docs/en/learn-consensus">consensus</a> and <a target="_blank" href="https://wiki.polkadot.network/docs/en/learn-governance">governance</a>.</p>

<p>Just like Polkadot and Kusama, Acuity launched in proof-of-authority mode. We now need to switch to proof-of-stake. Polkadot and Kusama both launched with the Sudo pallet (god-mode) enabled and used this to call <a target="_blank" href="https://polkadot.js.org/api/substrate/extrinsics.html#forcenewera">staking.forceNewEra</a> to initiate this transition.</p>

<p>However, as Acuity does not have the Sudo pallet enabled, it is necessary to make the staking.forceNewEra call via the proper governance mechanism. To this end I have created an Acuity Council <a target="_blank" href="https://polkadot.acuity.social/#/council/motions">motion</a>. This needs to be voted on by council members so that it can go to a general referendum. Please join the council and vote for this motion.</p>

<p>There is currently another referendum in progress. Under normal circumstances only one referendum can be running at a time and each referendum takes 1 week. This means that the referendum to start staking cannot finish until 9th October. Then there is an enactment delay of 8 days. So staking will probably begin on 17th October. This will give us time for many validators to get ready.</p>

<p>Once PoS has begun, any ACU that the current PoA accounts have accrued will be donated to the treasury.</p>

<p>If you need any assistance with this process, please drop into the Staking channel on <a target="_blank" href="https://discordapp.com/invite/GxD7adN">Discord</a>.</p>

<p>This is what I am currently considering including in the subsequent council proposal:
  <ul>
    <li>Enable fast-track legislation: Polkadot governance has a fast-track mechanism that allows a proposal to be brought to referendum immediately, with a far shorter voting period than normal (3 days) and a near zero enactment period. This would enable me to propose system upgrades that could be enacted much more quickly.</li>
    <li>Vest the project revenue: on MIX the project revenue was gradually <a target="_blank" href="https://docs.mix-blockchain.org/en/latest/issuance.html">released</a> at a ever-decreasing rate, ending October 18th 2022. The unreleased revenue was brought across to the new ACU blockchain, but it needs to be vested to match the original schedule.</li>
    <li>Enable <router-link to="/trade">ACU</router-link> pallet: expose ACU to DeFi on Ethereum.</li>
  </ul>
</p>
