```diff
- THIS IS FOR EDUCATIONAL PURPOSES ONLY I'M NOT RESPONSIBLE FOR HOW THIS KNOWLEDGE IS USED
```
This is an ongoing experiment to try to find ways around Windows Defender's method of catching layerless zip bombs. Mainly a fun & educational hobby for me as it showcases how Windows deals with seemingly infinite zip sizes. A layerless zipbomb is still a zipbomb, so its main goal is to wreck an unsuspecting system but for the most part it can be reversed.

Be nice. Don't turn this in to your poor professors.
---
## Layerless Zip Bombs?

Traditionally zip bombs such as 42.zip rely on zipping things throughout multiple layers. Antiviruses evolved to learn to unpack only the first 3 or so layers then stop, which defeats the main use of the bomb. A researcher by the name of David Fifield found a way to make it so you'd just need to unpack the first layer to unleash everything that the zip bomb has. I won't go indepth about how it works here, but I do highly reccomend you [visit his site](https://www.bamsoftware.com/hacks/zipbomb/) and [watch his showcase](https://www.bamsoftware.com/talks/woot19-zipbomb/) to learn more.

## Story
July of 2020 I saw that the way Windows Defender deals with a layerless zip bomb is to actually unzip it first. This is pretty hilarious, especially since the research showcasing this type of attack was already talked about for half a year. I decided to look into if this vulnerability still exists, and if not, how I can defeat it.

Here we are 3 years later, testing it on a Windows 10 system on the latest July 2023 update (10.0.19045 Build 19045). I chose Windows 10 because it reflects what most users own today, and that it relatively has the same Windows Defender version as Windows 11. Most of Windows 11's increased security comes from physical hardware requirements, such as TPM, not through software.

## Browser Handling
Some browsers are confused as to what the layerless zip bomb actually is, and some flag it as malware but won't push it. 
- Firefox immediately flags it as malware, but if you tell it to allow the bomb it'll do so. 
- Internet Explorer just uses Windows Defender (we'll get to that later).
- Chromium based browsers download it, but continues to buffer afterwards. The buffer after the download is finished is actually the browser unpacking it, presumably to check for viruses. Disk space is being eaten away with the drive at 100% usage.

## Windows Defender

I admit I didn't think Windows Defender would be this predictable or bad at handling a layerless zip bomb. There's 2 files worth noting here, zbxl.zip and zblg.zip. the zbxl is the massive 46mb zip that extracts to 4.5pb. The zblg is a 10mb zip that extracts to 281tb. Everything done is on a fresh new copy of Windows 10 on default settings.

### How does it handle zbxl?

zbxl is the zip I saw being unpacked by Windows Defender as it tries to scan it for malware back in 2020. Today that is no longer the case. Instead, Windows Defender immediately calls it malware as soon as it's downloaded, and will refuse to scan it. Quoting, "Item skipped during scan... due to exclusion or network scanning settings."

### How does it handle zbxg? 

It doesn't! I'm not kidding, it completely bypasses Windows Defender real-time detection. To add insult to injury, if you scan it, it'll literally try to unpack it. After a while the scan will conclude, saying no threats were found and returns your disk space back. 

### Why does Windows Defender only catch the petabyte zip?

Probably because Microsoft decided to target this specific hash, seeing as how its easily available. Or there's a very concrete file size number inside a zip that Windows is looking out for. I have my doubts about the second method purely due to how quickly Windows Defender flagged and contained the zip file. Either way, they both explain why a zip file containing a measly 281tb slipped on by.
