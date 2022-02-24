---
title: "Insert Image with Markdown"
date: 2022-02-24T10:36:29+08:00
description: "Explain how to insert image in markdown writting"
tags: ["writting"]
---

Images help us better describe our ideas. When I write my blogs, I find two
approaches to inserting them. I note down these two in this blog.

<!--more-->

## The Markdown Way

Markdown comes with _native_ image support. It's similar to how we insert a
hyperlink.

```markdown
<!--Hyperlink-->
[This_is_an_example_website](https://example.com)

<!--Image-->
![Rosmontis](/images/rosmontis.jpg)
```

In this way, image is shown in its original size.

![Rosmontis](/images/rosmontis.jpg)

It's obvious that this image is too big to fit in our blog! We shouldn't let it
mess up our typesettings, thus we need an approach to modifying images' size
(width and height), alignment, and etc. Unfortunately, as we can see from the
markdown codes above, there's no way that we can set down these configurations.

## The HTML Way

Markdown's inline HTML support provides the solution that's just what we need.
HTML provides `<div>` and `<img>` tags to config alignment and image size. Here,
take another blog, [About][1] as an example.

```html
<!--
    "align" specifies alignment, available values can be
    "left", "right", "center", "justify".
-->
<div align="center">
<img src="/images/rosmontis.jpg" width="15%", height="15%"/>
</div>
```

The image is shown as below.

<div align="center">
<img src="/images/rosmontis.jpg" width="15%", height="15%"/>
</div>

This looks better, isn't it?

## Further Read

The tutorials at w3school.com give detailed description of HTML. You can find
more details about [div][1], [img][2], and _etc_. there.

[1]: https://www.w3school.com.cn/tags/att_div_align.asp
[2]: https://www.w3school.com.cn/tags/tag_img.asp
