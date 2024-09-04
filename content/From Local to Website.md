---
title: From Local to Website
draft: false
tags:
  - build
---
### Before the content...

Quartz documentation : [Welcome to Quartz 4 (jzhao.xyz)](https://quartz.jzhao.xyz/)

Official documentation is a all-in-one super detailed guidline. Reading the documentation is always helpful.

Actually, I followed a youtube video to build the website. I think the video involves almost all the details of the building process, which is super friendly to beginners. I will put the link below [How to publish your notes for free with Quartz (youtube.com)](https://www.youtube.com/watch?v=6s6DT1yN4dw)  

And watching a video is much easier than reading passages or documents (lol). Nevertheless, I will go through the whole building process of **mine**. Due to the differences between systems and environments, this tutorial might not be helpful for all people. But I hope it could help most of us who want to master how to publish our Obsidian contents for free, instead of spending our bucks for food on the official publish service QwQ.

Anyway, let's start !

### Prerequisites

- VS Code
	- with WSL (because I'm using windows)
		- Open Terminal , click '+'  , then choose Linux/WSL and just install WSL
		- Connect with WSL and use the Linux terminal
		- ![[Pasted image 20240904163854.png]]
	- with Github Actions (maybe)
- Nodejs at least v20 with npm at least v9.3.1
	- Open the terminal and follow the guide here [nodesource/distributions: NodeSource Node.js Binary Distributions (github.com)](https://github.com/nodesource/distributions?tab=readme-ov-file#installation-instructions)
	- Command `node -v`  checks the version of your Nodejs
	- Ubuntu
		```bash
		sudo apt-get install -y curl # check if you have curl
		curl -fsSL https://deb.nodesource.com/setup_lts.x -o nodesource_setup.sh
		sudo -E bash nodesource_setup.sh
		sudo apt-get install -y nodejs # install and setup
		node -v # verify the version of node
		npm -v # verify the version of npm
		```


### Install and initialize Quartz

[Welcome to Quartz 4 (jzhao.xyz)](https://quartz.jzhao.xyz/)

```bash
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create
```

In this process, we need to choose the way we create a new quartz. I chose to create an empty quartz and treat links as shortest path, which is enough for most Obsidian vaults.

For users in China (like me), we might encounter a long wait when we enter the command `npm i`. Please be patient. Under most circumstances, it will take 5~10 minutes to install all the dependencies. If you have a bad connection, please try reseting the source.

mirror source of huaweicloud

```bash
npm config set registry https://mirrors.huaweicloud.com/repository/npm/
npm config get registry # to verify the source
```

mirror source of tencent cloud

```bash
npm config set registry http://mirrors.cloud.tencent.com/npm/
npm config get registry # to verify the source
```

Once the installation is done, you can open your folder in VS Code for the convenience of later operations.

### Set up Github repository

[Setting up your GitHub repository (jzhao.xyz)](https://quartz.jzhao.xyz/setting-up-your-GitHub-repository)

If you haven't have SSH keys, add new keys for your Github account first.

```bash
ls -al ~/.ssh # to check if you have ssh keys in local
```

If you already have one, you will see files like `id_rsa` , `id_rsa.pub`. Otherwise, you need to generate one and add it to agent.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Then, open the key file. 

```bash
cat ~/.ssh/id_ed25519.pub
```

 And copy the content of the key.

Login to Github. Click `Settings` > `SSH and GPG keys`. Click `New SSH key` , paste your key and `Add SSH key`

You can test the connection with the code below.

```bash
ssh -T git@github.com
```

If congifure correctly, you will see:

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

After all these steps, we just need to follow the steps in the documentation.

Create a new repository without `README`, license  or `gitignore` files.

At quick setup page, click 'SSH' and copy the remote URL. Replace the `REMOTE-URL` with your remote URL

```bash
# list all the repositories that are tracked
git remote -v 
# if the origin doesn't match your own repository, set your repository as the origin
git remote set-url origin REMOTE-URL
npx quartz sync --no-pull
```

When everything has done, our folder will be synchronized to the repository.

### Set up your Obsidian vault

[Authoring Content (jzhao.xyz)](https://quartz.jzhao.xyz/authoring-content)

Open Obsidian and choose our folder as a vault.

You can install plugins as usual.

Our Markdown files are supposed to be stored in `content` folder and they'll be published automatically later.

And it's recommended to create a template with the official given note.

```md
---
title: Example Title
draft: false
tags:
  - example-tag
---
 
The rest of your content lives here. You can use **Markdown** here :)
```

Use command

```bash
npx quartz sync
```

to save your changes to Github

If you want to see the website locally, look here [Building your Quartz (jzhao.xyz)](https://quartz.jzhao.xyz/build)

Just a few lines of code enables you to preview your site locally.

### Publish with Github Pages

[Hosting (jzhao.xyz)](https://quartz.jzhao.xyz/hosting#github-pages)

Following the steps and create the `deploy.yml` file in your folder.

Then go to github `Settings` > `Pages` > `Source` and select `Github Actions`

Commit all the changes by doing `npx quartz sync` (This is also the method for commiting all your changes in the future)

And your site will be deployed at `<github-username>.github.io/<repository-name>`

