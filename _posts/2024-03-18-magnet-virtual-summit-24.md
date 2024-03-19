---
title: Magnet Virtual Summit'24
date: '2024-03-18 19:16:34'
categories: [ Writeups, Forensics ]
tags: [ Forensics ]
img_path: /img/MVSCTF/
---

MVS CTF was a CTF that took place on 6th March 2024; a three-hour-long event. Frankly, that was quite a short amount of
time to solve all the challenges especially at midnight in Singapore. I took part in last year's MVS CTF, and that was
my first actual CTF ever. I learnt SO much, and my interest in Digital Forensics grew from it. I was beyond glad to have
received the email for this year's MVSCTF again, and again, have learnt so so much in this year's one.

Anyway, here is the writeup for the challenges that I managed to solve!

Unfortunately this is the only image I have of CTFd at some point during the CTF, and I didn't manage to get the final
scoreboard before it was taken down...

![MVS CTF](CTFd.png)

This might get a little lengthy, so use the table of contents below to navigate through the categories :)

## Tools Used

| Tool                                                                                                                                                                                                                                                                                                                                 |                                                                                                                              Description                                                                                                                               | Free? |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----:|
| [ALEAPP](https://github.com/abrignoni/ALEAPP)                                                                                                                                                                                                                                                                                        |                                                         A comprehensive mobile forensics analysis tool for parsing Android artifacts from various sources such as backups, logical images, cloud services etc.                                                         |   ✓   |
| [ILEAPP](https://github.com/abrignoni/iLEAPP/)                                                                                                                                                                                                                                                                                       |                                                                                                    A complementary tool to ALEAPP, focusing on iOS devices instead.                                                                                                    |   ✓   |
| [Magnet AXIOM](https://www.magnetforensics.com/products/magnet-axiom/?utm_source=Google&utm_medium=Search&utm_campaign=2024_AXIOM_Google_ads&utm_source=Google&utm_medium=Search&utm_campaign=2023_AXIOM_productpage&gad_source=1&gclid=CjwKCAjwzN-vBhAkEiwAYiO7oJlGKbvNEIc5ZXRWlfjqjoDqxguipGd8zKzm2ymER8k7K1CLGIqzNhoCMawQAvD_BwE) | An enterprise-grade digital forensic platform designed to streamline investigative processes across various digital sources like computers, smartphones, and cloud services. It offers advanced capabilities for acquiring, analysing, and reporting digital evidence. |   ✗   |
| [ArtiFast](https://forensafe.com/blogs/iOSSnapChat.html)                                                                                                                                                                                                                                                                             |                                                                             A digital forensic tool designed for rapid artifact extraction and analysis from computers and mobile devices.                                                                             |   ✓   |

## iOS

Personally, I found the iOS challenges more difficult than the Android ones, but these were rather interesting! I mainly
used iLEAPP for these challenges.

### Questions (17/23)

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
    ![iLEAPP](ios_10.2.png){: width="250"}

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

    There were a few games: Among Us, Subway Surfers, Terrarium, Call of Duty Shooter, Clash of Clans.

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

    For this challenge, I had to compare the `Message Sent` from the iOS device with the `Message Received` from the
    Android device.

21. Follow the Breadcrumbs (50 points)

    > How many times did Chad’s keyboard become visible within the Amazon app on 12/24/2023?

    This challenge took me a really long time to figure out. I chanced
    upon [this Apple developers article](https://developer.apple.com/documentation/sensorkit/srdeviceusagereport/applicationusage/3752156-textinputsessions)
    that described a property called `textInputSessions` that tracks
    > when the user raises a keyboard and ends when the keyboard dismisses.

    I saw that iLEAPP did have a `Biome Text Input Sessions report` feature, so I filtered for Amazon within it.

    ![iLEAPP](ios_21.png)

    There were only two sessions on 2023-12-24, so the answer was 2.

    Ans: `2`

22. Read my mind (50 points)

    > What message was sent to Rocco in a video game?

    This challenge too, took me an awful long time. I sifted through iLEAPP's `Photos.sqlite Analysis report`
    and `App Snapshots (screenshots) report` but couldn't find anything. I skipped this initially but later found it
    while going through the files in the `private/var/mobile/Media/DCIM/100APPLE` directory. There were A LOT of stuff
    in there:
    ```bash
    kairos@pop-os:~/CTF/MVS24/00008110-000925383620A01E_files_full/private/var/mobile/Media/DCIM/100APPLE$ l
    IMG_0001.PNG IMG_0011.MOV IMG_0021.MOV IMG_0032.MOV IMG_0043.PNG
    IMG_0002.PNG IMG_0012.PNG IMG_0022.HEIC IMG_0033.PNG IMG_0044.HEIC
    IMG_0003.PNG IMG_0013.PNG IMG_0022.MOV IMG_0034.PNG IMG_0044.MOV
    IMG_0004.HEIC IMG_0014.PNG IMG_0023.HEIC IMG_0035.PNG IMG_0045.HEIC
    IMG_0004.MOV IMG_0015.HEIC IMG_0024.HEIC IMG_0036.PNG IMG_0045.MOV
    IMG_0005.HEIC IMG_0016.HEIC IMG_0025.MOV IMG_0037.PNG IMG_0046.HEIC
    IMG_0006.MP4 IMG_0017.HEIC IMG_0026.HEIC IMG_0038.MP4 IMG_0047.PNG
    IMG_0007.PNG IMG_0018.AAE IMG_0027.HEIC IMG_0039.MP4 IMG_0048.PNG
    IMG_0008.JPG IMG_0018.HEIC IMG_0028.HEIC IMG_0040.AAE IMG_0049.PNG
    IMG_0009.MP4 IMG_0019.HEIC IMG_0029.HEIC IMG_0040.MOV IMG_0050.PNG
    IMG_0010.AAE IMG_0020.HEIC IMG_0030.HEIC IMG_0041.AAE IMG_0051.PNG
    IMG_0010.MOV IMG_0020.MOV IMG_0031.HEIC IMG_0041.MOV
    IMG_0011.AAE IMG_0021.HEIC IMG_0031.MOV IMG_0042.PNG
    ```

    I searched through, yes, (almost) every single file there. I found this image and I thought it was the answer:

    ![IMG](ios_22.1.PNG)

    But the question asked for what Rocco received, not sent.

    Eventually, I watched the videos and found this snippet:

    ![img](ios_22.2.png)

    And got the answer!!! Also, I have no idea why the colours look like that in the video.

    Ans: `why do that when it should be simply`


23. Chat GPT is my PREFERENCE for AI (50 points) - Unsolved

    > What is the ChatGPT userID associated with chadwickmr95@gmail.com

## Cipher

The cipher questions in MVSCTF'23 were really simple, but this year's was definitely more tricky.

### Questions (9/12)

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

   ![Giovan](cipher_8.1.png)

   I've seen this cipher also in my cryptography module, so I knew this was the Pigpen cipher. With reference to the
   image
   from [this article](https://highschool.spsd.org/crypt/pigpen.html), I decoded the message by hand on the good
   ol' `nano`.

   ![Pigpen](cipher_8.2.png)

   This wasn't the answer though, so I tried
   using [dcode's cipher identifier tool](https://www.dcode.fr/cipher-identifier)
   to possibly identify the cipher. The top result was the Vigenère cipher. Inputting the message and the key `LOVED`, I
   got the answer!

   Ans: `PIGSARETRULYAMAZINGANIMALS`

9. Surfing sound waves in California searching for hidden messages (25 points)
   > Song.wav

   The file can be accessed [here](/img/MVSCTF/Song.wav).
   Usually, when given an audio steganography challenge, I would use `Audacity` to look at the audio file. Toggling
   the `Spectrogram` view:
   ![Audacity](cipher_9.png)

   Ans: `HotelCalifornia`

10. ROTten people hiding their secrets! (25 points)
    > Steganography.rtf

    The file can be downloaded [here](/img/MVSCTF/Steganography.rtf). `rtf`, or Rich Text Format, is a file format that
    is used for documents — something like your Microsoft Docx files. Docx files can be unzipped and dumped, but
    unfortunately this `rtf` file couldn't be unzipped.

    ```bash
    kairos@pop-os:~/Downloads$ unzip Steganography.rtf
    Archive:  Steganography.rtf
    End-of-central-directory signature not found.  Either this file is not
    a zipfile, or it constitutes one disk of a multi-part archive.  In the
    latter case the central directory and zipfile comment will be found on
    the last disk(s) of this archive.
    unzip:  cannot find zipfile directory in one of Steganography.rtf or
    Steganography.rtf.zip, and cannot find Steganography.rtf.ZIP, period.
    ```

    I opened the file in LibreOffice Writer but there was nothing much. I looked at the header and footers — nothing.

    ![LibreOffice](cipher_10.1.png)

    Out of desperation, I dumped the strings of the file and beyond the usual document text (fonts and whatnot), at the
    bottom, there seemed to be a string after the hex:

    ```bash
    kairos@pop-os:~/Downloads$ strings Steganography.rtf 
    {\rtf1\adeflang1025\ansi\ansicpg1252\uc1\adeff31507\deff0\stshfdbch31506\stshfloch31506\stshfhich31506\stshfbi31507\deflang1033\deflangfe1033\themelang1033\themelangfe0\themelangcs0{\fonttbl{\f0\fbidi \froman\fcharset0\fprq2{\*\panose 02020603050405020304}Times New Roman;}{\f34\fbidi \froman\fcharset0\fprq2{\*\panose 02040503050406030204}Cambria Math;}
    ... (Truncated)
    e5e9ac39da01feffffff00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000ffffffffffffffffffffffff00000000000000000000000000000000000000000000000000000000
    00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000ffffffffffffffffffffffff0000000000000000000000000000000000000000000000000000
    000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000ffffffffffffffffffffffff000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000105000000000000}}
    Yzuzex_flk
    ```

    Given that `ROT` was in the hint, I tried to ROT the string
    using [dcode's ROT Cipher Tool](https://www.dcode.fr/rot-cipher) in brute force mode and got the string.

    ![dCode](cipher_10.2.png)

    Ans: `Hiding_out`

11. EXIF data, the memory foam of photography, never forgets the shot you took! (50 points)
    > nicedog.jpg

    We are provided with an image:

    ![nicedog](nicedog.jpg)

    As per the `EXIF` hint, I used [Aperisolve](https://www.aperisolve.com/) to take a look at the image metadata. I got
    a table of the image's metadata (I truncated some lines due to its length):

    | EXIFTOOL                |                                               |
                                                                                                                                                                                                                                            |-------------------------|-----------------------------------------------|
    | ExifToolVersion         | 12.16                                         |
    | FileSize                | 623 KiB                                       |
    | FileType                | JPEG                                          |
    | FileTypeExtension       | jpg                                           |
    | MIMEType                | image/jpeg                                    |
    | ExifByteOrder           | Big-endian (Motorola, MM)                     |
    | YResolution             | 1                                             |
    | Make                    | Apple                                         |
    | Model                   | iPhone SE                                     |
    | ExifVersion             | 0230                                          |
    | ComponentsConfiguration | Y, Cb, Cr, -                                  |
    | FlashpixVersion         | 0100                                          |
    | LensSerialNumber        | 5a6d3931626d52665a6d78685a773d3d              |

    The lens serial number looked a bit unusual. Looked like a hex string, so I used a hex to text converter
    from [Dupli Checker](https://www.duplichecker.com/hex-to-text.php) and got a string: `Zm91bmRfZmxhZw==`. This then
    looked like a Base64 string, so I went to convert it using [Base64 Decode](https://www.base64decode.org/) and got
    the answer.

    Ans: `found_flag`

12. Why did the balloon go to therapy? It needed to OPEN PUFF about its inflated emotions! (50 points) - Unsolved
    > puffr.bmp

    ![puffr](puffr.bmp)

    A `BMP` file, or a bitmap file, is a raster graphics image file format. I tried dumping the strings, looking at the
    image in GIMP and reading up more about BMP steganography, but unfortunately didn't manage to solve this :(

## Android

I found the Android challenges to be more like a walk in the park compared to the iOS challenges, but there were a few
harder questions that I didn't manage to get. I mainly used ALEAPP for these challenges.

### Questions (17/22)

1. Press x to Respawn (5 points)
   > On what platform did Rocco share his Call of Duty Username?

   Given `share`, we can safely assume that there was some sort of social media or messaging platform used. Looking at
   the parsed data, there were Facebook (and Messenger), Discord, Twitter, SMS, and YouTube. I searched
   for `Call of Duty` and `CoD` through the various features, and found most of the conversation regarding it was on
   Twitter.

   ![ALEAPP](android_1.png)

   I couldn't find the exact DM, but I found quite a bit of chatter from Chadwick and Rocco about wanting to play CoD
   together, so I tried Twitter.

   Ans: `Twitter`

2. Warm Up (5 points)
   > What Southern state’s sports team did Rocco search up? (STATE ONLY)

   There was Chrome and Firefox browsing history in the parsed data, so I looked through them. In the Chrome history, a
   Ragin Cajuns football record was searched up, so I assumed it was that.

   ![ALEAPP](android_2.1.png)

   Searching the team on Google showed that it was the Louisiana Ragin' Cajuns.

   Ans: `Louisiana`

3. Can you Handle this (5 points)
   > What was Rocco’s Twitter account name?

   Under the Twitter dumps from earlier, I came across a bunch of JSON content. Lookiing under `screen_name` for Rocco:

   ![ALEAPP](android_3.1.png)

   Ans:`RoccoSachs96775

4. Need to reach those heights (5 points)
   > What is the SIM operator name?

   ALEAPP has a `SIM_info_0` / `Device Info report` feature.

   ![ALEAPP](android_4.png)

   Ans: `Boost Mobile`

5. Not to be basic but... (5 points)

   > What is the default Internet Browser?

   Under the `App Roles report` feature and searching for `browser`, the default browser was returned:

   ![ALEAPP](android_5.png)

   Ans: `Chrome`

6. Survival Mode Activated (5 points)
   > What conference did Rocco show interest in?

   Earlier as I was scrolling through his search history, I came across something called `Preppercon` in one of them.

   ![ALEAPP](android_6.1.png)

   Some conferences are called something-con, such as HITCON. So I tried that.

   Ans: `Preppercon`

7. Sign me up! (5 points)
   > What email is associated with the device?

   Under the `Accounts report`:

   ![ALEAPP](android_7.png)
   There were a few instances of `roccotsachs@gmail.com`.

   Ans: `roccotsachs@gmail.com  `

8. Not so popular (5 points)
   > How many messages were sent from Rocco in Twitter Direct Messages?

   This one took me quite some time to dig through. ALEAPP doesn't have a Twitter DMs parser, so I looked through the
   Twitter DB from the logical data file itself.

   I'm not that familiar with Android file structure, so I simply searched up `Twitter` in the directory to find where
   the Twitter data resides in.

   ![ALEAPP](android_8.1.png)

   So that's `/data/data/com.twitter.android`. There is a databases folder in it, and I found one pretty large db
   called `1719897971716685824-66.db`. Opening it up in SQLite Browser showed 38 tables, but one of them was
   called `conversations`. Under the `users_username` column, Rocco appears 8 times.

   ![SQLite](android_8.2.png)

   Ans: `8`

9. You can never be too ready (10 points)
   > How many additional survival tips were provided in the $9 book Rocco was looking into?

   I came across this in Google Photos:

   ![ALEAPP](android_9.png)

   Ans: `72`

10. Tag you’re it! (10 points)
    > What city was the user in when they identified an AirTag on them?

    There is an `Android Airtag Scans report` feature in ALEAPP that provided the location of the AirTag scan.

    ![ALEAPP](android_10.1.png)

    Keying in the latitude and longitude into Google Maps:

    ![Google Maps](android_10.2.png)

    Ans: `Windsor`

11. A game of Cat and Mouse (10 points) - Unsolved
    > What game did two beloved characters promote in an Ad?

12. Always achieving new heights (10 points) - Unsolved
    > What was the new score achieved on the video game Rocco watched on YouTube?

13. Remember your floaties (10 points)
    > What fun outdoor activity location was searched for?

    Initially, I was looking at the Chrome search history, but anyway realised there was a `Google Maps Searches Report`
    feature in ALEAPP. It resulted in one place:

    ![ALEAPP](android_13.png)

    Ans: `Big Water Campground`

14. R-E-J-E-C-T-E-D Rejected (10 points)
    > When was the last shutdown that was initiated by Rocco? (YYYY-MM-DD HH:MM:SS) UTC 24 hour time.

    Under the `Shutdown Checkpoints report` feature, I sorted the table by the latest timestamp.

    ![ALEAPP](android_14.png)

    Ans: `2023-12-28 23:47:29`

15. No two cents about them (10 points)

    > According to exCHANGEs in discord with Chad, what did Chad want back from Rocco?

    Looking at the `Discord Chats report` and searching for `chadwickgames`:

    ![ALEAPP](android_15.png)

    Ans: `Money`

16. Out of Stock (25 points) - Unsolved
    > What is the most recent score in Subway Surfer

17. So Salty!(25 points)

    > What is the handle of the person who is talking about how upset they are with Rocco?

    I remember coming across some sort Twitter message about Rocco as I was going through the Google Photos. Going back:

    ![ALEAPP](android_17.jpeg){: width="250" }

    Ans: `@larissajenna9`

18. Secrets Secrets are no Fun (25 points)
    > What did Rocco search in the App Store to download the app used to hide photos?

    Under the `Google Play Searches report` table, there were quite a few searches. Going through some of the unfamiliar
    ones like `home depot`, `root checker` and `calculator vault` by searching up more information on them on Google, I
    found out that the `calculator vault` was an app used to hide photos.

    Ans: `calculator vault`

19. LIVE your life (25 points)  - Unsolved
    > What two sports did rocco capture in a photo (__ and __)

20. Don’t let them see you down (25 points)
    > What was added using photoshop

    There isn't a Photoshop parser in ALEAPP, so I decided to look through the media files in the logical data file
    itself. In `/data/media/0/Pictures/Photoshop Express/`, there were several PNG files. All of them looked pretty much
    the same though. Here's one of them:

    ![ALEAPP](android_20.1.png){: width="250" }

    This image looks a little familiar — I saw something like this in the Google Photos earlier.

    ![ALEAPP](android_20.2.jpeg){: width="250"}

    Comparing the two images, the success sticker was the thing that was added.

    Ans: `success`

21. It’s the eye of the tiger (25 points) - Unsolved
    > When is Rocco's Bday? (YYYY-MM-DD)

22. Stalker Alert (50 points)
    > Shortly after logging into Facebook with IP address 72.38.231.98, a photo was taken. Where was this photo taken?

    In the given Facebook files, there is a `security_and_login_information` file. Viewing the `logins_and_logouts.html`
    file, we can see all the login information, along with timestamps and IP addresses.

    One of the entries for the login was:

    | Time                    |                |
    |-------------------------|----------------|
    | Dec 27, 2023 11:16:01am |                |
    | IP address              | 72.38.231.98   |
    | Site                    | m.facebook.com |

    So the photo should be taken on December 27th around 11:16am.

    Under the `Emulated Storage Metadata - Images report` feature, there was a table containing the information of the
    images taken along with their timestamps. Searching for images taken on 2023-12-27, there were a few:

    ![ALEAPP](android_22.1.png)

    The earliest one was taken at `2023-12-27 16:30:49+00:00`. I wasn't sure if this was the right one since that wasn't
    that close to 11.16am, but that was my only lead. I grabbed the image from the logical data file:
    ![ALEAPP](android_22.2.png)

    No idea where this was. But since it was residing in the phone itself (and not taken from some social media
    platform), the Exif data should still be intact. Using `exiftool` to view the metadata (truancated a bit due to the
    length):
    ```bash
    kairos@pop-os:~/Downloads$ exiftool PXL_20231227_163049844.jpg 
    File Name                       : PXL_20231227_163049844.jpg
    MIME Type                       : image/jpeg
    Make                            : Google
    Camera Model Name               : Pixel 3a XL
    Date/Time Original              : 2023:12:27 11:30:49
    Create Date                     : 2023:12:27 11:30:49
    Lens Model                      : Pixel 3a XL back camera 4.44mm f/1.8
    GPS Latitude Ref                : North
    GPS Longitude Ref               : West
    GPS Altitude Ref                : Above Sea Level
    GPS Time Stamp                  : 16:25:42
    GPS Img Direction Ref           : Magnetic North
    GPS Img Direction               : 89
    GPS Date Stamp                  : 2023:12:27
    GPS Altitude                    : 152.5 m Above Sea Level
    GPS Date/Time                   : 2023:12:27 16:25:42Z
    GPS Latitude                    : 42 deg 16' 28.50" N
    GPS Longitude                   : 83 deg 0' 9.31" W
    GPS Position                    : 42 deg 16' 28.50" N, 83 deg 0' 9.31" W
    Lens ID                         : Pixel 3a XL back camera 4.44mm f/1.8
    ```

    Grabbing the coordinates and putting it into Google Maps (`42°16'28.5"N 83°00'09.3"W`):

    ![Google Maps](android_22.3.png)

    Ans: `Devonshire Mall`

# Conclusion
48/57 questions solved, and I'm pretty proud of that. This is a huge improvement from last year's performance. Immensely grateful for the organisers for putting together such a great CTF! See you next year :D
