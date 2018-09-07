---
title: Key pair
permalink: /keypair
icon: key
order: 01
---

A key pair is cryptographically signed public and private key set, which is used as an alternative to a password. To access a virtual machine instand created on the Nectar Research Cloud, you will need to use Secure Shell (SSH) with a key pair.

If you have an existing key pair you'd like to use, you can import the public part of this key pair into your Nectar Research Cloud account, otherwise we can generate a new one though the dashboard.

## Generating a new Key Pair

To generate a new key pair, go to [Project > Compute > Key Pairs](https://dashboard.rc.nectar.org.au/project/key_pairs). You can click the `Create Key Pair` button to get started.

Enter a memorable name, and click the `Create Key Pair` button. Your *public* key will be saved within your Research Cloud account, and you will be prompted to save your *private* key.

Save this somewhere safe.

## Using a Key Pair

We will be using this new key pair to gain access the `console` of a *virtual machine instance* we create in the next step, using Secure Shell (SSH). 
