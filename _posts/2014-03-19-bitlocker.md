---
layout: post
title: USB Key + Bitlocker
---

I wanted to decrypt a USB key which had been encrypted by Bitlocker as it was becoming a PITA to use. It had been encrypted on Windows 7 Enterprise but  the PC I have at home has Windows 7 Professional and I couldn't find anything obvious that would allow me to do it until I found this ...

> manage-bde -off <volume:>

Run as administrator from the command line. Job done.