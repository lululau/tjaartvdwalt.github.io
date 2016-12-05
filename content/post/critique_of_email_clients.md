+++
date = "2016-12-05T10:07:45"
image = ""
math = true
tags = ["opinion"]
title = "A critique of email clients"

+++
This might sound crazy, but in 2016, but I have not found an email client that I really want to use.

Let me walk through the mail clients I have tried, what are good about them, and what I would like to see changed.

## Critisism of current clients ##

### Thunderbird ###

I really like Thunderbird, but I do think Mozilla does not give it the attention it deserves, and it seems to play second fiddle to Firefox. For example, it uses the same plugin architecture as Firefox, but the available plugins are not comparable in number or quality to that of Firefox. In some cases it also allows you to install "Firefox specific" plugins that do not really work in the Thunderbird context. It also lacks a mobile companion.

### GMail ###

When I talk about GMail I really refer to 2 separate things: The GMail web client, and the mobile applications for Android/iOS.

On the web client: NO! Stop it! You should not read your email in a browser!

My main criticism of the web client is that it only works for GMail managed domains, and you can only view one mailbox at a time, so no unified inbox. It also does not have a plugin system (other than some experimental features Google allows you to enable manually).

On the mobile client: I really like it. After a recent update, you can use it as a POP/IMAP client for arbitrary mailboxes, giving you the unified inbox lacking in the web client. But alas, it does not support plugins. Using the web and the mobile client together, or even using Thunderbird with the mobile client is a fairly good option, but you have to live with the fact that each have different capabilities, making your experience much different based on the platform.

### K9 Mail ###

This is a lesser known, but great, email client for Android. Unfortunately it is only available for the one platform.

### mu ###

Not many people outside of the Emacs community would have heard of this, but it used to be my primary email client for a number of years. It is likely the most customizable "email searcher" ever. You can write elisp functions to accomplish just about anything. It does have problems though:

- It does not handle the downloading of mail, for that you have to use an external mail synchronization tool such as offlineImap or mbsync.
- You are tied to Emacs. It goes without saying that there is no mobile version.
- Its not exactly what I would call "modern". 
- Tinkering with configuration takes a lot of time, and sharing configuration is not always simple.

## My ideal mail client ##

Taking all of these criticisms into consideration, what would my perfect mail client to look like?

### It should work on most platforms ###

It should have a mobile, desktop and web interface. Ideally it should also be easily portable to new platforms. As far as possible these interfaces should have similar capabilities.

### It should be extensible ###

In my mind `mu`  sets the bar that any mail client should be measured against when it comes to extensibility. Unfortunately it lacks a way to deliver these capabilities to users that do not want to spend their lives tinkering with email configuration.

I think we can learn a lot from how the Atom editor approached a similar problem in the text editor space. They harnessed the good parts of Emacs/Vim, while making it more accessible to modern users (who do not have the same type of patience with computers we had in the 1980's).

### It should be Free ###

Call it Open Sourced if you like. I believe the developer community will rally around a well implemented free mail client, similar to how Telegram achieved success, in the face of proprietary messaging systems like Whatsapp.
