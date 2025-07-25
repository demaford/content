---
title: Recording a media element
slug: Web/API/MediaStream_Recording_API/Recording_a_media_element
page-type: guide
---

{{DefaultAPISidebar("MediaStream Recording")}}

While the article Using the MediaStream Recording API demonstrates using the {{domxref("MediaRecorder")}} interface to capture a {{domxref("MediaStream")}} generated by a hardware device, as returned by {{domxref("MediaDevices.getUserMedia()","navigator.mediaDevices.getUserMedia()")}}, you can also use an HTML media element (namely {{HTMLElement("audio")}} or {{HTMLElement("video")}}) as the source of the `MediaStream` to be recorded. In this article, we'll look at an example that does just that.

## Example of recording a media element

### HTML

```html hidden
<p>
  Click the "Start Recording" button to begin video recording for a few seconds.
  You can stop recording by clicking the "Stop Recording" button. The "Download"
  button will download the received data (although it's in a raw, unwrapped form
  that isn't very useful).
</p>
<br />
```

Let's start by looking at the key bits of the HTML. There's a little more than this, but it's just informational rather than being part of the core operation of the app.

```html
<div class="left">
  <div id="startButton" class="button">Start Recording</div>
  <h2>Preview</h2>
  <video id="preview" width="160" height="120" autoplay muted></video>
</div>
```

We present our main interface in two columns. On the left is a start button and a {{HTMLElement("video")}} element which displays the video preview; this is the video the user's camera sees. Note that the [`autoplay`](/en-US/docs/Web/HTML/Reference/Elements/video#autoplay) attribute is used so that as soon as the stream starts to arrive from the camera, it immediately gets displayed, and the [`muted`](/en-US/docs/Web/HTML/Reference/Elements/video#muted) attribute is specified to ensure that the sound from the user's microphone isn't output to their speakers, causing an ugly feedback loop.

```html
<div class="right">
  <div id="stopButton" class="button">Stop Recording</div>
  <h2>Recording</h2>
  <video id="recording" width="160" height="120" controls></video>
  <a id="downloadButton" class="button">Download</a>
</div>
```

On the right we see a stop button and the `<video>` element which will be used to play back the recorded video. Notice that the playback panel doesn't have autoplay set (so the playback doesn't start as soon as media arrives), and it has [`controls`](/en-US/docs/Web/HTML/Reference/Elements/video#controls) set, which tells it to show the user controls to play, pause, and so forth.

Below the playback element is a button for downloading the recorded video.

```html hidden
<div class="bottom">
  <pre id="log"></pre>
</div>
```

```css hidden
body {
  font:
    14px "Open Sans",
    "Arial",
    sans-serif;
}

video {
  margin-top: 2px;
  border: 1px solid black;
}

.button {
  cursor: pointer;
  display: block;
  width: 160px;
  border: 1px solid black;
  font-size: 16px;
  text-align: center;
  padding-top: 2px;
  padding-bottom: 4px;
  color: white;
  background-color: darkgreen;
  text-decoration: none;
}

h2 {
  margin-bottom: 4px;
}

.left {
  margin-right: 10px;
  float: left;
  width: 160px;
  padding: 0px;
}

.right {
  margin-left: 10px;
  float: left;
  width: 160px;
  padding: 0px;
}

.bottom {
  clear: both;
  padding-top: 10px;
}
```

Now let's have a look at the JavaScript code; this is where the majority of the action happens, after all!

### Setting up global variables

We start by establishing some global variables we'll need.

```js
let preview = document.getElementById("preview");
let recording = document.getElementById("recording");
let startButton = document.getElementById("startButton");
let stopButton = document.getElementById("stopButton");
let downloadButton = document.getElementById("downloadButton");
let logElement = document.getElementById("log");

let recordingTimeMS = 5000;
```

Most of these are references to elements we need to work with. The last, `recordingTimeMS`, is set to 5000 milliseconds (5 seconds); this specifies the length of the videos we'll record.

### Utility functions

Next, we create some utility functions that will get used later.

```js
function log(msg) {
  logElement.innerText += `${msg}\n`;
}
```

The `log()` function is used to output text strings to a {{HTMLElement("div")}} so we can share information with the user. Not very pretty but it gets the job done for our purposes.

```js
function wait(delayInMS) {
  return new Promise((resolve) => setTimeout(resolve, delayInMS));
}
```

The `wait()` function returns a new {{jsxref("Promise")}} which resolves once the specified number of milliseconds have elapsed. It works by using an [arrow function](/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) which calls {{domxref("Window.setTimeout", "setTimeout()")}}, specifying the promise's resolution handler as the timeout handler function. That lets us use promise syntax when using timeouts, which can be very handy when chaining promises, as we'll see later.

### Starting media recording

The `startRecording()` function handles starting the recording process:

```js
function startRecording(stream, lengthInMS) {
  let recorder = new MediaRecorder(stream);
  let data = [];

  recorder.ondataavailable = (event) => data.push(event.data);
  recorder.start();
  log(`${recorder.state} for ${lengthInMS / 1000} seconds…`);

  let stopped = new Promise((resolve, reject) => {
    recorder.onstop = resolve;
    recorder.onerror = (event) => reject(event.name);
  });

  let recorded = wait(lengthInMS).then(() => {
    if (recorder.state === "recording") {
      recorder.stop();
    }
  });

  return Promise.all([stopped, recorded]).then(() => data);
}
```

`startRecording()` takes two input parameters: a {{domxref("MediaStream")}} to record from and the length in milliseconds of the recording to make. We always record no more than the specified number of milliseconds of media, although if the media stops before that time is reached, {{domxref("MediaRecorder")}} automatically stops recording as well.

- We first create the `MediaRecorder` that will handle recording the input `stream`.
- `data` is an array, initially empty, that holds the {{domxref("Blob")}}s of media data provided to our {{domxref("MediaRecorder.dataavailable_event", "ondataavailable")}} event handler.
- The `ondataavailable` assignment sets up the handler for the {{domxref("MediaRecorder.dataavailable_event", "dataavailable")}} event. The received event's `data` property is a {{domxref("Blob")}} that contains the media data. The event handler pushes the `Blob` onto the `data` array.
- We start the recording process by calling {{domxref("MediaRecorder.start", "recorder.start()")}}, and output a message to the log with the updated state of the recorder and the number of seconds it will be recording.
- We create a new {{jsxref("Promise")}}, named `stopped`, which is resolved when the `MediaRecorder`'s {{domxref("MediaRecorder.stop_event", "onstop")}} event handler is called, and is rejected if its {{domxref("MediaRecorder.error_event", "onerror")}} event handler is called. The rejection handler receives as input the name of the error that occurred.
- We creates another new `Promise`, named `recorded`, which is resolved when the specified number of milliseconds have elapsed. Upon resolution, it stops the `MediaRecorder` if it's recording.
- Finally, we use {{jsxref("Promise.all")}} to create a new `Promise` which is fulfilled when both of the two `Promise`s (`stopped` and `recorded`) have resolved. Once that resolves, the array data is returned by `startRecording()` to its caller.

### Stopping the input stream

The `stop()` function stops the input media:

```js
function stop(stream) {
  stream.getTracks().forEach((track) => track.stop());
}
```

This works by calling {{domxref("MediaStream.getTracks()")}}, using {{jsxref("Array.forEach", "forEach()")}} to call {{domxref("MediaStreamTrack.stop()")}} on each track in the stream.

### Getting an input stream and setting up the recorder

Now let's look at the most intricate piece of code in this example: our event handler for clicks on the start button:

```js
startButton.addEventListener(
  "click",
  () => {
    navigator.mediaDevices
      .getUserMedia({
        video: true,
        audio: true,
      })
      .then((stream) => {
        preview.srcObject = stream;
        downloadButton.href = stream;
        preview.captureStream =
          preview.captureStream || preview.mozCaptureStream;
        return new Promise((resolve) => {
          preview.onplaying = resolve;
        });
      })
      .then(() => startRecording(preview.captureStream(), recordingTimeMS))
      .then((recordedChunks) => {
        let recordedBlob = new Blob(recordedChunks, { type: "video/webm" });
        recording.src = URL.createObjectURL(recordedBlob);
        downloadButton.href = recording.src;
        downloadButton.download = "RecordedVideo.webm";

        log(
          `Successfully recorded ${recordedBlob.size} bytes of ${recordedBlob.type} media.`,
        );
      })
      .catch((error) => {
        if (error.name === "NotFoundError") {
          log("Camera or microphone not found. Can't record.");
        } else {
          log(error);
        }
      });
  },
  false,
);
```

When a {{domxref("Element/click_event", "click")}} event occurs, here's what happens:

- {{domxref("MediaDevices.getUserMedia")}} is called to request a new {{domxref("MediaStream")}} that has both video and audio tracks. This is the stream we'll record.
- When the Promise returned by `getUserMedia()` is resolved, the preview {{HTMLElement("video")}} element's {{domxref("HTMLMediaElement.srcObject","srcObject")}} property is set to be the input stream, which causes the video being captured by the user's camera to be displayed in the preview box. Since the `<video>` element is muted, the audio won't play. The "Download" button's link is then set to refer to the stream as well. Then, we arrange for `preview.captureStream()` to call `preview.mozCaptureStream()` so that our code will work on Firefox, on which the {{domxref("HTMLMediaElement.captureStream()")}} method is prefixed. Then a new {{jsxref("Promise")}} which resolves when the preview video starts to play is created and returned.
- When the preview video begins to play, we know there's media to record, so we respond by calling the [`startRecording()`](#starting_media_recording) function we created earlier, passing in the preview video stream (as the source media to be recorded) and `recordingTimeMS` as the number of milliseconds of media to record. As mentioned before, `startRecording()` returns a {{jsxref("Promise")}} whose resolution handler is called (receiving as input an array of {{domxref("Blob")}} objects containing the chunks of recorded media data) once recording has completed.
- The recording process's resolution handler receives as input an array of media data `Blob`s locally known as `recordedChunks`. The first thing we do is merge the chunks into a single {{domxref("Blob")}} whose MIME type is `"video/webm"` by taking advantage of the fact that the {{domxref("Blob.Blob", "Blob()")}} constructor concatenates arrays of objects into one object. Then {{domxref("URL.createObjectURL_static", "URL.createObjectURL()")}} is used to create a URL that references the blob; this is then made the value of the recorded video playback element's [`src`](/en-US/docs/Web/HTML/Reference/Elements/video#src) attribute (so that you can play the video from the blob) as well as the target of the download button's link.

  Then the download button's [`download`](/en-US/docs/Web/HTML/Reference/Elements/a#download) attribute is set. While the `download` attribute can be a Boolean, you can also set it to a string to use as the name for the downloaded file. So by setting the download link's `download` attribute to "RecordedVideo.webm", we tell the browser that clicking the button should download a file named `"RecordedVideo.webm"` whose contents are the recorded video.

- The size and type of the recorded media are output to the log area below the two videos and the download button.
- The `catch()` for all the `Promise`s outputs the error to the logging area by calling our `log()` function.

### Handling the stop button

The last bit of code adds a handler for the {{domxref("Element/click_event", "click")}} event on the stop button using {{domxref("EventTarget.addEventListener", "addEventListener()")}}:

```js
stopButton.addEventListener(
  "click",
  () => {
    stop(preview.srcObject);
  },
  false,
);
```

This calls the [`stop()`](#stopping_the_input_stream) function we covered earlier.

### Result

When put all together with the rest of the HTML and the CSS not shown above, it looks and works like this:

{{EmbedLiveSample('Example_of_recording_a_media_element', '600', '440', , , , 'camera;microphone')}}

You can also open this example in the playground using the "Play" button, which allows you to look at the combined code, including the parts hidden above because they aren't critical to the explanation of how the APIs are being used.

## See also

- [Media Capture and Streams API](/en-US/docs/Web/API/Media_Capture_and_Streams_API) landing page
- {{domxref("MediaDevices.getUserMedia()")}}
