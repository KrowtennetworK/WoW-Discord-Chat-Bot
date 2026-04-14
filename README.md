# WoW-Discord-Chat-Bot
This bot will allow you to type in discord and the chat will appear in-game in whatever channel you want.  Also if you join the bot's in-game character to your guild, you can relay your guild chat!

# WoWChat Setup for AzerothCore (Windows)

This guide shows how to set up **WoWChat** on a **Windows** machine for an **AzerothCore Wrath of the Lich King 3.3.5** server.

With this setup, players can chat between a Discord channel and in-game WoW channels like:

- LookingForGroup
- Guild chat
- Officer chat
- General / custom channels

This setup was tested on an AzerothCore-based private server and confirmed working.

---

## Features

- Discord -> WoW channel relay
- WoW -> Discord channel relay
- Two-way relay (`direction=both`)
- Guild chat relay
- Public/custom channel relay
- Works on Windows
- Uses a dedicated WoW bot character
- Uses a Discord bot account

---

## Requirements

Before starting, make sure you have:

- A working **AzerothCore 3.3.5** server
- A **Windows** machine to run WoWChat
- **Java 8+** installed  
  Java 17 x64 is recommended
- A **Discord server**
- A **Discord bot application**
- A dedicated **WoW bot account + character**

---

## 1. Install Java

WoWChat requires Java.

1. Install **Java 8 or newer**
2. Open Command Prompt
3. Confirm Java is installed:

```bat
java -version
