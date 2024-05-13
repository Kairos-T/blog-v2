---
layout: post
title: 'A Quick Look at Cosmic DE'
date: 2024-05-12 22:54 +0800
categories: [ Linux ]
tags: [ Linux ]
img_path: img/cosmic
---

Recently, I have been scrolling through [Pop!_OS' Subreddit](https://www.reddit.com/r/pop_os/), and have been seeing a
whole slew of posts complimenting the new Cosmic DE. Naturally, I bit the bullet and decided to give it a try. After
all, I have been in love with Pop!_OS for a while now. I've been using the COSMIC DE for a grand total of ONE (1) day,
so take my opinions with a grain of salt.

## Wait... What's Cosmic?

COSMIC is a DE developed in Rust, making it lightweight and fast. It is made for Pop!_OS and other distros by System76.
It is designed similarly to Pop!_OS' DE (which is pretty much a 'better' GNOME), but with a faster codebase and snappier
features. Additionally, it is trying to move away from GTK-based GNOME for most of its components.

Currently, COSMIC is almost in its stable alpha stage. It is, however, available for testing
under [cosmic-epoch](https://github.com/pop-os/cosmic-epoch), which is what I am currently using.

## UI Components

On the first look (after a little bit of customisation), COSMIC does look like Pop!_OS' DE fused with xfce elements for
the navigation bar. COSMIC is really customisable, which is what I love most about it.

![cosmic](cosmic.png)

There is insane granularity in the settings. One of the pretty useful feature is in the battery settings.

![battery](battery.png)

It allows you to set the maximum charge of the battery to 80% to prolong the battery life. This is a feature that my
laptop has, but only accessible via the BIOS or the Lenovo Vantage app which is only available on Windows :( So having
this here is a really nice touch.

For the sound settings on the navigation bar, it has a sweet little feature that allows you to configure the input and
output devices, which is pretty useful for me since I often switch between my headphones, speakers, and mics. There is
also a feature for the media control button, which is just the rewind/pause-play/forward button, but still a nice touch.

![sound](sound.png)

The other stuff are quite standard to Pop!_OS' DE, including the app launcher.

![launcher](launcher.png)

## Additional Features

COSMIC has a COSMIC App Store It is a replacement for the Pop!_Shop, which is infamous for being
extremely sluggish — which even the Pop community discourages using. The COSMIC App Store is a lot faster, and neater.

![shop](shop.png)

COSMIC also ships with a new terminal and text editor, which are both pretty neat with slight performance improvements.

One feature that I absolutely love is the snappy tiling feature. It is a feature that was brought over from Pop!_OS, but
it seems to be a lot more polished in COSMIC; it just works.

![tiling](tiling.png)

There is also an auto-tiling feature, which I have always used in the default Pop DE.

## The Good and The Ugly

### The Good (things that I like):

I love the customisability of COSMIC, and its performance is pretty good. The customisations are endless, yet intuitive,
perfect for someone like me who loves to experiment with different settings for the perfect setup. The installation was
also really simple, with the running of a few commands straight from the command line, then simply rebooting.

### The Ugly (dealbreakers):

To preface, I am absolutely not complaining about the DE. I understand that it is still in its alpha stage, so bugs and
issues are definitely expected!

One of the issues is due to the incomplete features.

![incomplete](dev.png)

There are (understandably) a bunch of features still in development, including customising keyboard shortcuts, which I
rely heavily on in my workflow. Another issue is more pressing: the freezing of apps. I'm not too sure what causes it,
but at certain points, some parts (like certain buttons) of my browser, terminal, or code editor would just freeze for a
few seconds, which is pretty annoying. Also, I had some issues with the drag-and-dropping of files (would either not
allow that feature, or only copy the file path), which is also a pretty big part of my workflow.

## Conclusion

COSMIC is a really promising DE, and I am genuinely so excited to switch over to it once it is in its official stable
release. For now, I'm sticking with the default Pop!_OS DE, but I will definitely be keeping an eye out for COSMIC's
development. I am really excited to see what the future holds for COSMIC, and I am sure that it will be a game-changer
for the Linux community!!

Byebye :D
