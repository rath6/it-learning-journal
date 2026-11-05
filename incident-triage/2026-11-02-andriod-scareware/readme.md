# Android scareware triage: fake "79 viruses" alerts

**Date:** 2026-11-02
**Status:** No recurrence after 24 hours
**Scope:** One Samsung Android phone belonging to someone I know, handled with the owner's permission. Personal details are redacted.
**Tools:** Kali Linux, adb (Android platform tools), VirusTotal. All free!!

## Summary

The owner installed a free PDF reader and shortly afterwards started seeing full-screen alerts claiming their Google account was deactivated and that 79 viruses were on the device. These alerts are scareware. I removed the suspect apps in Safe Mode, then used adb from Kali to audit what remained, inspect the suspect app's permissions and check for persistence. The app was adware built around an ad SDK. It did not hold the permissions needed for overlays, accessibility abuse or silent installs. No alerts appeared in the 24 hours after cleanup. The direct source of the alerts was not conclusively identified.

## Timeline

1. Owner reports fake virus and account alerts after installing a PDF reader.
2. Booted into Safe Mode and uninstalled the reader and other unrecognised apps.
3. Connected the phone to Kali over USB debugging and audited it with adb.
4. Pulled the app's base APK and hashed it for reputation checks.
5. Dumped requested and granted permissions.
6. Checked device admin apps and accessibility services for persistence.
7. [Checked Chrome and Samsung Internet notification permissions, result.]
8. Monitored for 24 hours with no further alerts.

## Method

Listed third-party packages to find anything unexpected:

    adb shell pm list packages -3

Located and pulled the suspect APK, then hashed it:

    adb shell pm path <package>
    adb pull '<path>/base.apk' reader_base.apk
    sha256sum reader_base.apk

Dumped permissions and checked persistence:

    adb shell dumpsys package <package> | grep -i permission
    adb shell dumpsys device_policy | grep -i admin
    adb shell settings get secure enabled_accessibility_services

## Findings

The package was a generic document reader clone. Its permissions pointed to advertising rather than surveillance. It requested ad identifier and ad services permissions and referenced AppLovin's AppHub service, which is an ad and app distribution SDK. It also requested boot and foreground service permissions, which let an app restart and keep running in the background.

| Check | Result |
|---|---|
| Draw over other apps (SYSTEM_ALERT_WINDOW) | Not requested |
| Accessibility service binding | Not requested |
| Install other packages | Not requested |
| Notifications and storage (runtime) | Requested, not granted |
| Device admin | Only the Samsung Knox Guard client, a stock component |
| Enabled accessibility services | [paste result] |

SHA-256 of base.apk: `c3e1bcfa815016f8238e27c40c6b6446eea210bd2f53779287e070153f17a8e5`


VirusTotal returned no detections for this hash. That is weak evidence on its own, because freshly repacked adware is often not flagged yet.

The package still appeared in the third-party list after my Safe Mode removal. "com.alldocumentsreader.fileopener.viewandreadfiles.ppt.word.excel.docs.reader"

## What I could not confirm

Because the app could not draw overlays or send notifications, it may not have been the direct source of the alerts. My working hypothesis is that they came from a malicious site, possibly through browser notification permission. I did not capture a live popup to identify the foreground package, so the root cause is unconfirmed. Twenty-four hours without alerts is encouraging but not proof, so a factory reset remains the fallback if they return.

## Remediation

Removed the reader and other unrecognised apps, revoked unknown special access permissions, cleared browser site permissions and data, and advised the owner to change passwords for any account they entered into the alerts. Owner had entered credentials but was unaware of which apps so told them to change all passwords on a different device.

## What I learned
I learned how to install and use Android debugging tools on Kali Linux, essentially. 

## What confused me

What consfused allot was actually getting use too the android layout and getting into safe mode/ getting into developer mode in order to get it to talk to me Kali linux machine through usb

## Evidence

Screenshots are in `evidence/`, with personal details redacted.
