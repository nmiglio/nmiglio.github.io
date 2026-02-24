---
layout: article
title: "Permanently Disable Virtualization-Based Security (VBS) in Windows 11"
date: 2026-02-22
modified: 2026-02-24

---

Virtualization-based security, aka **VBS**, allows Windows to create a secure memory enclave that's isolated from unsafe code. Another built-in feature called Hypervisor-Enforced Code Integrity (HVCI) uses the capabilities of VBS to prevent unsigned or questionable drivers and software from getting into memory. Together VBS and HVCI add a layer of protection that limits how much damage malware can do, even if it gets past your antivirus software.

So why would you think about disabling it? Well.. **Virtualization Based Security can slow apps (and games) down by 5 to 15%**. 
Yep, you read it. Are you OK with that? I was not. In addition I need to run VirtualBox to spin a few virtual machines. When VBS is enabled the performance of the VM are throttled down. And I wanted to squeeze all the computational power available. The risk should not be so high if you know what you are doing.. You can decide for yourself, but I wanted out. So, how do I disable it? It should be an easy toggle to switch somewhere.. Well here is where the fun begins.

### There is no simple toggle to switch

Alright, so I decided to reclaim those precious frames per second. You'd think in this day and age, there'd be a big, shiny "Go Faster" button in the Windows settings. You're partially right. My first stop on this adventure was the "Core Isolation" page in Windows Security settings (search for it in the Start Menu). In there, there is a toggle for "Memory Integrity." I switched it off. "Ha!" I thought, "That was easy!" A quick reboot with a smug grin on my face..

### Opting out is not that easy

After rebooting, I checked if it worked. Command Prompt. I typed `systeminfo`. I looked at the "Virtualization-based security" line. **"Running"**. "Not that easy..". At this point, I almost took the `regedit` path, but in my experience it is always a descent into a frustrating rabbit hole of editing obscure keys you don't really know what they are doing. You'll find guides telling you to hunt down keys like `EnableVirtualizationBasedSecurity` under `DeviceGuard`.

Let me save you the trouble: **don't bother**. While you can manually change these registry keys, it's a temporary fix at best.

### After a reboot we have to start over

Here’s the frustrating part. Even if you get the registry settings just right, the change often won't survive. A Windows update, a driver installation, or even a stern look from your motherboard's firmware can be enough for Windows to "helpfully" re-enable VBS on the next reboot. This is often because of deep-system settings that consider its absence a "problem" to be "fixed." This is the point where you either give up or bring out the big guns.

### The Definitive Fix

I was determined to find the correct and permanent way to handle this is. and I found it in a powerful PowerShell script provided by Microsoft itself, called the 'Device Guard and Credential Guard readiness tool'.

This script was the final word on the matter. It does everything the right way. I ran it with the `-Disable` flag. It cleaned up not only all the necessary registry keys automatically (so you I didn't have to), but it also performed some pre-boot magic. Using the Boot Configuration Data (`bcdedit`) tool it scheduled a special task for the next boot telling my computer: "Hey, before you even load Windows next time, run this special program (`SecConfig.efi`) and turn off VBS at the firmware level." This was it.

### The Final Boss: The Reboot Prompt

This was the crucial step that the script set up. After running the script and rebooting, I didn't go straight to Windows. I was greeted by a confirmation screen asking if I wanted to disable these security features.

**HELL YEAH!** I had to press a specific key (I think it was F3) to confirm. This was the system's final check, ensuring that I - the human in command - was authorizing this change. Once confirmed, the system finished booting.

### The check after reboot

Then came final moment of truth. Once back in Windows, I opened a Command Prompt and typed `systeminfo` once again. I scrolled down the list and found the line for "Virtualization-based security". And there I saw the confirmation that I had won the battle this time: I could read the two most beautiful words in the English language: "Not enabled".

### Done

And this was it.I successfully navigated the labyrinth of Windows settings, bypassed the registry trap, and used the right crushing tool to disable VBS for good. Now I can go enjoy the extra horsepower and smoother performance. 

I think I earned them.



