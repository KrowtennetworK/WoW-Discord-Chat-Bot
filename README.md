# WoWChat Setup Guide for AzerothCore 3.3.5 on Windows

This guide shows you how to set up WoWChat on a Windows PC for an AzerothCore Wrath of the Lich King 3.3.5 server.

It is written for people with no prior experience with Java, Discord bots, or config files.

When you are done, you will be able to:

- send messages from Discord into WoW
- see messages from WoW in Discord
- relay custom channels like LookingForGroup
- relay Guild chat both ways
- expand later into Officer chat, announcements, and more

--------------------------------------------------
WHAT WOWCHAT DOES
--------------------------------------------------

WoWChat is a bridge between a Discord server and an older World of Warcraft server.

It logs into Discord using a Discord bot, and logs into your WoW server using a real WoW account and character. Once both sides are connected, it relays chat between them.

Important: Do not use this on a WoW account that has characters you care about. Create a dedicated bot account and bot character for this setup.

--------------------------------------------------
WHAT YOU NEED BEFORE YOU START
--------------------------------------------------

You will need:

- a working AzerothCore 3.3.5 server
- a Windows computer to run WoWChat
- a Discord server
- permission to manage that Discord server
- a dedicated WoW account and character for the bot
- Java 17 installed on Windows
- the WoWChat files downloaded and extracted

--------------------------------------------------
PART 1 - INSTALL JAVA ON WINDOWS
--------------------------------------------------

WoWChat runs on Java, so Java must be installed first.

Step 1: Download Java

Open this link in your browser:

https://adoptium.net/temurin/releases/?version=17

Step 2: Choose the correct download

On the download page, choose:

Operating System: Windows
Architecture: x64
Package Type: JDK
File Type: .msi

That is the easiest installer for most Windows users.

Step 3: Run the installer

Double-click the .msi file you downloaded.

During setup, make sure these options are enabled if you see them:

Add the installation to PATH
Update JAVA_HOME

Then finish the install.

Step 4: Confirm Java works

Open a new Command Prompt window and type:

java -version

If Java installed correctly, Windows will show a version number.

Example:

openjdk version "17.x.x"

If you get an error saying java is not recognized, close Command Prompt, reopen it, and try again.

--------------------------------------------------
PART 2 - CREATE A DISCORD BOT
--------------------------------------------------

Now you need to create the Discord bot that WoWChat will use.

Step 1: Open the Discord Developer Portal

Go here:

https://discord.com/developers/applications

Log in with your Discord account if needed.

Step 2: Create a new application

Click:

New Application

Give it a name. Example:

WoWChat

Then click:

Create

Step 3: Add a bot user

In the left sidebar, click:

Bot

Then click:

Add Bot

Confirm it.

Step 4: Copy the bot token

Still on the Bot page, find the token section.

Click:

Reset Token

or

Copy Token

Save that token somewhere safe for now. You will paste it into wowchat.conf later.

Never post your Discord bot token publicly. Anyone with the token can control your bot.

Step 5: Turn on the required intents

Still on the Bot page, scroll down to:

Privileged Gateway Intents

Turn on all 3:

Presence Intent
Server Members Intent
Message Content Intent

Then click:

Save Changes

These are required for WoWChat to work correctly.

--------------------------------------------------
PART 3 - INVITE THE DISCORD BOT TO YOUR SERVER
--------------------------------------------------

Creating the bot is not enough. You must also add it to your Discord server.

Step 1: Open the Installation page

In the left sidebar of the Developer Portal, click:

Installation

Step 2: Configure install settings

Set it up like this:

Installation Contexts
Guild Install = On
User Install = Off

Install Link
Discord Provided Link

Default Install Settings

Under Guild Install, add:

bot

You do not need slash commands for basic WoWChat setup, so this is optional:

applications.commands

Step 3: Set the bot permissions

Enable at least:

View Channels
Send Messages
Read Message History

Step 4: Save and invite

Click:

Save Changes

Copy the install link shown on the page.

Open that link in your browser.

Choose your Discord server and authorize the bot.

Step 5: If Discord does not show an install link

If no invite link appears, use a manual invite URL.

Replace YOUR_CLIENT_ID_HERE with your bot application's client ID:

https://discord.com/oauth2/authorize?client_id=YOUR_CLIENT_ID_HERE&scope=bot&permissions=68608

The permission number 68608 gives the bot:

View Channels
Send Messages
Read Message History

Step 6: Confirm the bot is in your server

Open your Discord server and make sure the bot appears in the member list.

If it does not, the invite did not finish correctly.

--------------------------------------------------
PART 4 - CREATE THE DISCORD CHANNEL YOU WANT TO USE
--------------------------------------------------

Make a text channel in Discord for the relay.

Example channel names:

wow-lfg
guild-chat

Make sure the bot can access that channel.

Check channel permissions

Right-click the channel and open:

Edit Channel -> Permissions

Make sure the bot has:

View Channel
Send Messages
Read Message History

If the bot cannot see the channel, WoWChat will fail even if the bot was invited correctly.

--------------------------------------------------
PART 5 - COPY THE DISCORD CHANNEL ID
--------------------------------------------------

WoWChat works best when you use the Discord channel ID instead of the channel name.

Step 1: Enable Developer Mode in Discord

In Discord, go to:

User Settings -> Advanced -> Developer Mode

Turn it on.

Step 2: Copy the channel ID

Right-click the Discord text channel you want to use.

Click:

Copy Channel ID

Save that number. You will paste it into wowchat.conf.

--------------------------------------------------
PART 6 - CREATE THE WOW BOT ACCOUNT AND CHARACTER
--------------------------------------------------

WoWChat also needs a real WoW login.

Step 1: Create a dedicated WoW account

Create a new WoW account just for the bridge.

Do not use your personal account.

Example:

Account: Discord
Password: yourpasswordhere

Step 2: Create a character on that account

Log into your server and create a character for the bot.

Example:

Character: VoiceOfGod

Step 3: Put the character where it needs to be

Log into the bot character once manually and make sure it can:

- log in normally
- join the chat channel you want to relay
- speak in that chat channel

For example, if you want to relay LookingForGroup, make sure the bot character can join and talk in that channel.

Tip: It is usually best to use a normal player account for the bot instead of a GM account.

--------------------------------------------------
PART 7 - DOWNLOAD AND EXTRACT WOWCHAT
--------------------------------------------------

Download WoWChat from the project releases page and extract it somewhere easy to find.

Project page:

https://github.com/KrowtennetworK/WoW-Discord-Chat-Bot

Example folder:

C:\WoWChat\

Inside that folder you should have files like:

wowchat.jar
wowchat.conf
run.bat

--------------------------------------------------
PART 8 - EDIT WOWCHAT.CONF
--------------------------------------------------

Open wowchat.conf in a text editor such as Notepad or Notepad++.

Replace the whole file with this example:

# =========================
# WoWChat - Windows / AzerothCore WotLK 3.3.5
# Beginner-friendly working example
# =========================

discord {
  # Paste your Discord bot token here
  token="PASTE_YOUR_DISCORD_BOT_TOKEN_HERE"

  # Set to 0 to stop Discord messages starting with a dot from acting like in-game GM commands
  enable_dot_commands=0

  # Discord channels where dot commands are allowed
  # Not important for basic setup, but safe to keep limited
  enable_commands_channels=[
    "PASTE_YOUR_DISCORD_CHANNEL_ID_HERE"
  ]

  enable_tag_failed_notifications=0
  item_database=""
}

wow {
  # Leave this as Mac unless your server blocks Mac logins and also has Warden disabled
  platform=Mac

  enable_server_motd=1

  # Your WoW server version
  version=3.3.5

  # Your realmlist host
  realmlist=YOUR_REALMLIST_HERE

  # The exact realm name shown on the character list screen
  realm="YOUR_REALM_NAME_HERE"

  # Your dedicated WoW bot account
  account="YOUR_BOT_ACCOUNT_HERE"
  password="YOUR_BOT_PASSWORD_HERE"
  character="YOUR_BOT_CHARACTER_HERE"
}

guild {
  online {
    enabled=0
    format="[%user] has come online."
  }
  offline {
    enabled=0
    format="[%user] has gone offline."
  }
  promoted {
    enabled=0
    format="[%user] has promoted [%target] to [%rank]."
  }
  demoted {
    enabled=0
    format="[%user] has demoted [%target] to [%rank]."
  }
  joined {
    enabled=0
    format="[%user] has joined the guild."
  }
  left {
    enabled=0
    format="[%user] has left the guild."
  }
  removed {
    enabled=0
    format="[%target] has been kicked out of the guild by [%user]."
  }
  motd {
    enabled=0
    format="Guild Message of the Day: %message"
  }
  achievement {
    enabled=0
    format="%user has earned the achievement %achievement!"
  }
}

chat {
  channels=[
    {
      # Relay your public channel both ways
      direction=both
      wow {
        type=Channel
        channel="INSERT IN_GAME CHAT CHANNEL NAME HERE"
        format="[%user]: %message"
      }
      discord {
        channel="PASTE_YOUR_DISCORD_CHANNEL_ID_HERE"
        format="[%user]: %message"
      }
    }
    {
      # Relay guild chat both ways
      direction=both
      wow {
        type=Guild
        format="[%user]: %message"
      }
      discord {
        channel="PASTE_YOUR_GUILD_DISCORD_CHANNEL_ID_HERE"
        format="[%user]: %message"
      }
    }
  ]
}

filters {
  enabled=0
  patterns=[]
}

--------------------------------------------------
PART 9 - REPLACE THE PLACEHOLDER VALUES
--------------------------------------------------

Now edit the example config and replace these values with your real ones.

Discord section

Replace:

PASTE_YOUR_DISCORD_BOT_TOKEN_HERE

with your Discord bot token.

Replace:

PASTE_YOUR_DISCORD_CHANNEL_ID_HERE

with the channel ID you copied from Discord.

Replace:

PASTE_YOUR_GUILD_DISCORD_CHANNEL_ID_HERE

with your guild chat Discord channel ID if you want a separate guild relay.

WoW section

Replace:

YOUR_REALMLIST_HERE

with your realmlist host.

Example:

123.45.678

Replace:

YOUR_REALM_NAME_HERE

with the exact realm name shown on your realm list.

Replace:

YOUR_BOT_ACCOUNT_HERE
YOUR_BOT_PASSWORD_HERE
YOUR_BOT_CHARACTER_HERE

with the bot account and character details you created.

--------------------------------------------------
PART 10 - SAVE THE FILE
--------------------------------------------------

Save the file as:

wowchat.conf

Make sure it stays in the same folder as:

wowchat.jar
run.bat

--------------------------------------------------
PART 11 - START WOWCHAT
--------------------------------------------------

Open the WoWChat folder.

Double-click:

run.bat

Or open Command Prompt in that folder and run:

java -jar wowchat.jar wowchat.conf

If everything is correct, WoWChat should:

- connect to Discord
- log into your WoW server
- log into the bot character
- join the configured WoW channel
- begin relaying chat

--------------------------------------------------
PART 12 - TEST THE RELAY
--------------------------------------------------

Test Discord -> WoW

1. Open the linked Discord channel
2. Type a message
3. Check the matching WoW chat channel in-game

Test WoW -> Discord

1. Log into another player character in WoW
2. Join the same channel the bot is relaying
3. Type a message
4. Check Discord to see if it appears there

If you set this in the config:

direction=both

both directions should work.

--------------------------------------------------
PART 13 - COMMON PROBLEMS AND FIXES
--------------------------------------------------

Problem: java is not recognized

Java is not installed correctly, or PATH was not added.

Fix

Reinstall Temurin Java 17 and make sure the installer adds Java to PATH.

Then open a brand new Command Prompt window and run:

java -version

Problem: Discord says privileged intents are missing

You did not enable all three required intents.

Fix

Go to:

Discord Developer Portal -> Your App -> Bot

Turn on:

Presence Intent
Server Members Intent
Message Content Intent

Save changes, then restart WoWChat.

Problem: The bot is online in Discord but not talking in the server

Check these values in wowchat.conf:

realmlist
realm
account
password
character

A typo in any of those can stop WoW login.

Problem: Discord -> WoW works, but I do not see the message in-game

Check these things:

- the relay channel in the config matches the actual WoW channel
- your player is watching the same channel
- the bot actually joined the correct WoW channel
- the bot can speak in that channel

If you are testing LookingForGroup, make sure the player you are watching with is also in LookingForGroup.

Problem: WoW -> Discord does not work

Make sure the relay uses:

direction=both

or:

direction=wow_to_discord

Also check that the Discord channel ID is correct and the bot can see that channel.

Problem: You don't know that language

The bot character may not be able to speak in that WoW chat normally.

Fix

Log into the bot character manually and test the chat channel yourself.

If the character cannot speak there manually, fix that first before testing WoWChat again.

--------------------------------------------------
PART 14 - SECURITY TIPS
--------------------------------------------------

Never upload or post any of these publicly:

- your Discord bot token
- your WoW bot account password
- your private server login details

If you ever paste them publicly by mistake:

- reset the Discord bot token immediately
- change the WoW bot account password
- update wowchat.conf

--------------------------------------------------
PART 15 - MAKE A BACKUP ONCE IT WORKS
--------------------------------------------------

As soon as your bridge is working, make a backup copy of your config.

Example:

wowchat-working-backup.conf

That way, if you break the config later, you can restore it quickly.

--------------------------------------------------
PART 16 - GOOD NEXT FEATURES TO ADD
--------------------------------------------------

Once the basic setup works, you can expand it with:

- Guild chat relay
- Officer chat relay
- General chat relay
- achievement announcements
- guild join/leave notices
- spam filtering
- multiple Discord channels
- item links to Discord

--------------------------------------------------
QUICK REFERENCE
--------------------------------------------------

Java test

java -version

Start WoWChat

java -jar wowchat.jar wowchat.conf

Discord settings you must enable

- Presence Intent
- Server Members Intent
- Message Content Intent

Best practice

- use a dedicated WoW bot account
- use Discord channel IDs instead of channel names
- back up your working config

--------------------------------------------------
CREDITS
--------------------------------------------------

- WoWChat
- AzerothCore
- Eclipse Temurin / Adoptium
- Discord Developer Portal
