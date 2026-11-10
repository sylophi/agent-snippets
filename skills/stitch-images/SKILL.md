---
name: stitch-images
description: Use when several images you're handing the user would read better as one combined image.
---

# stitch-images

Combine images when seeing them together helps the user compare or follow
them. Send them separately when each needs its own full view.

Choose the layout, crop, captions, annotations and styling to suit the
content. Look at the result before sending it.

## If using montage

`magick montage` handles most grids, rows and stacks:

```sh
magick montage -background '#1a1a1a' -fill white -pointsize 26 \
  -gravity north -tile 2x -geometry +14+14 \
  -label 'First caption' one.png \
  -label 'Second caption' two.png \
  out.png
```

- Without `-geometry`, tiles shrink to thumbnails. `+14+14` only adds padding and keeps native size. To shrink, fit on width alone (`620x+14+14`), because `620x620` strands captions.
- Without `-gravity north`, shorter tiles center vertically in their row.
- A shorter tile after a taller one in a row gets its caption at the bottom of the row. Bake captions in first:
  `magick -background '#1a1a1a' -fill white -pointsize 26 -gravity center \( tall.png label:'Tall' -append \) \( short.png label:'Short' -append \) miff:- | magick montage - -background '#1a1a1a' -gravity north -tile 2x -geometry +14+14 out.png`
- On ``unable to read font``, pass `-font` with a name from `magick -list font`.
