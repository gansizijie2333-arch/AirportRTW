AirportRTW – RTL8852CE Wi-Fi / Bluetooth Coexistence Fix

An experimental modification of AirportRTW, based on version 2.3.3, focused on improving Wi-Fi and Bluetooth coexistence for Realtek RTL8852CE on macOS.

The main goal is to address Bluetooth audio interruptions and instability, particularly when Wi-Fi is connected and handling network traffic while using Bluetooth A2DP audio devices.

Status: Experimental

This modification introduces additional RTL8852C coexistence handling. It is intended for testing and further development. Actual Bluetooth audio stability improvements have not yet been conclusively verified.

Background

On some RTL8852CE systems running macOS, Bluetooth may work normally when Wi-Fi is disabled, but experience intermittent disconnections, audio stuttering, or instability when Wi-Fi is enabled.

This project explores improvements to the existing RTW89 Bluetooth coexistence mechanism, especially when Wi-Fi traffic is active.

Main Changes

Compared with the original AirportRTW 2.3.3 source, this fork includes the following changes:

1. Improved A2DP State Detection

* Detect changes in the Bluetooth A2DP activity state.
* Trigger coexistence policy updates when the A2DP state changes.
* Avoid relying exclusively on unrelated Bluetooth scoreboard changes.

2. Wi-Fi Traffic-Aware Coexistence

Introduces a new function:

rtw89_btc_ntfy_wl_traffic()

This function allows the coexistence mechanism to react to changes in Wi-Fi traffic conditions.

It monitors:

* WLAN busy state.
* Wi-Fi traffic direction (TX/RX).
* Bluetooth connection and A2DP activity.
* Relevant WLAN operating states.

When the aggregated WLAN busy state or traffic direction changes, the coexistence policy can be reevaluated.

3. RTL8852C Traffic Monitoring

When the full tracking watchdog is disabled, RTL8852C now retains host-side TX/RX traffic sampling through the existing two-second tracking interval.

This allows traffic-aware coexistence updates without executing the watchdog’s periodic throughput-offload commands.

4. Coexistence Diagnostics

Additional diagnostic flags are available through IORegistry:

ioreg -l -w 0 | grep -E '"RTW89Coex(Flags|Reason|Action|H2CFailures)"'

Relevant bits in RTW89CoexFlags:

Bit	Meaning
8	WLAN busy
9	Bluetooth A2DP active
10	WLAN busy with download traffic

Hardware & Software

* Target hardware: Realtek RTL8852CE
* Driver family: RTW89
* Operating system: macOS
* Base project: AirportRTW 2.3.3
* Architecture: x86_64

The original AirportRTW project supports multiple Realtek RTW88 and RTW89 devices. This modification specifically focuses on RTL8852C coexistence behavior.

Build Instructions

Requirements:

* macOS
* Full Xcode installation
* Xcode Command Line Tools
* Python 3

Run the following command from the project root:

bash build-airportrtw-load-coex.sh

The script performs:

1. Focused Bluetooth coexistence tests.
2. Release build for the Sequoia x86_64 target.
3. Info.plist validation.
4. Kernel extension binary verification.

Expected output:

build/releases/Release/Sequoia/AirportRTW.kext

Keep a backup of your existing working kext before replacing it.

Testing

The recommended test scenario is:

1. Boot macOS with the modified AirportRTW.kext.
2. Connect a Bluetooth A2DP audio device, such as AirPods.
3. Connect to a Wi-Fi network.
4. Play a local audio file.
5. Compare Bluetooth audio stability under:
    * Wi-Fi idle conditions.
    * Continuous upload traffic.
    * Continuous download traffic.

Use the same Bluetooth device and comparable network conditions for each test.

Important: The included test harness verifies coexistence policy dispatch and hardware scoreboard update ordering. It does not measure actual Bluetooth airtime or guarantee improved audio stability.

Patch

The complete source modification is also provided as:

rtw89-load-coex.patch

It can be reviewed or applied to a separate clean checkout of the matching upstream version.

Base commit:

eaeab26ffd9b8842ba4c47134f1687d868a0ae47

Credits

* souissiak1-maker/AirportRTW – Original macOS Realtek Wi-Fi driver.
* OpenIntelWireless/itlwm – Architectural foundation acknowledged by the original project.
* Realtek RTW89 driver contributors.

Disclaimer

This is an experimental community modification.

It is not an official AirportRTW release and is not affiliated with the original maintainers.

Hardware compatibility and Bluetooth coexistence behavior may vary depending on the Wi-Fi card, firmware, Bluetooth device, and macOS version.

Please report test results, logs, and hardware configurations through GitHub Issues.