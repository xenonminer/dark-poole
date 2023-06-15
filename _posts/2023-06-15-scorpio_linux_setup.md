---
layout: post
title: Scorpio Linux Setup
---

# Description

In this blog post, you will learn how to setup the [Scorpio Scoring Engine](https://github.com/Cybertron51/scorpio) for Linux OSes.

## What is Scorpio?

Scorpio is a Python based scoring engine created to score images and their vulnerabilities. Scorpio checks if a vulnerability has been fixed and provides live feedback back to the students.

## How does Scorpio work?

The image creator (you) will implement vulnerabilities onto a virtual machine and record them onto engine.py, which runes every 30 seconds. As the students secure the system, the scoring engine will update Template.html (Scoring Report) to display the fixed vulnerabilities.

# Instructions for setup

## Tools

Before you get started, these are some important tools to make the setup process quicker
- Install python2 ```sudo apt install python2 -y```
- Install pip ```sudo apt install python3-pip -y```
- Install git ```sudo apt install git -y```

## Steps

### Step 1: Open/Create a Sandbox Virtual Machine

To get started, you will want to create your base image first. You can grab free sandbox images at [linuxvmimages](https://www.linuxvmimages.com/). Since this post is meant for Linux and CyberPatriot, you want to grab a [Ubuntu 20.04 Image](https://www.linuxvmimages.com/images/ubuntu-2004/) or a [Ubuntu 22.04 Image](https://www.linuxvmimages.com/images/ubuntu-2204/). Also make sure to grab the Vmware one and NOT the VirtualBox one.

Then, you want to extract the image and receive the image folder.

After extracting, open the .vmx file by double-clicking on it or go into Vmware and click "Open a Virtual Machine" then click the .vmx file.

Then run the image by clicking the play button.

### Step 2: Setting up/Implementing Vulnerabilities in the Image

Now that you have the sandbox image open, you want to implement all your vulnerabilities into the image.

I will use 4 examples of vulnerabilities you can setup. **(Don't limit yourself to these)**

1. Adding an unauthorized user onto the system
    - ```sudo adduser username```
    - Example: gif_here
2. Modifying the GUI Software & Updates Settings
    - Example: gif_here
3. Installing a malicious package
    - ```sudo apt install package```
    - Example: gif_here
4. Adding a malicious file
    - ```touch /path/to/random/directory/file```
    - Example: gif_here

During this step, you also want to create the Forensics Questions and the "Set Name for Scoring Report" file. Create these files all on the Desktop for your main user.

The Forensics will be in the format "Forensics_#.txt". Create them using the command ```touch Forensics_#.txt```. Format of forensics will be:
```
(Example: Example_Answer)

Question: Question_Here

Answer: Answer_Here
```

Also create the "Set Name for Scoring Report" on the Desktop of the main user: ```touch "Set Name for Scoring Report"```. Format of this file will be:
```
YOUR FULL NAME: [id]
```

Other files you want to set up properly are
- Your main user's .bash_history file + root's .bash_history file
  - To set this up (Example of user joe): ```rm -rf /home/joe/.bash_history; ln -s /dev/null /home/joe/.bash_history; sudo rm -rf /root/.bash_history; sudo ln -s /dev/null /root/.bash_history```
- Autologin for your main user with gdm3
  - To set this up (Example of user joe): ```sudo nano /etc/gdm3/custom.conf
  Then edit lines: AutomaticLoginEnable=True
  AutomaticLogin=joe
  ``` Make sure these lines are uncommented as well

### Step 3: Cloning Scorpio Repository into the system + Setting up directories

After setting up and adding all the vulnerabilities into the image, you can start setting up the engine.

First, clone the engine repository: ```git clone https://github.com/Cybertron51/scorpio.git```

Then, this repository will need to be in /opt with the name temp: ```sudo mv scorpio /opt/temp```

### Step 4: Adding vulnerabilities to the engine



### Step 5: Encrypting the engine



### Step 6: Setting up the engine service



### Step 7: Powering Off and Exporting

