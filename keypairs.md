---
title: Key pairs
permalink: /keypairs
icon: key
order: 01
---

A key pair is a cryptographically signed public and private key set, which can be used as an alternative to a password. To access a virtual machine instand created on the Nectar Research Cloud, you will need to use Secure Shell (SSH) with a key pair.

If you have an existing key pair you'd like to use, you can import the public component of this key pair, otherwise a new one can be generated directly into your Nectar Research Cloud account.

## Generating a new Key Pair

To generate a new key pair, go to [Project > Compute > Key Pairs](https://dashboard.rc.nectar.org.au/project/key_pairs). You can click the `Create Key Pair` button to start the wizard.

Enter a memorable name for your new key pair, then click the `Create Key Pair` button. Your *public* key will be saved within your Research Cloud account, and you will be prompted to save your *private* key to your computer.

Save this somewhere safe.

## Using your Key Pair

We will be using this new key pair to gain access the `console` of a *virtual machine instance* in the [Using SSH]({{ "/using-ssh" | prepend: site.baseurl }}) step, but we'll need to boot an `Instance` first.
