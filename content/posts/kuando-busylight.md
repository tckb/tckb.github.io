---
author :  'Chandra Tungathurthi'
date :  2020-07-07T13:59:56.851Z
title : |
 Hacking Kuando Busylight for your own good 
description : |
 Hacking Kuando Busylight to create new and fancy status lights. This article explains how the communication protocol works.
keywords :  
 - 'Kuando Busylight'
images :
 - /img/busylight.jpg
aliases:
  - /2020/busylight-driver/
tags:
- Busylight
---
![Kuando Busylight UC Omega interfaced with Raspberry pi running Busylight REST interface](/img/busylight.jpg) _Kuando Busylight Omega interfaced with Raspberry pi running Busylight REST interface_  

## _How Kuando Busylight actually works(-ish)..._

Kuando [Busylight Omega](https://www.plenom.com/products/kuando-busylight-omega/) is a wonderful product as a status indicator whether in a personal space or in a work environment. For the uninitiated, Busylight is a small status "blub" that shows different status indicators depending on your presence status whether "in-call" or "busy" or simply "do-not" disturb me.

Here's how it works from their promo:

{{< youtube lhaQfAR8z8o >}}

Busylight in-general, is targeted more towards the "corporate" crowd, meaning it has fantastic integration to enterprise applications like Microsoft Lync, Skype for Business, 3CX etc., but not much for the public.

The mood of the light, ringtones etc can not be used outside the boundaries these applications. Although in theory you can use it to pretty much any application, if you "crack the code".

To their defense, they did release [desktop application](https://www.plenom.com/downloads/download-software/) and an "[SDK](https://www.plenom.com/downloads/download-software/#sdk)", (which is available __only on request__). BUT a huge complaint on my side is that there are no SDKs available for either Java or related. Working with JavaScript and/or any non-open source languages is a big no-no for me. There were some projects available [here](https://github.com/porsager/busylight) and [here](https://github.com/mccarthyryanc/busylight) that tried a way to access Busylight programmatically. There were fine, but almost all of them riddled with this ["magic" initial value](https://github.com/porsager/busylight/blob/master/lib/busylight.js#L43) that has no explanation. The one I hate the most is working with "magic" values that preassumbly has no reasons behind it.

But, a great resource along with these project is this unappreciated [communication protocol document](https://github.com/porsager/busylight/blob/master/Busylight.API.rev.2.2.-.22052015.pdf).

Bingo! 🎇🌟 this was a gold mine for me, this means I can communicate with Busylight pure and unadulterated, without any middle man. This idea was very much appealing to me. It didn't take too long for analyzing the protocol, after few days of trial and errors, I disentangled this magic code that everyone seems to be using without any logic what so-ever.

## The Communication Protocol

The "protocol" is nothing but a series of bytes that are sent across to Kaundo Busylight via the underlying USB comm layer. For sake of explanation lets call this a **Protocol Specification (ProtocolSpec)**. This article or any future articles does not cover the actual transfer of this signal. Instead we will focus on the structure of this payload, how to construct it, manipulate it to get different light settings and assume that the transfer actually works from the underlying driver.

### The Protocol Specification

As per [rev2.2](https://github.com/tckb/busylight_driver/blob/b59e4988e78c3d245ae6158690d1001475e71ae6/docs/Busylight.API.rev.2.2.-.22052015.pdf) the following spec works for the following Plenom devices --

| Serial | Device Model | 
|-|-| 
| 0x3BCA | Busylight Alpha | 
| 0x3BCB | Busylight UC | 
| 0x3BCC | Kuando Box | 
| 0x3BCD | Busylight Omega |

The specification is just 64 byte payload arranged in 8x8 byte grid. Each 8 byte order is a **ProcotolStep.** Every **StepByte** represents an unsigned integer with values 0 - 255.

![An 8x8 byte grid overview of the Communication protocol specification for all Kuando Busylight devices](/img/busylight_protocol.png)_An 8x8 byte grid overview of the Communication protocol specification for all Kuando Busylight devices_


There can be 7 protocol steps (7x8 bytes) that constitutes the actual **data payload** and the last 8x8 bytes are the so-called **Meta payload**; of which the last two bytes 63, 64 bytes are the checksum of the data payload (ordered as [MSB](https://en.wikipedia.org/wiki/Bit_numbering#Most_significant_byte), LSB). The checksum itself is just the sum of the all values in the data payload. This can be calculated as follows:

// checksum formula here

The Charecteristics of each step is defined by this so-called _Command Byte_ and Kuando Busylight can technically run 7 steps at a time on a single **ProtocolSpec** until the signal timeout. So, for Busylight to run the same spec _continuously_ one must send this **KeepAlive** command periodically before the timeout.

### StepBytes

A ProtocolStep starts with a commandbyte which defines what kind of protocolspec that is being sent to the device. A detailed explanation of each stepbyte is available as follows -

| CommandByte | byte format | notes |
|:-:|:-:|:-:|
| RESET\_DEVICE | 00100000 | resets the device |
| START\_BOOT\_LOADER | 01000000 | starts the boot loader |
| KEEP\_ALIVE | 1000XXXX | XXXX 4 bits for timeout |
| NEXT\_STEP | 00010XXX | XX 3 bits (0-7) defines the step to execute next |  
  
| RepeatByte | byte format | notes 
|:-:|:-:|:-:|
| \*\*\*\* | XXXXXXXX | 8 bits (0-255) defines how many times the current step should execute |  
  
| ColorByte | byte format | notes |
|:-:|:-:|:-:|
| RED | XXXXXXXX | 8 bits (0-255) defines the \_intensity\_ (not pixel) value of red LED in the device |
| GREEN | XXXXXXXX | 8 bits (0-255) defines the \_intensity\_ (not pixel) value of green LED in the device |
| BLUE | XXXXXXXX | 8 bits (0-255) defines the \_intensity\_ (not pixel) value of blue LED in the device |  
  
| TimeByte | byte format | notes |
|:-:|:-:|:-:|
| TIME\_ON | XXXXXXXX | 8 bits (0-255) defines the time duration in seconds (value/10, max 25.5 seconds) where the lights (red, green, blue) leds are switched on. |
| TIME\_OFF | XXXXXXXX | 8 bits (0-255) defines the time duration in seconds (value/10, max 25.5 seconds) where the lights (red, green, blue) leds are switched off. | 

| ToneByte | byte format | notes 
|:-:|:-:|:-:|
| \*\*\*\* | CTTTTVVVV | \*\* || C \[Tone Override Flag\] | C | 1 bit = 1 / Previous Tone will be overridden with the current one, = 0 / Tone Settings will be ignored |
| T \[ToneId\] | TTTT | 4 bits (0-15)   1 = OPENOFFICE, 2 = QUIET, 3 = FUNKY, 4 = FAIRY\_TALE, 5 = KUANDO\_TRAIN, 6 = TELEPHONIC\_NORDIC, 7 = TELEPHONIC\_ORIGINAL,  8 = TELEPHONIC\_PICKMEUP 9 = IM\_1, 10 = IM\_2, 13 = IM\_3 |
| V \[ToneVolume\] | VVV | 3 bits (0-7)  0 - no volume, mute 7 - max volume |  

| MetaStep | | |
|:-:|:-:|:-:|
| SensitivityByte | XXXXXXXX | 8 bits (0-31) configure sensitivity (only for Kuando Box) |
| TimeoutByte | XXXXXXXX | 8 bits (1-30) seconds before the current spec times out (only for Kuando Box) |
| TriggerByte | XXXXXXXX | 8 bits (1-250) milliseconds trigger time (only of Kuando Box) |
| ChecksumMSBByte | XXXXXXXX | Most significant byte of 2-byte-Checksum |
| ChecksumLSBByte | XXXXXXXX | Least significant byte of 2-byte-Checksum |

On first glance, this is sounds quite intimidating, in-fact this _is_ but on the other hand it gives a lot of opportunity to explore more and gives us more control over what is possible with the device. After digging a little bit more, I wrote [*Java* driver](https://github.com/tckb/busylight_driver) for communicating with Busylight device.

### So, Now what? What is possible with learning about this Spec

Here's what is possibly possible by learning about this spec. I wrote a [nice REST interface](https://github.com/tckb/busylight_rest) which uses this driver. 

The rest server can now run on a [raspberry pi](https://www.raspberrypi.org/) and then the uses are limitless :) 

![Raspberry Pi running Kuando Bustlight REST interface](/img/busylight_rpi.png)_Raspberry Pi running Kuando Bustlight REST interface_



for example, you can use your beloved [Apple Shortcuts](https://www.youtube.com/watch?v=3QBQz5ATS3s) for setting your status, may be sprinkle-in  a voice action perhaps?


{{< tweet 1171320757550485504 >}}

What would you use it for?