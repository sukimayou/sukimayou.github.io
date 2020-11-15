---
id: 23
title: extracting 3DS music into soundfonts and MIDI
date: 2019-10-05T14:09:24+00:00
author: sukimayou
layout: post
av_cpm_meta:
  - 'a:2:{s:7:"message";s:0:"";s:9:"published";b:0;}'
avopt_banners_inside_post:
  - 'true'
avopt_banners_on_page:
  - 'true'
categories:
  - Uncategorized
---
here&#8217;s a text file on my PC:

<pre class="wp-block-code"><code>use ninfs

depending on Windows or Linux respectively
py -3 -m ninfs gui
python3 -m ninfs gui

if on SD card, drag Nintendo 3DS folder in with movable.sed (found in NAND or in backups or whatever I have it)
then mount and get your app files

then put app files in ninfs and extract romfs and get bcsar files

edit 11/05/2019: for cia you can use
ctrtool --contents=contents game.cia
ctrtool --exheader=exheader.bin --exefsdir=exefs --romfsdir=romfs --logo=logo.bcma.lz --plainrgn=plain.bin &lt;contents.0000.xxxxxxxx>

then go to BCSAR_ExtractTools (found on github), extract bcsar and then convert all bcwav to wav (i used audacity fork by jackoalan, hopefully there's a better way)
edit 11/09/2019: do not use audacity fork, use foobar2000 with the vgmstream plugin as it will export the loops properly when converting everything at once (add all songs to a playlist, select all and convert to WAV)

convert bcseq to MIDI and run sf2comp with text file in root of extracttools also the wav files as well

mess with soundfont using polyphone (or viena i guess?) if you need to but it should work

and that should be it

also note: VGMTrans (VGMTrans_WTL.exe) is way better but does not support BCSAR, it does support BRSAR on the Wii and you can export MID and SF2 way more easily</code></pre>