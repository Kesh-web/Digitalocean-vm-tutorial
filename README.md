# Digitalocean-vm-tutorial
This is an Arch Linux Setup on DigitalOcean using cloud-init and doctl.

**What is Digital Ocean?**\
Digital ocean is a cloud infrastructure provider that offers cloud computing services. It allows you to manage virtual private servers that they call "Droplets".

The purpose of a Droplet is to be able to operate indepedantly and it runs its own operating system. You would be able to install manage applications, databases, websites, and other services. 

Some examples of using a Droplet is to host websites, host services such as (email, file storage), and running databases.

With that in mind the importance of setting up a secure VPS (virtual private server) is extremely crutial. Imagine setting up a file storage with your sensitive information. Without proper security your data could be exposed. Another example is preventing unauthorized access by setting up SSH Keys (will be explained in section 1).

**What is cloud-init?**\
It is an open source tool used to automate configuration of cloud instances when they are first launched. It is supported across all major public cloud providers, provisioning systems for private cloud infrastructure, and bare-metal installations.


## Creating SSH Keys 
To start we will be creating SSH keys which is 

- You should be in your home directory which is ~ to start.
- Once completed you can **type** the following code in your terminal.

```ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"```

To explain the following command /
1. **ssh-keygen** is the base of the command to create the public and private keys.
2. **-t** specifies the type of key (Ed25519 is an elliptic curve signing algorithm using EdDSA and Curve25519.)
3. **-f** specifies the file path that the key will be generated in.
4. **do-key** is the name we will be giving the generated key. Its important to manage names properly if you have more than one SSH key.
4. **-C** attaches a comment to the key(in our case putting an email to remember.)











