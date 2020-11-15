---
id: 29
title: junction maker (get list of symlinks and commands in Windows)
date: 2019-10-05T14:52:15+00:00
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
here&#8217;s some Python code I wrote a while back (nowhere near efficient or perfect) to get the junctions from NTFSLinksView (as in, you export your symlinks from there and then I wrote a script to mass convert them into commands)

<pre class="wp-block-code"><code># Junction Parser
# This program parses the text output of symbolic links in NTFSLinksView and makes an array of junctions
# It optionally runs mklink /J to set the links if you want

from __future__ import print_function
import os

junctionsdumppath = "" # INPUT PATH GOES HERE
f = open(junctionsdumppath, "r")

#=
#Name
#Full Path
#Type
#Target Path
#Created Time
#=
# 7 lines, we need to check 3rd, 4th, and 5th

# then skip a line, then check if the equals thing is there, else stop reading
# for each 3rd 4th 5th line if 4th is 'Junction' then .append([3rd,5th])

# then mklink function can be looped which would run command 'mklink /J "' + a[0] + '" "' + a[1] + '"' i think

def chunkparser(astr):
    try:
        parsed = []
        thelist = astr.split("\n")
        if thelist[3] == "Type              : Junction":
            parsed.append(thelist[2][thelist[2].find(":")+2:])
            parsed.append(thelist[4][thelist[4].find(":")+2:])
            return parsed
        else:
            return "don't add"
    except Exception:
        return "error"

def mklink(element):
    return 'mklink /J "' + element[0] + '" "' + element[1] + '"'

# default will say "Junction created for _____ &lt;&lt;===>> _____"

junctionstr = [item.replace("\x00", "").replace("\n", "") for item in f.readlines() if item.replace("\x00", "") != "\n"]

junctionchunk = []
i = 0
while len(junctionstr) - i >= 7:
    tempstr = ""
    for j in range(i, i+7):
        tempstr += junctionstr[j] + "\n"
    junctionchunk.append(tempstr)
    i += 7

# print(junctionchunk[10]) # for testing

junctionlist = [chunkparser(item) for item in junctionchunk if chunkparser(item) != "don't add"]
print(junctionlist)

# to export mklink commands to a text file, uncomment this
#'''
mklinkpath = "" # PUT OUTPUT PATH HERE
a = open(mklinkpath, "w")
for junction in junctionlist:
    a.write(mklink(junction) + "\n")
a.close()
#'''

# mklink code, uncomment to run
# for thing in junctionlist:
#     os.system(mklink(thing))

f.close()</code></pre>