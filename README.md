#AirportRTW

macOS Wi-Fi driver for supported Realtek RTW88 and RTW89 PCIe cards. One kext contains both HALs and selects the backend automatically by PCI ID.

#Compatibility

Based on AirportRTW 2.3.3, with experimental RTL8852CE Wi-Fi and Bluetooth coexistence improvements.

#Supported cards:

RTW88:
RTL8723DE, RTL8812AE, RTL8814AE, RTL8821AE, RTL8821CE, RTL8822BE, RTL8822CE

RTW89:
RTL8851BE, RTL8852AE, RTL8852BE, RTL8852CE, RTL8922AE

#Changes

* Improved Wi-Fi traffic-aware Bluetooth coexistence.
* Added A2DP activity detection.
* Added WLAN traffic state notifications for RTL8852C.
* Experimental fix for Bluetooth audio interruptions under Wi-Fi traffic.

#How to Use

1. Copy AirportRTW.kext to EFI/OC/Kexts.
2. In OpenCore Kernel -> Add, load the kexts in this order: Lilu, AMFIPass, AirportRTW.
3. Set Misc -> Security -> SecureBootModel to Disabled.
4. Reboot and test.

#Credits

Special thanks to souissiak1-maker/AirportRTW for the original driver, and OpenIntelWireless/itlwm for its modular architecture.

Note: Bluetooth coexistence improvements are experimental and require further hardware testing.