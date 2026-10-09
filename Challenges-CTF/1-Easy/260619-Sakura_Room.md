# **[Sakura Room](https://tryhackme.com/room/sakura)**
### *Finished June 19 2026 | Write-Up Update*

## Introduction

The Sakura Room tests the skills of various OSINT techniques, from finding basic metadata inside an image, to searching github repository commit history, and tracking location based on social media posts.

## Walkthrough
### Before You Start
You have three images to download for this room.
- **[sakurapwnedletter.svg](https://raw.githubusercontent.com/OsintDojo/public/3f178408909bc1aae7ea2f51126984a8813b0901/sakurapwnedletter.svg)** for Task 2
- **[taunt.png](https://raw.githubusercontent.com/OsintDojo/public/main/taunt.png)** and **[114524735-c105ee80-9c45-11eb-93ff-95b64e9246ec.png](https://user-images.githubusercontent.com/64150407/114524735-c105ee80-9c45-11eb-93ff-95b64e9246ec.png)** for Task 5.

## Task 1. INTRODUCTION: Are you ready to begin?
>***Answer: ```Let's Go!```***  
*✅ Solved | super hard ig*

Needless to say, type ***Let's Go!*** without quotes to continue!
<br><br>

## Task 2. TIP-OFF: What username does the attacker go by?
>***Answer: ```SakuraSnowAngelAiko```***  
*✅ Solved | Easy*

![sakurapwnedletter.svg](https://raw.githubusercontent.com/OsintDojo/public/3f178408909bc1aae7ea2f51126984a8813b0901/sakurapwnedletter.svg)

### A. Binary?
On the image left by the attacker, it says 'You've Been Pwned!', and in the background, there is a big series of binary codes, that seems to mean something.  
The binary transcription I made was this:
```bin
01000001 00100000 01110000 
01101001 01100011 01110100 
01110101 01110010 01100101 
00100000 01101001 01110011 
00100000 01110111 01101111 
01110010 01110100 01101000 
00100000 00110001 00110000 
00110000 00110000 00100000 
01110111 01101111 01110010 
01100100 01110011 00100000 
01100010 01110101 01110100 
00100000 01101101 01100101 
01110100 01100001 01100100 
01100001 01110100 01100001 
00100000 01101001 01110011 
00100000 01110111 01101111 
01110010 01110100 01101000 
00100000 01100110 01100001 
01110010 00100000 01101101 
01101111 01110010 01100101 
```

What can we do with this binary data? It doesn't seem like an ASCII art that we could pull a sillouette of something, so we should **decode** this to something readable.  
However, binary data strings could be anything. Every digital data format is inherently a line of binary 0s and 1s in their core, so we can't assume what is this encoded from.  

We could assume that this is definitely not an image, video, or an audio file for sure. This binary string is only 60 bytes (480 bits), but the **headers** - the crucial data that tells the software what file format they are - for these types of multimedia files take up *hundreds or thousands of bytes.* 

Let's try the two most likely decoded formats for an 8-spaced binary string: **numbers** and **text**.


#### Attempt 1: Numbers

To convert a binary string to numbers, you can use websites like **[RapidTables Binary to Decimal Converter](https://www.rapidtables.com/convert/number/binary-to-decimal.html)**. You can paste the binary numbers through the *Enter binary number* input.  
This is the result I got from the number with 8-digit spaces,
```num
65 32 112 105 99 116 117 114 101 32 105 115 32 119 111 114 116 104 32 49 48 48 48 32 119 111 114 100 115 32 98 117 116 32 109 101 116 97 100 97 116 97 32 105 115 32 119 111 114 116 104 32 102 97 114 32 109 111 114 101
```
and without spaces.
```num
794176675658346707555608396635564868776955582073393659976676561349917661190417515620346879897859074803781268834917758694494030545446598322647653
```

hmm... These numbers doesn't ring a bell. Let's try to decode this as text.

#### Attempt 2: Text (ASCII/UTF-8)
RapidTables also provides a **[Binary to Text Translator](https://www.rapidtables.com/convert/number/binary-to-ascii.html)**. You can paste the binary numbers through the *Paste binary code numbers or drop file:* input.  
This is the result I got:
```text
A picture is worth 1000 words but metadata is worth far more
```
This is much more informative! The text mentions to look at the **metadata** of the pwned image given. Metadata could contain hidden information like the image author, GPS coordinates, and much more.

### B. Fetching Metadata
I used **[ExifTool](https://exiftool.org/)** via macOS zsh installation to fetch the metadata for the image. This program is available for Windows, macOS, and Linux environments. If you don't feel like installing software, you may also use online image metadata tools.

***Tip:** If you are unfamilliar with ExifTool, you may refer to ```insert THM-Write-Ups/Learning-Resources/ExifTool.md link here``` and **[ExifTool.org](https://exiftool.org/)** for more information about the software.*

Run ExifTool to display the metadata of the pwned image file. For example, I ran this code on zsh.
```bash
exiftool /Users/(USERNAME)/Downloads/sakurapwnedletter.svg
```
The full ExifTool output should look like this,
<details>
<summary>Full ExifTool Output</summary>

```
ExifTool Version Number         : 13.55
File Name                       : sakurapwnedletter.svg
Directory                       : /Users/stardust/Downloads
File Size                       : 850 kB
File Modification Date/Time     : 2026:06:19 14:24:58+09:00
File Access Date/Time           : 2026:06:30 20:19:27+09:00
File Inode Change Date/Time     : 2026:06:30 20:19:25+09:00
File Permissions                : -rw-r--r--
File Type                       : SVG
File Type Extension             : svg
MIME Type                       : image/svg+xml
Xmlns                           : http://www.w3.org/2000/svg
Image Width                     : 116.29175mm
Image Height                    : 174.61578mm
View Box                        : 0 0 116.29175 174.61578
SVG Version                     : 1.1
ID                              : svg8
Version                         : 0.92.5 (2060ec1f9f, 2020-04-08)
Docname                         : pwnedletter.svg
Export-filename                 : /home/SakuraSnowAngelAiko/Desktop/pwnedletter.png
Export-xdpi                     : 96
Export-ydpi                     : 96
Metadata ID                     : metadata5
Work Format                     : image/svg+xml
Work Type                       : http://purl.org/dc/dcmitype/StillImage
Work Title                      : 
```
</details>

but the only line we will need is this: the **Export-filename** row.

```text
Export-filename                 : /home/SakuraSnowAngelAiko/Desktop/pwnedletter.png
```

This row seems to contain the original path of the image file from the attacker's PC. According to the username section (between ```home``` and ```Desktop```), we can find that the attacker goes by the name ***SakuraSnowAngelAiko***.
<br><br>

## Task 3. RECONNAISSANCE
From the background context from this task, ```SakuraSnowAngelAiko``` seems to have reused their username on other social media platforms.  
This means that we will definitely be using **Cross-Platform Username Tracking**, where we can search up this username on a search engine and find any corresponding accounts.

Searching up query ```SakuraSnowAngelAiko``` on google gave me these results:

> <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAMAAABEpIrGAAAAb1BMVEX////4+Pi3ubtvcnZNUVU+Q0cpLjLr6+x3en0sMTYkKS59gIORk5aUl5n8/Pzw8PFTV1tbX2Pc3d5DSEzn5+g3PECLjpFKTlKFh4qxs7XCxMUuMze/wcLh4uPV1tZzd3o/Q0jOz9CmqKpjZ2qfoaTxAyfNAAABPUlEQVR4AW3TBYKDMBQE0AltAgzuzur9z7ibH5oKfWjc4UEFl6s2Rl8vgcJZGMX04iTEM5UaPomzHA+KkidVAa/WfKNpffMd32oKCHUlWfb27Q19ZSMVrNHGTMDckMtQLqSegdXGpvi3Sf93W9UudRby2WzsEgL4oMvwoqY1AsrQNfFipbXkCGh1BV6oT1pfRwvfOJlo9ZA5NAonStbmB1pawBuDTAgkX4MzV/eC2H3e0C7lk1aBEzd+7SpigJOZVoXx+J5UxzADil+8+KZYoRaK5y2WZxSdgm0j+dakzkIc2kzT6W3IcFnDTzdt4sKbWMqkpNl229IMsfMmg6UaMsJXmv4qCMXDoI4mO5oADwyFDnGoO3KI0jSHQ6E3eJum5TP4Y+EVyUOGXHZjgWd7ZEwOJzZRjbPQt7mF8P4AzsYZpmkFLF4AAAAASUVORK5CYII="  width="20"> GitHub  
> https://github.com › sakurasnowangelaiko
> ### [Aiko sakurasnowangelaiko](https://github.com/sakurasnowangelaiko)
> Aiko sakurasnowangelaiko. **A free & open modern, fast email client** with user-friendly encryption and privacy features
> ***

> <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAAAAABXZoBIAAAA/0lEQVR4AbXPIazCMACE4d+L2qoZFEGSIGcRc/gJJB5XMzGJmK9EN0HMi+qaibkKVF1txdQe4g0YzPK5yyWXHL9TaPNQ89LojH87N1rbJcXkMF4Fk31UMrf34hm14KUeoQxGArALHTMuQD2cAWQfJXOpgTbksGr9ng8qluShJTPhyCdx63POg7rEim95ZyR68I1ggQpnCEGwyPicw6hZtPEGmnhkycqOio1zm6XuFtyw5XDXfGvuau0dXHzJp8pfBPuhIXO9ZK5ILUCdSvLYMpc6ASBtl3EaC97I4KaFaOCaBE9Zn5jUsVqR2vcTJZO1DdbGoZryVp94Ka/mQfE7f2T3df0WBhLDAAAAAElFTkSuQmCC" width="20"> X · SakuraLoverAiko   
> 200+ followers
> ### [Aiko (@SakuraLoverAiko) / Posts / X](https://x.com/SakuraLoverAiko)
> **Aiko** (@SakuraLoverAiko) - Posts - | X (formerly Twitter) Joined: Jan 24, 2021 8Posts 1Following 200Followers Posts. Hi there! I'm @AiKOABE3!
> ***

## 3-1. What is the full email address used by the attacker?
>***Answer: ```SakuraSnowAngel83@protonmail.com```***  
*✅ Solved | Medium*

Let's look at the first search result, the Github page. We can see five pinned repositories:  
**[cpuminer](https://github.com/sakurasnowangelaiko/cpuminer)**, **[Mailpile](https://github.com/sakurasnowangelaiko/Mailpile)**, **[xmrig](https://github.com/sakurasnowangelaiko/xmrig)**, **[IO](https://github.com/sakurasnowangelaiko/IO)**, and **[PGP](https://github.com/sakurasnowangelaiko/PGP)**.

But here, we will only focus on the non-forked repositories, **[IO](https://github.com/sakurasnowangelaiko/IO)**, and **[PGP](https://github.com/sakurasnowangelaiko/PGP)**, since they are the most likely repositories to contain codes or informations that ```SakuraSnowAngelAiko``` wrote by themselves. 

**The reason we are skipping all the forked repositories will be explained later in Task 4.**

### A. IO
The IO reposiory contains two *java* files, ```helloworld``` and ```test```. However, these files only contain short codes that seems to do nothing really useful.

- ```helloworld``` literally prints ```"Hello World!"``` and does nothing else.
- ```test``` writes 100 random integers to a text file generated into the local machine.

Let's check the other repository next.

### B. PGP

There is only one file in this repository, which is ```publickey```. In the text file, it starts with ```-----BEGIN PGP PUBLIC KEY BLOCK-----```, and there is a huge block of garbled text below.

This could also seem helpless, but this is actually the most likely part to contain an email address.

PGP stands for ***Pretty Good Privacy***, which is a popular encryption protocol used to protect digital information from unauthorized access. To encrypt the data, PGP uses a public key and a private key.

***A Public key***, which is the text on this repository, is the code the sender uses to encrypt the data to send to the reciever. It is basically like a **P.O box address**, where anyone can look at it and drop packages into it.

A PGP public key is an encoded text consisted of *the key itself, key metadata, user identity, and etc.* The **user identity** section has the owner's name, notes, and *their email address* - which is exactly what we need here. Let's try to decode this key to see if we can fetch it.

### My Solution: Base64 Decoder
Before making this write-up, I also did not know what PGP was, so I closely examined what this could possibly be encoded to.
```
mQGNBGALrAYBDACsGmhcjKRelsBCNXwWvP5mN7saMKsKzDwGOCBBMViON52nqRyd...
```
After skimming through, I noticed that the text consists of uppercase and lowercase alphabets, numbers, and these three familiar symbols: ```+ / =```. The first thought that came through my head was that **this is Base64 encoded**.  
To decode this, I used an an online decoder tool **[BASE64Decode.org](https://www.base64decode.org/)**. Simply paste the encoded text and click **DECODE** to get the result.

At first, the result looked even worse; it was now full of cursed and unrecognizable symbols that even my editor software VScode was failing to display. However, there was one part that was readable:
```
`�h\^B5|f70
<8 A1X7+˱gp7e0Ey_&B3iβe�\`=(GYK:*a[/1H=\Л4QirsCY,|V(HS`2ߖR3!#FBk:Z0V%H�;f)Ѓ!Y'D8Zu3[15}4wj9:n~txrzvs1Ր+5m/rcah}Pdl(�HWfƻҶܢ[8[Nf81H/FVmF)2Mx\+NG'u.W6E~;.;h`V[6x'sAԐ(��
SakuraSnowAngel83@protonmail.com
```
***SakuraSnowAngel83@protonmail.com***. This part that was 'alive' serendipitously turned out to be the email address that we wanted.

### Original Solution: OpenPGP Utility
If I knew what PGP was, I would have likely went for this easier route; **using dedicated OpenPGP tools that basically fetches data for you.**  
The best tool for this is **[pgp.help](pgp.help)**, which is an encryption preview tool that shows you what will the encrypted output look like when you input a message through a user's PGP public key.

To use this, you must **copy the entire text from the github *publickey* file, including from ```-----BEGIN PGP PUBLIC KEY BLOCK-----``` to ```-----END PGP PUBLIC KEY BLOCK-----```.**  
Then, paste it in the **IMPORT KEY (Paste PGP or AGE Key...)** box. Right as you paste the key, the box will literally display the email address in big bold letters, like this.

> ### SakuraSnowAngel83@protonmail.com
> Unsaved  
> ```ID: ecdd0fd294110450```  
> ##### *[Show more details...]()*

***Tip:** To clarify this is a real email address, you can send a mail to it by yourself. It will successfully send, which indicates a valid address.*

The attacker's email address is ***SakuraSnowAngel83@protonmail.com***.

## 3-2. What is the attacker's full real name?
>***Answer: ```Aiko Abe```***  
*❌ Not Solved | Very Easy, One of the websites removed*

### A. From Twitter

When you go to ```SakuraSnowAngelAiko```'s Twitter page '@SakuraLoverAiko', you can find a post that says:

> ### <img src="https://pbs.twimg.com/profile_images/1353157001933541377/LIQFF66o_400x400.jpg" width="20"> Aiko
> @SakuraLoverAiko 
> Silly me, I forgot to introduce myself!
>
> Hi there! I'm [@AikoAbe3](https://x.com/AiKOABE3)! 
> ###### 12:56 PM · Jan 30, 2021

The account handle ```AiKOABE3``` does look like a legit Japanese name, 'Aiko Abe'.  
However, We don't know if this is the full name. Lots of people in social media do intentionally shorten their user handles for less general tediousness. 'Aiko' could be short for 'Aikomi' or 'Aikova'. 'Abe' could be short for 'Abeno', 'Abematsu', 'Abekawa', and much more.  
Also, it's possible that ```AiKOABE3``` could be a fake name or just a random nickname (like 'SakuraSnowAngel'). It is an incredibly common practice to make a non-real name persona, mainly for *privacy protections*.

We need a more of an official profile to clarify whatever the attacker's full name is. Luckily, there is one more website associated to the attacker's username ```SakuraSnowAngelAiko```, which is their **LinkedIn profile**.

***❗️IMPORTANT:** The LinkedIn account for Aiko Abe no longer exists. The account was likely flagged as a fake persona ID by the moderators and deleted from the platform. I initially skipped this step in my walkthrough and verified the solution from the **[official write-up from OSINTDojo.](https://github.com/expressito/SakuraRoom/blob/main/README.md)***

> ### <img src="https://pbs.twimg.com/profile_images/1353157001933541377/LIQFF66o_400x400.jpg" width="80">
> ## Aiko Abe
> Senior Software Engineer  
>Japan · [Contact info]()
> 
> [Connect]() &emsp; Message &emsp; More


According to the LinkedIn profile (with the same profile picture as their Twitter account), we can see that the attacker's real name was indeed ***Aiko Abe***.

### Takeaway
So our earlier suspicion was right all along. Did we just waste time searching this up when we could have just gone with the guess 'Aiko Abe'? Why could we trust their LinkedIn profile but not their other Twitter account?

The biggest difference between the two platforms is that **LinkedIn enforces a very strict legal name policy. It is prohibited to use pseudonyms, fake names, or even numbers in your name field when you are creating an account.** This is why we can trust the name on this platform, because the name should be real for the account to even exist in the first place.  
In Twitter, the vast majority of users use non-official names. There are accounts like ```@HumansNoContext```, ```@literallymecats```, or ```@nocontextmemes```. These are obviously not names of people, and it is totally fine to use them, because its a free social environment where anyone could make an account and interact with people on the platform.  
However, LinkedIn is a business social networking service, where **people build their digital resumes consisted of their real achievements, find job opportunities, and establish serious business interactions.** Therefore, legal information is necessitated to connect with real corporations.

<br><br>

## Task 4. UNVEIL

Let's go back to the **[Github repositories](https://github.com/sakurasnowangelaiko)**. Right now, there are only 5 pinned repositories at the home screen, where 3 of them are forks and 2 of them are original repos that we already looked at. There must be more stuff hidden behind the account.

Clicking on the **Repositories(9)** tab will show us 4 more repos that was not visible on the pinned tab. One of the new repositories is named ETH, which is the only original repo between the four. This repository is where we are going to get the most information.

### Before we Move On...

Both when finding the ETH and the PGP repository back in task 2, we excluded all the forked repositories. This is because all of the forks are most likely to not contain any personal or ```SakuraSnowAngelAiko```- exclusive information.

To understand why, there are two Github features to know first; **commit** and **fork**.

A ***Commit*** is like a project update snapshot or a 'savestate' like in a game. Whenever you 'commit' your project, Git records exactly what all your project files look like right now, bundles it with metadata (like who made which parts) and stores it in the project repository's **commit history** forever. The repository will have every single version each time you committed, which is **not removable** (unless you use force pushes or delete the repository entirely).

***Forking*** a repository is like a *remix*; You can copy a project from someone else, and 



## 4-1. What cryptocurrency does the attacker own a cryptocurrency wallet for?
>***Answer: ```Ethereum```***  
*✅ Solved | Easy*