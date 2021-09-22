---
layout: post
title: React-File Uploading with FilePond
date:   2018-12-03 19:07:19
modified:  2018-12-03
comments: true
---

[FilePond](http://pqina.nl/filepond/) is a relatively new JavaScript file upload libary.

#### Install FilePond

```javascript
npm install filepond react-filepond --save
```

#### Import Project

```javascript
import { FilePond } from 'react-filepond';
import 'filepond/dist/filepond.min.css';
```

Next step is to add the component to the HTML.

```javascript
<FilePond
    onupdatefiles={(fileItems) => {
        this.setState({files: fileItems.map(fileItem => fileItem.file)});}}
    onDrop={this.handleUploadImages}/>
```

onupdatefiles will change our state.

```javascript
constructor(props){
    super(props)
    this.state = {
        files: []
    }
}
```

By default FilePond only accepts one file. But if we want, we can increase this properties. We can see filepond component. If you want, you can use preview option. Install package and import it inside component.

```javascript
npm install filepond-plugin-image-preview --save
```

Now, you can use this package.

![Redux Firebase]({{ site.url }}/assets/img/filepond.gif)

```javascript
import { FilePond, registerPlugin } from 'react-filepond';
import FilePondPluginImagePreview from 'filepond-plugin-image-preview';
import 'filepond-plugin-image-preview/dist/filepond-plugin-image-preview.min.css';
registerPlugin(FilePondPluginImagePreview);
```

Resources:

[Filepond Int.](http://itnext.io/uploading-files-with-react-and-filepond-f8a798308557)

If you wonder, how to use filepond inside to react project. You can check [this](http://github.com/ebrugulec/image-text-detection) link. And if you want to upload these file cloudinar. You can check [this](http://medium.com/@ebrugulec/react-upload-file-with-cloudinary-5030766ae099) post.
