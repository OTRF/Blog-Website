---
layout: post
current: post
cover:  assets/images/blog/2020-12-29_11_custom_domain_complete.png
navigation: True
title: Adding a Custom Domain to Microsoft 365
date: 2020-12-28 11:00:00
tags: [DNS, Azure, Microsoft 365]
class: post-template
subclass: 'post'
author: roberto
---

I wanted to set up a federated trust between my on-prem Active Directory (AD) in my lab environment and my Microsoft 365 subscription to allow [federated authentication](https://docs.microsoft.com/en-us/microsoft-365/enterprise/plan-for-directory-synchronization?view=o365-worldwide#federated-authentication) to Office 365 applications. One of the main steps was to add the domain name I used in my on-prem AD lab to the Azure Active Directory (Azure AD) tenant of my Microsoft 365 subscription.

In this post, I will show you how to do that through the Microsoft 365 admin console.

Every new Azure AD tenant comes with an initial domain name, `<domainname>`.onmicrosoft.com. However, you can't change or delete the initial domain name, but you can add new custom domain names.

Once again, if you want on-prem AD user accounts to authenticate to Office 365 apps in the cloud with the same on-prem password, you need to synchronize them with the Azure Active Directory (Azure AD) tenant of your Microsoft 365 subscription which requires you to add the on-prem domain to Microsoft 365.

## What is Microsoft 365?

> Microsoft 365 is an integrated bundle of Windows 10, Office 365 and Enterprise Mobility + Security (aka EMS, which includes Intune device management, analytics and some Azure Active Directory capabilities), sold on a subscription basis.

## What is Office 365?

> It is a set of cloud based business applications like Exchange, Office Apps, SharePoint, OneDrive. It is a part of Microsoft 365.

## Quick Recipe

* Purchase Domain
* Access Microsoft 365 admin console
* Add custom domain
* Verify domain ownership
* Connect domain to MS 365 Services (Optional)

## 1. Purchase Domain

You need to use a domain that you already own or purchase one. If this is for a lab environment, most likely you will have to buy one. I purchased mine from [namecheap.com](https://www.namecheap.com/)

![](assets/images/blog/2020-12-29_01_purchase_domain.png)

## 2. Access Microsoft 365 Admin Console

* Go to [https://admin.microsoft.com](https://admin.microsoft.com)
* Go to Settings > Domains

## 3. Add Custom Domain

Click on `Add Domain` as shown in the image below

![](assets/images/blog/2020-12-29_02_current_domains.png)

Enter the name of the domain you just purchased or you already own

![](assets/images/blog/2020-12-29_03_add_custom_domain.png)

## 4. Verify Domain Ownership

You will need to proof that you own the domain. I usually choose the verification option to add a TXT record to the DNS records of my domain.

![](assets/images/blog/2020-12-29_04_verify_domain.png)

**Add TXT Records to Domain Settings**

* Log on to your domain provider console
* Select domain DNS settings
* Add TXT record

![](assets/images/blog/2020-12-29_05_verify_domain.png)

![](assets/images/blog/2020-12-29_06_update_domain_records.png)

## 5. Connect Domain to MS 365 Services (Optional)

Next, you will have the option to attach specific Microsoft 365 services (i.e. Exchange) to your domain.

![](assets/images/blog/2020-12-29_07_connect_ms_services.png)

**Add Additional DNS Records**

![](assets/images/blog/2020-12-29_08_additional_dns_records.png)

![](assets/images/blog/2020-12-29_09_additional_dns_records.png)

![](assets/images/blog/2020-12-29_10_additional_dns_records.png)

That's it! You have successfully added a custom domain to your Microsoft 365 subscription.

![](assets/images/blog/2020-12-29_11_custom_domain_complete.png)

![](assets/images/blog/2020-12-29_12_current_domains.png)

One thing you can do is check the Azure Active Directory (Azure AD) tenant of your Microsoft 365 subscription, and you will now see the custom domain there and verified. Go to [https://aad.portal.azure.com](https://aad.portal.azure.com)

![](assets/images/blog/2020-12-29_13_ad_custom_domains.png)

I hope you enjoyed this post!

## References

* https://docs.microsoft.com/en-us/microsoft-365/admin/setup/add-domain?view=o365-worldwide
* https://admin.microsoft.com
* https://www.namecheap.com/
* https://www.onmsft.com/feature/whats-the-difference-between-office-365-and-microsoft-365
* https://www.zdnet.com/article/what-is-microsoft-365-microsofts-most-important-subscription-bundle-explained/
* https://docs.microsoft.com/en-us/microsoft-365/admin/get-help-with-domains/what-is-a-domain?view=o365-worldwide
* https://docs.microsoft.com/en-us/microsoft-365/enterprise/plan-for-directory-synchronization?view=o365-worldwide#federated-authentication