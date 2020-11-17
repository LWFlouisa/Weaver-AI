---
title: Image Detection In Ruby, And Why It Works
published: true
---
In general, you cannot just take a raw image in JPEG format and have a program compare these. The reason is that it ends up comparing the filenames, and not the content of these files. What you need to do is first convert those images to .txt format from JP2A, read in those files, and then compare those read in files.

The reason this is able to work, is that it's not actually important to capture the entirety of an image, but rather its essential essence. This opens up the possibilities for having an input vector be based on the essence of an image, from than typing out an input. Eventually going to try and see if this works for audio as well, though I wouldn't place my bets on it.

My end goal is to make it so that I can take a picture of my emotion, and then try and guess what my emotion is from that essence of an image.
