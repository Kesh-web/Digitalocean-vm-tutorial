# Digitalocean-vm-tutorial
This is an Arch Linux Setup on DigitalOcean using cloud-init and doctl.

## Overview ##

**What is Digital Ocean?**\
Digital ocean is a cloud infrastructure provider that offers cloud computing services. It allows you to manage virtual private servers that they call "Droplets".

The purpose of a Droplet is to be able to operate indepedantly and it runs its own operating system. You would be able to install manage applications, databases, websites, and other services. 

Some examples of using a Droplet is to host websites, host services such as (email, file storage), and running databases.

With that in mind the importance of setting up a secure VPS (virtual private server) is extremely crutial. Imagine setting up a file storage with your sensitive information. Without proper security your data could be exposed. Another example is preventing unauthorized access by setting up SSH Keys (will be explained in section 1).

**What is cloud-init?**\
It is an open source tool used to automate configuration of cloud instances when they are first launched. It is supported across all major public cloud providers, provisioning systems for private cloud infrastructure, and bare-metal installations.

**What is doctl?** \
Doctl is a command line interface for DigitalOcean. It allows you to create, configure, destroy DigitalOcean Droplets, firewalls, database clusters, etc.


## Creating SSH Key Pairs
To start we will be creating SSH keys which allows you to connect to remote servers over an encrypted connection.

- You should be in your home directory which is ~ to start.
- Once youre in your home directory **type** 

``` mkdir .ssh ``` 

This will create the folder that store the SSH keys in. 

- Once completed you can **type** the following code in your terminal.

```ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"```

To explain the previous command 
1. **ssh-keygen** is the base of the command to create the public and private keys.
2. **-t** specifies the type of key (Ed25519 is an elliptic curve signing algorithm using EdDSA and Curve25519.)
3. **-f** specifies the file path that the key will be generated in.
4. **do-key** is the name we will be giving the generated key. Its important to manage names properly if you have more than one SSH key.
4. **-C** attaches a comment to the key(in our case putting an email to remember.)

#### Copying SSH Keys to clipboard
To copy the SSH Public Key to your clipboard, go to your terminal and **type**. Note that this is how I do it on my system (Arch Linux), but you should check how to copy a file to a clipboard on your system.

```wl-copy < ~/.ssh/do-key.pub``` 

This will add the Public Key to your clipboard which you can then add to your Digital Ocean account in the cloud-init section
## Installing ```doctl```
- You should be in your home directory which is ~ to start.
```sudo pacman -S doctl```
#### Creating an API token
After installing doctl we need to create an API token to grant acount access to doctl. On Digital Oceans homepage look for API on the left side task bar.

![alt text](image.png)




## Setting up cloud-init for the First Configuration
Now we will be setting up cloud-init written in YAML format.\
**What is Yaml?**\
  YAML is a human-friendly data serialization
  language for all programming languages.

 To start we need to create a config file that we will add to our Digital Ocean vm. (UNFINISHED)
```
#cloud-config
users:
  - name: "Add your name here"
    ssh-authorized-keys:
      - "Add your public key here starting with ssh-ed25519"
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: sudo
packages:
  - htop
  - vim
disable_root: true
```
Ill clarify what each line of code is for in the configuration.
1. **#cloud-config** is used by cloud-init to recongize and process the file correctly.
2. **users:** configures a user account for the vm.
3. **packages:** adds a list of packages to be installed on the vm.
4, **disable_root** prevents the root user from logging in the server via SSH, which improves security.











