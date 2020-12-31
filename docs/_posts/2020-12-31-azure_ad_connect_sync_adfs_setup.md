---
layout: post
current: post
cover:  False
navigation: True
title: How to Set Up Azure AD Connect to Sync and Federate Custom Domain with On-Prem Directory
date: 2020-12-31 15:00:00
tags: [Azure,ADFS]
class: post-template
subclass: 'post'
author: roberto
---

I deployed a lab environment to learn more about federation access between an "on-prem" lab environment and the cloud. I basically wanted to learn how to federate a custom domain in my Azure AD tenant from my Microsoft 365 subscription with on-prem directory.

In this post, I will show you how to set up [Azure AD connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect) on a domain controller to sync and federate an Azure AD custom domain with on-prem directory. This is done to leverage on-prem Active Directory Federation Services (ADFS) and allow on-prem users to authenticate to cloud services with the same credentials.

## What is Azure AD Connect?

> Azure AD Connect is the Microsoft tool designed to meet and accomplish your hybrid identity goals.

## WHy Use Azure AD Connect?

> Integrating your on-premises directories with Azure AD makes your users more productive by providing a common identity for accessing both cloud and on-premises resources.

## Initialize Azure AD Connect Setup

Click on the Azure AD connect icon after downloading it and installing it your domain controller. You can download and install it with the following PowerShell commands (Remember this is not setting it up. Only installing Azure AD Connect resources)

```PowerShell
Resolve-DnsName download.microsoft.com
$AADConnectDLUrl="https://download.microsoft.com/download/B/0/0/B00291D0-5A83-4DE7-86F5-980BC00DE05A/AzureADConnect.msi"
$exe="$env:SystemRoot\system32\msiexec.exe"

$tempfile = [System.IO.Path]::GetTempFileName()
$folder = [System.IO.Path]::GetDirectoryName($tempfile)

$webclient = New-Object System.Net.WebClient
$webclient.DownloadFile($AADConnectDLUrl, $tempfile)

Rename-Item -Path $tempfile -NewName "AzureADConnect.msi"
$MSIPath = $folder + "\AzureADConnect.msi"

Invoke-Expression "& `"$exe`" /i $MSIPath /qn /passive /norestart"
```

Double-click on the Azure AD Connect Icon on the desktop

![](assets/images/blog/2020-12-31_01_dc_azuread_connect_setup.png)

## Custom Setup

Select the `Customize` option

![](assets/images/blog/2020-12-31_02_dc_azuread_connect_custom.png)

## Required Components

Do not select any options. Just keep the default setup and click `Install`

![](assets/images/blog/2020-12-31_03_dc_azuread_connect_required.png)

![](assets/images/blog/2020-12-31_04_dc_azuread_connect_required.png)

## Select Sign-In Methods

We are going to use our "on-prem" ADFS server as the indentity provider to handle federation services. Therefore, we need to click on `Federation with AD FS`

![](assets/images/blog/2020-12-31_05_dc_azuread_connect_signon_method.png)

## Enter Azure AD Global Admin Creds

![](assets/images/blog/2020-12-31_06_dc_azuread_connect_azuread.png)

## Set Up Sync

Enter "On-Prem" Active Directory

![](assets/images/blog/2020-12-31_07_dc_azuread_connect_sync_ad.png)

## Create Azure AD Sync Account

![](assets/images/blog/2020-12-31_08_dc_azuread_connect_sync_account.png)

![](assets/images/blog/2020-12-31_09_dc_azuread_connect_sync_ad.png)

## Set Up Azure Sign-In to use same creds as our "On-prem" directory

![](assets/images/blog/2020-12-31_10_dc_azuread_connect_sync_signin.png)

## Select OUs to Sync

![](assets/images/blog/2020-12-31_11_dc_azuread_connect_sync_ou.png)

## Identify Users

![](assets/images/blog/2020-12-31_12_dc_azuread_connect_sync_identify_users.png)

## Synchronize All Users

![](assets/images/blog/2020-12-31_13_dc_azuread_connect_sync_all_users.png)

## Skip Optional Features

![](assets/images/blog/2020-12-31_14_dc_azuread_connect_sync_skip_opt.png)

## Provide Creds for On-prem Domain

![](assets/images/blog/2020-12-31_15_dc_azuread_connect_domain_creds.png)

## Choose Existing ADFS Server

![](assets/images/blog/2020-12-31_16_dc_azuread_connect_use_existing_adfs.png)

## Select Azure AD domain to federate with on-prem directory

![](assets/images/blog/2020-12-31_17_dc_azuread_connect_use_domain_to_federate.png)

## Ready to configure

![](assets/images/blog/2020-12-31_18_dc_azuread_connect_ready_configure.png)

## Configuration Complete

![](assets/images/blog/2020-12-31_19_dc_azuread_connect_config_complete.png)

## Verify Federation Connectivity

![](assets/images/blog/2020-12-31_20_dc_azuread_connect_verify_connectivity.png)

## References
* https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect