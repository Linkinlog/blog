---
layout: post
title:  "Managing dotfiles in style"
date:   2022-12-18 16:20:05 -0500
categories: git dotfiles
---

We've all been there, you're on a new computer (or at least one you haven't configured yet), and you're wondering how to get all your configs back in order as fast as possible, so you can get going again. 

There are many ways to handle this, from automation programs like Ansible or Puppet to manually downloading and replacing/symlinking. However, often these each come with their downsides, whether it be development time or adding extra programs to limited systems. I have found what I believe to be the best combination of solid version control, ease of use, and extensibility. 

## Introducing: bare Git repos and Git submodules

Through the utilization of a bare Git repo that we place in our home directory (`~/`) and the usage of Git submodules within that repo, we can effectively reduce the number of programs needed to be just Git. As well as giving us the ability to have more granular control our config files via individual repos, allowing us to work with full branches for each different program config. 

## So what is a bare Git repo, and how do I use it?

A bare Git repo is fairly similar to a non-bare repo, however, there are a few key differences. For example, bare repos do not contain any of your source files within the repo itself (if it did we would have duplicate files, which works fine for normal repos, but this works in our favor here). Also, all the information regarding the repo is stored wherever the repo is made, not in a `.git` folder like usual as mentioned above. There is a great article about it written here. 

We can utilize a bare repo similar to a normal one, with a simple `git init --bare ~/.dotfiles`, this will make a new bare repo named `.dotfiles` located in our home directory. From there we can make some special aliases to interact with this git repo without needing to be inside of it and without conflicting with other git repos. Don't forget to put it in your rc file!

```bash
alias cf='/usr/bin/git --git-dir=~/.dotfiles/ --work-tree=~/'

echo "alias cf='/usr/bin/git --git-dir=~/.dotfiles/ --work-tree=~/'" >> ~/.zrc
```

This will allow us to run `cf` from anywhere to interact with our new bare repo. Next let's make it so that we aren't showing any untracked files, as there will be quite a bit. 

```bash
cf config --local status.showUntrackedFiles no
```

Lastly, let's make sure we ignore our `.dotfiles` directory from git. 

```bash
echo "~/.dotfiles" >> ~/.gitignore
```

Great, now we can test this out by getting a status, `cf status`, and we can begin adding our config files. 


## Version Control-ception

Now that we have our fancy new repo set up to handle our config files, let's talk about submodules. We can think of submodules as just a git repo within a git repo. First off we are going to need to make a new git repo for each configuration we want to back up. Let's start with our tmux config. This is an easy one as it is just one file (`.tmux.conf`) and we can easily put it in a `~/.config/tmux/` directory.

So go ahead and make a temp working directory with `mkdir ~/Documents/tmux`. Then we can run `cd $_` to go to the directory we just made and run `git init` to make a new git repo. From here we can go through the normal steps of making a git repo, we can then copy/move our tmux config into this directory with 

```bash
mv ~/.tmux.conf ~/.config/.tmux.conf
```

After that, just track it with git and push it up! 

Once we have our tmux GitHub repo, we can link it up to our new bare repo. Let's first make sure we do not have a folder already there 
```bash
rm ~/.config/tmux
```
Next up we add our tmux repo as a submodule
```bash
cd ~/.config && cf submodule add -b {branch_name} {repo_link} tmux
```

Make sure to replace the `branch_name` with the branch you would like to be tracking and the `repo_link` with the link you would usually use to clone the repo. 

At this point we can just rinse and repeat, making a new repo for each config we want to add, putting our configs in them, adding them as submodules, and we are done! To get the full usage of Git submodules, be sure to read up on their commands with 
```bash
cf submodule -h
```

## Pulling our data back on a new computer

So now that we have everything tracked in GitHub, how do we pull it down to a new computer?? Rather simple, but a small bit different from usual of course. We can run through the alias we set above as usual. Then we will run the `git clone` command but for bare repos.
```bash
cf clone --bare {repo_link}
```

Running `cf status` again we can see that all our files are showing up as deleted, so let's get them back.

```bash
cf checkout
```

Ah, there we go. Assuming there were no conflicts, all our configuration files are back to how we had them, and we only needed to use git. For more reading on submodules, feel free to check out the official GitHub blog [here](https://github.blog/2016-02-01-working-with-submodules/).

Until next time, 

Dahlton
