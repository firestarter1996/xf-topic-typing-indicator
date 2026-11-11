# 💬⌨️👀 Topic Typing Indicator

A plugin for Nagram XF. In a forum group, every topic shows its own typing indicator, even when people type in several topics at the same time. The topic tabs get animated typing dots too.

## Why I made it

I keep a forum group with a topic per project, and a bot that answers in several topics at once. Stock Nagram XF shows typing for only one topic of a group at a time, so the topic I was sitting in often looked idle while the bot was writing in it. The topic tabs never showed typing at all.

## What it changes

Forum groups only:

* Every topic with someone typing shows "is typing" in its own header, at the same time as the other topics.
* Every topic tab with someone typing shows animated dots over its title. This works for the tabs across the top and the tabs down the side.
* The wording, names, emoji and actions (typing, recording, sending a photo and so on) are the same as stock. Blocked users stay hidden when that option is on.
* Private chats, normal groups and channels behave exactly as before. A group where only one topic has someone typing is left to stock.

Setting: the tab dots can be switched off. The header fix is always on.

## Install

1. Use the plugin build of Nagram XF and turn on the plugin engine (Settings, N Settings, Plugins).
2. Download `topic_typing.plugin` from the latest release.
3. Send it to your Saved Messages, tap it and choose install. You can also use the plus button on the Plugins screen.
4. Make sure Topic Typing Indicator is switched on.

## How it works

`MessagesController.updatePrintingStrings` walks every group and, inside the loop over that group's topics, puts a new empty map for the group on every pass. So when people type in two or more topics of one group, every topic but the last loses its typing text. Nagram fixed this upstream; Nagram XF 12.10.1 does not have the fix yet.

The plugin hooks `updatePrintingStrings`. After each stock run it rebuilds the typing text of every topic in any group with typing in two or more topics, with the stock wording, and posts it on the UI thread right after the stock result, then asks for a redraw so the open header picks it up. It also keeps its own list of which topics have someone typing, and draws a `TypingDotsDrawable` on each matching tab (`TopicsTabsView` horizontal and vertical tab views). Every decision goes to logcat under the tag `TopicTyping`.

## Tested

Nagram XF 12.10.1 plugin build, plugin SDK 1.4.6.7, on a Pixel 6 Pro running the same build and settings as my daily phone. The test forum group used the tabs layout, like my real group, and a bot sent typing into the Start, DIR and WSL2 topics at the same time while I sat in Start.

* Stock, plugin off: five screenshots over 20 seconds, no tab showed typing, and the Start header never said typing, though the bot was typing in Start the whole time.
* Plugin on: the Start, DIR and WSL2 tabs showed typing dots in the same frame, and the Start header said "is typing" whenever the bot was typing in Start.

Not covered by that test: the side tabs layout and the topic list screen. The code handles both, but I only checked the top tabs on screen.

## Background use (1.0.6)

Android closed Nagram XF for using too much CPU in the background (43.6 seconds in 5 minutes on my phone), and part of that was this plugin. Since 1.0.5 the once a second dots alignment stops when the app goes to the background and when no dots are on screen, and starts again when you come back. Since 1.0.6 every typing dots animation is paused while the app is in the background and resumed when you return, so no typing indicator asks for frames while nobody can see it.

## License

MIT

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=firestarter1996/xf-topic-typing-indicator&type=Date)](https://star-history.com/#firestarter1996/xf-topic-typing-indicator&Date)
