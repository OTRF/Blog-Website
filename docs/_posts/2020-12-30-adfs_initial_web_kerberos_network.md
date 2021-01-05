---
layout: post
current: post
cover:  assets/images/blog/2020-12-30_16_adfs_client_enter_creds.png
navigation: True
title: Exploring ADFS Initial Web Traffic and Kerberos Authentication via Fiddler and Wireshark
date: 2020-12-30 11:00:00
tags: [ADFS, Fiddler, Wireshark]
class: post-template
subclass: 'post'
author: roberto
---

I recently wanted to learn more about the internals of Active Directory Federation Services (ADFS) and created an Azure Resource Manager (ARM) [template](https://github.com/OTRF/Blacksmith/tree/master/templates/azure/Win10-AD-ADFS) to deploy a basic lab environment. As part of my initial research process, I wanted to understand how a user got authenticated before getting an authentication token to access a cloud service. Thefore, I figured it would be good to document some of it by inspecting the network traffic it generates.

In this post, I will show you how to use [fiddler](https://www.telerik.com/fiddler) and [Wireshark](https://www.wireshark.org/) to inspect the network traffic generated while accessing and providing credentials to the default ADFS Sign On page via the Intranet. This basic example does not require to have a SAML Service Provider (Relatying Party Trust) set up since, once again, I am interested in the process before getting an authentication token.

## Requirements
* An Active Directory (AD) Environment with a Domain Controller (DC)
* An Active Directory Federation Service (ADFS) server
  * A federation server farm set up
* A domain joined computer
* [Optional] You can deploy all the above in less than 30 minutes with [this ARM template](https://github.com/OTRF/Blacksmith/tree/master/templates/azure/Win10-AD-ADFS) from the project [Blacksmith](https://github.com/OTRF/Blacksmith).
* On the Domain Joined Computer:
    * Install [Fiddler Classic](https://www.telerik.com/download/fiddler)
    * Install [Fiddler extension](https://github.com/dotnet/Kerberos.NET/releases) based on the [Kerberos.NET](https://github.com/dotnet/Kerberos.NET) library : Download `setup.exe` and run it.
    * Install [Wireshark](https://www.wireshark.org/download.html)
* On the ADFS Server:
    * Install [Wireshark](https://www.wireshark.org/download.html)

Once you have all the requirements above taken care of, I recommend to validate your ADFS setup.

## Validate ADFS Server Setup (Server Side)
* RDP to ADFS Server
* Check if the ADFS serve is running
```PowerShell
Get-Service adfssrv
```
* Check ADFS properties
```PowerShell
Get-AdfsProperties
```

![](assets/images/blog/2020-12-30_01_adfs_server_check_properties.png)

* You can get the ADFS Service `HostName` by running the following command (You will need that value in the next section)
```PowerShell
Get-AdfsProperties | select-object HostName
```

* Check ADFS servers available and certificates

![](assets/images/blog/2020-12-30_02_adfs_server_check_adfs_servers.png)

* We are going to be using the ADFS Sign On page via the Intranet so we also need to check if that feature is enabled. We do it by verifying if the ADFS property `EnableIdpInitiatedSignonPage` is set to `True`
```PowerShell
Get-AdfsProperties | select-object EnableIdpInitiatedSignonPage
```

* If property is set to `False`, run this command to enable it and restart the ADFS service
```PowerShell
Set-AdfsProperties -EnableIdpInitiatedSignonPage $true
Restart-Service -Name adfssrv
``` 

* In order to avoid continuous credentials prompt while authenticating and capturing/proxying traffic with fiddler, we need to disable the `Extended Protection for Authentication` property in the ADFS server.
```PowerShell
Get-AdfsProperties | select ExtendedProtectionTokenCheck
Set-ADFSProperties -ExtendedProtectionTokenCheck:None
Restart-Service -Name adfssrv
Get-AdfsProperties | select ExtendedProtectionTokenCheck
```

![](assets/images/blog/2020-12-30_07_adfs_server_disable_epa.png)

## Validate ADFS Server Setup (Client Side)
From a domain joined computer, we can check the ADFS server setup by exploring the federation server service metadata

* RDP to domain joined endpoint
* Browse to `https://<ADFS HostName>/adfs/fs/federationserverservice.asmx` (replace `<ADFS HostName>` with the `HostName` value of the ADFS service obtained in the previous section)

![](assets/images/blog/2020-12-30_03_adfs_client_check_server_metadata.png)

Another easy way to validate that the federation service is running an handling kerberos authentication is by browsing to `https://<ADFS HostName>/adfs/ls/IdpInitiatedSignon.aspx` and entering domain credentials (replace `<ADFS HostName>` with the `HostName` value of the ADFS service obtained in the previous section)

![](assets/images/blog/2020-12-30_04_adfs_client_check.png)

* If everything is working properly (at least on-prem via the intranet), you will get the `You are signed in` message

![](assets/images/blog/2020-12-30_05_adfs_client_check.png)

That was very easy right? Now, let's set up our network traffic captures.

## Client Network Capture Setup
* Log on (if you disconnected) to domain joined computer
* Open fiddler
* Enable `Decrypt HTTPS traffic` option in Tools > Options > HTTPs
* Trust the fiddler root certificate

![](assets/images/blog/2020-12-30_08_adfs_client_https_options.png)

* I used Microsoft Edge for this blog post. Therefore, We need to enable Microsoft Edge to send network traffic to the local computer (AppContainer Loopback Exemption). WinConfig > Microsoft Edge > Save Changes

![](assets/images/blog/2020-12-30_09_adfs_client_appcontainer.png)

* Make sure you are capturing traffic and that the `Kerberos` extension is available under the `Inspectors` options. You can simply open Microsoft Edge and go to any site (just to generate traffic and not to validate Kerberos parsers)

![](assets/images/blog/2020-12-30_10_adfs_client_capture_extension.png)

* Close browser and remove all sessions captured by Fiddler. Getting ready!

![](assets/images/blog/2020-12-30_11_adfs_client_fiddler_remove_sessions.png)

* Open Edge from Fiddler and Filter Web Browser Only

![](assets/images/blog/2020-12-30_13_adfs_client_fiddler_browser.png)

* Filter traffic to only capture from web browsers

![](assets/images/blog/2020-12-30_14_adfs_client_fiddler_browser.png)

* Open Wireshark and apply the following filter:

`ip.addr == <DC IP Address> or ip.addr == <ADFS IP Address>`

![](assets/images/blog/2020-12-30_12_client_wireshark_filter.png)

## ADFS Server Network Capture Setup
* Open Wireshark and apply the following filter:

`ip.addr == <DC IP Address> or ip.addr == <Domain Joined Computer IP Address>`

![](assets/images/blog/2020-12-30_12_adfs_server_wireshark_filter.png)

## Capture Network Traffic
* Once again, on the client, browse to `https://<ADFS FQDN>/adfs/ls/IdpInitiatedSignon.aspx` and click on `Sign In`

![](assets/images/blog/2020-12-30_15_adfs_client_signon_page.png)

* Enter Domain Credentials

![](assets/images/blog/2020-12-30_16_adfs_client_enter_creds.png)

* You will see a few events captured by fiddler. I recommend to filter the results on the specific `microsoftedgecp` process that handled the authentication as shown below:

![](assets/images/blog/2020-12-30_17_adfs_client_filter_traffic.png)

* You will also see a few events in Wireshark (both client and server). I am interested in the `KRB5` protocol ones as shown below

![](assets/images/blog/2020-12-30_18_client_server_krb5_traffic.png)

* Stop fiddler and wireshark captures. Time to learn a little bit more about this process

## Inspect Network Traffic

### 1. Access to ADFS Sign On Page
Client Connects to the ADFS server via `https://adfs.solorigatelabs.com/adfs/ls/idpinitiatedsignon.aspx ` . Here is where we get to the Open Threat Research banner with the option to `Sign In` and the message `You are not signed in. Sign in to this site`. Nothing special. Just a simple GET request to get to ADFS sign on page.

![](assets/images/blog/2020-12-30_19_adfs_client_fiddler_otr_banner.png)

### 2. Redirection to ADFS Server and SamlRequest Generation
Client clicks on the `Sign In` button. That click makes a `POST` request to `POST https://adfs.solorigatelabs.com/adfs/ls/idpinitiatedsignon.aspx?client-request-id=6717a4e4-50f7-4655-7700-0080000000d5`.

The ADFS server receives the requests and returns a `302 Response Code` and it sets the new `Location` to `https://adfs.solorigatelabs.com:443/adfs/ls/wia?client-request-id=6717a4e4-50f7-4655-7700-0080000000d5` where we can see the use of Windows Integrated Authentication (WIA) which is used for authentication requests that occur within the organization's internal network (intranet) for any application that uses a browser for its authentication.

![](assets/images/blog/2020-12-30_20_adfs_client_samlrequest_302.png)

One thing also that was provided in the 302 Redirect Message was a `MSISSamlRequest` value inside of a cookie which contained the `SamlRequest` (AuthnRequest). A SAML Request is usually sent by the SAML service before the user is redirected to the Identify Provider (ADFS Server itself). The identity provider then decodes the SAML request and gets ready to authenticate the user.

This was the `MSISSamlRequest` value inside of the cookie (Base64 Encoded)

```Block
QmFzZVVybD1odHRwcyUzYSUyZiUyZmFkZnMuc29sb3JpZ2F0ZWxhYnMuY29tJTJmYWRmcyUyZmxzJTJmXFNBTUxSZXF1ZXN0PWZaRkJhNFF3RUlYJTJmU3NoZEUyM1ZOYmlDVkFwQ2UlMmJtV0hub3BNY1p1SUNZMms1VCUyYiUyZkVZOWxpN002ZkhldlBtWUJ2aWlWOVlGZnpVdjhpdEk4R2pveiUyZmpqUkxOVHpzY3B5U2JCayUyZnVNRjBsZDB6R1pxckVveXJJcTZUeGo5Q1lkS0d2T09FOHBSZ05Ba0lNQno0MlBFczJ6aE1ZcFhtbkY2QjNMYUVyciUyZkIyalByWW93JTJmMmV2SHElMmZBaU9FVHpPa1lMVjE2cE43cWZrSXFiRExyaE1OQktNSGEwQnVtNE16ekhKUXdBeGZKREF2MktWN2ZtTHhDQ1lPRXdzR1ZpblVyT1NFMGMlMmJpRGJBZDlYWjZkZFpiWVRWdW14M0dIZEhiSVE0ZzNRYUQydzBtc25UOTQlMmJWZmxtaiUyYlZrSUM4UzZBYjhoUjFEYms3eXZhWHclM2QlM2RcUHJvdG9jb2xCaW5kaW5nPXVybiUzYW9hc2lzJTNhbmFtZXMlM2F0YyUzYVNBTUwlM2EyLjAlM2FiaW5kaW5ncyUzYUhUVFAtUmVkaXJlY3RcU2lnbmF0dXJlPVJtQlJxa1ZDYjB0cUtPeEFSR1BTJTJiUFFGS3JXVkUyTEM0JTJiJTJiR2tMZENSVmVXUkFJdTElMmZGeUFYWW10R3d3WVhlS2VOYVBMMjdnUExmSWVWanltYyUyYmdlWmhXOUV4UFdkQUM1djlYUEtmNzFSQ29wNE45Y0ZodWV2M1FGY09vV0E0Z2d2NU1sY3VLMXl4eGVOZDJXQ0JFc2VDSFE5WCUyZmlCSWg2ZldxeXNKWjlrRlBOSkMlMmJkWWZ5UFpPTkpqRWRYUUViTHVueWVwM0Y4bkpUbFBUdjBMZ04ydWRKdWxpdzhORm0lMmIyNDR3OFdlNmVBOFJGYURYU2FaSDA3Rm1pdkxPNTQ2MDV4WDg0djh4bWUlMmJQV0xhRkU3U3N0aEtiSjloJTJidk4lMmJ3MURQenR1d3IxVng5NTFVQUNJMkhJdUdYT0N2WFVHendVWFVyNGRlWHdtMlBMYkVDckZ4OGclM2QlM2RcU2lnQWxnPWh0dHAlM2ElMmYlMmZ3d3cudzMub3JnJTJmMjAwMSUyZjA0JTJmeG1sZHNpZy1tb3JlJTIzcnNhLXNoYTI1NlxRdWVyeVN0cmluZ0hhc2g9OWkzZmpUcHFxY1RlUDl3MktBYk5HZmM4RHdnV0k1ZmZXaE5SM0lrMm1IYyUzZA==
```

![](assets/images/blog/2020-12-30_20_adfs_client_samlrequest_strings.png)

After base64 decoding that value, we get to the Url encoded value of `BaseUrl` which contains the `SamlRequest`

```Block
BaseUrl=https%3a%2f%2fadfs.solorigatelabs.com%2fadfs%2fls%2f\SAMLRequest=fZFBa4QwEIX%2fSshdE23VNbiCVApCe%2bmWHnopMcZuICY2k5T%2b%2fEY9li7M6fHevPmYBviiV9YFfzUv8itI8Gjoz%2fjjRLNTzscpySbBk%2fuMF0ld0zGZqrEoyrIq6Txj9CYdKGvOOE8pRgNAkIMBz42PEs2zhMYpXmnF6B3LaErr%2fB2jPrYow%2f2evHq%2fAiOETzOkYLV16pN7qfkIqbDLrhMNBKMHa0Bum4MzzHJQwAxfJDAv2KV7fmLxCCYOEwsGVinUrOSE0c%2biDbAd9XZ6ddZbYTVumx3GHdHbIQ4g3QaD2w0msnT94%2bVflmj%2bVkIC8S6Ab8hR1Dbk7yvaXw%3d%3d\ProtocolBinding=urn%3aoasis%3anames%3atc%3aSAML%3a2.0%3abindings%3aHTTP-Redirect\Signature=RmBRqkVCb0tqKOxARGPS%2bPQFKrWVE2LC4%2b%2bGkLdCRVeWRAIu1%2fFyAXYmtGwwYXeKeNaPL27gPLfIeVjymc%2bgeZhW9ExPWdAC5v9XPKf71RCop4N9cFhuev3QFcOoWA4ggv5MlcuK1yxxeNd2WCBEseCHQ9X%2fiBIh6fWqysJZ9kFPNJC%2bdYfyPZONJjEdXQEbLunyep3F8nJTlPTv0LgN2udJuliw8NFm%2b244w8We6eA8RFaDXSaZH07FmivLO54605xX84v8xme%2bPWLaFE7SsthKbJ9h%2bvN%2bw1DPztuwr1Vx951UACI2HIuGXOCvXUGzwUXUr4deXwm2PLbECrFx8g%3d%3d\SigAlg=http%3a%2f%2fwww.w3.org%2f2001%2f04%2fxmldsig-more%23rsa-sha256\QueryStringHash=9i3fjTpqqcTeP9w2KAbNGfc8DwgWI5ffWhNR3Ik2mHc%3d
```

After Url decoding the `baseUrl` value, we get the following value:

```Block
https://adfs.solorigatelabs.com/adfs/ls/\SAMLRequest=fZFBa4QwEIX/SshdE23VNbiCVApCe+mWHnopMcZuICY2k5T+/EY9li7M6fHevPmYBviiV9YFfzUv8itI8Gjoz/jjRLNTzscpySbBk/uMF0ld0zGZqrEoyrIq6Txj9CYdKGvOOE8pRgNAkIMBz42PEs2zhMYpXmnF6B3LaErr/B2jPrYow/2evHq/AiOETzOkYLV16pN7qfkIqbDLrhMNBKMHa0Bum4MzzHJQwAxfJDAv2KV7fmLxCCYOEwsGVinUrOSE0c+iDbAd9XZ6ddZbYTVumx3GHdHbIQ4g3QaD2w0msnT94+Vflmj+VkIC8S6Ab8hR1Dbk7yvaXw==
```

We can then take the deflated base64 encoded SAML Message inside of the `SamlRequest` and decode and inflate it.

```Block
<samlp:AuthnRequest ID="_80182abd-1dca-41a5-990b-d7b5566760ff" Version="2.0" IssueInstant="2021-01-05T07:03:10.092Z" Destination="https://adfs.solorigatelabs.com/adfs/ls/" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"><Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://ADFS.solorigatelabs.com/adfs/services/trust</Issuer></samlp:AuthnRequest>
```

This message contains details needed by the ADFS server (Identity Provider) to process the user authentication. You can also decode all those string with tools from here [https://www.samltool.com/online_tools.php](https://www.samltool.com/online_tools.php).

**Recipe:**
* Base64 Decode
* URL Decode
* Base64 Decode + Inflate

### 3. Enter Credentials in Logon Form
After the previous redirection, the client receives a `401 Response Code`. This is simply telling the client to authenticate against the ADFS server (Identity Provider). Here is where we entered our domain credentials `User:pgustavo`.

![](assets/images/blog/2020-12-30_21_adfs_client_401_login_form.png)

### 4. Kerberos Authentication
The Identity Provider decodes the `SAMLRequest` and performs the user authentication via Kerberos.

**AS-REQ** (Client -> Domain Controller
The client first sends a request to the Kerberos Authentication Service (AS) to get TGT.

* Client-Principal: pgustavo
* Principal Service: krbtgt/SOLORIGATELABS

![](assets/images/blog/2020-12-30_22_client_wireshark_asreq.png)

**PREAUTH_REQUIRED** (Domain Controller -> Client)
>Kerberos Pre-Authentication is a security feature which offers protection against password-guessing attacks. The AS request identifies the client to the KDC in Plaintext. If Kerberos Pre-Authentication is enabled, a Timestamp will be encrypted using the user's password hash as an encryption key. If the KDC reads a valid time when using the user's password hash, which is available in the Microsoft Active Directory, to decrypt the Timestamp, the KDC knows that request isn't a replay of a previous request.

Error Code: 25 - HEX -> 0x19 ([Source](https://ldapwiki.com/wiki/Kerberos%20Error%20Codes))

![](assets/images/blog/2020-12-30_22_client_wireshark_required_auth.png)

**AS-REQ** (Client -> Domain Controller)
The client sends another request to the Kerberos Authentication Service (AS), but with the encrypted timestamp this time.

![](assets/images/blog/2020-12-30_22_client_wireshark_asreq2.png)

**AS-RESP** (Domain Controller -> Client)
Client receives a response with a TGT for user pgustavo. This is just to request a service ticket to access the ADFS service.

![](assets/images/blog/2020-12-30_22_client_wireshark_asresp.png)

**TGS-REQ** (Client -> Domain Controller)
Client requests access to `adfs.solorigatelabs.com` to the Ticket Granting Server (TGS)

![](assets/images/blog/2020-12-30_22_client_wireshark_tgsreq.png)

**TGS-REP** (Domain Controller -> Client)
TGS responds with a service ticket to the `adfs.solorigatelabs.com` (HTTP)

![](assets/images/blog/2020-12-30_22_client_wireshark_tgsresp.png)

**AS-REQ** (ADFS -> Domain Controller)
After pgustavo enters its credentials, the ADFS server handles the authentication process and sends a request to the Kerberos Authentication Service (AS) on behalf of pgustavo.

* User being authenticated: **pgustavo**
* Service requested: **krbtgt**
* Host Address: **ADFS01 Server**

![](assets/images/blog/2020-12-30_22_adfs_server_wireshark_asreq.png)

**PREAUTH_REQUIRED** (Domain Controller -> ADFS)

![](assets/images/blog/2020-12-30_23_adfs_server_wireshark_preauth.png)

**S4U2self TGS Exchange (Delegation) - TGS-REQ** (ADFS -> Domain Controller)
S4U2Self delegation sub-protocol is used by the ADFS service to obtain a ticket on behalf of `pgustavo`

> In a KRB_TGS_REQ and KRB_TGS_REP subprotocol message sequence, a Kerberos principal uses its ticket-granting ticket (TGT) to request a service ticket to a service. The TGS uses the requesting principal's identity from the TGT passed in the KRB_TGS_REQ message to create the service ticket.

> In the S4U2self TGS exchange subprotocol extension, a service requests a service ticket to itself on behalf of a user. The user is identified to the KDC by the user name and user realm. Alternatively, the user might be identified using the user's certificate.

`The service uses its own TGT and adds a new type of padata`

> If the service possesses the user certificate, it can obtain a service ticket to itself on that user's behalf using the S4U2self TGS exchange subprotocol extension, with a new padata type PA-S4U-X509-USER (ID 130)

![](assets/images/blog/2020-12-30_24_adfs_server_wireshark_s4u2self.png)

![](assets/images/blog/2020-12-30_25_adfs_server_wireshark_s4u2self.png)

**S4U2self TGS Exchange (Delegation) - TGS-REP** (Domain Controller -> ADFS)
The ADFS service gets a ticket with the username `pgustavo` and the service name mapped to the `adfsuser` account.

![](assets/images/blog/2020-12-30_26_adfs_server_wireshark_s4u2self.png)

**AP-REQ** (Client -> ADFS)
After pgustavo is authenticated via Kerberos, it then sends a kerberos ticket in the authorization header (Negotiate). This is parsed by the Fiddler `Kerberos` extension.

![](assets/images/blog/2020-12-30_27-adfs_client_fiddler_kerberos_apreq.png)

### 4. Successful Authentication & Authentication Cookies
A final redirection occurs since we are simply authenticating against the ADFS server. Remember there are no service providers set up.

![](assets/images/blog/2020-12-30_28_adfs_client_fiddler_redirection.png)

We also finally get "Authentication Cookies" such as `MSISAuth`

> The MSISAuth (MSISAuth + MSISAuth1 + …) are the encrypted cookies used to validate the SAML assertion produced for the client. The cookie is used for subsequent authentications against the ADFS. These are what we call the “authentication cookies”, and you will see these cookies ONLY when AD FS 2.0 is the IdP. Without these, the client will not experience SSO.

![](assets/images/blog/2020-12-30_30_adfs_client_browser_signed_in.png)

## Final Diagram!
After going through all those steps, I put together this image to summarize the main steps. This is going to help me as a reference for when I add service providers.

![](assets/images/blog/2020-12-30_31_adfs_idpinitiated_signon_flow.png)

That's it! I hope you enjoyed this post and helps you as a reference for other projects!

## References

* https://syfuhs.net/a-fiddler-extension-for-kerberos-messages
* https://github.com/dotnet/Kerberos.NET
* https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-initiatedsignon
* https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/technical-reference/understanding-key-ad-fs-concepts
* https://medium.com/@robert.broeckelmann/active-directory-federation-services-adfs-and-kerberos-f36c71e13be5
* https://social.technet.microsoft.com/wiki/contents/articles/1426.ad-fs-2-0-continuously-prompted-for-credentials-while-using-fiddler-web-debugger.aspx
* https://www.samltool.com/online_tools.php
* https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler-ws-fed
* https://www.youtube.com/watch?v=e8x-pQJSB40
* https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-intranet-forms-based-authentication-for-devices-that-do-not-support-wia
* https://ldapwiki.com/wiki/Kerberos%20Pre-Authentication
* http://www.ipfonix.com/single-sign-on.pdf
* https://medium.com/@robert.broeckelmann/active-directory-federation-services-adfs-and-kerberos-f36c71e13be5
* https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-sfu/cd9d5ca7-ce20-4693-872b-2f5dd41cbff6