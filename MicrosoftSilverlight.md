# Introduction #

Microsoft provide an NPAPI Silverlight plugin for Mac OS X. The Netscape Security Wrapper can proxy requests to Silverlight perfectly, however many Silverlight websites do not appear to recognise the availability of Silverlight playback.

This article discusses the cause and suggests a solution.

# Details #

_If you are not interested in the cause of this problem, you can skip this section and jump to the [Solutions](MicrosoftSilverlight#Solutions.md) below._

Microsoft provide a JavaScript file called Silverlight.js to developers that is intended to display a friendly installation button if the browser doesn't support Silverlight.

http://archive.msdn.microsoft.com/silverlightjs

This file appears to be popular in the Silverlight development community. Unfortunately, it tests for Silverlight availability against the advertised plugin names, instead of MIME types.

This requires a plugin named exactly "Silverlight Plug-In" to exist, regardless of whether the playback would work or not, as demonstrated in the Silverlight.js code snippet below.

```
            var plugin = navigator.plugins["Silverlight Plug-In"];
            if (plugin)
            {
                if (version === null)
                {
                    isVersionSupported = true;
                }
```

# Solutions #

The easiest solution is just to provide a dummy plugin that satisfies this requirement, this can be done on OS X by simply removing the advertised MIME types from Silverlight.

```
$ sudo -s
bash-3.2# cp -r /Library/Third\ Party\ Plug-Ins/Silverlight.plugin /Library/Internet\ Plug-Ins/DummySilverlight.plugin
bash-3.2# PlistBuddy -x /Library/Internet\ Plug-Ins/DummySilverlight.plugin/Contents/Info.plist      
Command: delete WebPluginMIMETypes
Command: add WebPluginMIMETypes dict
Command: save
Saving...
Command: exit
```

Browsers will not use this plugin as it does not advertise any supported MIME Types.

In fact, you do not need to copy the entire Directory Bundle, which for some reason is around 70MB. Only the Info.plist and the matching file from the `CFBundleExecutable` key are required.

```
$ PlistBuddy -x /Library/Internet\ Plug-Ins/DummySilverlight.plugin/Contents/Info.plist 
Command: print CFBundleExecutable
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<string>agcore</string>
</plist>
Command: exit              
$ ls -l /Library/Internet\ Plug-Ins/DummySilverlight.plugin/Contents/MacOS/agcore 
-rwxr-xr-x  1 root  wheel  15036056 Feb 29 14:05 /Library/Internet Plug-Ins/DummySilverlight.plugin/Contents/MacOS/agcore
```

Now you can ignore this Plugin, and configure the managed version via `nssecurity.ini`.

```
[Silverlight]
FriendlyWarning=Silverlight? Why would you want that?!
LoadPlugin=/Library/Third Party Plug-Ins/Silverlight.plugin
AllowedDomains=*.netflix.com
AllowInsecure=1
```

To verify the problem is solved, follow the steps below.

  1. Start an NPAPI compatible browser (Chrome, Safari) and visit about:plugins
  1. Verify you see a Silverlight Plugin advertising no MIME types.
  1. Verify you see Netscape Security Wrapper advertising the Silverlight MIME types:
    * `application/x-silverlight`
    * `application/x-silverlight-2`
  1. Visit an AllowedDomain and verify the content plays.
  1. Visit a domain that is not allowed, and verify the content is blocked (with the warning message, if configured).

# Notes #

  * This solution was tested on Safari and Chrome with Lion.