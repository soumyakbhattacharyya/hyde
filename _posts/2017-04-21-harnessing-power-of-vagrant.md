---
layout: post
title: Harnessing Power of Vagrant
---

This post is a consolidation of most useful and relevant information about [Vagrant](https://www.vagrantup.com/), as I realized in my day to day life. I have seen Vagrant leaving upto the promise that they quote on their website ... quite well 

> Vagrant is a tool for building and managing virtual machine environments in a single workflow. With an easy-to-use workflow and focus on automation, Vagrant lowers development environment setup time, increases production parity, and makes the "works on my machine" excuse a relic of the past.

I believe Vagrant is a broad subject of it's own. To keep things into perspective, I will document following information in a sequence that they would prove to be useful, while getting started

1. Booting up the *box* 
2. SSH - ing into it
3. Using Cloud *as provider*
4. Glossary of useful commands 

### Booting up the box

*Boxes* as [they](https://www.vagrantup.com/intro/getting-started/boxes.html) say are base images that Vagrant uses to clone a virtual machine. Before using one of them from [this list](http://www.vagrantbox.es/) ensure you have installed and configured [Vagrant](https://www.vagrantup.com/) and it's compatible [Virtualbox](https://www.virtualbox.org/) following [recommendation](https://www.vagrantup.com/docs/virtualbox/). Virtualbox works as the *default provider* for Vagrant. It is important that recommended compatibility between Vagrant and Virtualbox is ensured for a smooth usage experience.

Assuming at this point you have got Vagrant and Virtualbox installed on your machine, issue following command to boot Oracle Linux 7 VM right away

```shell
 $ vagrant box add dev-box http://cloud.terry.im/vagrant/oraclelinux-7-x86_64.box
 $ vagrant init dev-box
 $ vagrant up
```

Disclaimer : the box I have chosen is my personal favorite as it comes with pre - installed software. Please feel free to consult [the list](http://www.vagrantbox.es/) to choose one which is appropriate for your situation. 

### SSH - ing into it 

Once the VM has come alive, its the time to SSH into it. Vagrant has native support to have you SSH - ing in the box. However, if you prefer *Putty* over the default option, you may following instructions found [here](https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Connect-to-Your-Vagrant-Virtual-Machine-with-PuTTY) to convert *insecure_private_key* from Vagrant to *.ppk* file that Putty understands.

It is worth mentioning that there exists a [cooler](https://github.com/nickryand/vagrant-multi-putty) option which helps in seamlessly using Putty eliminating any hassle, whatsoever. 

### Using Cloud as provider

Vagrant can also use AWS as its provider. 

TBD



#### References 

* About my favorite base box : https://github.com/terrywang/vagrantboxes/blob/master/oraclelinux-7-x86_64.md
* Basic commands : http://www.vagrantbox.es/
* Command cheat sheet : https://gist.github.com/wpscholar/a49594e2e2b918f4d0c4
* Another command cheat sheet with explanation : http://notes.jerzygangi.com/vagrant-cheat-sheet/
* On connecting with virtual boxes with Putty : https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Connect-to-Your-Vagrant-Virtual-Machine-with-PuTTY
* On connecting with virtual boxes with Putty (without any hassle) : https://github.com/nickryand/vagrant-multi-putty