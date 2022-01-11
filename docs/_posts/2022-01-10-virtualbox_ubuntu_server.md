---
layout: post
current: post
cover:  assets/images/blog/virtualbox_ubuntu_desktop_server/tomcat_ubuntu_machine.png
navigation: True
title: 'VirtualBox: Creating an Ubuntu Server 20.04.3 LTS Virtual Machine'
date: 2022-01-10 13:00:00
tags: [VirtualBox, Ubuntu]
class: post-template
subclass: 'post'
author: jose
---

In this blog post, we will share the steps that you can follow to create an Ubuntu Server virtual machine in VirtualBox (Version 6.1.30). We are using the name **tomcat** for this virtual machine, but you can use your preferred name. Also, for the network adapter, we are using the name **Log4Shell**, but you can use your preferred name.

# Downloading ISO Files

## Ubuntu 20.04.3 LTS - Server (Tomcat)

Go to [https://ubuntu.com/download/server](https://ubuntu.com/download/server) and download the ISO file by clicking in **Download Ubuntu Server 20.04.3 LTS**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/download_iso_ubuntu_server.png)

# Creating Virtual Machine - VirtualBox

## NAT (Network Address Translation) Network

Go to *File > Preferences > Network* and create a NAT network adapter. We are naming it **Log4Shell** and using a CIDR **192.168.50.0/24**. Do not forget to check the **Supports DHCP** option.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/virtualbox_nat_network.png)

## Ubuntu Server Virtual Machine

Go to *Machine > New* and create a new virtual machine. We are naming it **tomcat** and using type **Linux** and version **Ubuntu (64-bit)**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat.png)

Select the preferred amount of memory (RAM). We will use **2048 MB**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_ram.png)

Create a virtual hard disk using default options.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_virtual_hard_disk.png)

Select the tomcat virtual machine and go to *Machine > Settings*. Go to the **Storage** tab and select the **Choose a Disk File**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_add_iso.png)

Choose the ISO file for Ubuntu Server you downloaded before.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_choose_iso.png)

Go to the **Network** tab and attach the **Log4Shell** network adapter you created before. Click on **OK**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_network_adapter.png)

Select the tomcat virtual machine and go to *Machine > Start*. This will start the Ubuntu virtual machine.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_start_vm.png)

Press **enter** and continue the installation process using default options.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_install_ubuntu.png)

Use **tomcat** as the name, server's name, and username. Also, you should choose and confirm your password.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_user_password.png)

Using the **space bar**, select the **Install OpenSSH server** option.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_openssh.png)

Wait for the installation process to complete.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_wait_installation.png)

When the installation is complete, using the tab key select **Reboot Now** and press enter.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_restart_now.png)

Press **ENTER**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_press_enter.png)

Press **enter**, type the user **tomcat** and press enter, finally type the **password** and press enter.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_login.png)

Awesome!! You have created the tomcat's virtual machine.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_vm_ready.png)

## Need to Access your Server Remotely?

Since we installed the **OpenSSH server** when creating our virtual machine, we should be able to access our server using a **SecureShell (SSH)** connection. First, let's check the status of the SSH service and get the IP address of our server by running the following commands:

```
service ssh status

ip a
```

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_ssh_status.png)

We just validated that the SSH service is active or running and our server is listenning on **port 22**. Also, the IP address of our server is **192.168.50.5** .

Let's stablish a SSH remote connection from an Ubuntu Desktop virtual machine that is part of the same network (If you need to create an Ubuntu Desktop virtual machine, you can follow the steps from a previous blog [here](https://blog.openthreatresearch.com/virtualbox_ubuntu_desktop)).

In your Ubuntu Desktop machine, open the **terminal** application and run the following commands (user, IP address, and Password of your server will be required):

```
ssh tomcat@192.168.50.5
```

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_ssh_connection.png)

You should be able to interact with your server from the remote computer through a SSH connection.

## Running Out of Hard Disk Space?

First, **power off** your virtual machine. Then go to *File > Virtual Media Manager* and select the **Hard disks** tab within the **Properties** section. Look for and select the Ubuntu Server's virtual hard disk.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_virtual_media_manager.png)

Update the **size** of the virtual hard doisk based on your requirements. Click on **Apply** and then click on **Close**.

![](assets/images/blog/virtualbox_ubuntu_desktop_server/create_ubuntu_tomcat_virtual_media_manager_update.png)

You can now start your virtual machine again.