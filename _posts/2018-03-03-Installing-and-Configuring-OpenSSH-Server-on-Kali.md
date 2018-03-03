---
title: Installing and Configuring OpenSSH on Kali
published: true
---

# Installing OpenSSH
Installing OpenSSH is very easy with `apt` on Kali.

`apt install openssh-server`

Once installed, configure the `ssh` server to run at bootup

`systemctl enable ssh`

# Configuring OpenSSH
Root does not have access to use SSH by default, so if you don't create additional users you will need to allow `root` to logon. 

To do this edit the config with `nano /etc/ssh/sshd_config` and change the `#PermitRootLogin prohibit-password` to remove the # comment.

In addition to removing the comment, we will edit the line to allow us to logon with ssh. We will secure this later.

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

Recently malware has been noted to collect private keys on behalf of the malicous actor and retain these for use at any point. Adding a passphrase is incredibly important. We can address the inconvenience of a passphrase for Windows clients later on.

### Generating keys on Windows with PuTTY Key Generator
1. Open PuTTY Key Generator (`puttygen.exe`)
2. The defaults are acceptable, RSA/2048, click Generate
3. Randomly move the mouse cursor over the window to generate randomness
4. Specify a passphrase
5. Click `Save private key` and `Save public key`

### Generating keys on Linux
```
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/demo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): **********
Enter same passphrase again: **********
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
Finally restart the SSH service to reload the configuration

`systemctl restart ssh`

# Using the SSH keys

## Windows (PuTTY)

Using PuTTY there are two main options, the more secure and the less secure options. The security is actually dependant on the addition of a passphrase, however typing a passphrase is about as inconvenient as typing a password. The PuTTY Agent can handle providing authentication to PuTTY as needed without having to retype the password each time its needed.

### PuTTY Agent (More Secure)
1. Open PuTTY Agent
    PuTTY Agent opens in the Notification Area on Windows (near the clock)
2. Right click and choose Add Key
3. Choose Add Key
4. Locate the Key file and choose Open
5. Enter the Passphrase
    This is the only time you will need to type the passphrase, even if you connect to multiple servers (assuming they all have your public key installed)

#### Bonus PuTTY Agent Tip
In order to load keys into PuTTY Agent at Windows bootup, you can copy the PuTTY Agent shortcut to `C:\programdata\Microsoft\Windows\Start Menu\Programs\StartUp` and append the path to the key files after the executable name.

`C:\Program Files\PuTTY\pageant.exe" C:\key1.ppk C:\key2.ppk`

### PuTTY Config (Less Secure)
1. Open PuTTY and add the IP Address / name of the server
2. Under Connection/Data specify the username
3. Under Connection/SSH/Auth Specify the location of the private key file
4. Under Session specify a name in the Saved Sessions section, and Click Save

## Linux
If the keys are successfully installed to the remote server you can simply logon with

`ssh username@remote_host`

If you did not supply a passphrase for your private key, you will be logged in immediately. If you supplied a passphrase for the private key when you created the key, you will be required to enter it now. Afterwards, a new shell session should be spawned for you with the account on the remote system.

