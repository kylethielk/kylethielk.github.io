---
author: admin
comments: true
date: 2014-03-21 13:14:14+00:00
layout: post
slug: build-ios-app-with-jenkins-code-signing-issues
title: 'Build iOS App with Jenkins: Code Signing Issues.'
wordpress_id: 1025
categories:
- Tech
---

Building an iOS application in xCode is easy. It automatically handles all of your certificates and provisioning profiles. Sure it can be finicky at times, but for the most part it just works.

Automatically building your application (i.e with Jenkins) is not so easy. You have to ensure that your private key, certificate and provisioning profile are configured and available. Recently I found out that this is easier said than done.

**Ensure your provisioning profile is available**

This one is easy. Simply download your provisioning profile from the Developer Center to the build machine and double click it (make sure you are logged in as the user that will be used for your automated builds). xCode will automatically add it to your list of available provisioning profiles. You can see a list of all of your provisioning profiles by browsing to:

_~/Library/MobileDevice/Provisioning\ Profiles/_

**Add your Certificate and Private Key**

This is the tricky part. On your development machine you are more than likely running as an administrator (whereas on your build machine the 'build user' should not be). Simply adding your certificate and key file to the System/Login Keychain and just expecting them to work....well it won't work. The solution is to create a new keychain, add your certificate/key here and tell your build to use this new keychain. _Note: the creation of a new keychain is not 100% necessary, but I prefer it. Once you start building multiple projects on one machine it helps keep things organized._

The following is the pseudo code of what I use. First create new keychain:

{% codeblock lang:bash %}
#Create keychain
security create-keychain -p <i>YourPassword</i> NewKeychain.keychain

#Import Cert/Key - The -A flag is what allows non-interactive use of cert
security import /path/to/my.cert -t cert -k NewKeychain.keychain -A
security import /path/to/private.p12 -t agg -k NewKeychain.keychain -A
{% endcodeblock %}

The build script can then be adjusted as so:
{% codeblock lang:bash %}
#Load keychain, and unlock it
security list-keychain -s "/var/lib/jenkins/Library/Keychains/NewKeychain.keychain"
security default-keychain -s "/var/lib/jenkins/Library/Keychains/NewKeychain.keychain"
security unlock-keychain -p "YourPassword" "/var/lib/jenkins/Library/Keychains/NewKeychain.keychain"

<Run Build>


#Reset to login keychain
security list-keychain -s "/var/lib/jenkins/Library/Keychains/login.keychain"
security default-keychain -s "/var/lib/jenkins/Library/Keychains/login.keychain"

{% endcodeblock %}

