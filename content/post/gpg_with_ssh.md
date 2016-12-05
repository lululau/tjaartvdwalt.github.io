+++
date = "2015-06-03T18:26:24"
image = ""
math = true
tags = ["linux"]
title = "Using gpg-agent to cache your ssh passphrase"

+++

> **UPDATE 2015-08-26**
>
> This method described here is tricky to get working, and I think it could cause problems in future when I have forgotten the details. I have reverted to the standard config of using `gpg-agent` for gpg-keys, and `ssh-agent` for ssh-keys. In addition I now use [keychain](http://www.funtoo.org/Keychain) to cache the passwords for both of the agents. This way I have my encrypted private keys, I don't have any non-standard configuration, and I only have to type in my passwords once for each session.

Recently I was thinking about a backup strategy for `$HOME`. A big problem for me is how to backup my private keys safely. I found some inspiration after reading [this](https://help.github.com/articles/working-with-ssh-key-passphrases/) GitHub article. I would encrypt my private key with a passphrase, and could then back it up with more confidence. 

A problem for me is that I have grown accustomed to using my unencrypted ssh key to authenticate to Git servers. I did not want to have to type my passphrase with each access. Luckily, as stated in the GitHub article you can use `ssh-agent` to securely cache your passphrase. However since I was already using GnuPG for encrypting my website passwords I wondered if I could use that to cache my passwords instead of ssh-agent. I found a Arch Wiki [article](https://wiki.archlinux.org/index.php/SSH_keys#GnuPG_Agent) that explains how this can be done. I could not follow all the steps exactly. Here is a brief explanation of how I configured `gpg-agent` to cache my ssh passphrase.

# Configure gpg-agent to support ssh

You will need to edit/create the configuration file `~/.gnupg/gpg-agent.conf` to *enable-ssh-support*. I also increased the default *time to live* for my cached passwords to 12 hours.

    # ssh support
    enable-ssh-support
    
    # Cache settings. (Caches for 12 hours)
    default-cache-ttl 43200
    default-cache-ttl-ssh 43200

# Start gpg-agent

Your gpg-agent needs to be started if you want to use it to cache your ssh passphrase. Since I am using *systemd* I created a user service. The file is located at `~/.config/systemd/user/gpg-agent.service`

    [Unit]
    Description=GnuPG private key agent
    IgnoreOnIsolate=true
    
    [Service]
    Type=forking
    ExecStart=/usr/bin/gpg-agent --daemon
    ExecStop=/usr/bin/pkill gpg-agent
    Restart=on-abort

# export SSH

You also need to export the `SSH_AUTH_SOCK` environment variable with the value of the *gpg-agent* socket. For version 2 of GnuPG it looks like this:

    export SSH_AUTH_SOCK=~/.gnupg/S.gpg-agent.ssh

Since I am using zsh I added this line to my `~/.zshrc` file.

# Run ssh-add

Once the configuration is done, you need to run `ssh-add` once to cache the passphrase. Once you have entered the `ssh passphrase` the gpg-agent will popup prompting you for a `gpg passphrase` that will encrypt your `ssh passphrase`.

This seems a bit complicated, but in future you will be prompted for your `gpg passphrase` when you try to access your ssh key. Your `ssh passphrase` will get cached for `default-cache-ttl-ssh` seconds, and you will not get prompted for a passphrase again until the ttl expires.
