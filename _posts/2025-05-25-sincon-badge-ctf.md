---
layout: post
title: 'SINCON 2025 Badge CTF Writeup'
date: 2025-05-25 18:22 +0800
categories: [CTF]
tags: [CTF]
img_path: "/img/sincon25/"
---

[SINCON 2025](https://www.infosec-city.com/sin-25) just ended, and I managed to snag their digital badge. There are a total of 8 challenges on the badge, and this post will cover the solutions.

## Setup

At the [Electronic Badge Kampung](https://www.infosec-city.com/post/sin25-k-e-badge), we could solder 8 different coloured LEDs to our liking. 

![badge](badge.png)

This took me awhile + around 3 crashouts to get it down as I realised I do not have any dexterity :(. After soldering it on, I was given a coin battery to power it up. The 8 soldered LEDs would power up for a bit, then go off. The main goal was to solve the challenges to get the 8 LEDs to light up.

I used the Arduino IDE to interact with the badge, and connected it to my laptop via a USB-C cable. Sending `***` to the serial monitor starts the CTF.

![IDE](ide.png)

## Challenges

### Challenge 1

```
baaba abaaa abbab aaaba abbba abbab

aaabb baaab aaaaa aabba abbba abbab
```

I dumped it into [DCode's Cipher Identifier Tool](https://www.dcode.fr/cipher-identifier) and it identified it as Bacon Cipher. Decoding it gives us `SINCONDRAGON`. Input `sincondragon` into the serial monitor, and the first LED lights up!

### Challenge 2

```
That’s one beautiful lanyard, innit?
```

The SINCON badge comes with a cool pretty lanyard, and it looks like there's a cipher on it. I took a photo of it and uploaded it to Google Lens. 

![chall2](chall2.png)

Looks like a runic cipher. Comparing it with the image, I got the flag `sinconparc`.

### Challenge 3

```
aGFja2Vyd2FyZS5pby9zaW5jb24yMDI1LWNoYWxsZW5nZS1h
```

Throwing it into Dcode again, it identifies it as Base64. Decoding it gives us [hackerware.io/sincon2025-challenge-a](https://hackerware.io/sincon2025-challenge-a). Opening the link gives us this page:

![chall3](chall3-pt1.png)

`black` and `white` links to two halves of a QR code with inverted colours. I had to invert the colours of one side, and joined them together to form the QR code.

![chall3-pt2](chall3-pt2.png)

Scanning the QR code, we get the following text: `jrypbzr gb gur cnex g0n-c4l0u`. This again looks like some cipher, hence Dcode came in handy. Dcode identified it as ROT13, and decoding it gives us `welcome to the park t0a-p4ybh`. Inputting `t0a-p4ybh` into the serial monitor lights up the third LED.

### Challenge 4

```
aGFja2Vyd2FyZS5pby9zaW5jb24yMDI1LWdlcmJlci56aXA=
```

Again, this is Base64 encoded. Decoding it gives us [hackerware.io/sincon2025-gerber.zip](https://hackerware.io/sincon2025-gerber.zip). Downloading the zip file, I found a bunch of files with the `.gbr` file inside. Researching a little about what it is, I found that `.gbr` files are Gerber files used in PCB design. We can open it in a Gerber viewer, which I did using the [PCBWay Gerber](https://www.pcbway.com/project/OnlineGerberViewer.html). Uploading the zip file renders the PCB design.

![chall4](chall4-pt1.png)

After some clicking around and comparing it with the physical badge I had, I found an extra bit of text at the back. 

![chall4-pt2](chall4-pt2.png)

Isolating that layer and mirroring it, I found this:

![chall4-pt3](chall4-pt3.png)

We are given yet another cipher, though with a hint. I tried [Dcode's Rail Fence Cipher](https://www.dcode.fr/rail-fence-cipher), and played around with the values.

![chall4-pt4](chall4-pt4.png)

After some trial and error, I got the text `explore~the~...dr4g0n–c1ty`. Inputting `dr4g0n–c1ty` into the serial monitor lights up the fourth LED.

### Challenge 5

```
aGFja2Vyd2FyZS5pby9zaW5jb24yMDI1LWRyYWdvbi1yb2Fy
```

This is too, Base64 encoded. Decoding it gives us [hackerware.io/sincon2025-dragon-roar](https://hackerware.io/sincon2025-dragon-roar). Opening the link gives us a page that says:

```
A Roar
Wanna hear the dragon roar?
Find an audio file of same name as this webpage.
If anyone asks: the meme is the secret to the universe!
```

Audio files are usually in `.mp3`/`.mp4`/`.wav` format, so I tried those. Appending `.wav` worked, and I downloaded the file. I first tried viewing the spectrogram of the audio file, but it didn't yield anything useful. 

Running `strings` on it gave this:

```bash
$ strings sincon2025-dragon-roar.wav
... (truncated)
Mixtape For Hacking In The WildCOMM
Maybe try plugging in ... The HeadphoneTPE1
Pacific Hacker BearTDRC
2023TXXX
Software
Lavf58.29.100TCOP
TLAN
TPUB
TXXX
TRACKTOTAL
LIST
INFOIART
Kingdom Power Music
ICMT,
https://www.youtube.com/watch?v=rUWALfala08
ICRD
20210405
INAMC
Rick ASTLEY - Never Gonna Give You Up (REMIX) [No Copyright Music]
ISFT"
Lavf58.29.100 (libsndfile-1.0.31)
```

Hokay, the audio is some rickroll. I tried some other steganography tools and thought about what requires/prompts for a password, and `steghide` came to mind. I tried extracting it with `steghide extract -sf sincon2025-dragon-roar.wav`, and it prompted for a password. I tried `rickroll` and it worked :P

It returned a file called `dragonsecret.txt`. Cat-ing the file gives us: 

```bash
$ cat dragonsecret.txt
The dragon will... bre4th-f1r3
```

Inputting `bre4th-f1r3` into the serial monitor lights up the fifth LED.

### Challenge 6

```
aGFja2Vyd2FyZS5pby9zaW5jb24yMDI1LWNoYWxsZW5nZS1i
```

Again, decoding this Base64 gives us [hackerware.io/sincon2025-challenge-b](https://hackerware.io/sincon2025-challenge-b). 

This time, it gives us a page with the following text:

```
The Flag
Here for the flag? Say no more.
Find an image file of same name as this webpage.
```

Like the previous challenge, I tried appending the different file extensions (images for this). `.png` worked, and I downloaded the file.

![chall6](sincon2025-challenge-b.png)

At the bottom, we see some funky stuff going on. I used Google Lens for this, and through some [Etsy](https://www.etsy.com/listing/880334685/maritime-semaphore-flags-signal-flags) match, I found that it was some Semaphore signalling flags. Matching the flags with the pictures, we get `dr4g0n-fru1t`. Inputting `dr4g0n-fru1t` into the serial monitor lights up the sixth LED.

### Challenge 7

```
g13 f4 -16 e3 g7 t1 d10 014 419 h2

s9 p17 i8 412 l5 y20 r11 118 a6 n15
```

Dcode identified this as [Letter Positions](https://www.dcode.fr/letter-positions). What it yielded was a little weird: `theflagisdrgn-py`. `drgn-py` was not right, so I assumed the numbers didn't get decoded right. Trying out some combinations of numbers, I got `dr4g0n-p14y`, which turned out to be the right flag. Inputting `dr4g0n-p14y` into the serial monitor lights up the seventh LED.

### Challenge 8

```
aGFja2Vyd2FyZS5pby9zaW5jb24yMDI1LWNoYWxsZW5nZS1j
```

Also Bas64 encoded, decoding it gives us [hackerware.io/sincon2025-challenge-c](https://hackerware.io/sincon2025-challenge-c).

This time, it gives us a page with the following text:

```
Hackerwares
The Message
Flip phone in my hand,
press the code of ancient fire-
3 777 2 4 (0) 66 - 66 (3) 7777 8
the dragon answers.
```

I grew up bringing a Nokia phone to kindergarten (albeit secretly), so this was familiar to me. The numbers correspond to the T9 keyboard, where each number corresponds to a set of letters. 

With reference to a T9 keyboard, we can decode the message to `drag0n-n3st`. Inputting `drag0n-n3st` into the serial monitor lights up the eighth LED.

![badge](solve.jpg)

Now, all the LEDs light up in sequence! :D

## Conclusion

SINCON 2025 was very fun!! Met so many cool people there :) See you guys next year! 