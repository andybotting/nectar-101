---
title: Using SSH
permalink: /using-ssh
icon: terminal
order: 04
---

To access the console of your virtual machine instance, you will need an SSH client. Linux and Mac OS systems should have an SSH client already installed, accessible from your terminal. Windows systems might need to download an SSH client, such as PuTTY, if one is not already installed.

### Linux and Mac OS
Initate an SSH connection to your Master Node, using the ubuntu user and password provided for your lab environment.

```
ssh ubuntu@<your-instance-ip-address>
```

```console
The authenticity of host '23.253.111.163 (23.253.111.163)' can't be established.
ECDSA key fingerprint is SHA256:V9uiWfD7AiO3OzmYqbbu2g/pmrc9FLzgxGOhInZjTYg.
Are you sure you want to continue connecting (yes/no)?
```

Type `yes`, and press `enter` to continue.

```
yes
```

```console
Warning: Permanently added '130.56.248.49' (ECDSA) to the list of known hosts.
--------------------------------------------------------------------------------
Nectar Ubuntu 18.04 LTS (Bionic Beaver)

Image details and information is available at:
  https://support.ehelp.edu.au/support/solutions/articles/6000106269
--------------------------------------------------------------------------------

[...]

ubuntu@my-instance:~ $
```

### Windows 

#### OpenSSH for Windows

Windows 10 now includes OpenSSH, which is installed by default since the April 2018 update. You can find more information from this [Microsoft Blog Post](https://blogs.msdn.microsoft.com/powershell/2017/12/15/using-the-openssh-beta-in-windows-10-fall-creators-update-and-windows-server-1709) about how to install and use it.

#### PuTTY

If you don't have it installed already, [download](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) and install the PuTTY SSH client for Windows.

Windows systems using PuTTY, enter your instance IP Address. Ensure the Connection Type is set to SSH, and the Port is set to 22, then click `Open`. Follow the prompts to accept the connection. Login using the ubuntu user.
