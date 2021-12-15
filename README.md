# 7dtd-Discordian-Linux

A Discord bot designed to integrate with ServerTools server manager for 7 Days to Die dedicated servers.

The purpose of this program is to bridge the chat services between discord and the 7 days to die game server of choice utilizing ServerTools as web api. 
It allows for multiple servers to operate through a single bot.
The program operates as a stand alone executable. 
This may not be allowed on host services. 
Make sure you have permission to run executable files before setting up the bot and wasting your time.

Prerequisites:  You must have tmux installed on your Linux server
    Ubuntu: sudo apt get install tmux
    CentOS: sudo yum -y install tmux

If you can not run a linux based executable file, you can still use ServerTools for game chat to be sent outbound to a Discord channel of your choice.
However you will not be able to send Discord chat inbound to the game server.

Instructions for both are as follows:

INBOUND CHAT FROM DISCORD TO SERVER

STEP 1

EXPLOSIONS!
Just kidding.

Make sure you are using version 19.6.9 or higher of ServerTools. If you are not, download and install the latest version at 
https://github.com/dmustanger/7dtd-ServerTools/releases

Other features and tools from ServerTools do not need to be utilized if you do not wish to. It is required for
communications between the game and discord to function however.

After you have verified your ServerTools version and installation, you may start your server as you would normally.

If you have not used ServerTools before, it will create a new folder inside your main server directory named ServerTools. Inside of this folder you will
find the main ServerToolsConfig.xml that is used to control ServerTools options. Open this file and scroll to the section that says Tool Name="Discord_Bot".

Enable this by setting the Enable option to true, as seen in this example. Enable="True". Save the file. ServerTools will now generate a file named
DiscordToken.txt and place it inside of the ServerTools folder you found your ServerToolsConfig file. You will need this later.

Look for Tool Name="Web_API" and enable it as well. It will use the port you have set as the Port option. This port must be open. If you are using a host,
this port will need to be opened for you. Not all hosts will allow this. If your host will not, do not continue this process. The ServerTools Web API and
Discordian bot require an open port for communications and can not function without it.

Take the Discordian bot files out of the zip file and place them in their own folder of your choice. You can place them anywhere.

Open your web browser to the following address https://discord.com/developers/applications

Make sure you are logged in to your Discord account and then select the New Application button. This will create a new application under your profile.
Name it and place the description of your choice.

On the left side of the screen is a few tabs named General Information, 0Auth2, Bot, Rich Presence and App Whitelist. Click on the Bot tab.

You will now see a new section that covers bots for your application. Select add bot / create bot. It will warn you that this is permanent and can not be 
undone. Accept the warning.

You have created your first bot for this application. You can name it anything you like.

There is an important section under the bot called Token. It is kept hidden on purpose. You can reveal it to read it if you wish but DO NOT SHARE IT!
It is important to never share this token with anyone else. If you ever feel it has been compromised, come back to this page/application, then bot and
regenerate a new token. This will void the old token, making it unusable. You will also need to use the new token it generates instead of the old one.

Go to the left side tabs and select 0Auth2. Scroll down to the section named SCOPES. You will see multiple boxes you can select under this. Choose bot.
Scroll down further to see the permissions given to this bot. Select at minimum the following: View Channels, Send Messages, Embed Links, Attach Files,
Read Message History, Use External Emoji, Add Reactions. You can select more options if you wish but the bot and the features planned will only utilize
these.

Between the section you selected bot and the permissions section is an http link with a big copy button beside it. Copy this link and paste it in to your
web browser. It will load a new page where you can add your new bot to your guild. Another name for a Discord server is a guild. If you are confused by 
the term guild, it is in reference to the Discord server you are adding the bot to.

You should now have an offline bot in your guild with the name you set for your bot. You can go back and change this name at any time at the applications
link from earlier.

Locate the Config.xml that was provided with the Discordian files you placed in its own folder. Open this file. Take notice it has two examples surrounded
by comment tags that look like this <!--  -->. Anything between these tags are treated as a comment and ignored. At the top you will notice a
Discord Token that is blank. You want to take the bot token off the developers application page from earlier that you were warned should not be shared
with anyone else and place it here as the Discord Token. Example: <Discord Token="54321TU4MTI0NjU4ODg0NjE5.12345.KLcfcItdwwlJ0EmTFYD12345-Ok" />

Remove the comment tags from the first entry and add your game server ip address as the Server IP. Add the port that ServerTools Web API is utilizing and
should be open at this time.

Locate the DiscordToken.txt that was generated by ServerTools upon enabling the Discord_Bot tool earlier. It can be found in your ServerTools folder.
Take the token and place it in the Config.xml as the ServerTools_Token. Example: ServerTools_Token="123454t8Un/+bndU2Yt4PU0CeTZ8CyACkjlC+54321="

You will need the channel id of the channel you wish chat to pass back and forth between. Go to the list of channels in your guild and right click the
channel. You should see an option called copy id. If you do not it is because you must enable developer mode in Discord. Access your user settings. Under
the advanced tab you will find the option Developer mode. Enable this option and return to the list of channels. Right click on the channel you wish
to pass chat between and click copy id. Place this id in the Config.xml as the Channel_Id. Example: Channel_ID="123425001664774321"

Select a command prefix that will be used in Discord to trigger special commands. Example: Command_Prefix="#"

The final entry should look like this:
<Server IP="123.1.2.3" Port="8084" ServerTools_Token="123454t8Un/+bndU2Yt4PU0CeTZ8CyACkjlC+54321=" Channel_ID="123425001664774321" Command_Prefix="!" />

Save the file.

With your game server running and Discord_Bot and Web_API tools enabled, you can now get ready to run the bot. It will use your Config.xml to establish a
connection to the correct guild based on the Discord Token and the game server based on the IP and port. It will only try to pass chat messages
from the channel id you have specified. If this channel id is incorrect, you will be unable to connect nor send messages in to game.

You can setup multiple servers in the same manner when running ServerTools. It only requires one bot using one bot token to handle all of them. Each 
ServerTools must generate its own secure token and have access through the Web API to function. They must all use individual ports for communications when
hosted behind a single IP address.

To get started, run: chmod +x Discordian-Core

This will enable your linux system to run it as an executable.

Now to start the bot, run: tmux new-session -d -s "discordian" ./Discordian-Core
To stop the bot, run: tmux kill-session -t discordian

The start command will need to be re-run each time the server is rebooted, but not when the game server is restarted as it runs independantly from the game server.

Discordian will run as a service and operates an output log for reference and exception errors. It will auto reconnect to discord and the game server. Server restarts will
not effect the bot as it will reconnect when available. You will need to disable it in your services to turn it off.

As additional features, you may set the message prefix/color, name color and message color that the message will utilize when sent from Discord to game.
These are found under tool name Discord_Bot_Extended

OUTBOUND CHAT FROM SERVER TO DISCORD

STEP 1

Fire!
Maybe kidding?

The bot is NOT required to be active or setup for outbound chat from the game server to be sent in to discord. Discord offers a feature called a webhook.
We will be utilizing this as it is far more efficient than passing data through the bot itself.

Go to your list of Discord channels and choose the one you wish to have game chat be sent to. Click to edit the channel and then select Integrations. 
There you will see a webhook section. Click create webhook. It will create a semi hidden bot to this channel that will handle the webhook. Name it as you
wish but it will utilize this name when posting messages to the channel. If you wish to change this name, you can alter the webhook bot name at any time.

You will see an option to click named copy Webhook URL. Copy this URL. Access your ServerToolsConfig.xml found in your ServerTools directory. Locate the
tool named Discord_Bot and you will find an option named Webhook. Place your webhook URL here and save the file.

If your Discord_Bot tool is enabled and the webhook is valid, all in game chat will now be sent to the channel you chose.
