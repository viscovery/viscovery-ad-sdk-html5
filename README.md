# Viscovery-html5-ad-sdk
Document of Viscovery AD SDK

## Intro

Viscovery aims to help our video publishers to monetize their video content with content-relative ads. The whole adTech includes the integration of Viscovery's FITAMOS computer vision recognition technology.

## Resources
* [SDK static files](https://vsp.viscovery.com/visSDK/lib/js)
* [demo](https://vsp.viscovery.com/visSDK)

## Get Started

### Import

ViscoverySDK need to be import to your html file

```html
<script type="text/javascript" src="{host}/visSDK.2.0.js"></script>
```

### HTML Structure

Developer must fulfill following requests

* have a video player
* have an ad-container for displaying instream-ad
* have a outstream container for displaying outstream-ad
* make sure that ancestor of ad-container align with top-left of video player
* make sure that growing-width and growing-height of outstream container is what you expect

### HTML Structure Suggestion

#### HTML
```html
<div id="container">
  <div id="content">
    <video id="content-player" class="player-layout" preload="metadata" width="854" height="480" playsinline controls>
      <source src="https://your.video.path"></source>
    </video>
  </div>
  <div id="adContainer"></div>
  <div id="outstream"></div>
</div>
```

\*playsinline: you can refer **Mobile Fullscreen Hacking** section to understand why you should add `playsinline` attribute.

#### Style
```css
#container {
  position: relative;
}

#adContainer {
  position: absolute;
  top: 0px;
  left: 0px;
}
```

### Initialize SDK
```html 
<script type="text/javascript">
  window.onload = function () {
    viscoveryAd.init({
      contentPlayer: '#content-player',
      adContainer: '#adContainer',
      outStreamContainer: '#outstream',
      apiKey: 'your-api-key', 
      videoUrl: "https://your.video.path",
      videoId: 'video-id',
      playerControl: {
        // play: '',
        // pause: '',
        // getSeeking: '',
        // getCurrentTime: '',
        // getPaused: '',
      },
      bottomOffset: 30,
      mobileFullscreenHacking: true,
    });
  }
</script>
```

## API Reference
### `init(args)`

initialize Viscovery AD SDK

#### Arguments
* `contentPlayer` (*string*): css selectors of content player
* `adContainer` (*string*): css selectors of instream ad container
* `outStreamContainer` (*string*): css selectors of outstream ad container
* `api_key` (*string*): api key
* `video_url` (*string*): content video url\*1
* `video_id` (*string*): content video id\*1
* `[bottomOffset]` (*number*) **default 35**: nonlinear instream ads offset from bottom providing you to avoid overlay on player control bar
* `[mobileFullscreenHacking]` (*boolean*) **default true**: determine if you allow Viscovery Ad SDK replace your mobile native screen with our definition
* `playerControl` \*2
* `playerControl.play` (*function*): play function for content player
* `playerControl.pause` (*function*): pause function for content player
* `playerControl.getSeeking` (*function*): seeking getter of content player 
* `playerControl.getCurrentTime` (*function*): current time getter of content player
* `playerControl.getPaused` (*function*): paused state of content player

\*1 You can only give video url or video id. You will not get right ads if you give them both but they don't match.

\*2 if `play`, `pause`, `seeking`, `currentTime` or `paused` of your player don't match native html5 video player property name, you should give SDK these by yourself.

## Mobile Fullscreen Hacking

Due to limitation of mobile browser, Viscovery Ad SDK could not implement ad displaying on mobile browser. However, if `mobileFullscreenHacking` is `true`, Viscovery Ad SDK will achieve the fullscreen mode by implementing following specs.

* Content player is always in-line mode in **portrait**.
* Content player is always fullscreen mode in **landscape**.

Besides turning `mobileFullscreenHacking` into `true`, developer should add `playsinline` attribute to video tag.

## Support player
- html video tag
- videojs
- dailymotion

## Support Inventory size
- PC instream linear
- PC instream non-linear
- PC outstream non-linear
- Mobile instream linear
- Mobile instream non-linear
- Mobile outstream non-linear
