---
layout: post
title: React-Upload File with Cloudinary
date:   2018-12-03 19:07:19
modified:  2018-12-03
comments: true
img: cloudinary.jpg
---

The React SDK does not include a function for uploading images to Cloudinary. We can also write own JavaScript code for direct uploading with an upload preset.

Before you start, you can read that post. Here I’ve told you how to upload images to the browser.

```javascript
const API_KEY=xxXXxx
const UPLOAD_PRESENT_ID=xxXXxx
```

<script src="http://gist.github.com/ebrugulec/22b0e2fb1527e3c42bcd2ae01da14d17.js"></script>

I want to upload just one file every upload, therefore i used this.state.files[0].

**upload_preset** is important section. You can check Setting > Upload > Upload presets for your present id. But don’t forget to check, upload present must be **Unsigned**.

Now you can upload your images.

And also you can check [this](http://github.com/ebrugulec/image-text-detection) project for how to use inside the real project.