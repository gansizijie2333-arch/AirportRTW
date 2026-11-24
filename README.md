# AirportRTW

macOS Wi-Fi driver for supported Realtek RTW88 and RTW89 PCIe cards. One kext contains both HALs and selects the backend automatically by PCI ID.

## Compatibility

Based on AirportRTW 2.3.3, with experimental RTL8852CE Wi-Fi and Bluetooth coexistence improvements.

**Supported cards:**

**RTW88:**  
RTL8723DE, RTL8812AE, RTL8814AE, RTL8821AE, RTL8821CE, RTL8822BE, RTL8822CE

**RTW89:**  
RTL8851BE, RTL8852AE, RTL8852BE, RTL8852CE, RTL8922AE

## Changes

- Improved Wi-Fi traffic-aware Bluetooth coexistence.
- Added A2DP activity detection.
- Added WLAN traffic notifications for RTL8852C.
- Experimental Bluetooth audio coexistence improvements.

## How to Use

1. Copy AirportRTW.kext to EFI/OC/Kexts.
2. In OpenCore Kernel -> Add, load Lilu, AMFIPass, AirportRTW in this order.
3. Set Misc -> Security -> SecureBootModel to Disabled.
4. Reboot and test.

## Credits

Special thanks to [souissiak1-maker/AirportRTW](https://github.com/souissiak1-maker/AirportRTW) and [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm).