---
layout: post
comments: true
title: "How to watch movie together with Skype on Mac"
date: 2018-07-30T18:27:10-07:00
categories: "notes"
tags: notes skype vlc
---
## Problem
I want to watch movie with my son when he is far away and be able to comment in voice, control playback with pause/play

## Assumptions
+ Skype screen sharing/mic/speakers just work
+ Movie is translated from Mac (macOS High Siera 10.13.6)
+ Skype version 8.26.0.70

## Solution
### Aggregated device
In order to have movie sound track and my voice on another side we need some kind of mixer for these sources.

Fortunately, macOS has such a solution out of the box called 'Audio MIDI Setup'

![Audio MIDI Setup Icon](/assets/audio_midi_setup_icon.png)

We need to create Aggregated device composed from output and mic

![Create Aggregated Device](/assets/create_aggregate_device.png)

With configuration like this

![Aggregated Device Configuration](/assets/aggregate_device_configuration.png)

Now let us use it for standard input (in other case Skype doesn't see it somehow)

![Use Aggregated Device for Input](/assets/use_device_for_input.png)


### Skype configuration
How let us teach Skype to use our new device instead of standard mic and not adjust sound too much.

![Skype Configuration](/assets/skype_mic_configuration.png)

## Thats it!
Just make a call, start sharing your screen and play movie.
Sure, some sound/mic adjustment is needed.
My are following

#### Volume levels

System Volume

![System Volume](/assets/system_volume_level.png)

VLC Volume

![VLC Volume](/assets/vlc_volume_level.png)
