TediCross
=========
TediCross is a bot which bridges a chat in [Telegram](https://telegram.org) with a channel in [Discord](https://discordapp.com/).

There is no public TediCross bot. You need to host it yourself. To host a bot, you need [nodejs](https://nodejs.org). The bot requires NodeJS 6 or higher


TediCross News Channel
----------------------

We now have a Telegram channel where we post news about the bot! Join us at https://t.me/TediCross


Features & known bugs
---------------------

The bot is able to relay text messages and media files between Discord and Telegram. @-mentions, URLs, code (both inline and block-style) works well

For a list of known bugs, or to submit a bug or feature request, see this repo's "Issues" tab


Step by step installation:
--------------------------
Setting up the bot requires basic knowledge of the command line, which is bash or similar on Linux/Mac, and cmd.exe in Windows

 1. Install [nodejs](https://nodejs.org)
 2. Clone this git repo, or download it as a zip or whatever
 3. Enter the repo
 4. `npm install`
 5. Make a copy of the file `example.settings.json` and name it `settings.json`
 6. Aquire a bot token for Telegram ([How to create a Telegram bot](https://core.telegram.org/bots#3-how-do-i-create-a-bot)) and put it in the settings file
   - The Telegram bot must be able to access all messages. Talk to [@BotFather](https://t.me/BotFather) to disable privacy mode for the bot
 7. Aquire a bot token for Discord ([How to create a Discord bot](https://github.com/reactiflux/discord-irc/wiki/Creating-a-discord-bot-&-getting-a-token)) and put it in the settings file
 8. Add the Telegram bot to the Telegram chat
   - If the Telegram chat is a supergroup, the bot also needs to be admin of the group, or it won't get the messages. The creator of the supergroup is able to give it admin rights
 9. Add the Discord bot to the Discord server (https://discordapp.com/oauth2/authorize?client_id=YOUR_CLIENT_ID_HERE&scope=bot&permissions=248832). This requires that you have admin rights on the server
 10. Start TediCross: `npm start`
 11. Ask the bots for the remaining details. In the Telegram chat and the Discord channel, write `@<botname> chatinfo`. Put the info you get in the settings file
 12. Restart TediCross

Done! You now have a nice bridge between a Telegram chat and a Discord channel


Settings
--------

As mentioned in the step by step installation guide, there is a settings file. Here is a description of what the settings do.

* `telegram`: Object authorizing and defining the Telegram bot's behaviour
	* `auth.token`: The Telegram bot's token. It is needed for the bot to authenticate to the Telegram servers and be able to send and receive messages. If set to `"env"`, TediCross will read the token from the environment variable `TELEGRAM_BOT_TOKEN`
	* `useFirstNameInsteadOfUsername`: **EXPERIMENTAL** If set to `false`, the messages sent to Discord will be tagged with the sender's username. If set to `true`, the messages sent to Discord will be tagged with the sender's first name (or nickname). Note that Discord users can't @-mention Telegram users by their first name. Defaults to `false`
	* `colonAfterSenderName`: Whether or not to put a colon after the name of the sender in messages from Discord to Telegram. If true, the name is displayed `Name:`. If false, it is displayed `Name`. Defaults to false
	* `skipOldMessages`: Whether or not to skip through all previous messages cached from the telegram-side and start processing new messages ONLY. Defaults to true. Note that there is no guarantee the old messages will arrive at Discord in order
* `discord`: Object authorizing and defining the Discord bot's behaviour
	* `auth.token`: The Discord bot's token. It is needed for the bot to authenticate to the Discord servers and be able to send and receive messages. If set to `"env"`, TediCross will read the token from the environment variable `DISCORD_BOT_TOKEN`
	* `skipOldMessages`: Whether or not to skip through all previous messages sent since the bot was last turned off and start processing new messages ONLY. Defaults to true. Note that there is no guarantee the old messages will arrive at Telegram in order. **NOTE:** [Telegram has a limit](https://core.telegram.org/bots/faq#my-bot-is-hitting-limits-how-do-i-avoid-this) on how quickly a bot can send messages. If there is a big backlog, this will cause problems
* `debug`: If set to `true`, activates debugging output from the bot. Defaults to `false`
* `bridgeMap`: An array containing all your chats and channels. For each object in this array, you should have the following properties:
	* `name`: A internal name of the chat. Appears in the log
	* `telegram`: ID of the chat that is the Telegram end of this bridge. See step 11 on how to aquire it
	* `discord.guild`: ID of the server the Discord end of the bridge is in. If a message to the bot originates from within this server, but not the correct channel, it is ignored, instead of triggering a reply telling the sender to get their own bot. See step 11 on how to aquire it
	* `discord.channel`: ID of the channel the Discord end of the bridge is in. See step 11 on how to aquire it

The available settings will occasionally change. The bot takes care of this automatically

FAQ
---

### What kind of machine do I need to run this?

Anything capable of running [NodeJS](https://nodejs.org) should be able to run TediCross. People have had success running it on ordinary laptops, raspberry pis, Amazon Web Services, Google Cloud Platform, and other machines. It runs on both Linux and Windows, and probably also macOS. It does NOT, however, run on [Heroku](https://heroku.com)

The machine must be on for TediCross to work

### Just how much knowledge of the command line do I need to get the bot working?

Not much at all. Almost all the commands are written in the installation guide exactly as they should be entered. The only thing you need to know in addition is the [`cd`](https://en.wikipedia.org/wiki/Cd_(command)) command, in order to navigate to wherever you unpacked TediCross

### The bot just responds with a generic message telling me to get my own TediCross instance

This happens when you have not entered correct chat IDs in the settings file. See step 11 in the step by step installation guide for instructions on how to get these.

A small gotcha here is that Telegram group chats always have a negative chat ID. Remember to include the "-" in the settings file!

### The Telegram bot doesn't relay messages sent by other bots

The Telegram team unfortunately decided that bots cannot interact with each other, fearing they would get stuck in infinite loops. This means it is impossible, under any circumstances, for TediCross to relay messages from other Telegram bots to Discord. Discord does not have this limitation, and the Discord side of the bot will happily relay messages from other Discord bots to Telegram

See https://core.telegram.org/bots/faq#why-doesn-39t-my-bot-see-messages-from-other-bots

### When running `npm install`, it complains about missing dependencies?

The [Discord library](https://discord.js.org/#/) TediCross is using has support for audio channels and voice chat. For this, it needs some additional libraries, like [node-opus](https://www.npmjs.com/package/node-opus), [libsodium](https://www.npmjs.com/package/libsodium) and others. TediCross does not do audio, so these warnings can safely be ignored

### TediCross spams errors in the console saying "terminated by other long poll or web hook"

This happens when two applications use the same Telegram bot token, or someone has set a webhook on the Telegram bot token. You may simply have accidently launched two instances of TediCross, or someone else has somehow gotten hold of your token

If you haven't accidently launched two instances of TediCross, assume the token is compromised. First, talk to [@BotFather](https://t.me/BotFather) to generate a new token for the bot. Then go to https://api.telegram.org/bot<TOKEN>/deleteWebhook (with `<TOKEN>` replaced with your actual token) to get rid of any webhook set for the bot. Then update the settings file, and restart the bot

### How do I update TediCross?

Most updates are annouced on the [TediCross News channel](https://t.me/TediCross). Only very minor ones are not

If you cloned the git repo, just do a `git pull`. Running `npm install` may or may not be necessary. It doesn't hurt to run it anyway

If you downloaded TediCross as a zip, do step 2, 3 and 4 in the installation guide again. Then move `settings.json` and the whole `data/` directory from the old version to the new one and start it.


Other questions?
----------------

If you need any help, ask [@Suppen](https://t.me/Suppen) on Telegram
