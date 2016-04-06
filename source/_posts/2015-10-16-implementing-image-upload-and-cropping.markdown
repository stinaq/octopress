---
layout: post
title: "Implementing image upload and cropping with IE9 and CORS support"
date: 2015-10-16 16:01
comments: true
categories: 
- File Upload
- JavaScript
- Cropping
---

This is how I implemented the user story that a user wants to change his or her profile picture by uploading a new one and choosing which part of it to show by cropping it.

I needed IE9 support, which made it all a lot harder. The end result is not very complex, code wise, but it took a long time, for me at least, to come up with the best way to go about this, and fit all the pieces together. I'm using two jQuery plugins, [Jcrop](http://deepliquid.com/content/Jcrop.html) for cropping and [jQuery File Upload](https://github.com/blueimp/jQuery-File-Upload/wiki) for uploading. 

*Note* that there are some requirements on the server side. The actual cropping is done at the backend, not on the client, and of course you need a server that can store your images. You also need to have CORS set up on your server (?).

Here's the workflow that I came up with:

1. The user loads the page and all the elements are loaded.
2. The user clicks 'browse computer' (what is this? clarify it is input field) to find a suitable image to upload.
3. When user has selected an image, it is sent with a POST request to the server, which stores it in a temporary location.
4. The new temporary image is loaded back onto the page and the cropping plugin is activated (image source is set to the temporary location).
5. The user chooses which part of the image he or she wants by dragging the cropping rectangle.
6. Once satisfied, the user saves the image, and the coordinates are sent to the server with a POST request. The image is not sent again, only the coordinates are needed.
7. The server takes the temporary image and crops it with the coordinates and moves the cropped version to the final location.
8. The finished image can be loaded back onto the page from the final location (for example user/image).

Why do we need to make such a fuss?
----------------------------------
This workflow may seem longer and more complex than should be required. The main problem here is IE and especially IE9 (no surprise there). The server I was using was located on a different domain from the site, so I also had some CORS issues to contend with. The largest problem I had was when I was trying to crop the image before sending it, using Canvas (what is this?). This will not work. You need to set the image source for a canvas and IE9 or IE10 does not support, at all, the source of the canvas image to be different than the site (this includes files chosen using a file input, as their origin is null). So if you try to load the image from the computer, it will not work. And it will also not work with my separate domains setup. This can't be fixed with CORS in IE9-10 (citation required). So let go of the canvas idea.

This means uploading the image first and then cropping it. This is where you might want to try and make your own FormData object and set image, with base64. This, also, is not supported by IE9. There is no way to create your own xhr request and POST an image with it. What is needed is a form tag that does the actual posting. 

I was working here with a SharePoint site, and those are wrapped in one gigant form tag, and you can't have nestled form tags. So I needed an iFrame with the form tag in it. Here's where jQuery File Upload is your savior. Though the documentation is somewhat lacking, once you get it to work it actually works, and you don't have to implement the actual file upload youself, which is a really good thing.

On to the actual implementation.

Implementing the file upload
----------------------------
First step is to include all the necessary files for the plugins to your page, of course. From Jcrop I just included the `jquery.jcrop.min.js`. In jQuery File Uplpoad, which is a bit more complex, I ended up needing the files `jquery.fileupload.js`, `jquery.fileupload-image.js`, `jquery.fileupload-process.js`, `jquery.fileupload-validate.js` and `jquery.iframe-transport.js`, but this could differ for your needs. Read the documentation carefully and try not to rip out your hair in the process (it is quite a challenging read, the information is all over the place). I also needed to deploy a file called `result.html` found in the project. This is to make it work in IE9, more on that below.

I added this markup:

    <input id="fileupload" type="file" name="files[]">

And to my JavaScript file I added this:

```js
$('#fileupload').fileupload({
    forceIframeTransport: true,
    url: == url to your server ==,
    // The regular expression for allowed file types, matches
    // against either file type or file name:
    acceptFileTypes: /(\.|\/)(jpe?g|png)$/i,
    success: function (e, data) {
        $uploadPanel.hide();
        showCroppingPanel();
    },
    fail: function (e, data) {
        console.log(e);
        console.log(data);
    },
    done: function (e, data) {
        console.log(data);
    }
    }).on('fileuploadprocessalways', function (e, data) {
        var currentFile = data.files[data.index];
        if (data.files.error && currentFile.error) {
          // there was an error, do something about it
            console.log(currentFile.error);
            $wrongFileMessage.show();
        }
    });

    // This reditect the POST to a html file
    // needed for the iframe, and perhaps for the CORS support
    $('#fileupload').fileupload('option',
        'redirect',
        '/result.html?%s'
    );
```

Let's go through it step by step.

`.fileupload` initializes and after that you give it options. The `forceIframeTransport` forces it to always create an iframe to make the POST request to. This is needed for IE9, which does not support the sending of images via xhr.

`url` should be self explanatory, it's the location of your server, to where the POST request will be made.

`acceptFileTypes` takes a regular expression to match the file ending. And this is where I found a bit of a gotcha. You can only catch this "error" (when a user uploads the wrong file type) if you also implement the `fileuploadprocessalways` bit of the code above. This "pauses" the upload if there is an error, and you can for example show an error message to the user, which is what I did. 

The last part is also for the iFrame. It redirects the POST to an iFrame and the HTML file is needed there. Don't forget the `%s` at the end there, it won't work without it.

Once you get all this to work, the image should magically be sent to the server. One big problem for me at first was the success handler not running, even though the image was successfully uploaded and the server returned 200. This was because I, according to the startup guide, had specified that the result should be in JSON, but the server was not sending JSON.

My main problem now is that the error handler is not run even though the server returns an error. Don't yet know why this is.

Implementing the cropping
-------------------------
At this point you hopefully have a temporary image on the server. Create an `<image>` tag on your page, and in the success handler of the previous part, add the newly uploaded image url as source. I did it like this:

```js
$croppingImage.attr('src', "http://myserver/user/picture-temp");
```

And then wait for it to load, and activate the Jcrop plugin on it:

```js
$croppingImage.load(function () {
    $croppingImage.Jcrop({
       setSelect: [96, 96, 0, 0],
       minSize: [minSize, minSize],
       aspectRatio: 1,
       onSelect: setCoordinates
    }, function(){
       jcrop_api = this;
    });

});
```

As usual there are a lot of options that you can implement, I just wanted the selection to be a square. 

Here I give the callback setCoordinates for each time a user updates the selection.

Those coordinates are then used when the user clicks a button. 

```js
$cropAndSaveButton.on('click', function (x1, x2, y1, y2) {
        // This happens when a user clicks to crop and save

        var requestUrl = "user/picture/crop-and-enable/" + x1 + "/" + y1 + "/" + x2 + "/" + y2;

        $.ajax({
            url: requestUrl,
            method: POST
        }).done(reloadPage);
    });
```

Depending on your serverside implementation here, you could send the coordinates in different ways, this is the way this backend had implemented it.

This was just an overview of how the flow and process of uploading and cropping an image could work, to help give an idea of how to think. I hope it will help someone out there.