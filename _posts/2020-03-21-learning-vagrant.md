---
layout: post
title:  "Setting up Vagrant"
date:   2020-03-21 10:36:00 +0200
categories: programming
tags: [vagrant, virtualization]
---

I've been programming long enough now to finally start sorting out my development environments in a more organized fashion. Since most of my programming has been in Python, virtual environments have been good enough. But when deploying my own projects to servers, the process hasn't really been optimal.  A combination of Python venvs, local linux servers in Virtualbox, home test server and multiple hosting platforms in the web is quite disogranized.

So this is what I did to get started (skipping the installation of Vagrant itself):

```bash
# Add Ubuntu 18.04 LTS Server from Vagrant cloud
vagrant box add ubuntu/bionic64 

# Create a vm directory and init from there
mkdir ~/dev/vms/bionic && cd ~/dev/vms/bionic
vagrant init ubuntu/bionic64

# Boot up the installation
vagrant up
```

All of this goes smoothly, but the boot process gives you a gentle warning: "guest additions on this VM do not match the installed version of VirtualBox" which might be a problem for sharing folders. This seems like it might be a big problem for workflow.

So how to upgrade the guest additions? Turns out the easiest is to use a [plugin](https://github.com/dotless-de/vagrant-vbguest) (having to install plugins before getting anything done is usually not a good sign but I guess we play by the book here).

```bash
# Install plugin to keep Virtualbox guest additions up-to-date
vagrant plugin install vagrant-vbguest
vagrant halt
vagrant reload
```

And then login to the newly installed server
```bash
vagrant ssh
```

Well that was quite easy. I will document the set up of an actual dev server on another post when I get that figured out.



