---
title: Magnet Virtual Summit'24
date: '2024-03-18 19:16:34'
categories: [ Writeups, Forensics ]
tags: [ Forensics ]
img_path: /img/MVSCTF/
---

MVS CTF was a CTF that took place on 6th March 2024; a three-hour-long event. Frankly, that was quite a short amount of
time to solve all the challenges especially at midnight in Singapore. But anyway, here is the writeup for the challenges
that I managed to solve!

Unfortunately this is the only image I have of CTFd at some point during the CTF, and I didn't manage to get the final
scoreboard before it was taken down...

![MVS CTF](CTFd.png)

This might get a little lengthy, so use the table of contents below to navigate through the categories :)

## Tools Used

| Tool                                                                                                                                                                                                                                                                                                                                 | Description | Free? |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------:|:-----:|
| [ALEAPP](https://github.com/abrignoni/ALEAPP)                                                                                                                                                                                                                                                                                        |             |   ✓   |
| [ILEAPP](https://github.com/abrignoni/iLEAPP/)                                                                                                                                                                                                                                                                                       |             |   ✓   |
| [Magnet AXIOM](https://www.magnetforensics.com/products/magnet-axiom/?utm_source=Google&utm_medium=Search&utm_campaign=2024_AXIOM_Google_ads&utm_source=Google&utm_medium=Search&utm_campaign=2023_AXIOM_productpage&gad_source=1&gclid=CjwKCAjwzN-vBhAkEiwAYiO7oJlGKbvNEIc5ZXRWlfjqjoDqxguipGd8zKzm2ymER8k7K1CLGIqzNhoCMawQAvD_BwE) |             |   ✗   |
| [ArtiFast](https://forensafe.com/blogs/iOSSnapChat.html)                                                                                                                                                                                                                                                                             |             |   ✓   |

## iOS

Personally, I found the iOS challenges more difficult than the Android ones, but these were rather interesting! I mainly
used iLEAPP for these challenges.

### Questions

1. Why are your messages green? (5 points)
   > On what date did Rocco and Chadwick first meet in person according to their conversations? YYYY-MM-DD format

   I used iLEAPP's `SMS & iMessage Messages (Threaded)` function for this. Scrolling through the various chats, I found
   one
   between Rocco and Chadwick.

   ![iLEAPP](ios_1.1.png)

   Looking through the chat, we can see when they first met:

   ![iLEAPP](ios_1.2.png)

   Ans: `2023-12-17`
2. Where /r u going on safari? (5 points)

   > What subreddit was visited in a browser?

   The huge clue was the browser, so I used iLEAPP's `Biome Safari` function. Simply searching for `Reddit`, I got the
   subreddit:

   ![iLEAPP](ios_2.1.png)

   Ans: `Twitch`

3. IMAGEine living in pain (5 points)

   > Chad seemed to be searching for pain relief medicine in a store, how much did it cost?

   The clue here is `IMAGE`, and I used the `Photos.sqlite Analysis` function to take a look at the images on the
   device.
   After a little bit of scrolling, I found the image:

   ![iLEAPP](ios_3.1.png)

   Ans: `10.99`

4. Your keyboard is salt-y (5 points) - Unsolved

   > How many total words were typed on the device?

5. Answer the call (5 points)

   > What is the guild ID of the discord server Chad was in?

   Since this was a question on Discord, I looked at the `Discord Messages` function. What I saw a lot was this Channel
   ID
   string:

   ![ILEAPP](ios_5.1.png)

   I wasn't sure if that was the Guild ID, so I plugged it into
   this [Discord Lookup Tool](https://discordlookup.com/guild/) and found that it was indeed the Guild ID for Call of
   Duty!

   ![Discord Lookup](ios_5.2.png)

   Ans: `136986169563938816`

6. Don't ghost me (5 points)

   > At what time did Chadwick get annoyed at MYAI? YYYY-MM-DD HH:MM:SS UTC

   MyAI is a Snapchat AI bot, and unfortunately, iLEAPP doesn't have a Snapchat parser. I used ArtiFast from Forensafe
   for
   this. Will upload the images soon!

   Ans: `2023-12-26 23:27:45`

7. Build me up, buttercup (5 points)

   > What is the current build version?

   I used iLEAPP's `iOS Build report` function for this.

   ![iLEAPP](ios_7.1.png)

   Ans: `20F75`

8. Warning Signs (5 points)

   > How many days did it take Chad to be warned about his Data Usage?

   Earlier on, on the first question, I remember chancing upon some sort of ISP messages. Under
   the `SMS & iMessage - Messages` tables, I searched for `BOOST`, the ISP I had found earlier.

   ![iLEAPP](ios_8.1.png)

   Within the table, two messages stood out:

   | Message Timestamp   | Read Timestamp      | Message                                                                                                                                                                       | Service | Message Direction | Message Sent | Message Delivered | Message Read | Account | Account Login | Chat Contact ID |
                   |---------------------|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|-------------------|--------------|-------------------|--------------|---------|---------------|-----------------|
   | 2023-11-29 17:40:02 | 2023-11-29 17:46:37 | BSTFreeMSG: Welcome to Boost Mobile! Get the most out of your new iPhone with the BoostOne app and more: http://bst-m.co/KyFLsm4E Reply END to STOP                           | SMS     | Incoming          |              | Yes               | Yes          | E:      | E:            | 91065           |
   | 2023-12-17 00:27:48 | 2023-12-17 00:28:33 | BOOST: You've used 4.27 GB of data. Once you reach 5.00 GB your data may be impacted. To get more data, shop Extras from your My Boost dashboard. https://id.boostmobile.com/ | SMS     | Incoming          |              | Yes               | Yes          | E:      |               |                 |

   The first message was the welcome message, and the second was the warning message. So it took 18 days for Chad to be
   warned.

   Ans: `18`

9. Watching streams to stay current (10 points)

   > What is the name of Chad's streaming channel?

   Given that we saw `r/twitch` earlier, I assumed that the streaming platform would be Twitch. However, searching far
   and
   wide in iLEAPP, I couldn't find anything related to Chad's Twitch channel. Going back one step, I searched
   for `Stream`
   and found some stuff related to streaming in the Discord messages:

   ![iLEAPP](ios_9.1.png)

   There was a YouTube link sent, and clicking it would show his channel:

   ![YouTube](ios_9.2.png)

   Ans: `ChadwickGames`

10. What question did Chadwick ask to AI? (10 points)

    > What question did Chadwick ask to AI?

    This question was a little tricky, I *knew* that it was a ChatGPT question, but I couldn't figure out how to get it.
    I searched the Safari history, Discord, SMS... but I couldn't find anything. Later on, I saw
    the `App Snapshots (screenshots)` report in iLEAPP and searched through it. Essentially, iOS takes a screenshot of
    apps when they are sent to the background. You can read up more about this on
    this [stackoverflow post](https://stackoverflow.com/questions/27265957/ios-takes-a-screenshot-of-app-every-time-it-is-sent-to-the-background-how-woul).

    I found a screenshot of the ChatGPT question:

    ![iLEAPP](ios_10.1.png)
    ![iLEAPP](ios_10.2.png)

    Ans: `How to make online friends`
    Honestly, same.

11. Watch me sUAVely win this game (10 points)

    > How many kills did Chad have on his CoD Mobile winning game?

    Earlier, on Chadwick's YouTube channel, I saw a few videos, including CoD games. Scrolling to the end of the video:

    ![YouTube](ios_11.1.png)

    Ans: `7`

12. For when I cant Find My gear (10 points) - Unsolved

    > What outdoor activity store did Chadwick Visit?

13. Just a couple steps away (10 points)

    > How many steps did Chad take on 12/3/2023?

    This was pretty simple, I used the `Health - Steps report` function in iLEAPP. Searching for 2023-12-03, I could
    find
    all the steps logged on that day.

    ![iLEAPP](ios_13.1.png)

    Adding up all the steps, I got the answer.

    Ans: `968`

14. Another regularly scheduled program (10 points)
    
    > What Tattoo shop was visited on 12/27/2023?
    


## Cipher

## Android

