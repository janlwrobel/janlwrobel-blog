---
title: "PowerShell Scripts: When Online Archive Hits the 100 GB Wall"
date: 2026-05-08
draft: false
description: How to enable auto-expanding archive in Exchange Online when Online Archive starts hitting storage limits.
tags:
  - Microsoft365
  - Exchange-Online
  - PowerShell
  - Online-Archive
  - Troubleshooting
categories:
  - Microsoft 365
author: Jan Wróbel
toc: true
---
## Quick Fix

User screaming that mailbox is full?

Check this first: [Jump to TL;DR](#tldr)


```powershell
Get-Mailbox "user@domain.com" | FL AutoExpandingArchiveEnabled
Enable-Mailbox "user@domain.com" -AutoExpandingArchive```
```

Full explanation below.
## Problem

A user's Online Archive was approaching the standard archive limit.

In Exchange Online, archive mailboxes normally provide up to 100 GB of storage. After that, Microsoft can automatically extend the archive using auto-expanding archiving.

The problem is:

>most users have absolutely no idea what Online Archive even is until it breaks.

And most admins only remember this feature exists when someone suddenly opens a ticket saying:

>"My mailbox is full."

---
## Symptoms

The user may report things like:

- Online Archive looks full
- Emails are not moving into archive as expected
- Archive cleanup stops helping
- Mailbox/archive quota warnings
- Outlook performance gets worse because old mail is not moving properly
- Users receive scary Microsoft 365 emails saying the archive is full and immediately open a high-priority ticket
- Users report that "their mailbox is full", even though the real problem is the Online Archive quota
- Users usually have no idea what Online Archive even is, because Outlook hides most of the complexity

---
## Root Cause

The Online Archive mailbox hit the normal storage wall.

By default, Exchange Online archive mailboxes have limits.

To go beyond that, auto-expanding archive must be enabled.

This feature automatically adds extra archive storage behind the scenes when Microsoft detects that the archive is approaching capacity.

According to Microsoft documentation, archive storage can grow up to 1.5 TB.

---
## What Happened on the Ticket

### Step 1 — Connected to Exchange Online

Opened PowerShell and connected to Exchange Online:

```powershell
Connect-ExchangeOnline
```

This opened the Microsoft login window.

After entering admin credentials, Microsoft displayed another prompt asking:

> "Sign in to all apps and websites on this device?"

Unless you intentionally want to add the admin account to your Windows profile, always select:

>**No, this app only**

Otherwise, Windows may start attaching the admin Microsoft 365 account to your local machine, which can later create:

- Office login confusion
- OneDrive sync problems
- SharePoint credential issues
- Unwanted device registration in Entra ID
- Admin account appearing inside Windows account settings

For daily admin workstations: always use **"No, this app only"**.

After signing in successfully, the Exchange Online V3 module loaded normally.

The console displayed information about REST-backed cmdlets and EXO V3.

That confirmed the session was connected properly.

### Step 2 — Checked Organisation-Level Setting

First, checked whether auto-expanding archive was enabled globally:

```powershell
Get-OrganizationConfig | FL AutoExpandingArchiveEnabled
```

Result:

```
AutoExpandingArchiveEnabled : True
```

So the tenant-level setting was already enabled.

**Important detail:**

Microsoft documentation mentions that enabling it globally does not automatically change the mailbox property to `True` on every mailbox.

This confuses many admins the first time they check it.

### Step 3 — Checked the User Mailbox

Checked mailbox-level status:

```powershell
Get-Mailbox "user@domain.com" | FL AutoExpandingArchiveEnabled
```

Result:

```
AutoExpandingArchiveEnabled : False
```

So organisation-level setting was enabled, but the mailbox itself still showed `False`.

### Step 4 — Enabled Auto-Expanding Archive

Enabled the feature for the mailbox:

```powershell
Enable-Mailbox "user@domain.com" -AutoExpandingArchive
```

Exchange returned mailbox information, confirming the command completed successfully.

### Step 5 — Verified Configuration

Checked organisation configuration again:

```powershell
Get-OrganizationConfig | FL AutoExpandingArchiveEnabled
```

Result:

```
AutoExpandingArchiveEnabled : True
```

At this point, the configuration was complete.

---

## Useful Commands

**Connect to Exchange Online**
```powershell
Connect-ExchangeOnline
```

**Check organisation-level setting**
```powershell
Get-OrganizationConfig | FL AutoExpandingArchiveEnabled
```

**Check mailbox setting**
```powershell
Get-Mailbox "user@domain.com" | FL AutoExpandingArchiveEnabled
```

**Enable auto-expanding archive**
```powershell
Enable-Mailbox "user@domain.com" -AutoExpandingArchive
```

**Check archive quota and status**
```powershell
Get-Mailbox "user@domain.com" | FL DisplayName,ArchiveStatus,ArchiveQuota,ArchiveWarningQuota,AutoExpandingArchiveEnabled
```

---

## What Can Go Wrong

### It is not instant

Users often expect the archive size to increase immediately.

That is not how this works.

Microsoft may take time to provision additional archive storage.

In some cases, it can take days before changes become visible.

### You cannot disable it later

Once auto-expanding archive is enabled, it cannot be disabled afterwards.

This is not a temporary switch.

### Outlook search can confuse users

Search inside expanded archives is not always intuitive.

Users may think emails disappeared, while Outlook is simply searching the wrong scope.

Classic Outlook especially likes turning this into another helpdesk ticket.

### Archive folders may move automatically

Microsoft can split archive content across additional storage locations automatically.

This can slightly change how archive folders appear inside Outlook.

Usually harmless.

Still enough to scare users.

### Not designed for mailbox dumping

Microsoft specifically says this feature is not intended for dumping massive amounts of mail into a single archive mailbox through journaling or transport rules.

---

## Pros and Cons

**Pros**

- Allows archive growth beyond 100 GB
- Can scale automatically up to 1.5 TB
- No manual archive expansion needed later
- Works well for long-term email retention

**Cons**

- Cannot be disabled later
- Expansion is not immediate
- Outlook search behaviour can become messy
- Users often panic when folder structure changes slightly

---

## References

Microsoft documentation:

- [Auto-expanding archiving overview](https://learn.microsoft.com/en-us/purview/autoexpanding-archiving)
- [Enable auto-expanding archiving](https://learn.microsoft.com/en-us/purview/enable-autoexpanding-archiving)

---

## TL;DR

User reported mailbox/archive issues because Online Archive was approaching its storage limit.

Connected to Exchange Online PowerShell and verified:

```powershell
Get-OrganizationConfig | FL AutoExpandingArchiveEnabled
```

Then enabled auto-expanding archive for the mailbox:

```powershell
Enable-Mailbox "user@domain.com" -AutoExpandingArchive
```

The archive can now grow automatically beyond the normal 100 GB limit.

Just do not expect it to happen instantly.
