---
title: "BitLocker Recovery, 7q6ch, and Broken Microsoft 365 Sign-In"
date: 2026-05-11
draft: false
description: "How a Dell BIOS update and BitLocker recovery event broke Microsoft 365 authentication on a Windows device."

tags:
  - Microsoft365
  - BitLocker
  - EntraID
  - Outlook
  - Teams
  - troubleshooting
  - 7q6ch
  - AzureAD
  - authentication

categories:
  - Trenches

author: "Jan Wróbel"
toc: false
---
---
## TL;DR

User was suddenly signed out from all Microsoft 365 applications after a BitLocker recovery event.

Outlook, Teams, and Office started throwing a `7q6ch` authentication error, while browser access continued working normally.

The device itself was still healthy and properly joined to Entra ID.

The issue was resolved by re-authenticating the Work or School account after signing back into Windows using the `Other user` option.

---

## The Symptoms

![7q6ch](/images/trenches/7q6ch-signin-error-after-bitlocker1.png)

User reported:

- signed out from Outlook, Teams, and Office
- unable to sign back into Microsoft applications
- `7q6ch` sign-in error
- browser access to Microsoft 365 still working

There was also an important detail:

Earlier that morning, the device had entered BitLocker recovery mode and required the recovery key before Windows would boot.

---

## First Checks

TPM status was checked:

```
tpm.msc
```

Result:

- TPM healthy
- TPM ready for use

![TPM](/images/trenches/7q6ch-signin-error-after-bitlocker4.png)

Entra/Azure AD device registration was then verified:

```
dsregcmd /status
```

Important values:

```
AzureAdJoined : YES
DeviceAuthStatus : SUCCESS
TpmProtected : YES
```

![Entra/Azure AD](/images/trenches/7q6ch-signin-error-after-bitlocker3.png)

The device itself still appeared healthy and trusted.

Credential Manager was also checked, but no Microsoft or Azure AD credentials were present.

The machine initially still appeared connected under:

```
Settings → Accounts → Access work or school
```

---

## Possible Cause

Dell Update history showed that a BIOS update together with multiple chipset and driver updates had installed several days earlier.

![DellCommand_logs](/images/trenches/7q6ch-signin-error-after-bitlocker2.png)

Windows also displayed a Secure Boot related warning in Event Viewer:  
  
```text  
Updated Secure Boot certificates are available on this device but have not yet been applied to the firmware.
  
FirmwareVersion:1.21.0
```

Most likely scenario:

- BIOS/Secure Boot update triggered BitLocker validation
- authentication trust/session became invalid
- Microsoft 365 user tokens/WAM authentication chain broke
- device trust itself remained healthy

---

## Fix

Instead of rebuilding the profile or resetting TPM:

1. User was signed out from Windows
2. Logged back in using `Other user`
3. Windows detected an issue with the Work or School account
4. User was prompted to reauthenticate
5. “Allow for all apps” option was selected

After reauthentication:

- Teams started working
- Outlook authenticated successfully
- Microsoft 365 sign-in functionality returned

---

## Lessons Learned

- BitLocker recovery events can sometimes affect Microsoft 365 authentication
- `dsregcmd /status` is extremely useful for quickly verifying device trust
- Healthy TPM and successful Azure AD join do not always mean user authentication tokens are healthy
- Reauthenticating the Work or School account can sometimes fully restore broken Microsoft 365 authentication without rebuilding the profile

Real IT from the trenches.
What actually breaks in IT.