---
layout: post
title: Remote Debugging with PyCharm
date: 2015-09-17 20:15:19.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags:
- debugging
- development
- how-to
- ide
- pycharm
- python
- remote
- software
- technology
- tutorial
meta:
  geo_public: '0'
  _edit_last: '10080708'
  _wpcom_is_markdown: '1'
  _publicize_job_id: '14892955525'
author:
  login: wswld
  email: seva17@gmail.com
  display_name: wswld
  first_name: ''
  last_name: ''
---

I'm working in a project now, that requires a certain (server) environment to 
run, hence it is developed on my local machine and then gets deployed on remote 
server. I thought I'm gonna say bye bye to my favorite PyCharm feature, namely 
the debugger, but to my surprise remote debugging has been supported for years 
now. It took some time to figure out (tutorials online are a bit ambiguous), so 
here is a short report on my findings.

For the sake of this tutorial let's assume the following:

* **Remote host:** `foo_host.io`,
* **Remote user:** `foo_usr` (`/home/foo_usr/`),
* **Local user:** `bar_usr` (`/home/bar_usr/`),
* **Path to the project on the local machine:** `/home/bar_usr/proj`.

Here goes the step-by-step how-to:

1. First we need to set up remote deploy, if you haven't done so already. Go to 
   *Tools* → *Deployment* → *Configuration*. And set up access to your remote 
   server via SSH. I'd use:

    * **Type:** SFTP,
    * **SFTP host:** `foo_host.io` (don't forget to test the connection before 
       applying),
    * **Port:** 22 (obvously),
    * **Root path:** `/home/foo_usr`,
    * **User name:** `foo_usr`,
    * **Auth type:** Key pair (OpenSSH or PyTTY),
    * **Private key file:** `/home/bar_usr/.ssh/id_rsa` (you'd need to generate 
      the key and `ssh-copy-id` it to the remote machine, which is outside of 
      the scope of this tutorial).

1. Go to *Mappings* tab and add *Deployment path* on server (pehaps, the name 
   of your project),
1. Now under *Tools* → *Deployment* you have an option to deploy your code to 
   remote server. These first three steps could be replaced with simple 
   Git-repository on the side of the server, however I sometimes prefer this 
   way.
1. Now when you have the deployment set up you can go *Tools* → *Deployment* → 
   *Upload to ...*, note however, that it deploys only the file you have opened 
   or the directory you selected in the project view, so if you need to sync 
   the whole project just select your project root.
1. I use `virtualenv`, so at this step I need to `ssh` into the remote machine 
   and set up `virtualenv` in your project directory 
   (`/home/foo_usr/test/.env`), which is outside of the scope of this tutorial. 
   If you're planning on using the global Pyhton interpreter, just skip this 
   step.
1. Now let's go *File* → *Settings* → *Project ...* → *Project Interpreter*. 
   Using gear button select *Add Remote*. The following dialog window would let 
   you set up a remote interpreter over SSH (including remote `.env`), Vagrant 
   or using deployment configuration you have set up previously. For the sake 
   of this tutorial I'm going to put something like that here (using SSH of 
   course):

   * **Host:** `foo_host.io`,
   * **Port:** 22 (which is there by default),
   * **User name:** `foo_usr`,
   * **Auth type:** Key pair (OpenSSH or PyTTY),
   * **Private key file:** `/home/bar_usr/.ssh/id_rsa`,
   * **Python interpreter path:** `/home/foo_usr/proj/.env/bin/python`

1. If you set up everything correctly, it should list all the packages 
   installed in your remote environment (if any) and select this interpreter 
   for your project.
1. Now let's do the last, but the most important step: configure debugging. Go 
   to *Edit Configurations...* menu and set things up accordingly. For our 
   hypothetical project I will use the following:

   * **Script:** `proj/run.py` (or something along these lines),
   * **Python interpreter:** just select the remote interpreter you have set up 
     earlier,
   * **Working directory:** `/home/bar_usr/proj/` (note that this is working 
     directory on local machine),
   * **Path mapping:** create a mapping along the lines of 
     `/home/bar_usr/proj = /home/foo_usr/proj` (although this seems pretty 
     easy, it may get tricky sometimes, when you forget about mappings and move 
     the projects around, be careful).

That's about it. Now we should have a more or less working configuration that 
you could use both for debugging and running your project. Don't forget to 
update/redeploy your project before running as the versions may get async and 
PyCharm would get all whiny about missing files.
