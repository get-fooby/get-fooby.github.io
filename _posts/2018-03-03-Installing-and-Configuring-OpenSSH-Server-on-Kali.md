---
title: Installing and Configuring OpenSSH on Kali
published: false
---

# Installing OpenSSH
Installing OpenSSH is very easy with `apt` on Kali.

`apt install openssh-server`

Once installed, configure the `ssh` server to run at bootup

`systemctl enable ssh`

# Configuring OpenSSH
Root does not have access to use SSH by default, so if you don't create additional users you will need to allow `root` to logon. 

To do this edit the config with `nano /etc/ssh/sshd_config` and change the `#PermitRootLogin prohibit-password` to remove the # comment.

Before we take this, to make life easier in a momemnt, we will remove the comment and set it to `yes`.

This will allow us to copy the keys to the server instead of editing the `authorized_keys` file manually.

```
nano /etc/ssh/sshd_config
Edit from:
#PermitRootLogin prohibit-password
Change to:
PermitRootLogin yes
```
# Creating SSH Keys

Instead of typing the password over the ssh session, OpenSSH supports public key cryptography with the use of a Private/Public key pair.

## Generate the keys
```
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/demo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/demo/.ssh/id_rsa.
Your public key has been saved in /home/demo/.ssh/id_rsa.pub.
The key fingerprint is:
4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a
The key's randomart image is:
+--[ RSA 2048]----+
|          .oo.   |
|         .  o.E  |
|        + .  o   |
|     . = = .     |
|      = S = .    |
|     o + = +     |
|      . o + o .  |
|           . o   |
|                 |
+-----------------+
```
The public key is now located in `/home/demo/.ssh/id_rsa.pub`. The private key (identification) is now located in `/home/demo/.ssh/id_rsa`. 

## Copy the keys to the server
Once the key pair is generated, we need to upload this to the server.

### Uploading the keys from Linux to Linux
For Linux there are a few options, the easiest is the `ssh-copy-id` command. 

    `ssh-copy-id demo@198.51.100.0`

This will automatically connect and update the `authorized_keys` file.

Alternatively you can use pipe a few commands together like this

`cat ~/.ssh/id_rsa.pub | ssh demo@198.51.100.0 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys"`

### Uploading the keys from Windows to Linux
For Windows you will need to ssh to the server and edit the `authorized_keys` file in `nano` or `vi` then append the contents of the public key file `id_rsa.pub` to the end.

Restart the OpenSSH server with `systemctl restart ssh` to reload the config.

# Securing OpenSSH
Once logging on with SSH keys only has been tested, edit the `sshd_config` file and set `PermitRootLogin` to `without-password`

```
nano /etc/ssh/sshd_config
Edit from:
PermitRootLogin yes
Change to:
PermitRootLogin without-password
```

`systemctl restart ssh`
