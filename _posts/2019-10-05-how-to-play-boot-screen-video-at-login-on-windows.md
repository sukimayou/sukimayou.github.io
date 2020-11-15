---
id: 36
title: how to play boot screen video at login on Windows
date: 2019-10-05T15:39:57+00:00
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
i used this to play the PS1 boot screen at startup every time, is pretty cool

  1. make a .bat file
  2. depending on video player put this in .bat file:
      * for VLC: `"C:\Program Files (x86)\VideoLAN\VLC\vlc.exe" <filename> -f --play-and-exit --no-one-instance --mouse-hide-timeout=1 --video-on-top --no-video-title-show`
      * for MPC-HC: `"C:\Program Files (x86)\MPC-HC\mpc-hc.exe" <filename> /play /close /fullscreen /new`
  3. create a shortcut for the .bat file and put it in `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`
  4. it should work and to skip it click as soon as it pops up, then ESC, ALT+F4