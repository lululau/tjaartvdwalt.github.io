+++
date = "2015-09-02T11:16:50"
image = ""
math = true
tags = ["software"]
title = "photo_rename 0.1.1"

+++

I want to announce the release of version `0.1.1` of my Free Software tool [photo_rename](https://github.com/tjaartvdwalt/photo_rename/). To understand the importance of this release, you need to know some of its back story:

# The tragedy #

A couple of years ago, in a comedy of errors, I accidentally deleted my photo albums from my computer. In a further twist, my off-site backup was not working correctly. I did not realize the mistake immediately, so traditional undelete tools could not recover the files, since I had written to the disk since the accident. Eventually I managed to save most of my images using [Foremost](http://foremost.sourceforge.net/).

> This is not a post on my recovery process, but, Foremost is a great little tool when other undelete methods are not available.


# The manual solution #

The big problem I had after the recovery process was that `Foremost` could not recover the original file names. This meant that I had a pile of 29685 images, all with generic file names, all in a single directory, to sort through. This was a massive undertaking, but my wife and I sorted the images into different albums based on what we could remember. We did not try to change the file names individually, because (even with my perfectionist tendencies) we would have gone insane.

# A side project #

Some time after this, I wrote `photo_rename` . This tool renames images into what is essentially the *Android Stock Camera* naming scheme: `IMG_yyyymmdd_hhmmss.jpg`.

The reason I wrote  this was that my wife and I were taking photos on multiple devices. We wanted to consolidate the images into a single album, but because our devices used different naming schemes the files would not be chronologically ordered when we browse the album.

From the start of this project  I had thought about fixing my recovered photo names using this tool, but my initial version used the JPEG file's `mtime` property to extract the file creation date. For my recovered photos this would be the date and time on which they were recovered, not the date on which they were taken. This was not the solution I was looking for.

# "I love it when a plan comes together" #

Finally, today, I released version `0.1.1` of `photo_rename` which solves this issue. The tool now uses the *exif meta data* that is embedded inside the JPEG image in order to create the new filename. This method is much more reliable, as this date is not as easily changed as the `mtime`. Finally, after almost two years, I can put this issue to rest.
