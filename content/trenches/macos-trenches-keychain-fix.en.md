---
title: Outlook and Teams Sign-In Loop on macOS After Password Reset (Keychain Fix)
date: 2026-05-12
draft: false
description: How a corrupted macOS Keychain caused Outlook, Teams, JumpCloud, and Cloudflare Access sign-in loops after a password reset.
tags:
  - macOS
  - Microsoft365
  - Outlook
  - Teams
  - JumpCloud
  - Cloudflare
  - Keychain
  - troubleshooting
categories:
  - Trenches
author: Jan Wróbel
toc: true
---
---
## TL;DR

After a password reset through JumpCloud, Outlook, Teams, Cloudflare Access, and other desktop applications on macOS stopped authenticating correctly.

Browser access continued working normally, which initially made the issue look like a Microsoft 365 or Conditional Access problem.

The actual root cause was a corrupted macOS Keychain that failed to synchronise with the new login password.

Recreating the Keychain resolved the issue completely.

---
## Quick Fix

If Microsoft 365 apps on macOS keep asking for credentials after a password reset:

1. Open `~/Library/`
2. Rename `Keychains` → `Keychains-old`
3. Sign out of macOS
4. Sign back in
5. macOS recreates the Keychain automatically

This resolved:
- Outlook sign-in loops
- Teams crashes
- JumpCloud authentication failures
- Cloudflare Access reauthentication prompts

---

## Problem

Sometimes macOS authentication issues look like:

- broken Microsoft 365
- MFA failures
- Teams bugs
- Conditional Access problems
- or even a corrupted Office installation

But sometimes the issue is much deeper inside macOS itself.

This one came straight from the trenches.

---

## Environment

- macOS
- Microsoft 365
- JumpCloud
- Cloudflare Zero Trust / Access
- Microsoft Teams
- Outlook

---

## Symptoms

After a user's password expired and was reset through JumpCloud, several applications on macOS stopped authenticating correctly.

Interestingly:

- browser access worked normally
- desktop applications were failing

The user experienced:

- Outlook repeatedly asking for the password after successful MFA
- Teams crashing immediately after launch
- JumpCloud Go failing authentication
- Cloudflare Zero Access repeatedly requesting sign-in
- unexpected Keychain password prompts
- Touch ID fingerprint registration failing

Classic symptom:

> "Everything works in the browser, but desktop apps are broken."

---

## Initial Troubleshooting

The issue initially looked related to:

- Microsoft authentication
- Teams cache corruption
- expired authentication tokens
- Conditional Access
- or Office itself

The following troubleshooting steps were completed:

- restarting the Mac
- clearing the Teams cache
- removing and recreating the Outlook profile
- clearing Microsoft-related entries from Keychain Access
- reviewing Teams crash logs
- retesting Microsoft 365 authentication

None of these steps resolved the issue.

---

## Root Cause

The issue was eventually traced back to the macOS Keychain database.

After the password reset, the local Keychain failed to properly synchronise with the user's new login password.

As a result:

- authentication tokens became invalid
- secure credentials could not be accessed correctly
- applications entered authentication loops
- some applications crashed during launch

Because modern macOS applications rely heavily on Keychain for secure credential storage, a corrupted Keychain can simultaneously affect:

- Outlook
- Teams
- JumpCloud
- Cloudflare Access
- and other SSO-integrated applications

---

## Fix — Recreate the macOS Keychain

### Step 1 — Open the User Library Folder

Open Finder → Go, then hold **Option** and select **Library**.

### Step 2 — Rename the Keychains Folder

Navigate to:

```
~/Library/Keychains
```

Rename the folder to:

```
Keychains-old
```

> Do not delete it.

### Step 3 — Sign Out and Back In

Have the user:

- sign out of macOS
- then sign back in

macOS will automatically recreate:

- the Keychain folder
- the login keychain database
- local authentication storage

---

## Result

After recreating the Keychain:

- Outlook signed in successfully
- Teams launched normally
- JumpCloud authentication worked correctly
- Cloudflare Access stopped requesting repeated authentication
- password prompts disappeared
- Touch ID registration worked again

No Office reinstall was required.

---

## Important Recovery Note

If the user later needs access to saved passwords or certificates from the old Keychain, navigate to:

```
~/Library/Keychains-old
```

Then open:

```
login.keychain-db
```

This allows recovery of:

- saved credentials
- certificates
- passwords
- older authentication records

---

## What Can Go Wrong

### Deleting the old Keychain completely

Do not permanently delete the old Keychain immediately.

Users may still need:

- saved Wi-Fi passwords
- certificates
- VPN credentials
- application logins

### Assuming it is a Microsoft 365 issue

Because browser authentication works normally, this issue can easily waste hours troubleshooting:

- Microsoft 365
- Teams
- Conditional Access
- MFA
- Outlook profiles
- or Office installations

Meanwhile, the actual problem exists locally inside macOS.

---

## Lessons From The Trenches

This initially looked like:

- broken MFA
- Teams instability
- Outlook corruption
- Conditional Access failures
- or Microsoft 365 authentication problems

In reality, the root cause was a corrupted macOS Keychain after a password reset.

Sometimes the entire authentication stack fails simply because macOS is holding outdated or broken credentials underneath the surface.

Real IT from the trenches. What actually breaks in IT.
