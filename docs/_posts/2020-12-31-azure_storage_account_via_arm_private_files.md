---
layout: post
current: post
cover:  False
navigation: True
title: How to Create an Azure Storage Account via Azure Resource Manager Templates to Host Private Files
date: 2020-12-31 10:00:00
tags: [Azure,Azure ARM]
class: post-template
subclass: 'post'
author: roberto
---

I recently created an Azure Resource Manager (ARM) template where I needed to install a Trusted Certificate Authority (CA) signed SSL certificate on a Windows server VM at deployment time. I tried to pass it as a base64 blob (`securestring`) to one of the template parameters, but it was still showing in PowerShell logs. Therefore, I decided to use an [Azure Storage Account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview) to host the certificate in a private container and access it via its own [Shared Access Signature (SAS)](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview) string.

In this post, I will show you how to create an Azure storage account via an ARM template, and how you can upload and download files in a secured way.

## What is an Azure Blob Storage?

Azure Blob storage is Microsoft's object storage solution for the cloud. Blob storage offers three types of resources:

* The storage account
* A container in the storage account
* A blob in a container

## What are Azure Storage Accounts?

A storage account provides a unique namespace in Azure for your data. Every object that you store in Azure Storage has an address that includes your unique account name.

## What are Azure Private Containers?

A container organizes a set of blobs, similar to a directory in a file system. A storage account can include an unlimited number of containers, and a container can store an unlimited number of blobs. We can also restrict access to a container by disabling public access to it and granting limited access using shared access signatures (SAS). The specific SAS that we use is an Account SAS.

## What is a Shared Access Signature (SAS)

> A shared access signature (SAS) provides secure delegated access to resources in your storage account

## What is an Account SAS?

An account SAS is secured with the storage account key. An account SAS delegates access to resources in one or more of the storage services.

## Deploying an Azure Account Storage and a Private container

I created an ARM template to automate the whole process:

* Download the following template: https://github.com/hunters-forge/Blacksmith/blob/azure/templates/azure/Storage-Account-Private-Container/azuredeploy.json
* [Install Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
* Run the following command to create an Azure Storage Account and an Azure Private Container in it. Make sure you define your Resource Group, Azure Storage Account Name and Azure Private Container Name

```bash
az group deployment create --resource-group <resourcegroup> --template-file azuredeploy.json --parameters storageAccountName=<name> containerName=<name>
```

* Thats it! If you go to your Azure Portal > Resource Groups > GroupName, you will see the Azure Storage Account resource available.
* One thing to remember is that you can get the Account SAS token by checking your deployment output values. I created the template so that it creates one for you and has it available as part of the output variables. You can refresh that token and get a new one if you feel like it.

## Upload SSL Certificate to Private Container

I could now upload my SSL certificate with the following Azure CLI command:

```bash
az storage blob upload --container-name <container-name> --file <local-filename> --name <filename-on-target> --connection-string <connection-string-SAS-token>
```

## Calling Private Resources from ARM Templates

All we need to do in an ARM templates is use the following URI syntax for every URL that we want to access. I like to set two parameters in my templates:

* **_artifactsLocation**: This is the Account Storage / Container URL (e.g https://name-of-storage-account.blob.core.windows.net/name-of-container/)
* **_artifactsLocationSasToken**: This is the Account SAS Token that you get after deploying your Azure Account Storage and Private container via the ARM template. Go to deployments, select your deployment and look at the Deployment Output values. Do NOT forget to add the `?` character before your SAS token.

**Example**:

```
"[uri(parameters('_artifactsLocation'), concat('private-script.ps1', parameters('_artifactsLocationSasToken')))]"
```

That's it! I hope you enjoyed this short post. I use this setup for several projects already!

## References

* https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview