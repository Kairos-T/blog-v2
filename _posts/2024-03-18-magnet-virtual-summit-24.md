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

15. I hear Stanley cups are all the rage (25 points)

    > What was the final score of the hockey game Chad went to? (home-away)

    I remember seeing some sports game as I was scrolling through the images on the devices in `Photos.sqlite Analysis`.

    ![iLEAPP](ios_15.1.jpg)

    The answer however, wasn't 3-2. Going back to iLEAPP, I could see when the images were taken:

    ![iLEAPP](ios_15.2.png)

    So I went to search for hockey games on 2023-12-22 at the Ball Arena. An article
    by [Reuters](https://www.reuters.com/sports/nhl-roundup-nathan-mackinnon-hits-milestone-avs-win-2023-12-22/) was the
    first result:

    ![Reuters](ios_15.3.png)

    Ans: `6-4`

16. Exuse Moi? What did you say? (25 points) - Unsolved
    > What is the content of the 2nd message that Chad deleted on Dec 18, 2023

17. Its been a long time (25 points) - Unsolved
    > When did chad last login to Facebook? YYYY-MM-DD HH:MM:SS UTC

18. Can anyone Kelp? (25 points)
    > What game was Chad asking to know the strategy to?

    This question took me quite awhile to figure out. Firstly, I listed out all the games that Chad had. I searched for
    the `Apps per screen report` in iLEAPP to find all the apps located on the device screens (pages).

    ![iLEAPP](ios_18.1.png)

    There were a few games:

- Among Us
- Subway Surfers
- Terrarium
- Call of Duty Shooter
- Clash of Clans

  Next was to figure out where Chadwick was asking for the strategy. It was probably some social media platform — he
  was asking people stuff anyway. I couldn't find anything on Discord or iMessage, so there was still Twitter,
  Facebook, Reddit and possibly YouTube. I remembered that under tha App Snapshots, I found Chad's Twitter and
  Facebook accounts (or rather that he had Facebook).

  His Twitter account didn't seem to have anything related to asking for strategies:

  ![iLEAPP](ios_18.2.png)

  So I moved on to Facebook.

  ![iLEAPP](ios_18.3.png)

  Facebook it was!
  Ans: `Terrarium`

19. The devil is in the details (25 points) - Unsolved
    > Whose bitmoji is dressed like a devil?

    Solved this via guesswork during the CTF, but I don't have the intended solution :(

20. The easy way or the hard way (25 points)

    > What is the timestamp of the message Chad sent to Rocco but was never received? YYYY-MM-DD HH:MM:SS UTC

To be continued!!

## Cipher

The cipher questions in MVSCTF'23 were really simple, but this year's was definitely more tricky.

### Questions

1. Why did the bicycle fall over? It was tired of all the ROTation! (5 points)
   > rfgq ayl lmr zc rfgq qgknjc

   This hinted at a ROT cipher, hence I used this [ROT Cipher Brute Force Tool](https://www.dcode.fr/rot-cipher) from
   dcode to decrypt the message.

   ![dCode](cipher_1.png)

   Ans: `this can not be this simple`

2. Have you ever tried reading the alphabet in reverse? (5 points)
   > Ru lmob dv xlfow gfim yzxp grnv

   I searched for reverse ciphers and found out about the Atbash cipher. I used
   this [Atbash Cipher Tool](https://www.dcode.fr/atbash-cipher) also from dcode to decrypt the message.

   Ans: `If only we could turn back time`

3. The train joke I wrote didnt gain any traction—it went off the RAIL! (5 points)
   > MO OFRSIB ECSNIENI ULSF

   In my cryptography module, I remember learning about the Rail Fence cipher. I used
   this [Rail Fence Cipher Tool](https://www.boxentriq.com/code-breaking/rail-fence-cipher) from Boxentriq to brute
   force the number of rails and decrypt the message.

   ![Boxentriq](cipher_3.png)

   Ans: `MOBILE FORENSICS IS FUN`

4. VIGorous ENcrypting? Embrace the Riddle’s Essence, it’s “essential”! (10 points)
   > QshprMzepw

   Looking at the capital letters, this hinted at a Vigenère cipher. I used
   this [Vigenère Cipher Tool](https://www.dcode.fr/vigenere-cipher) from dcode. Using the password `ESSENTIAL`:

   ![dCode](cipher_4.png)

   Ans: `MapleTrees`

5. BASH these ROTten criminals (10 points)
   > rj vuzcj n mncczza

   This one took an embarrassing amount of time for me to solve. From the hints, we can tell that there is a cipher
   related to `BASH` and `ROT`. The `ROT` cipher was most likely going to be the ROT ciphers from before, but I was a
   little confused. Had to think for a bit to realise that it was ATBASH and ROT...

   ![Atbash](cipher_5.1.png)
   _Atbash Decryption_

   ![ROT](cipher_5.2.png)
   _ROT Decryption_

   Ans: `we stole a balloon`

6. Why did the stego expert wear a ‘cloak’? To keep their messages undercover! (10 points) - Unsolved

   > We hope you are ‌‍⁢‌⁡‌⁢⁤⁡‌⁡‍‌‍⁣‍⁡⁡⁣⁣⁤⁢⁡⁡‍⁡⁢⁡‌⁡⁡⁡‍⁢⁢⁢⁢⁡‍⁡‍⁡⁢⁤⁡⁤‍‌‍⁢⁤‌⁡‍⁡⁢⁡‌⁢‍⁡⁢⁣‍‌⁤‍‌⁡‌⁢⁢⁢⁣‍having a great day!

   I know that this is some sort of zero-width character steganography, but I couldn't figure out how to decode it :(

   Converting it into hex, there are the typical `200D` and `200C` characters, but there were more, and I couldn't
   figure
   out how to map it to the binary system (as how to typically solve these kinds of challs) to decode it.

   ![Zero-width](cipher_6.1.png)

7. What is your favorite SHAKESPEARE play? (10 points) - Unsolved
   > lv bo sj cst ks tl, trel xw tyi ibecxadr

8. Giovan Battista Bellaso probably LOVED pigs (10 points)
   > Giovan.JPG

Unfortunately for the image/audio steg questions, I've lost the files after the CTF; i didn't think i'd do writeups lol... will update soon!

## Android

