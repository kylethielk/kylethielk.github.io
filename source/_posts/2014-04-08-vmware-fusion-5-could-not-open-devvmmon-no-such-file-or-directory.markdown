---
author: Kyle Thielk
comments: true
date: 2014-04-08 12:53:52+00:00
layout: post
slug: vmware-fusion-5-could-not-open-devvmmon-no-such-file-or-directory
title: 'VMWare Fusion 5: Could not open /dev/vmmon: No such file or directory.'
wordpress_id: 1039
categories:
- Tech
---

I ran into this issue recently with VMWare Fusion 5. All of the sudden none of my virtual machines would start. They would all error out with "Could not open /dev/vmmon: No such file or directory."

After much head banging I remembered that I had recently installed Intel HAXM. Taking a stab in the dark I uninstalled HAXM:

{% codeblock lang:bash %}
sudo /System/Library/Extensions/intelhaxm.kext/Contents/Resources/uninstall.sh
{% endcodeblock %}

Voila! All of my VM's happily start. My emulators run extremely slow, but at least I can run virtual machines. Will update if I figure out how to get them running in parallel.
