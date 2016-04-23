MN: Installing OS X
================================

Purchase and download OS X.
Use Unibeast to correctly copy the OS X installer on Flash the drive or [manually "flash" the OS X installer onto a flash drive + install clover there + copy necessary kexts, see below]

###Manually preparing Clover from sourceforge for the installed system: 

Install Clover.pkg using Legacy mode (non-UEFI), with start/stop scripts selected. Params I used: [Install Clover in the ESP, Bootloader->Install boot0ss in MBR, CloverEFI->CloverEFI 64-bits SATA, All themes, Install RC scripts on target volume]. Then mount EFI partition of the hard drive and copy **FakeSMC.kext**, perhaps also **AtherosE2200Ethernet.kext** into "kexts", otherwise the system won't boot.

In config.plist only enable NVidia injector (to make GT 220 work), no boot arguments are required (maybe -v for verbose boot). Optionally hide unwanted partitions from the list (list of all partitions can be found in log using *sudo cat /Library/Logs/CloverEFI/boot.log* - see Hide section in clover.plist, you will need to specify Volume identifier), set up the resolution (**1920x1020**), you can also customize volumes display in clover boot screen including titles using <custom> (for OS X) and <legacy> (for windows).

Install ALC 887 from *https://github.com/toleda*, having mounted the EFI partition. Make sure you are using Audio ID 1 (this one worked for me, other ids werent stable with MainStage). 

Part of Clover's config.plist to change


```
		......(here goes GUI section of the config.plist that we need to change)....
		.....note that keys that start with # are commented.....
		<key>Custom</key>
		<dict>
			<key>Entries</key>
			<array>
				<dict>
					<key>Disabled</key>
					<false/>
					<key>FullTitle</key>
					<string>OS X</string>
					<key>Hidden</key>
					<false/>
					<key>Type</key>
					<string>OSX</string>
					<key>Volume</key>
					<string>###here goes volume id###</string>
					<key>VolumeType</key>
					<string>Internal</string>
				</dict>
			</array>
			<key>Legacy</key>
			<array>
				<dict>
					<key>Disabled</key>
					<false/>
					<key>FullTitle</key>
					<string>Windows</string>
					<key>Hidden</key>
					<false/>
					<key>Title</key>
					<string>Windows</string>
					<key>Type</key>
					<string>Windows</string>
					<key>Volume</key>
					<string>###windows volume id###</string>
					<key>VolumeType</key>
					<string>Internal</string>
				</dict>
			</array>
		</dict>
		<key>Hide</key>
		<array>
			<string>HD(1,GPT,###idtohide###)</string>
			<string>HD(3,GPT,###someotheridtohide###)</string>
		</array>
		<key>ScreenResolution</key>
		<string>1920x1200</string>
		<key>Theme</key>
		<string>random</string>
	</dict>
```
