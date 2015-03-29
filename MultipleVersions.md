# Introduction #

It's unfortunately common to hear of proprietary enterprise tools that require a specific version of plugins like Java to be available, even if that version is old and insecure. Using the Netscape Security Wrapper, you can install multiple versions of the same plugin, and dictate which domains are allowed to use which version.

This works because the first plugin that requests a specific MIME type **and** is whitelisted is permitted, so fall-through cases can be implemented.

# Details #

Install the different plugins into different location, for example:

```
/Library/Third Party Plug-Ins/Java1.0.plugin
/Library/Third Party Plug-Ins/Java2.0.plugin
/Library/Third Party Plug-Ins/Java3.0.plugin
```

(Or on Linux, `/usr/lib/plugins/*`)

Now you can whitelist which sites are allowed to use each.

```
    [Java Version 1.2]
    FriendlyWarning=
    LoadPlugin=/Library/Third Party Plug-Ins/Java1.0.plugin
    AllowDomains=hr.corp.megacorp.com

    [Java Version 1.3]
    FriendlyWarning=
    LoadPlugin=/Library/Third Party Plug-Ins/Java2.0.plugin
    AllowDomains=legal.corp.megacorp.com

    [Java Version 1.3]
    FriendlyWarning=No Java Plugin allowed on this Domain.
    LoadPlugin=/Library/Third Party Plug-Ins/Java3.0.plugin
    AllowDomains=marketing.corp.megacorp.com
```

And so on.