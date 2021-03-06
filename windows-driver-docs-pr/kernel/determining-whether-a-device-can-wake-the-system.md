---
title: Determining Whether a Device Can Wake the System
author: windows-driver-content
description: Determining Whether a Device Can Wake the System
ms.assetid: 59f23035-4169-4dd4-ac60-882c32efda2c
keywords: ["wait/wake IRPs WDK power management , devices with wake capability", "power management WDK kernel , wake-up capabilities", "external wake signals WDK", "awakening devices", "wake-up capabilities WDK power management", "device wake ups WDK power management"]
ms.author: windowsdriverdev
ms.date: 06/16/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# Determining Whether a Device Can Wake the System


## <a href="" id="ddk-determining-whether-a-device-can-wake-the-system-kg"></a>


Some devices, such as keyboards, modems, and network cards, can respond to external signals while in a device sleep state. As part of its power management technology, the operating system provides a way for such devices to wake a sleeping system, which can then restore its previous context. The software wake-up mechanism allows a system to awaken from any state except S5 (**PowerSystemShutdown**), depending on support in the system and device hardware and BIOS. A system in state S5 must always be rebooted.

Although the operating system is designed to awaken from any of the intermediate sleep states, the exact wake-up capabilities vary from computer to computer and device to device. Not all computers support all system sleep states; therefore, the ability to wake from certain states is meaningless on some computers.

Similarly, most devices neither support all device power states (D0 through D3) nor support wake-up from all the device power states that they do support.

The sleep states that a device can enter, along with the states from which it supports wake up, are described at enumeration by the bus driver and are stored in the [**DEVICE\_CAPABILITIES**](https://msdn.microsoft.com/library/windows/hardware/ff543095) structure. The following table lists the members of this structure that are relevant to wait/wake support.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Member</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>[<strong>DeviceD1</strong>](deviced1-and-deviced2.md)</p></td>
<td><p>True if device supports state PowerDeviceD1.</p></td>
</tr>
<tr class="even">
<td><p>[<strong>DeviceD2</strong>](deviced1-and-deviced2.md)</p></td>
<td><p>True if device supports state PowerDeviceD2.</p></td>
</tr>
<tr class="odd">
<td><p>[<strong>WakeFromD0</strong>](wakefromd0--wakefromd1--wakefromd2--and-wakefromd3.md)</p></td>
<td><p>True if device can wake from PowerDeviceD0.</p></td>
</tr>
<tr class="even">
<td><p>[<strong>WakeFromD1</strong>](wakefromd0--wakefromd1--wakefromd2--and-wakefromd3.md)</p></td>
<td><p>True if device can wake from PowerDeviceD1.</p></td>
</tr>
<tr class="odd">
<td><p>[<strong>WakeFromD2</strong>](wakefromd0--wakefromd1--wakefromd2--and-wakefromd3.md)</p></td>
<td><p>True if device can wake from PowerDeviceD2.</p></td>
</tr>
<tr class="even">
<td><p>[<strong>WakeFromD3</strong>](wakefromd0--wakefromd1--wakefromd2--and-wakefromd3.md)</p></td>
<td><p>True if device can wake from PowerDeviceD3.</p></td>
</tr>
<tr class="odd">
<td><p>[<strong>DeviceState</strong>](devicestate.md) [PowerSystemMaximum]</p></td>
<td><p>Specifies highest device power state that this device can support for each system power state, from PowerSystemUnspecified to PowerSystemShutdown.</p></td>
</tr>
<tr class="even">
<td><p>[<strong>SystemWake</strong>](systemwake.md)</p></td>
<td><p>Specifies lowest system power state (S0 through S4) from which the system can be awakened.</p></td>
</tr>
<tr class="odd">
<td><p>[<strong>DeviceWake</strong>](devicewake.md)</p></td>
<td><p>Specifies lowest device power state (D0 through D3) from which the device can awaken.</p></td>
</tr>
</tbody>
</table>

 

The **DeviceWake** entry lists the lowest device power state from which the device can respond to a wake-up signal. The value PowerDeviceUnspecified indicates that the device cannot wake the system. The **SystemWake** entry lists the lowest system power state from which the system can be awakened. These values are based on the capabilities of the parent devnode and drivers should not change them. For more information, see [Reporting Device Power Capabilities](reporting-device-power-capabilities.md).

In general, a device can wake the system if the following are true:

-   The device is in a power state equal to or more-powered than the **DeviceWake** value.

-   The system is in a power state equal to or more powered than the **SystemWake** value.

 

 




