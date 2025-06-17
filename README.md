macOS Enrollment Guide
Client Preparation
On Client:

Erase device

MDM Server Preparation
On MDM Server:

Run ./tools/api/commands/bulk_remove_macos_vpp_apps

Bulk-remove apps (unregister app licenses from devices)



DEP Enrollment
Configuration:

Add device serial number to dep-profiles/dep.sloto.universal.json
Run ./tools/api/sync_dep_devices

Synchronize device with Apple DEP system for automated device enrollment



Apple Business Manager:

Log in to ABM (Apple Business Manager) with your Apple Development Account
Assign new device to your MDM server (mdm.sloto.space)
Run ./tools/api/sync_dep_devices
On ABM unassign the device from your MDM server (mdm.sloto.space)
Run ./tools/api/sync_dep_devices
On ABM assign the device again to your MDM server (mdm.sloto.space)
Run ./tools/api/sync_dep_devices twice

Client Installation
Base Installation:

Installation of base system + connect to WiFi
On iPhone run Apple Configurator app with logged corporate development account
Run the app and be close to macOS device
Enroll screen will appear, click on "Enroll this device"

Account Configuration:

Manually create Admin user account
Set device hostname with these commands:

bashsudo scutil --set LocalHostName devicehostname
sudo scutil --set ComputerName devicehostname
sudo scutil --set HostName devicehostname.domain.name
sudo dscacheutil -flushcache
sudo reboot
Profile Installation on MDM Server
Basic Profiles (only signed)
mdm-profiles/sloto.macos.appleRoot.profile.signed.mobileconfig

Includes Apple root CA as authority for Corporate MDM certificate

mdm-profiles/sloto.macos.Root.profile.signed.mobileconfig

Root profile - time server settings etc.

mdm-profiles/sloto.macos.DirectoryServices.profile.signed.mobileconfig

Connect to AD (only physically on site)

mdm-profiles/sloto.macos.EnergySaver.profile.signed.mobileconfig

Energy settings

MUNKI Profile Selection
Choose based on user type:
mdm-profiles/sloto.macos.Munki-Default.profile.signed.mobileconfig

Default >> Munki (SSC - Sloto Software Center app configuration)

mdm-profiles/sloto.macos.Munki-Tech.profile.signed.mobileconfig

Tech >> Munki (SSC - Sloto Software Center app configuration)

Additional Configuration:
mdm-profiles/sloto.macos.NoMAD.profile.signed.mobileconfig

NoMAD app configuration profile

mdm-profiles/sloto.macos.Restrictions.profile.signed.mobileconfig

OS restrictions profile

Account Profile Settings
Choose based on requirements:
mdm-profiles/sloto.macos.Account-Enabled.profile.signed.mobileconfig
mdm-profiles/sloto.macos.Account-Disabled.profile.signed.mobileconfig
Reboot and Login:

Reboot device and log in as AD user

Advanced Configuration
FileVault and Additional Profiles
mdm-profiles/sloto.macos.Filevault.profile.signed.mobileconfig

FileVault profile installation

mdm-profiles/wireguard_configs/...

Wireguard connection config profiles, based on user

Client Completion
FileVault Activation:

Log out from AD Account
Enter account password to enable FileVault
Log in to AD Account

Development Tools Installation:
bashsudo softwareupdate --install-rosetta
sudo xcode-select --install
Final Application Installation
MDM Agent and Software Center
bash./tools/api/commands/install_application DEVICE-UUID https://repo.sloto.space/munki/sloto_mdmagent.plist

MDM agent installation (requires connection to repo.sloto.space - must be within local network or connected to Wireguard)

bash./tools/api/commands/install_application DEVICE-UUID https://repo.sloto.space/munki/sloto_munki.plist

SSC Application installation (same requirements as above)

VPP Applications
bash./tools/api/commands/bulk_install_macos_vpp_apps

Based on DEVICE-UUID and serial number, installs all VPP (ABM) apps on device

Completion

Run Sloto Software Center app
Wait for initialization and installation of default apps


✅ DONE
