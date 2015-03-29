# Introduction #

Recent releases of Java for Mac OS X require some extra configuration steps to work with nssecurity. This is because Oracle use absolute symlinks, rather than relative symlinks.

This prevents the plugin bundle from being relocated to any directory other than /Library/Internet Plug-Ins unless the symlinks are updated accordingly.

This is easy to workaround.

# Details #

Remove Info.plist, and make it a symlink to Enabled.plist.

```
JavaAppletPlugin.plugin/Contents$ ls -l
total 20K
-rw-rw-r--  1 65387 wheel  734 Jan 30 01:34 Disabled.plist
-rw-rw-r--  1 65387 wheel 4.4K Jan 30 01:34 Enabled.plist
drwxrwxr-x  3 65387 wheel  102 Jan 30 01:34 Frameworks/
drwxr-xr-x 12 root  wheel  408 Jan 30 01:31 Home/
-rw-r--r--  1 root  wheel 4.4K Feb 14 14:05 Info.plist
drwxr-xr-x  3 65387 wheel  102 Feb 13 11:45 MacOS/
drwxrwxr-x  2 65387 wheel   68 Jan 30 01:31 Plugins/
drwxr-xr-x 11 65387 wheel  374 Feb  1 13:23 Resources/
```