# How to get started with Ubuntu server with Docker Compose and GitHub on DigitalOcean.

## Create a new droplet from Digital Ocean

1. Create a new droplet from the marketplace with Ubuntu and Docker.
2. Make sure to use an SSH key for better security

I recommend using [1Password](https://1password.com) to generate your SSH Key. With a Mac, you
can easily sign in securely with a fingerprint.

### SSH into your droplet

SSH into your droplet with your root user. I highly recommend checking out [Warp](https://www.warp.dev)
if you are on a Mac. It's a fantastic terminal application that will make your life easier.

## Update your sever
The following steps will make sure your server is running the latest versions of ubuntu, docker and git.

### Update and upgrade the Ubuntu sever 

You can check your Ubuntu version with `lsb_release -a`

```
sudo apt update
```

```
sudo apt upgrade
```

### Update Git

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

### Update docker

Check your docker version with `docker --version`, and for docker-compose version
use `docker compose version`

Follow the [official instructions](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:
   ```
   sudo apt-get update
   ```
   ```
   sudo apt-get install \
           ca-certificates \
           curl \
           gnupg \
           lsb-release
   ```

2. Add Docker’s official GPG key:

   ```
   sudo mkdir -p /etc/apt/keyrings
   ```
   ```
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   ```

3. Use the following command to set up the repository:

   ```
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. Update the apt package index, and install the latest version of Docker Engine, containers, and Docker Compose, or go
   to the next step to install a specific version:

   ```
    sudo apt-get update
   ```
   ```
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
   ```

Now docker should be updated. Check with `docker --version` again.

To update docker compose follow the guids:

```
sudo apt-get update
```

```
sudo apt-get install docker-compose-plugin
```

Using `docker compose version` should now show v2.12.2 or higher.

## Add a new user to server

You should not use your `root` user as default. Create a new user.

Create a new user. Replace curly-braces with your details.

```
adduser {username}
```

Add user to sudo group:

```
usermod -aG sudo {username}
```

Copy SSH keys from the root account (`s~/.ssh/authorized_keys`) to the home directory of your new user:

```
rsync --archive --chown={username}:{username} ~/.ssh /home/{username}
```

Add user to Docker group:

```
sudo usermod -aG docker {username}
```

### Log in as new user

Exit terminal and SSH back into your account using your newly created user. `ssh {username}@{doamin/ip-address}`.

## Add SSH for GitHub

First, make sure that you are logged in with your new user, and not `root`.

Add a global username for Git. Doesn't need to match your git account.

```
git config --global user.name "YOUR NAME"
```

```
git config --global user.email "name@example.com"
```

Generate a new SSH key:

```
ssh-keygen -t rsa -b 4096
```

The terminal will prompt you for: `Enter file in which to save the key (/home/{username}}/.ssh/id_rsa):`. I usually
keep the suggested location by pressing enter.

Then the terminal will prompt you for passphrase. Which you can leave blank if you'd like. Press enter.

Ensure that SSH agent is running:

```
eval "$(ssh-agent -s)"
```

If you get an ID you should have an agent running.

Now, add the ssh key to the agent:

```
ssh-add ~/.ssh/id_rsa
```

Now, we need to copy the SSH key:

```
cat ~/.ssh/id_rsa.pub
```

Copy the key that shows and go to your GitHub account.

### Add the SSH Key to your GitHub account
1. Click your avatar and go to `settings`
2. Go to `SSH and GPG Keys` on the left side menu, under the `Access` category.  
3. Click the "New SSH key" button.
4. Give the key a proper name. For instance: "Digital Ocean SITENAME SSH key".
5. Paste the copied SSH key from the terminal and click save.

Your server should now be able to access your git repositories.

## Add Swap Space on Ubuntu
One of the easiest way of increasing the responsiveness of your server and guarding against out-of-memory errors in applications is to add some swap space.

Check out the article on DigitalOcean:

[How To Add Swap Space on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04)

## Got any suggestions or improvements?
Please let me know if you got any suggestions or improvements to add to this article.