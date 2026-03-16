+++
title = "Using Yubikey 5 for SSH Key Storage and Authentication"
date = "2026-03-15T21:55:33-04:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Braden"
cover = ""
tags = ["yubikey", "ssh"]
keywords = ["yubikey", "fido", "ssh"]
description = "How to use a Yubikey Series 5 key to create and store an SSH private key for authentication."
showFullContent = false
readingTime = false
hideComments = false
+++

I spent a fair amount of time reading articles by both Yubikey and others that attempted to explain this process but I always hit a roadblock. I finally found an excellent article by Luiz Costa[^1] that worked perfectly.

I am referencing Luiz's article here as both a reminder for myself and to drive further traffic to it as it does an excellent job explaining this process, especially when using GNOME as it details how to get past GNOME keyring getting in the way.

You should definitely check out the full article linked below[^1], but here is a TLDR of the commands for quick reference.

# Key Generation and Usage Commands
The following assumes you have already used Yubikey Authenticator[^2] or Yubikey Manager[^3] (deprecated) to setup a FIDO2 PIN.

```bash
ssh-keygen -t ed25519-sk -O resident -O verify-required -O application=ssh:resourcenamehere -C "key-comment-here"
# Copy public key generated above to server you wish to authenticate to
SSH_AUTH_SOCK="" ssh -i .ssh/id_ed25519 -o IdentitiesOnly=yes user@example.com
# Import resident key file on a new computer (insert yubikey and run the below command)
ssh-keygen -K
```

###### References
[^1]:https://thenets.org/using-a-yubikey-5c-for-ssh-and-git-authentication-on-linux/
[^2]:https://www.yubico.com/products/yubico-authenticator/
[^3]:https://www.yubico.com/support/download/yubikey-manager/
