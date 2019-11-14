# Replay Buffer
What is replay buffer?<BR>
Replay Buffer
> Replay buffer allows you to save the last X seconds of Video and Audio to your disk on the press of a button.

This functions very similarly to NVDIA's Shadowplay and AMD's ReLive, and other programs that have a replay feature.

The main advantage of using Replay Buffer over these other alternatives is that Replay Buffer uses RAM as a temp storage. It does this rather than constantly writing to a drive (typically the temp folder in C:), which can burn up SSD writes.

##### Table of Contents
- [Replay Buffer](#Replay-Buffer)
- [Settings](#Settings)
  - [Output](#Output)
  - [Audio](#Audio)
  - [Video](#Video)
  - [Hotkeys](#Hotkeys)
- [Using Replay Buffer](#Using-Replay-Buffer)
- [Additional Information](#Additional-Information)

# Installing OBS
First, pick between [OBS Studio](https://obsproject.com/) and [StreamLabs OBS](https://streamlabs.com/).
I prefer OBS for local recordings as there are less hiccups due to SLOBS stream features.

# Settings
Here we'll go through all the settings necessary for both general recording and replay buffer.
## Output
First, start with the **Recording** tab, we'll be skipping over the **Stream** tab for this tutorial.
![](https://i.imgur.com/TGFwr03.png)
Set the **Output Mode** to Advanced.<BR>
Leave **Type** on Standard.<BR>
Set **Recording Path** to somewhere.<BR>
Set **Recoding Format** to mp4, *if you use multiple audio tracks (recommended).* You can use MKV, but I wouldn't recommend OBS's built in remux function. Look into FFMPEG for remuxing.<BR>
Skip **Audio Track** for now, we'll return to this.<BR>
This gets a little complicated. Refer to **[Differences in Encoders](#Differences_in_Encoders).**<BR>
**DO NOT SET RESCALE OUTPUT HERE.** THIS CAN FUCK YOU UP. DON'T DO IT.<BR>
For **Rate Control**, select CQP and set it to 20-21 (Lower = better quality). This results in large file size, but superb quality. Refer to [Rate Control](#Rate-Control) for the other options.<BR>
**Preset** should be set to either Max Quality or Low-latency Quality, whichever gives you the best results.<BR>
**Profile** should be set to high, but if you have issues use main. (High decodes better, but encoding can be worse *sometimes*).<BR>
**GPU** should be always 0, unless you have a spare GPU, but even then its typically not worth doing. [Explanation](#Mutli-GPU)<BR>
Leave **B-Frames** on 2, you generally shouldn't touch this unless you REALLY know what you're doing.<BR>

Now, on the **Audio** tab, set all the tracks to be 160 for their bitrate.
![](https://i.imgur.com/rb2QFhc.png)

As for the **Replay Buffer** tab, check the box for enabling it.
![](https://i.imgur.com/PIhUjsk.png)
Set **Maximum Replay Time** to whatever you desire. Try to keep this somewhere from 30s to 4m (240s). Anything longer and you're better off with a normal recording.<BR>
Set **Maximum Memory** from 1GB (1024MB) to 4GB (4096) depending on how long your buffer is, and your rate control.

## Audio
As for the **Audio** section in settings, there is very little to configure here.
![](https://i.imgur.com/rglgF5U.png)
Set **Sample Rate** to 44.1KHz, unless ALL *OTHER AUDIO DEVICES* are 48KHz.<BR>
Set **Devices** as necessary, disable devices you do not actively use.<BR>
Everything else is fine as default.

## Video
Now in the **Video** category in settings, there's only a few options but they are the most important.
![](https://i.imgur.com/5PCxy6K.png)
**Base Canvas Resolution** should be set to your native res.<BR>
**Output (Scaled) Resolution** should be set to your native res.<BR>
**Downscale Filter** can be anything, since we aren't descaling. For performance reasons, set this to *bicubic*.

## Hotkeys
In order to use **Replay Buffer**, there's only one option that is necessary here.
![](https://i.imgur.com/gkyfZ6G.png)
You **must** set a hotkey for **Save Replay** under **Replay Buffer**.<BR>
You can set hotkeys for **Start Replay Buffer** and **Stop Replay Buffer**, but they are unnecessary as you can start and stop it from the UI.

# Using Replay Buffer
Once you have the **Scene** selected you want to record, simply start the replay buffer with the **Start Replay Buffer** button in the bottom right of the main UI.
<BR>![](https://i.imgur.com/9T5HCch.png)<BR>
You can stop the replay buffer with the **Stop Replay Buffer** button once it is active.

Press the **Save Replay** hotkey anytime you want to save your buffer of **Maximum Replay Time**.

# Additional Information

## Differences in Encoders
There are 5 options for encoding, some might not be available to you.
* AMD VCE Encoders
* Intel QuickSync
* Nvidia NVENC
* Nvidia NVENC (New)
* x264

NVIDIA cards might have two options, or just one.
If your card is 1000 series or below, you'll want to stick to the old encoder, I found issues with encoding on new on a 1080.<BR>
If you have a 2000 series card, the new encoder will be your new best friend. Only use old if you have to.

For AMD make sure you have the AMD OBS plugin (may not be included).<BR>
Intel integrated chips can try to use QuickSync.<BR>
If no other options are available to you, you'll have to use x264.

## Rate Control
There are 4 options for rate Control
* VBR
* CBR
* CQP
* Lossless

VBR - *Variable Bitrate*: Chooses a variable bitrate based on two parameters, an average birate and a max birate. Decent quality, but can have inconsistent filesize.<BR>
CBR - *Constant Bitrate*: Uses a user-set bitrate to encode with. Good quality, allows for controlling filesize at the cost of quality.<BR>
CQP - *Constant Quantization Parameter*: it chooses a variable bitrate based on the QP and content being recorded, but is much better (faster, higher quality) than VBR.<BR>
Lossless: No rate control, meaning very little compression. **Massive file size**, no quality loss from source.

## Multi GPU
Why shouldn't I set my second GPU in OBS?
> Unless you have a SLI (NV Link) or XFIRE, this would have to transfer the B-Frames for encoding from one GPU across the PCI lanes to the CPU and back to the other GPU. Any processing power here would be lost to the transfer time.
