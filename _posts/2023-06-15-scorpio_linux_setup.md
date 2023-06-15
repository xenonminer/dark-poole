---
layout: post
title: Scorpio Linux Setup
---

# Description

In this blog post, you will learn how to setup the [Scorpio Scoring Engine](https://github.com/Cybertron51/scorpio) for Linux OSes.

## What is Scorpio?

Scorpio is a Python based scoring engine created to score images and their vulnerabilities. Scorpio checks if a vulnerability has been fixed and provides live feedback back to the students.

## How does Scorpio work?

The image creator (you) will implement vulnerabilities onto a virtual machine and record them onto engine.py, which runs every 30 seconds. As the students secure the system, the scoring engine will update Template.html (Scoring Report) to display the fixed vulnerabilities.

# Instructions for setup

If have already created the image and its vulns or just want to know how to setup the Scorpio Engine for Linux, [jump directly to Scorpio Setup](#step-3-cloning-scorpio-repository-into-the-system--setting-up-directories)

## Tools

Before you get started, these are some important tools to make the setup process quicker
- Install python2: ```sudo apt install python2 -y```
- Install pip: ```sudo apt install python3-pip -y```
- Install pyconcrete: ```sudo PYCONCRETE_PASSPHRASE=password_here pip install pyconcrete```
- Install git: ```sudo apt install git -y```

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
    - Example: ![gif]()
2. Modifying the GUI Software & Updates Settings
    - Example: ![gif]()
3. Installing a malicious package
    - ```sudo apt install package```
    - Example: ![gif]()
4. Adding a malicious file
    - ```touch /path/to/random/directory/file```
    - Example: ![gif]()

During this step, you also want to create the README, Forensics Questions, and the "Set Name for Scoring Report" file. Create these files all on the Desktop for your main user.

The README should be setup in Google Docs (Preferably). [Here is an example README for use](https://docs.google.com/document/d/1M4mljp9yEzb5GLsibm0Z7RqTzpiPQalwZlfn4i8nixo)
After creating the README, download the README as a pdf onto the system and rename it to README.pdf. Then move it onto the Desktop.
```bash
mv ~/Downloads/README_download ~/Desktop/README.pdf
```

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

After you have added the vulnerabilities on the system, you want to configure them being checked in the actual engine.

Navigate to **/opt/temp/** and open up **engine.py**

The lines you want to change are the ones that look like: 
```py
vulns.append(newCommandObject(
    'cat /home/kaisa/Desktop/Forensics_1.txt', 'Valoran City Park', 
    True, 6, 'Forensics Question 1 correct'))
```

These lines add a vuln object in with their specific parameters. Going left to right the parameters are:
- the command to check the vuln
- what to check for in the result of the command
- If the command was checked for that value, put True. If it was checked for not being that value, put False.
- Number of points assigned for that vulnerability
- Scoring Report message

To explain the 4 vulns from earlier that we setup, they would potentially look like this
1. Checking for unauthorized user deletion (user will be bob): 
    ```py
    vulns.append(newCommandObject(
        'cat /etc/passwd | grep -v "#" | grep bob | wc -l', 
        '1', False, 5, 'Removed unauthorized user bob'))
    ```
2. Fixing GUI Software & Updates Settings
    ```py
    vulns.append(newCommandObject(
        'cat /etc/apt/apt.conf.d/20auto-upgrades | grep "APT::Periodic::Update-Package-Lists" | grep "1" | wc -l', 
        '1', True, 5, 'Daily updates enabled'))
    ```
3. Checking for a deleted package
    ```py
    vulns.append(newCommandObject(
        'apt list --installed hashcat', 'installed', 
        False, 5, 'Prohibited software hashcat removed'))
    ```
4. Checking for a deleted file
    ```py
    vulns.append(newCommandObject(
        'ls /home/bob/.passwords.txt | wc -l', '0', 
        True, 5, 'Removed plaintext password file'))
    ```

Basically with these checks, you are just trying to find the result and see if it's there or not and modify the newCommandObject based on that.

### Step 5: Encrypting the engine

Now that you have added all the vulns into the engine, you want to encrypt the engine to make sure no one can easily read the vulns.

Encryption process:
Since pyconcrete should already be installed from earlier, you can now run
    ```pyconcrete-admin.py compile --source=/opt/temp --pye pyconcrete engine.pye```
to encrypt the engine.py into a engine.pye.

Now you want to save the original engine.py onto your Google Drive/Github just in case you want to make engine edits later. **There will most likely be bugs with the engine the first time around, so please do this!**

Then, delete the original engine.py from /opt/temp/ to make sure no one can directly see the vulns.

### Step 6: Setting up the engine service

After encrypting the engine, you want to make the engine a service so that it runs forever. (as long as it's not disabled/stopped)

Steps to create the **engine** service:
1. Create file **/lib/systemd/system/engine.service**: ```sudo nano /lib/systemd/system/engine.service```
2. Add contents
    - [Unit]
    - Description=Scoring Engine 
    - After=network.target 
    - StartLimitIntervalSec=0
    - [Service]
    - Type=simple 
    - Restart=always 
    - RestartSec=1 
    - User=root 
    - ExecStart=pyconcrete /opt/temp/engine.pye
    - [Install] 
    - WantedBy=multi-user.target

Contents will look like this

```bash
[Unit]
Description=Scoring Engine
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=root
ExecStart=pyconcrete /opt/temp/engine.pye

[Install]
WantedBy=multi-user.target
```

Finally, enable the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable engine
reboot
sudo systemctl status engine
```

### Step 7: Powering Off and Exporting

After setting up the engine service, the image is basically fully created with the vulnerabilities and the scoring engine.

Now you just want to power it off, export it, then test the image.

After powering off the image, go into the settings and modify the **Memory** to 4 GB and **Processors** to 2 GB. Also modify the image name to the theme of the image.

To export the image, you can just zip up the entire image directory using 7zip.

Then share the image!

### Step 8: Testing the Image

Download, Extract, Run the Image, and test out every vulnerability to make sure the scoring works properly