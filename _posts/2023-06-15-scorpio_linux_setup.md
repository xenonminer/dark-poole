---
layout: post
title: Scorpio Linux Setup
---

# Description

In this blog post, you will learn how to setup the [Scorpio Scoring Engine](https://github.com/Cybertron51/scorpio) for Linux OSes.

## What is Scorpio?

Scorpio is a Python-based scoring engine created to score image vulnerabilities. Scorpio checks if a vulnerability has been fixed and gives live feedback to the students.

## How does Scorpio work?

The image creator (you) will implement vulnerabilities onto a virtual machine and record them onto **engine.py** which runs every 30 seconds. As the students secure the system, the scoring engine will update **Template.html** (Scoring Report) to display the fixed vulnerabilities.

# Instructions for setup

If you have already created the image and its vulns or only want to know how to set up the Scorpio Engine for Linux, [jump directly to Scorpio Setup](#step-3-cloning-scorpio-repository-into-the-system--setting-up-directories).

## Tools

Before you get started, install these tools:
- python2: ```sudo apt install python2 -y```
- pip: ```sudo apt install python3-pip -y```
- pyconcrete: ```sudo PYCONCRETE_PASSPHRASE=password_here pip install pyconcrete```
- git: ```sudo apt install git -y```

## Steps

### Step 1: Open/Create a Sandbox Virtual Machine

To get started, first create your base image. You can grab free sandbox images at [linuxvmimages](https://www.linuxvmimages.com/). Since this post is meant for Linux and CyberPatriot, grab a [Ubuntu 20.04 Image](https://www.linuxvmimages.com/images/ubuntu-2004/) or a [Ubuntu 22.04 Image](https://www.linuxvmimages.com/images/ubuntu-2204/). Make sure to grab the Vmware image and **NOT** the VirtualBox.

Extract the image. After extracting, open the **.vmx** file by double-clicking it. Or, go into Vmware, click **Open a Virtual Machine**, then select the **.vmx** file to open.

Then run the image by clicking the play button.

### Step 2: Setting up/Implementing Vulnerabilities in the Image

Now that you have the sandbox image open, you want to implement all your vulnerabilities in the image.

Here's 4 examples of vulnerabilities you can set up.

1. Adding an unauthorized user onto the system
    - ```sudo adduser username```
    - Example: ![unauthuseradd](https://github.com/xenonminer/xenonminer.github.io/assets/46347858/7b540450-2b31-4f69-b3cf-68cd9eb850e6)
2. Modifying the GUI Software & Updates Settings
    - Example: ![guisandu](https://github.com/xenonminer/xenonminer.github.io/assets/46347858/0747bcfb-0e37-473d-b0d7-3699c728ad17)
3. Installing a malicious package
    - ```sudo apt install package```
    - Example: ![hashcatinstall](https://github.com/xenonminer/xenonminer.github.io/assets/46347858/2facee6e-ebef-45c1-8fa1-732b31ff6798)
4. Adding a malicious file
    - ```touch /path/to/random/directory/file```
    - Example: ![touchfile](https://github.com/xenonminer/xenonminer.github.io/assets/46347858/6b8b7e42-2a37-4d52-bf5e-bd4716cc2741)

After creating the vulnerabilities, you want to create the **README.pdf**, Forensic Questions, and the **Set Name for Scoring Report** files. Create these files on the Desktop of your main user.

The README can be written in Google Docs. [Here is an example README](https://docs.google.com/document/d/1M4mljp9yEzb5GLsibm0Z7RqTzpiPQalwZlfn4i8nixo). After creating the README, download the README as a **pdf** onto the system and rename it to **README.pdf**. Then move it onto the Desktop.

```bash
mv ~/Downloads/README_download ~/Desktop/README.pdf
```

The Forensic Question files should be in the format "Forensics_#.txt". Create them using the command ```touch Forensics_#.txt```. The accepted format is as follows:

```
(Example: Example_Answer)

Question: Question_Here

Answer: Answer_Here
```

Also, create the "Set Name for Scoring Report" on the Desktop of the main user: ```touch "Set Name for Scoring Report"```. The accepted format is as follows:
```
YOUR FULL NAME: [id]
```

Other things you want to configure are:
- Your main user's **.bash_history file** and root's **.bash_history** file. This clears your bash history permanently so students can't figure out what commands you ran.
  - To do this with example user ```joe```, run the commands
    ```bash
    rm -rf /home/joe/.bash_history; ln -s /dev/null /home/joe/.bash_history; sudo rm -rf /root/.bash_history; sudo ln -s /dev/null /root/.bash_history
    ```
- Autologin for your main user with gdm3
  - To set this up with ```joe```, edit the **/etc/gdm3/custom.conf**
  Then edit lines to look like the following:
  ```bash
  AutomaticLoginEnable=True
  AutomaticLogin=joe
  ``` 
  Make sure the lines are uncommented.

### Step 3: Cloning Scorpio Repository into the system + Setting up directories

After adding all the vulnerabilities, you can start setting up the engine.

First, clone the engine repository: ```git clone https://github.com/Cybertron51/scorpio.git```.

Then, this repository will need to be in **/opt** with the name **temp**: ```sudo mv scorpio /opt/temp```

### Step 4: Adding vulnerabilities to the engine

After you have added the vulnerabilities, you want to configure them to be checked in the actual engine.

Navigate to **/opt/temp/** and open up **engine.py**.

The lines you want to change are the ones that look like: 
```py
vulns.append(newCommandObject(
    'cat /home/kaisa/Desktop/Forensics_1.txt', 'Valoran City Park', 
    True, 6, 'Forensics Question 1 correct'))
```

These lines add a vuln object with specific parameters. Going left to right the parameters are:
- The command to check the vuln
- What to check for in the result of the command
- If the command was checked for that value, put True. If it was checked for not being that value, put False.
- Number of points assigned for that vulnerability
- Scoring Report message

The 4 vulns from earlier would look like this:
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

With these checks, you are trying to find the result to see if it's there and modify the ```newCommandObject``` based on that.

### Step 5: Encrypting the engine

Now, you want to encrypt the engine to make sure no one can read the vulns. Since pyconcrete should already be installed, you can now run:
```py
pyconcrete-admin.py compile --source=/opt/temp --pye pyconcrete engine.pye
```
to encrypt the **engine.py** into a **engine.pye**.

Now you want to save the original engine.py onto your Google Drive/Github just in case you want to make engine edits later. **There will most likely be bugs with the engine the first time around, so please do this!**

Then, delete the original **engine.py** from **/opt/temp/** to make sure no one can directly see the vulns.

### Step 6: Setting up the engine service

After encrypting the engine, you want to make the engine a service so that it runs forever (as long as it's not disabled/stopped).

Steps to create the **engine** service:
1. Create file ```/lib/systemd/system/engine.service```: ```sudo nano /lib/systemd/system/engine.service```
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

To export the image, you can just zip up the entire image directory using **7zip**.

### Step 8: Testing the Image

Download, Extract, Run the Image, and test out every vulnerability to make sure the scoring works properly.

Good luck!
