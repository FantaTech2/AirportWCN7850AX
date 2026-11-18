# AirportWCN7850

This is a working WiFi kext for the Qualcomm QCNCM865 module, the Fastconnect 7800.

The original kext from razvanccr has the kext working on 802.11ac, but I changed the kext where it now works on 802.11ax, so WiFi 6. On my 1GBPS symmetrical fiber connection, I easily get 900Mbps up and down where I got around 200 first; this is on a Fritz access point with Ethernet uplink to a Cisco switch.

The Bluetooth half of the chip is a separate project and unmodified by me: [BluetoothWCN7850](https://github.com/razvanccr/BluetoothWCN7850).

**Tested on:** Asus Vivobook 15 laptop with Qualcomm QCNCM865 NIC; but this should work on any hackintosh running macOS Tahoe 26 and containing this NIC.
## Download

Go to the releases on this Github page, download the ZIP file, extract it, then go to the Install step.

## Install

1. Copy `AirportWCN7850_11ax.kext` to `EFI/OC/Kexts` and add it to `Kernel → Add` with `MinKernel` 25.0.0.
2. Not alongside itlwm or AirportItlwm; disable an `SSDT-Disable_Network_GPP7` if your config hides the card, doing any of this will stop it from working altogether.
3. Reboot and join a network from the Wi-Fi menu.

`-qwzoff` in boot-args disables the driver, `-novht` falls back to 802.11n. For a first boot on unknown hardware read [Testing safely](docs/bring-up.md#testing-safely).

After this it should work, keep in mind your router or Access Point need to be capable of 11ax too. after joining the network you can open the WiFi menu in macOS while holding ALT, you can look at the PHY mode, and it should say 802.11ax.

## What does not work yet and is untested by me:

- WPA3/SAE, 802.1X without a supplicant; WPA1 and WEP use software crypto (this is what OpenBSD's `qwz(4)` supports).
- 802.11be/WiFi 7, 320 MHz and MLO: the link is 802.11ax and can run up to 160MHz, maybe even more, but this is what i tested.
- AirDrop/AWDL and the rest of Continuity (AWDL is not coming on this chip: [docs/awdl-feasibility.md](docs/awdl-feasibility.md)). Sleep/wake works for me.

## Licences and credits

- `qwz/qwz.c`, `qwz/if_qwz_pci.c`, `qwz/qwzreg.h`, `qwz/qwzvar.h`: OpenBSD `qwz(4)` (Stefan Sperling and the OpenBSD developers), ISC, with code derived from the Linux `ath12k` driver under the Linux Foundation / Qualcomm BSD licence.
- `itl80211/`, `include/`, `controller/` (except `ItlQwz.*`): itlwm by zxystd, GPL-2.0-or-later, with OpenBSD net80211 under its BSD/ISC licences. `airport/`: the AirportItlwm "WCL" layer by laobamac, as reshaped by [SimpleMtkWlan](https://github.com/pengubow/SimpleMtkWlan).
- `qwz/qwz_compat.*`, `controller/ItlQwz.*`, `scripts/`: GPL-2.0-or-later.
- WCN7850 firmware: Qualcomm, redistributable in binary form under `LICENSE.QualcommAtheros_ath10k`.
- Built with [MacKernelSDK](https://github.com/acidanthera/MacKernelSDK).

The whole kext is distributed under GPL-2.0-or-later (`LICENSE`).
