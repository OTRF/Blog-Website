---
layout: post
current: post
cover:  assets/images/blog/virtualbox_ubuntu_desktop_server/attacker_ubuntu_machine.png
navigation: True
title: 'VirtualBox: Creating an Ubuntu Desktop 20.04.3 LTS Virtual Machine'
date: 2022-01-10 12:00:00
tags: [VirtualBox, Ubuntu]
class: post-template
subclass: 'post'
author: jose
---

In this blog post, we will share the steps that you can follow to create an Ubuntu Desktop virtual machine in VirtualBox (Version 6.1.30). We are using the name **attacker** for this virtual machine, but you can use your preferred name. Also, for the network adapter, we are using the name **Log4Shell**, but you can use your preferred name.

# Downloading ISO Files

## Ubuntu 20.04.3 LTS - Desktop (Attacker)

Go to [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop) and download the ISO file by clicking in **Download**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/download_iso_ubuntu_desktop.png)

# Creating Virtual Machine - VirtualBox

## NAT (Network Address Translation) Network

Go to *File > Preferences > Network* and create a NAT network adapter. We are naming it **Log4Shell** and using a CIDR **192.168.50.0/24**. Do not forget to check the **Supports DHCP** option.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/virtualbox_nat_network.png)

## Ubuntu Desktop Virtual Machine

Go to *Machine > New* and create a new virtual machine. We are naming it **attacker** and using type **Linux** and version **Ubuntu (64-bit)**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker.png)

Select the preferred amount of memory (RAM). We will use **4096 MB**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_ram.png)

Create a virtual hard disk using default options.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_virtual_hard_disk.png)

Select the attacker virtual machine and go to *Machine > Settings*. Go to the **Storage** tab and select the **Choose a Disk File**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_add_iso.png)

Choose the ISO file for Ubuntu Desktop you downloaded before.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_choose_iso.png)

Go to the **Network** tab and attach the **Log4Shell** network adapter you created before. Click on **OK**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_network_adapter.png)

Select the attacker virtual machine and go to *Machine > Start*. This will start the Ubuntu virtual machine.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_start_vm.png)

Select **Install Ubuntu** and continue the installation process using default options.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_install_ubuntu.png)

Use **attacker** as the name, computer's name, and username. Also, you should choose and confirm your password.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_user_password.png)

Wait for the installation process to complete.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_wait_installation.png)

When the installation is complete, click on **Restart Now**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_restart_now.png)

Press **ENTER**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_press_enter.png)

Click on the user **attacker**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_select_user.png)

Insert the user's **password**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_insert_password.png)

Awesome!! You have created the attacker's virtual machine.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_vm_ready.png)

If necessary, you can add the following features to your new virtual machine:

    - Devices > Shared Clipboard > Bidirectional
    - Devices > Drag and Drop > Bidirectional
    - Devices > Insert Guest Additions CD Image

## Problems when Installing the Guest Additions CD Image?

You might get the following error: **This system is currently not set up to build kernel modules**

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_error_guest_additions.png)

Press **enter**, open a **terminal** window, and run the following commands:

```
sudo apt update
sudo apt install build-essential
```

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_install_build.png)

Now, you can start the installation process again.

## Running Out of Hard Disk Space?

First, **power off** your virtual machine. Then go to *File > Virtual Media Manager* and select the **Hard disks** tab within the **Properties** section. Look for and select the Ubuntu Desktop's virtual hard disk.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_virtual_media_manager.png)

Update the **size** of the virtual hard doisk based on your requirements. Click on **Apply** and then click on **Close**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_attacker_virtual_media_manager_update.png)

You can now start your virtual machine again.