---
title: Audio and video delivery
slug: Web/Media/Guides/Audio_and_video_delivery
page-type: guide
sidebar: mediasidebar
---

We can deliver audio and video on the web in a number of ways, ranging from 'static' media files to adaptive live streams. This article is intended as a starting point for exploring the various delivery mechanisms of web-based media and compatibility with popular browsers.

## Audio and video HTML elements

Whether we are dealing with pre-recorded audio files or live streams, the mechanism for making them available through the browser's {{ htmlelement("audio")}} and {{ htmlelement("video")}} elements remains pretty much the same. Currently, to support all browsers we need to specify two formats, although with the adoption of MP3 and MP4 formats in Firefox and Opera, this is changing fast. You can find compatibility information in the [Guide to media types and formats on the web](/en-US/docs/Web/Media/Guides/Formats).

To deliver video and audio, the general workflow is usually something like this:

1. Check what format the browser supports via feature detection (usually a choice of two, as stated above).
2. If the browser doesn't support playback of any of the provided formats natively, either present a still image or use a fallback technology to present the video.
3. Identify how you want to play/instantiate the media (e.g., a {{ htmlelement("video") }} element, or `document.createElement('video')` perhaps?)
4. Deliver the media file to the player.

### HTML Audio

```html
<audio controls preload="auto">
  <source src="audio-file.mp3" type="audio/mpeg" />

  <!-- fallback for browsers that don't support mp3 -->
  <source src="audio-file.ogg" type="audio/ogg" />

  <!-- fallback for browsers that don't support audio element -->
  <a href="audio-file.mp3">download audio</a>
</audio>
```

The code above will create an audio player that attempts to preload as much audio as possible for smooth playback.

> [!NOTE]
> The `preload` attribute may be ignored by some mobile browsers.

For further info see [Cross Browser Audio Basics (HTML Audio In Detail)](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Cross-browser_audio_basics#html_audio_in_detail)

### HTML video

```html
<video
  controls
  width="640"
  height="480"
  poster="initial-image.png"
  autoplay
  muted>
  <source src="video-file.mp4" type="video/mp4" />

  <!-- fallback for browsers that don't support mp4 -->
  <source src="video-file.webm" type="video/webm" />

  <!-- specifying subtitle files -->
  <track src="subtitles_en.vtt" kind="subtitles" srclang="en" label="English" />
  <track
    src="subtitles_no.vtt"
    kind="subtitles"
    srclang="no"
    label="Norwegian" />

  <!-- fallback for browsers that don't support video element -->
  <a href="video-file.mp4">download video</a>
</video>
```

The code above creates a video player of dimensions 640x480 pixels, displaying a poster image until the video is played. We instruct the video to autoplay but to be muted by default.

> [!NOTE]
> The `autoplay` attribute may be ignored by some mobile browsers. Also, the autoplay feature can be controversial when misused. It's strongly recommended that you read the [Autoplay guide for media and Web Audio APIs](/en-US/docs/Web/Media/Guides/Autoplay) to learn how to use autoplay wisely.

For further info see [\<video> element](/en-US/docs/Web/HTML/Reference/Elements/video) and [Creating a cross-browser video player](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/cross_browser_video_player).

### JavaScript Audio

```js
const myAudio = document.createElement("audio");

if (myAudio.canPlayType("audio/mpeg")) {
  myAudio.setAttribute("src", "audio-file.mp3");
} else if (myAudio.canPlayType("audio/ogg")) {
  myAudio.setAttribute("src", "audio-file.ogg");
}

myAudio.currentTime = 5;
myAudio.play();
```

We set the source of the audio depending on the type of audio file the browser supports, then set the play-head 5 seconds in and attempt to play it.

> [!NOTE]
> Play will be ignored by most browsers unless issued by a user-initiated event.

It's also possible to feed an {{ htmlelement("audio") }} element a base64 encoded WAV file, allowing to generate audio on the fly:

```html
<audio id="player" src="data:audio/x-wav;base64,UklGRvC…"></audio>
```

[Speak.js](https://github.com/kripken/speak.js/) employs this technique.

### JavaScript Video

```js
const myVideo = document.createElement("video");

if (myVideo.canPlayType("video/mp4")) {
  myVideo.setAttribute("src", "video-file.mp4");
} else if (myVideo.canPlayType("video/webm")) {
  myVideo.setAttribute("src", "video-file.webm");
}

myVideo.width = 480;
myVideo.height = 320;
```

We set the source of the video depending on the type of video file the browser supports we then set the width and height of the video.

## Web Audio API

In this example we retrieve an MP3 file using the {{domxref("Window/fetch", "fetch()")}} API, load it into a source, and play it.

```js
let audioCtx;
let buffer;
let source;

async function loadAudio() {
  try {
    // Load an audio file
    const response = await fetch("viper.mp3");
    // Decode it
    buffer = await audioCtx.decodeAudioData(await response.arrayBuffer());
  } catch (err) {
    console.error(`Unable to fetch the audio file. Error: ${err.message}`);
  }
}

const play = document.getElementById("play");
play.addEventListener("click", async () => {
  if (!audioCtx) {
    audioCtx = new AudioContext();
    await loadAudio();
  }
  source = audioCtx.createBufferSource();
  source.buffer = buffer;
  source.connect(audioCtx.destination);
  source.start();
  play.disabled = true;
});
```

You can [run the full example live](https://mdn.github.io/webaudio-examples/decode-audio-data/promise/), or [view the source](https://github.com/mdn/webaudio-examples/tree/main/decode-audio-data/promise).

Find more about Web Audio API basics in [Using the Web Audio API](/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API).

## getUserMedia / Stream API

It's also possible to retrieve a live stream from a webcam and/or microphone using `getUserMedia` and the Stream API. This makes up part of a wider technology known as WebRTC (Web Real-Time Communications) and is compatible with the latest versions of Chrome, Firefox and Opera.

To grab the stream from your webcam, first set up a {{htmlelement("video")}} element:

```html
<video id="webcam" width="480" height="360"></video>
```

Next, if supported connect the webcam source to the video element:

```js
if (navigator.mediaDevices) {
  navigator.mediaDevices
    .getUserMedia({ video: true, audio: false })
    .then((stream) => {
      const video = document.getElementById("webcam");
      video.autoplay = true;
      video.srcObject = stream;
    })
    .catch(() => {
      alert(
        "There has been a problem retrieving the streams - are you running on file:/// or did you disallow access?",
      );
    });
} else {
  alert("getUserMedia is not supported in this browser.");
}
```

To find out more, read our {{domxref("MediaDevices.getUserMedia")}} page.

## MediaStream Recording

New standards are being rolled out to allow your browser to grab media from your mic or camera using `getUserMedia` and record it instantly using the new MediaStream Recording API. You take the stream you receive from `getUserMedia`, pass it to a `MediaRecorder` object, take the resulting output and feed it to your audio or video source\*.

The main mechanism is outlined below:

```js
navigator.mediaDevices
  .getUserMedia({ audio: true })
  .then((stream) => {
    const recorder = new MediaRecorder(stream);

    const data = [];
    recorder.ondataavailable = (e) => {
      data.push(e.data);
    };
    recorder.start();
    recorder.onerror = (e) => {
      throw e.error || new Error(e.name); // e.name is FF non-spec
    };
    recorder.onstop = (e) => {
      const audio = document.createElement("audio");
      audio.src = window.URL.createObjectURL(new Blob(data));
    };
    setTimeout(() => {
      rec.stop();
    }, 5000);
  })
  .catch((error) => {
    console.log(error.message);
  });
```

See [MediaStream Recording API](/en-US/docs/Web/API/MediaStream_Recording_API) for more details.

## Media Source Extensions (MSE)

[Media Source Extensions](https://w3c.github.io/media-source/) is a W3C working draft that plans to extend {{domxref("HTMLMediaElement")}} to allow JavaScript to generate media streams for playback. Allowing JavaScript to generate streams facilitates a variety of use cases like adaptive streaming and time shifting live streams.

### Encrypted Media Extensions (EME)

[Encrypted Media Extensions](https://w3c.github.io/encrypted-media/) is a W3C proposal to extend `HTMLMediaElement`, providing APIs to control playback of protected content.

The API supports use cases ranging from basic clear key decryption to high value video (given an appropriate user agent implementation). License/key exchange is controlled by the application, facilitating the development of robust playback applications supporting a range of content decryption and protection technologies.

One of the principal uses of EME is to allow browsers to implement DRM ([Digital Rights Management](https://en.wikipedia.org/wiki/Digital_rights_management)), which helps to prevent web-based content (especially video) from being copied.

### Adaptive Streaming

New formats and protocols are being rolled out to facilitate adaptive streaming. Adaptive streaming media means that the bandwidth and typically quality of the stream can change in real-time in reaction to the user's available bandwidth. Adaptive streaming is often used in conjunction with live streaming where smooth delivery of audio or video is paramount.

The main formats used for adaptive streaming are [HLS](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Live_streaming_web_audio_and_video#hls) and [MPEG-DASH](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Live_streaming_web_audio_and_video#mpeg-dash). MSE has been designed with DASH in mind. MSE defines byte streams according to [ISOBMFF](https://dvcs.w3.org/hg/html-media/raw-file/tip/media-source/isobmff-byte-stream-format.html) and [M2TS](https://en.wikipedia.org/wiki/M2ts) (both supported in DASH, the latter supported in HLS). Generally speaking, if you are interested in standards, are looking for flexibility, or wish to support most modern browsers, you are probably better off with DASH.

> [!NOTE]
> Currently Safari does not support DASH although dash.js will work on newer versions of Safari scheduled for release with OSX Yosemite.

DASH also provides a number of profiles including onDemand profiles that require no preprocessing and splitting up of media files. There are also a number of cloud based services that will convert your media to both HLS and DASH.

For further information see [Live streaming web audio and video](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Live_streaming_web_audio_and_video).

## Customizing Your Media Player

You may decide that you want your audio or video player to have a consistent look across browsers, or just wish to tweak it to match your site. The general technique for achieving this is to omit the `controls` attribute so that the default browser controls are not displayed, create custom controls using HTML and CSS, then use JavaScript to link your controls to the audio/video API.

If you need something extra, it's possible to add features that are not currently present in default players, such as playback rate, quality stream switches or even audio spectrums. You can also choose how to make your player responsive — for example you might remove the progress bar under certain conditions.

You may detect click, touch and/or keyboard events to trigger actions such as play, pause and scrubbing. It's often important to remember keyboard controls for user convenience and accessibility.

A quick example — first set up your audio and custom controls in HTML:

```html
<audio id="my-audio" src="/shared-assets/audio/guitar.mp3"></audio>
<button id="my-control">play</button>
```

add a bit of JavaScript to detect events to play and pause the audio:

```js
const myAudio = document.getElementById("my-audio");
const myControl = document.getElementById("my-control");

function switchState() {
  if (myAudio.paused) {
    myAudio.play();
    myControl.textContent = "pause";
  } else {
    myAudio.pause();
    myControl.textContent = "play";
  }
}

function checkKey(e) {
  if (e.code === "Space") {
    // space bar
    switchState();
  }
}

myControl.addEventListener(
  "click",
  () => {
    switchState();
  },
  false,
);

window.addEventListener("keypress", checkKey, false);
```

{{EmbedLiveSample("customizing your media player", "", 200)}}

For more information, see [Creating your own custom audio player](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Cross-browser_audio_basics#creating_your_own_custom_audio_player).

## Other tips for audio/video

### Stopping the download of media

While stopping the playback of media is as easy as calling the element's `pause()` method, the browser keeps downloading the media until the media element is disposed of through garbage collection.

Here's a trick that stops the download at once:

```js
const mediaElement = document.querySelector("#myMediaElementID");
mediaElement.removeAttribute("src");
mediaElement.load();
```

By removing the media element's `src` attribute and invoking the load() method, you release the resources associated with the video, which stops the network download. You must call `load()` after removing the attribute, because just removing the `src` attribute does not invoke the load algorithm. If the `<video>` element also has `<source>` element descendants, those should also be removed before calling `load()`.

Note that just setting the `src` attribute to an empty string will actually cause the browser to treat it as though you're setting a video source to a relative path. This causes the browser to attempt another download to something that is unlikely to be a valid video.

### Seeking through media

Media elements provide support for moving the current playback position to specific points in the media's content. This is done by setting the value of the `currentTime` property on the element; see {{ domxref("HTMLMediaElement") }} for further details on the element's properties. Set the value to the time, in seconds, at which you want playback to continue.

You can use the element's `seekable` property to determine the ranges of the media that are currently available for seeking to. This returns a {{ domxref("TimeRanges") }} object listing the ranges of times that you can seek to.

```js
const mediaElement = document.querySelector("#mediaElementID");
mediaElement.seekable.start(0); // Returns the starting time (in seconds)
mediaElement.seekable.end(0); // Returns the ending time (in seconds)
mediaElement.currentTime = 122; // Seek to 122 seconds
mediaElement.played.end(0); // Returns the number of seconds the browser has played
```

### Specifying playback range

When specifying the URI of media for an {{ HTMLElement("audio") }} or {{ HTMLElement("video") }} element, you can optionally include additional information to specify the portion of the media to play. To do this, append a hash mark ("#") followed by the media fragment description.

A time range is specified using the syntax:

```plain
#t=[starttime][,endtime]
```

The time can be specified as a number of seconds (as a floating-point value) or as an hours/minutes/seconds time separated with colons (such as 2:05:01 for 2 hours, 5 minutes, and 1 second).

A few examples:

- `http://example.com/video.ogv#t=10,20`
  - : Specifies that the video should play the range 10 seconds through 20 seconds.
- `http://example.com/video.ogv#t=,10.5`
  - : Specifies that the video should play from the beginning through 10.5 seconds.
- `http://example.com/video.ogv#t=,02:00:00`
  - : Specifies that the video should play from the beginning through two hours.
- `http://example.com/video.ogv#t=60`
  - : Specifies that the video should start playing at 60 seconds and play through the end of the video.

## Error handling

Errors get delivered to the child {{ HTMLElement("source") }} elements corresponding to the sources resulting in the error.

This lets you detect which sources failed to load, which may be useful. Consider this HTML:

```html
<video>
  <source
    id="mp4_src"
    src="video.mp4"
    type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"' />
  <source
    id="3gp_src"
    src="video.3gp"
    type='video/3gpp; codecs="mp4v.20.8, samr"' />
  <source
    id="ogg_src"
    src="video.ogv"
    type='video/ogv; codecs="theora, vorbis"' />
</video>
```

Since Firefox doesn't support MP4 and 3GP on some platforms due to their patent-encumbered nature, the {{ HTMLElement("source") }} elements with the IDs "mp4_src" and "3gp_src" will receive `error` events before the Ogg resource is loaded. The sources are tried in the order in which they appear, and once one loads successfully, the remaining sources aren't tried at all.

### Checking whether the browser supports the supplied formats

Support for media formats are available on [Can I Use](https://caniuse.com/).

- [Audio MP3 (`type="audio/mpeg"`)](https://caniuse.com/mp3)
- [Audio Ogg (`type="audio/ogg"`)](https://caniuse.com/ogg-vorbis)
- [Video MP4 (`type="video/mp4"`)](https://caniuse.com/mpeg4)
- [Video WebM (`type="video/webm"`)](https://caniuse.com/webm)
- [Video Ogg (`type="video/ogg"`)](https://caniuse.com/ogv)

You can also search for [other media formats](/en-US/docs/Web/Media/Guides/Formats/Containers).

If a media format is supposed to be supported but the files you are supplying don't play, there are two possible issues:

#### 1. The media server is not delivering the correct mime types with the file

Although this is usually supported, you may need to add the following to your media server's `.htaccess` file.

```plain
# AddType TYPE/SUBTYPE EXTENSION

AddType audio/mpeg mp3
AddType audio/mp4 m4a
AddType audio/ogg ogg
AddType audio/ogg oga

AddType video/mp4 mp4
AddType video/mp4 m4v
AddType video/ogg ogv
AddType video/webm webm
AddType video/webm webmv
```

#### 2. Your files have been encoded incorrectly

Your files may have been encoded incorrectly — try encoding using one of the following tools, which are proven to be pretty reliable:

- [Audacity](https://sourceforge.net/projects/audacity/) — Free Audio Editor and Recorder
- [Miro](https://www.getmiro.com/) — Free, open-source music and video player
- [Handbrake](https://handbrake.fr/) — Open Source Video Transcoder
- [Firefogg](https://www.firefogg.org/) — Video and Audio encoding for Firefox
- [FFmpeg2](https://www.ffmpeg.org/) — Comprehensive command line encoder
- [Vid.ly](https://m.vid.ly/) — Video player, transcoding and delivery
- [Internet Archive](https://archive.org/) — Free transcoding and storage

### Detecting when no sources have loaded

To detect that all child {{ HTMLElement("source") }} elements have failed to load, check the value of the media element's `networkState` attribute. If this is `HTMLMediaElement.NETWORK_NO_SOURCE`, you know that all the sources failed to load.

If at that point you add another source, by inserting a new {{ HTMLElement("source") }} element as a child of the media element, Gecko attempts to load the specified resource.

### Showing fallback content when no source could be decoded

Another way to show the fallback content of a video, when none of the sources could be decoded in the current browser, is to add an error handler on the last source element. Then you can replace the video with its fallback content:

```html
<video controls>
  <source src="dynamicsearch.mp4" type="video/mp4" />
  <a href="dynamicsearch.mp4">
    <img src="dynamicsearch.jpg" alt="Dynamic app search in Firefox OS" />
  </a>
  <p>Click image to play a video demo of dynamic app search</p>
</video>
```

```js
const v = document.querySelector("video");
const sources = v.querySelectorAll("source");
const lastSource = sources[sources.length - 1];
lastSource.addEventListener(
  "error",
  (ev) => {
    const d = document.createElement("div");
    d.innerHTML = v.innerHTML;
    v.parentNode.replaceChild(d, v);
  },
  false,
);
```

## Audio/Video JavaScript libraries

A number of audio and video JavaScript libraries exist. The most popular libraries allow you to choose a consistent player design over all browsers and provide a fallback for browsers that don't support audio and video natively. Fallbacks have historically used now-obsolete plugins such as Adobe Flash or Microsoft Silverlight plugins to provide a media player in non-supporting browsers, although these are no longer supported on modern computers. Other functionality such as the [`<track>`](/en-US/docs/Web/HTML/Reference/Elements/track) element for subtitles can also be provided through media libraries.

### Audio only

- [SoundManager](https://www.schillmania.com/projects/soundmanager2/)
- [AmplitudeJS](https://521dimensions.com/open-source/amplitudejs)
- [HowlerJS](https://howlerjs.com/)

### Video only

- [flowplayer](https://flowplayer.com/): Gratis with a flowplayer logo watermark. Open source (GPL licensed.)
- [JWPlayer](https://jwpconnatix.com/): Requires registration to download. Open Source Edition (Creative Commons License.)
- [SublimeVideo](https://www.sublimevideo.net/): Requires registration. Form based set up with domain specific link to CDN hosted library.
- [Video.js](https://videojs.com/): Gratis and Open Source (Apache 2 Licensed.)

### Audio and Video

- [jPlayer](https://jPlayer.org/): Gratis and Open Source (MIT Licensed.)
- [mediaelement.js](https://www.mediaelementjs.com/): Gratis and Open Source (MIT Licensed.)

### Web Audio API

- [AudioContext monkeypatch](https://github.com/cwilso/AudioContext-MonkeyPatch): A polyfill for older versions of the Web Audio API; Open Source (Apache 2 Licensed.)

## Guides

- [Creating a cross-browser video player](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/cross_browser_video_player)
  - : A guide to creating a basic cross browser video player using the {{ htmlelement("video") }} element.
- [Video player styling basics](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Video_player_styling_basics)
  - : With the cross-browser video player put in place in the previous article, this article now looks at providing some basic, responsive styling for the player.
- [Cross-browser audio basics](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Cross-browser_audio_basics)
  - : This article provides a basic guide to creating an HTML audio player that works cross browser, with all the associated attributes, properties and events explained, and a quick guide to custom controls created using the Media API.
- [Media buffering, seeking, and time ranges](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/buffering_seeking_time_ranges)
  - : Sometimes it's useful to know how much {{ htmlelement("audio") }} or {{ htmlelement("video") }} has downloaded or is playable without delay — a good example of this is the buffered progress bar of an audio or video player. This article discusses how to build a buffer/seek bar using [TimeRanges](/en-US/docs/Web/API/TimeRanges), and other features of the media API.
- [HTML playbackRate explained](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/WebAudio_playbackRate_explained)
  - : The `playbackRate` property allows us to change the speed or rate at which a piece of web audio or video is playing. This article explains it in detail.
- [Using the Web Audio API](/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API)
  - : Explains the basics of using the Web Audio API to grab, manipulate and play back an audio source.

### Streaming media

- [Live streaming web audio and video](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Live_streaming_web_audio_and_video)
  - : Live streaming technology is often employed to relay live events such as sports, concerts and more generally TV and Radio programmes that are output live. Often shortened to just streaming, live streaming is the process of transmitting media 'live' to computers and devices. This is a fairly complex and nascent subject with a lot of variables, so in this article we'll introduce you to the subject and let you know how you can get started.
- [Setting up adaptive streaming media sources](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Setting_up_adaptive_streaming_media_sources)
  - : Let's say you want to set up an adaptive streaming media source on a server, to be consumed inside an HTML media element. How would you do that? This article explains how, looking at two of the most common formats: MPEG-DASH and HLS (HTTP Live Streaming.)
- [DASH Adaptive Streaming for HTML 5 Video](/en-US/docs/Web/API/Media_Source_Extensions_API/DASH_Adaptive_Streaming)
  - : Details how to set up adaptive streaming using DASH and WebM.

### Advanced topics

- [Adding captions and subtitles to HTML video](/en-US/docs/Web/Media/Guides/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video)
  - : This article explains how to add captions and subtitles to HTML {{ htmlelement("video") }}, using [Web_Video_Text_Tracks_Format](/en-US/docs/Web/API/WebVTT_API) and the {{ htmlelement("track") }} element.
- [Web Audio API cross browser support](/en-US/docs/Web/API/Web_Audio_API/Best_practices#cross_browser_legacy_support)
  - : A guide to writing cross browser Web Audio API code.
- [Easy audio capture with the MediaRecorder API](https://hacks.mozilla.org/2014/06/easy-audio-capture-with-the-mediarecorder-api/)
  - : Explains the basics of using the MediaStream Recording API to directly record a media stream.

## Reference

- [The video element](/en-US/docs/Web/HTML/Reference/Elements/video)
- [HTMLVideoElement API](/en-US/docs/Web/API/HTMLVideoElement)
- [MediaSource API](/en-US/docs/Web/API/MediaSource)
- [Web Audio API](/en-US/docs/Web/API/Web_Audio_API)
- [MediaStream Recording API](/en-US/docs/Web/API/MediaStream_Recording_API)
- [getUserMedia](/en-US/docs/Web/API/MediaDevices/getUserMedia)
- [Event index: Media](/en-US/docs/Web/API/Document_Object_Model/Events#media)
