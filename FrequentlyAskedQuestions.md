# Troubleshooting #

  * Q: On Mac, the plugin does not reflect recent changes to the configuration file.
  * A: Try removing `~/Library/Preferences/com.google.netscapesecurity.plist`, a stale cached plist might remain. Alternatively, touch the Info.plist file from the plugin bundle.