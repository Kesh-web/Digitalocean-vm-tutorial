# DigitalOcean-vm-tutorial
This is an Arch Linux Setup on DigitalOcean using cloud-init and doctl.

## Instruction Overview ##
These are the steps were going to walk through for setting up the DigitalOcean Droplet. The main ones are setting up SSH key pairs, doctl, and the cloud-init config file.


**What is DigitalOcean?**\
Digital ocean is a cloud infrastructure provider that offers cloud computing services. It allows you to manage virtual private servers that they call "Droplets".

The purpose of a Droplet is to be able to operate indepedantly and it runs its own operating system. You would be able to install manage applications, databases, websites, and other services. 

Some examples of using a Droplet is to
- Host websites
- Hosting file servers to store files securely 
- Database hosting (SQL, MongoDB)
- VPN Services
- Game Servers

\
With that in mind the importance of setting up a secure VPS (virtual private server) is extremely crucial. Imagine setting up a file storage with your sensitive information. Without proper security your data could be exposed. Another example is preventing unauthorized access by setting up SSH Keys (will be explained in section 1).

**What is cloud-init?**\
We will be using cloud-init to automate the configuration and setup of the Droplet. It is an open source tool used to automate configuration of cloud instances when they are first launched. It is supported across all major public cloud providers, provisioning systems for private cloud infrastructure, and bare-metal installations.

**What is doctl?** \
Doctl is a command line interface for DigitalOcean. It allows you to create, configure, destroy DigitalOcean Droplets, firewalls, database clusters, etc.

1. SSH Key Pairs 
2. Creating an API Token for access to ```doctl```
3. Setting up the Cloud-init configuration file
4. Installing ```doctl```
5. Uploading a custom image to DigitalOcean
6. Authenticating ```doctl``` using your API Token
7. Seting up a Droplet with ```cloud-init``` 
8. Connecting to the newly created Droplet 
9. Verifying cloud-init packages were installed 



##
## Creating SSH Key Pairs
To start we will be creating SSH keys which allows you to connect to remote servers over an encrypted connection.

- You should be in your home directory which is **~** to start.
- Once youre in your home directory **type** 

```
mkdir .ssh
 ``` 

This will create the folder that store the SSH keys in. 

- Once completed you can **type** the following code in your terminal.

```
ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"
```

To explain the previous command 
1. **ssh-keygen** is the base of the command to create the public and private keys.
2. **-t** specifies the type of key (Ed25519 is an elliptic curve signing algorithm using EdDSA and Curve25519.)
3. **-f** specifies the file path that the key will be generated in.
4. **do-key** is the name we will be giving the generated key. Its important to manage names properly if you have more than one SSH key.
4. **-C** attaches a comment to the key(in our case putting an email to remember.)



<br><br>

#### Copying SSH Keys to clipboard
To copy the SSH Public Key to your clipboard, go to your terminal and **type**. 
>[!NOTE]
>that this is how I do it on my system (Arch Linux), but you should check how to copy a file to a clipboard on your system.

```
wl-copy < ~/.ssh/do-key.pub
``` 

This will add the Public Key to your clipboard which you can then add to your Digital Ocean account in the cloud-init section

>[!NOTE]
> Keep this somewhere safe for now as we will need to paste it again.

<br><br>

## Installing ```doctl```
- You should be in your home directory which is ~ to start.


```
sudo pacman -S doctl
```

#### Creating an API Token
After installing doctl we need to create an **API Token** to grant acount access to doctl. On Digital Oceans homepage look for **API** on the left side task bar.
<br><br>
<img src= Assets/API-button.png style='width: 50%;'>

Give **Full Access**, a **Token Name**, and then generate the Token.

<img src= Assets/Create-access-token.png style='width: 50%;'>

Copy the Access Token and keep it somewhere safe.
<br><br>
<img src= Assets/Dont-forget-token.png>





## Setting up cloud-init for the First Configuration
Now we will be setting up cloud-init written in YAML format.\
**What is Yaml?**\
  YAML is a human-friendly data serialization
  language for all programming languages.

 To start we need to create a config file that we will add to our Digital Ocean vm. To do so we need to first install VIM which is a linux based text editor.

 ``` 
 sudo pacman -S vim
```

 After downloading we can add the cloud config file to onto our arch vm which automates the initalization.

 ``` 
 vim cloud-init-arch.yml
  ```



Paste the following config into **vim**.

You can press **i** to start editing and after adding the file **:wq** to save and exit **vim**

1. **:** enters the command-line mode
2. **w** writes what you entered
3. **q** quits out of vim 


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
  - curl
  - less
  - tree

disable_root: true
```
Ill clarify what each line of code is for in the configuration.
1. **#cloud-config** is used by cloud-init to recongize and process the file correctly.
2. **users:** configures a user account for the vm.
3. **packages:** adds a list of packages to be installed on the vm.
4, **disable_root** prevents the root user from logging in the server via SSH, which improves security.

<br><br>

## Installing ```doctl``` 
Now that we set up the #cloud-config file we can install ```doctl``` on the archlinux vm. Before we install it we need to install **wget** which is a program that retrieves content from web servers. To do so use the following command in your vm.

```
sudo pacman -S wget 
```

After that you can download ```doctl``` with the following 


```wget https://github.com/digitalocean/doctl/releases/download/v1.110.0/doctl-1.110.0-linux-amd64.tar.gz```

Then extract the file using ```tar``` which is a command used for extracting files from archives

```tar xf ~/doctl-1.110.0-linux-amd64.tar.gz```

Finally we can move the extracted files to your path and that will install it.

```sudo mv ~/doctl /usr/local/bin```

To verify its been installed run the following and youll see an output like this

```doctl version```

<img src= Assets/doctl-version.png>

<br><br>
## Uploading Custom Image to DigitalOcean
Now before we deploy the droplet with cloud-init, you'll need to upload the custom Arch Linux image to Digital Ocean. Once you upload the custom image, youll be able to create a Droplet with all preconfigured cloud file.

Start with selecting the project youre going to use.

<img src= Assets/Select-project.png style='width: 50%;'>


After selecting the project on the left side taskbar click on **Backups and Snapshots** and itll bring you to the page to add your custom image.


<img src= Assets/Select-backup.png style='width: 50%;'>


**Press** the big upload image button on the bottom right

<img src= Assets/CustomImageUpload.png >

Select the Arch Linux Image and open it 

<img src= Assets/File-upload.png >

Choose the Arch Linux Distro

<img src= Assets/upload-an-image.png style='width: 50%;'>

We will be choosing San Francisco 3 as the Datacenter

<img src= Assets/choose-datacenter.png style='width: 50%;'>


Then at the bottom you can find the upload button

<img src= Assets/blue-button-image.png style='width: 50%;'>
<br><br>

## Authenticating ```doctl``` using your API Token

Open your terminal and **type** this command in
You can change  anything about context to anything you want, but I just put my name.

```doctl auth init --context Kesh```


After adding your API Token youll see a green checkmark saying your token was validated. 


<img src= 'Assets/Validating.png' style='width= 50%' > 



You can **paste** this command to switch to profiles to the one you created.

```
doctl auth switch --context <Name of Profile> 
```

After that use this command below to get the details of your account.

```
doctl account get
```


If successful you'll see an output like this.



<img src= Assets/doctl-account-get.png>




<br><br>
## Seting up a Droplet with ```cloud-init``` 

Now that you have your cloud-init file, doctl first initalization, and you have your custom image uploaded to DigitalOcean we can set up your first Droplet. 

'''

doctl compute droplet create "Add droplet name here" 
  --size s-1vcpu-1gb 
  --image 165064168 
  --region sfo3 
  --ssh-keys "add ssh-key id" 
  --user-data-file "Path of cloud init file" 
  --wait
  
'''

**--size** specifices the type of pc created
**--image** is the distro we chose (Arch)
**--region** is the region of the server(San Franciso)
**--wait** makes sure the command waits until the droplet is created before returning back to the terminal

After creating your droplet use this command to see your existing projects.
This is show the name and Public IP of your new Droplet which we will use to connect to it in the next section.

```
doctl compute droplet list --format Name,PublicIPv4
```

<br><br>
## Connecting to your newly created Droplet ##

To connect you'll need to put this command below with the details from "droplet list" above.

```
ssh -i ~/.ssh/"key-name" "name"@"Public IPv4 Address""
```
- ```example-user```: Specifies the user from your config
- ```"your-droplet-ip-address"```: IP address of your Droplet

Once you're in your Droplet it'll show your name like this.

<img src=Assets/Droplet-terminal.png>

### Verify cloud-init packages were installed ###

Since we added some packages to the ```cloud-init``` config we can verify it was installed by running this command. One of those packages were vim so we can check.

```
vim --version
```
This is how it should look:

<img src=Assets/vim-version.png> 





After that you can do what you please with your newly created Droplet!
I hope this guide served you well in creating a new Droplet on DigitalOcean using doctl.
<br><br>

## References ##

pacman - ArchWiki. (n.d.). Wiki.archlinux.org. https://wiki.archlinux.org/title/pacman


‌contributors, M. C. and. (n.d.). Markdown Guide. Www.markdownguide.org. https://www.markdownguide.org/

cloud-init 24.3.1 documentation. (2017). Cloud-Init.io. https://docs.cloud-init.io/en/latest/index.html

How To Use Cloud-Config For Your Initial Server Setup | DigitalOcean. (n.d.). Www.digitalocean.com. https://www.digitalocean.com/community/tutorials/how-to-use-cloud-config-for-your-initial-server-setup

Introducing doctl: The command line interface to digitalocean. DigitalOcean. (n.d.). https://www.digitalocean.com/blog/introducing-doctl 

How to create a personal access token. DigitalOcean Documentation. (n.d.-b). https://docs.digitalocean.com/reference/api/create-personal-access-token/ 

How to automate droplet setup with cloud-init. DigitalOcean Documentation. (n.d.-a). https://docs.digitalocean.com/products/droplets/how-to/automate-setup-with-cloud-init/ 

Adding a new SSH key to your github account. GitHub Docs. (n.d.). https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account 

Chapter 2. introduction to cloud-init. Chapter 2. Introduction to cloud-init | Red Hat Product Documentation. (n.d.). https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_cloud-init_for_rhel_9/introduction-to-cloud-init_cloud-content 

Doctl compute droplet create. DigitalOcean Documentation. (n.d.-a). https://docs.digitalocean.com/reference/doctl/reference/compute/droplet/create/ 

Doctl Auth. DigitalOcean Documentation. (n.d.-a). https://docs.digitalocean.com/reference/doctl/reference/auth/ 

‌

‌



