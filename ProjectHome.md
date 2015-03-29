The Netscape Plugin Security Wrapper manages the loading of NPAPI (Netscape Plugin API) plug-ins, and applies simple policy decisions. The intention is to allow administrators to deploy deprecated, unreliable or unsafe third party plug-ins while minimising the security exposure.

Safari, Google Chrome, Firefox and other NPAPI compatible browsers are supported on OS X and Linux.

## Use Cases ##

  * Restrict plugins to certain domains.
  * Restrict the use of deprecated plugins to known outliers.
  * Allow internal corporate workflows that use insecure or deprecated plugins, without exposing the plugin to the hostile internet.
  * Allow multiple outdated plugin versions (e.g. Java) to co-exist for use in whitelisted, trusted, enterprise tools.

# Netscape Plugin Security Wrapper #

All configuration happens in the file /etc/nssecurity.ini, intended to be
manageable by cfengine, puppet, or other similar tools. The format is described
in the sample configuration file included.

The most basic policy decision is a domain whitelist. For example, by creating
a configuration like this:

```
    [Third Party Plugin]
    LoadPlugin=/usr/lib/thirdparty/plugin.so
    AllowDomains=*.corp.megacorp.com,*.lan
```

Or on Apple systems, which use directory bundles called .plugin instead of
shared objects:

```
    [Third Party Plugin]
    LoadPlugin=/Library/Third Party Plug-Ins/BrowserThing.plugin
    AllowDomains=*.corp.megacorp.com,*.lan
```

Now the plugin can only be instantiated by the domains listed. By default, the
plugins must be loaded over https, as this is the only way to have any
confidence the domain being reported by the browser is accurate. However, you
can disable the protocol checks like so if you really need it:

```
    [Third Party Plugin]
    LoadPlugin=/usr/lib/thirdparty/plugin.so
    AllowDomains=*.corp.megacorp.com,*.lan
    AllowInsecure=1
```


More information is available on the Wiki, NetscapeSecurity, or see an [example configuration file](https://code.google.com/p/nssecurity/source/browse/trunk/nssecurity.ini)