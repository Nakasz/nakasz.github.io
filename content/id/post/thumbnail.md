---
author: Lucas David Vadilho
title: Post Thumbnail
date: 2023-08-06
thumbnail:
    src: 'images/thumbnail.jpg'
    alt: 'Imi em meio as flores'
    object_position: '50% 100%'
    height: 250px
categories: ["heyo"]
tags: ["theme", "shortcodes"]
images: ['images/thumbnail.jpg']
---

You can now add thumbnails to your posts in {{< theme >}}!

<!--more-->

To add a thumbnail to your post you just need to add the variable `thumbnail` to the front-matter. For the thumbnail you can define:

| Parameter | Description | Default |
| --- | --- | --- |
| `src` | Image source, can be any URL or relative path | |
| `alt` | The image's alt value, can be any string | `thumbnail` |
| `object_postion` | Values that will be passed to the image's [object-position](https://developer.mozilla.org/en-US/docs/Web/CSS/object-position) | `50% 50%` |
| `height` | Thumbnail container height [^1] | `250px` |

[^1]: The thumbnail height default is `250px`. It is managed by the `--thumbnail-size` CSS variable, you can easily override it globally with custom CSS.

# Example

The thumbnail in this post was generated by the following section in the front-matter:

```yaml
thumbnail:
    src: 'images/thumbnail.jpg'
    alt: 'Post thumbnail'
    object_position: '50% 100%'
    height: 250px
```

# Credits

Imi -- the [character](https://www.tdvadilho.com/portfolio/?id=imiFlores) in this thumbnail -- was graciously provided by TD Vadilho, you should checkout his [website](https://www.tdvadilho.com?utm_source=heyo) and [YouTube](https://www.youtube.com/@TDVadilho)!