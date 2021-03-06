# backdoorLnkMacroStagerCellEmbed

Disclaimer: Made for coding experience and to test client security measures as a part of engagements.  DO NOT use against any systems / users on which you do not have explicit permission to test against.

NOTE: To utilize this stager several libraries must first be installed, please run the following command: pip install xlutils

This is a two-step attack vector, the initial macro that a user runs will configure targeted shortcuts on their desktop to run a powershell stager. The second step occurs when the user clicks on the shortcut, the powershell download stub that runs will first open the target executable, then clean all backdoored shortcuts on the user's desktop, and finally attempt to download & execute empire launcher code from an xml file hosted on a pre-defined web server. This xml contains a full empire launcher, which will be downloaded and ran in memory, in turn giving a full empire shell back. The XML is downloaded using XmlDocument.Load method as it is inherently proxy-aware, and allows for a clean download of the launcher code.

On run, the macro will search the user's desktop looking for any .lnk files (shortcuts) that match target executables as defined during generation of the macro.  Typical use-cases focus on highly-utilized shortcuts (iexplore, chrome, firefox, etc.)

The two-step approach is done to defeat application-aware security measures that flag on launches of powershell from unexpected programs, such as a direct launch from office applications. As the macro is pure VBA and does not leverage powershell or spawn any child processes it is less likely to be detected by these types of tools.  Moreover, modifications made by the macro should not require administrative rights on the endpoint.  Moving macro data to the contents of cells on the spreadsheet is done to evade antivirus, as certain strings will flag antivirus alerts in a macro, but not in a cell that is called by the macro.  The macro is currently configured as an Auto_Close() for some basic sandbox evasion, feel free to change to a different execution method as needed.

Usage: Drop backdoorLnkMacroCellEmbed.py into rootEmpireFolder/lib/stagers/windows and start empire, the stager should now show up in your stagers list (usestager windows/backdoorLnkMacroCellEmbed.py)

Upon execution of the stager three files are created / updated: an xls document (either new or containing existing contents) that data will be placed into, a macro that should be placed in the spawned xls document, and an xml that should be placed on a web server accessible by the remote system.  By default this xml is written to /var/www/html, which is the webroot on debian-based systems such as kali.  Ensure the xml is located on the webserver configured during setup of the stager and that this is a location accessible on the remote system.
