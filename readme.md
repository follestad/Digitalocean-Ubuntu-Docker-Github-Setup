# How to get started with DigitalOcean, Docker Compose and GitHub

## Create new droplet from Digital Ocean

1. Sign in to your account and create a new droplet from marketplace with Ubuntu and Docker.
2. Make sure to use an SSH key for better security

I recommend using [1Password](https://1password.com) to generate your SSH Key. With a Mac you
easily sign in securely with fingerprint.

## Step 1: SSH into your droplet

SSH into your droplet with your root user. I highly recommend to check out [Warp](https://www.warp.dev)
if you are on a Mac. It's a fantastic terminal application!

## Step 2: Update and upgrade your Ubuntu

You can check your Ubuntu version with `lsb_release -a`

```
$   sudo apt update
$   sudo apt upgrade
```

## Step 3: Update Git

Run these 3 commands to update Git.

```
sudo add-apt-repository -y ppa:git-core/ppa
```

```
sudo apt-get update
```

```
sudo apt-get install git -y
```

You can check your Git version with `git --version`. See if it matches
the [official git page](https://git-scm.com/downloads).

## Step 4: Update docker

Check your docker version with `docker --version`, and for docker-compose version
use `docker compose version`

Follow the [official instructions](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:

```
$   sudo apt-get update
$   sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
```

2. Add Docker’s official GPG key:

```
$   sudo mkdir -p /etc/apt/keyrings
$   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

3. Use the following command to set up the repository:

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

4. Update the apt package index, and install the latest version of Docker Engine, containerd, and Docker Compose, or go
   to the next step to install a specific version:

```
 $  sudo apt-get update
 $  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Now docker should be updated. Check with `docker --version` again.

To update docker compose follow the guids:

```
$   sudo apt-get update
$   sudo apt-get install docker-compose-plugin
```

Using `docker compose version` should now show v2.6.0 or higher.

## Step 5: Add a new user

You should not use your `root` user as default. Create a new user.

Create a new user. I will use {description} to explain what to add. For instance, if I use {username}, this should be
replaced with the username you want to use, without the curly braces.

```
$   adduser {username}
```

Add user to sudo group:

```
$  usermod -aG sudo {username}
```

Copy the root account’s~/.ssh/authorized_keys to your new user’s home directory so that
you could ssh as your new user: 
```
$  rsync --archive --chown={username}:{username} ~/.ssh /home/{username}
```
Add user to Docker group:
```
$  sudo usermod -aG docker {username}
```

## Step 6: Log in as new user
Exit terminal and SSH back into your account using your newly created user. `ssh {username}@{doamin/ip-address}`.

## Step 7: Add SSH for GitHub
Add a global username for Git. Doesn't need to match your git account.
```
$  git config --global user.name "YOUR NAME"
$  git config --global user.email "name@example.com"
```
Generate a new SSH key:
```
$  ssh-keygen -t rsa -b 4096
```
The terminal will prompt you for: `Enter file in which to save the key (/home/bonsy/.ssh/id_rsa):`. I usually just keep 
the suggested location by pressing enter.
Then the terminal will prompt you for passphrase. Which you can leave blank if you'd like. Press enter.

Ensure that SSH agant is running:
```
$  eval "$(ssh-agent -s)"
```
If you get an ID you should have an agent running.
Now add the ssh key to the agent:
```
ssh-add ~/.ssh/id_rsa
```
Now, copy the SSH key:
```
cat ~/.ssh/id_rsa.pub
```
Copy the key that shows and go to your GitHub account.

1. Click your avatar and go to settings
2. Go to SSH and GPG Keys in the menu
3. Add "New SSH key". 
4. Give the key a proper name like "Digital Ocean SITENAME SSH key".
5. Paste the copied SSH key and save.

Git should now work :) 