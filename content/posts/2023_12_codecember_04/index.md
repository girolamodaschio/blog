---
title: Balancing Memory and Convenience
date: "2023-12-06T08:12:03.284Z"
categories:
    - blog
tags:
  - codecember23
---
Hello reader,
With ~50GB of photos on your phone, the dilemma of organization looms. You weigh two strategies.

Strategy one involves a memory-heavy approach: transferring all photos to your computer, testing, deleting duplicates, and then uploading to iCloud. 
Pros include easier selection, but the downside is the process is not directly from your phone. 
Yet, you accept the memory trade-off for curation convenience.

Following the tutorial, you efficiently download photos, creating a data directory. 

```
for photo in api.photos.albums['Screenshots']:
    download = photo.download()
    with open(os.path.join('data', photo.filename), 'wb') as opened_file:
        opened_file.write(download.raw.read())
        print(f"{photo.filename}, {counter}/174")
        counter +=1
```

While effective, a concern arises about potential computer memory exhaustion due to the image volume.

Attempting to cross-verify with iCloud, you encounter a slow process, impeding progress past the proof-of-concept stage.

As you navigate these strategies, you're at a crossroads—balancing memory and efficiency is crucial for seamless photo management. 
Until next time, explore the delicate dance of memory and strategy in your quest for an organized digital photo collection.

Follow my work on github!
[github.com/girolamodaschio/codecember](github.com/girolamodaschio/codecember)
