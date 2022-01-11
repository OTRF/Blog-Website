---
layout: post
current: post
cover:  assets/images/blog/log4shell_environment/
navigation: True
title: 'CVE-2021-44228 Log4Shell: Preparing a Virtual Environment using VirtualBox'
date: 2022-01-11 12:00:00
tags: [Log4Shell,VirtualBox]
class: post-template
subclass: 'post'
author: jose
---

In this blog post, we will share the steps that you can follow to create an virtual environment in VirtualBox (Version 6.1.30) that you can use to simulate an attack that exploits **[CVE-2021-44228](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44228)**.

# Downloading ISO Files

## Ubuntu 20.04.3 LTS - Desktop (Attacker)

- Go to [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop) and download the ISO file by clicking in **Download**.

![](assets/images/blog/log4shell_environment/download_iso_ubuntu_desktop.png)

## Ubuntu 20.04.3 LTS - Server (Tomcat)

- Go to [https://ubuntu.com/download/server](https://ubuntu.com/download/server) and download the ISO file by clicking in **Download Ubuntu Server 20.04.3 LTS**.

![](assets/images/blog/log4shell_environment/download_iso_ubuntu_server.png)

# Creating Virtual Environment - VirtualBox

## NAT (Network Address Translation) Network

- Go to *File > Preferences > Network* and create a NAT network adapter. We are naming it **Log4Shell** and using a CIDR **192.168.50.0/24**. Do not forget to check the **Supports DHCP** option.

![](assets/images/blog/log4shell_environment/virtualbox_nat_network.png)

## Attacker Virtual Machine

- Go to *Machine > New* and create a new virtual machine. We are naming it **attacker** and using type **Linux** and version **Ubuntu (64-bi)**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker.png)

- Select the preferred amount of memory (RAM). We will use 4096 MB.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_ram.png)

- Create a virtual hard disk using default options.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_virtual_hard_disk.png)

- Select the attacker virtual machine and go to *Machine > Settings*. Go to the **Storage** tab and select the **Choose a Disk File**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_add_iso.png)

- Choose the ISO file for Ubuntu Desktop you downloaded before.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_choose_iso.png)

- Go to the **Network** tab and attach the **Log4Shell** network adapter you created before. Click on **OK**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_choose_iso.png)

- Select the attacker virtual machine and go to *Machine > Start*. This will start the Ubuntu virtual machine.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_start_vm.png)

- Select *Install Ubuntu* and continue the installation process using default options.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_install_ubuntu.png)

- Use **attacker** as name, computer's name, and username. Also, you should choose and confirm your password.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_user_password.png)

- Wait for the installation process to complete.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_wait_installation.png)

- When the installation is complete, click on **Restart Now**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_restart_now.png)

- Press **ENTER**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_press_enter.png)

- Click on the user **attacker**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_select_user.png)

- Insert the user's **password**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_insert_password.png)

- Awesome!! You have created the attacker's virtual machine.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_vm_ready.png)

- You can add the following features to your new virtual machine:

    - Devices > Shared Clipboard > Bidirectional
    - Devices > Drag and Drop > Bidirectional
    - Devices > Insert Guest Additions CD Image

- When installing the *Guest Additions CD Image**, you might get the following error: **This system is currently not set up to build kernel modules**

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_error_guest_additions.png)

- Press **enter**, open a **terminal** window, and run the following command and start the installation process again.

```
sudo apt update
sudo apt install build-essential
```

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_install_build.png)

## Tomcat Virtual Machine

- Go to *Machine > New* and create a new virtual machine. We are naming it **tomcat** and using type **Linux** and version **Ubuntu (64-bi)**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker.png)

- Select the preferred amount of memory (RAM). We will use 4096 MB.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_ram.png)

- Create a virtual hard disk using default options.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_virtual_hard_disk.png)

- Select the attacker virtual machine and go to *Machine > Settings*. Go to the **Storage** tab and select the **Choose a Disk File**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_add_iso.png)

- Choose the ISO file for Ubuntu Desktop you downloaded before.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_choose_iso.png)

- Go to the **Network** tab and attach the **Log4Shell** network adapter you created before. Click on **OK**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_choose_iso.png)

- Select the attacker virtual machine and go to *Machine > Start*. This will start the Ubuntu virtual machine.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_start_vm.png)

- Select *Install Ubuntu* and continue the installation process using default options.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_install_ubuntu.png)

- Use **attacker** as name, computer's name, and username. Also, you should choose and confirm your password.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_user_password.png)

- Wait for the installation process to complete.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_wait_installation.png)

- When the installation is complete, click on **Restart Now**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_restart_now.png)

- Press **ENTER**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_press_enter.png)

- Click on the user **attacker**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_select_user.png)

- Insert the user's **password**.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_insert_password.png)

- Awesome!! You have created the attacker's virtual machine.

![](assets/images/blog/log4shell_environment/create_ubuntu_attacker_vm_ready.png)






# References
* [PowerShell script to generate XML files with XPath queries based on OSSEM relationships mapped to ATT&CK and the creation of the file used with the new Azure Sentinel data connector](https://github.com/OTRF/OSSEM-DM/blob/main/scripts/ossemDM_XPath_Queries.ps1)
* [OSSEM Detection Model - GitHub repository](https://github.com/OTRF/OSSEM-DD)
* [OSSEM Project Website](https://ossemproject.com/intro.html)
* [Azure Sentinel To-Go - XML and JSON files](https://github.com/OTRF/Azure-Sentinel2Go/tree/master/azure-sentinel/linkedtemplates/data-collection-rules/rules/ossem-attack)
* [Azure Sentinel To-Go - GitHub repository](https://github.com/OTRF/Azure-Sentinel2Go)