+++
title = "GoPass, Yubikey, and GPG"
date = 2020-01-02
description = "Streamlining GoPass with a YubiKey smartcard for GPG decryption."
[taxonomies]
tags = ["gpg", "yubikey", "gopass"]
+++

`TL:DR - Yubikey is an awesome little tool that streamlines GoPass on-top of all the other great things it does.`

I use GoPass to store my passwords. It's a great tool that I would recommend everyone use if you are familiar with cli. Side note, if you are not using any password manager you should stop reading and go set one up, it's 2019 there are a plethora of good ones to choose from (pass/gopass, lastpass[paid], and KeePassX) alongside many more.

The one downside I always had with GoPass was that I would have to always type my GPG password to unencrypte my passwords every time I needed to use my password. This took time and depending on my coffee consumption several attempts to get it right. To solve this I decided to set my YubiKey up to hold my GPG sub keys and use it as a smartcard to then decrypt my passwords.

The best guide that I have found on setting up my Yubkey with GPG is drduh's github guide at [YubiKey-Guide](https://github.com/drduh/YubiKey-Guide). It is clean, concise and easy to follow.

Once you have your GPG keys and subkeys created and then stored on your Yubkey it is as simple as running GoPass and typing in your Yubikey password.
