<div align="center">
 <img style="padding:0;vertical-align:bottom;" height="158" width="311" src="BSF.png"/>
 <p>
  <h1>
   CSIRT-Collect
  </h1>
 </p>

</div>

A set of PowerShell scripts to collect memory and (triage) disk forensics for incident response investigations.


:fire: Watch this space. Major update coming early next week. :fire:

The default script leverages a network share, from which it will access and copy the required executables and subsequently upload the acquired evidence to the same share post-collection.

Permission requirements for said directory will be dependent on the nuances of the environment and what credentials are used for the script execution (interactive vs. automation)

In the demonstration code, a network location of `\\Synology\Collections` can be seen. This should be changed to reflect the specifics of your environment.

Collections folder needs to include:
- subdirectory KAPE; copy the directory from existing install
- subdirectory MEMORY; 7za.exe command line version of 7zip and Magnet RAM Capture.

For a walkthough of the script https://bakerstreetforensics.com/2021/12/13/adding-ram-collections-to-kape-triage/

## CSIRT-Collect

- Maps to existing network drive -
- - Subdir 1: “Memory” – Winpmem and 7-Zip executables
- - Subdir 2: ”KAPE” – directory (copied from local install)
- Creates a local directory on asset
- Copies the Memory exe files to local directory
- Captures memory with Magnet RAM Capture
- When complete, ZIPs the memory image
- Renames the zip file based on hostname
- Documents the OS Build Info (no need to determine profile for Volatility)
- Compressed image is copied to network directory and deleted from host after transfer complete
- New temp Directory on asset for KAPE output
- KAPE KapeTriage collection is run using VHDX as output format [$hostname.vhdx]
- VHDX transfers to network
- Removes the local KAPE directory after completion
- Writes a “Process complete” text file to network to signal investigators that collection is ready for analysis

## CSIRT-Collect_USB

This script will:
- capture a memory image with Magnet Ram Capture,
- capture a triage image with KAPE,
- check for encrypted disks,
- recover the active BitLocker Recovery key,
all directly to the USB device.

Prerequisites:

On the root of the USB:
- CSIRT-Collect_USB.ps1
- folder (empty to start) titled 'Collections'
- KAPE folder from default install. Ensure you have EDDv300.exe in \modules\bin\EDD
- MEMORY folder with MRC.exe and 7za.exe inside

Execution:
- Open PowerShell as Adminstrator
- Navigate to the USB device
- Execute `./CSIRT-Collect_USB.ps1`

For a walkthrough of the USB version https://bakerstreetforensics.com/2021/12/17/csirt-collect-usb/
