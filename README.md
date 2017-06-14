# Viscovery-html5-ad-sdk
Document of Viscovery Ad sdk

## Intro

Viscovery aims to help our video publishers to monetize their video content with content-relative ads. The whole adTech includes the integration of Viscovery's FITAMOS computer vision recognition technology.

## Resources

+ [Demo](https://vsp.viscovery.com/visSDK/)
+ [SDK static files](https://vsp.viscovery.com/visSDK/lib/js/)




## Prerequisites

Before you begin, setup the following files

+ index.html
+ style.css

Setup a basic server to host your file

+ If you have python, you can run python -m SimpleHTTPServer in the directory containing index.html and point your browser to localhost:8000

+ You can also use gulp or webpack to serve the static files

## Get Start

### Import

The IMA SDK and ViscoverySDK need to be import to your html file

```html
<script src="//imasdk.googleapis.com/js/sdkloader/ima3.js"></script>
<script type="text/javascript" async src="https://vsp.viscovery.com/visSDK/lib/js/visSDK.{version_number}.js"></script>
```

### Dom position structure of player and adContainer

Below is the sample html/css layout, shown the integration of your H5 video tag player and Viscovery Ad SDK

<b>index.html</b>
```html
<html lang="en">
<head>
  <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
  <title>Viscovery Demo Page</title>
  <meta name="description" content="Viscovery 成立於2013年，專注於影音辨識技術開發，擁有多項演算法專利，被 Google 評選為成功企業與創新科技公司。經過多年圖像辨識技術研發的積累，及實地操作大量應用場景的基礎上，Viscovery 成功開發出 VDS 智能影音探索平台。" />
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link type="image/png" rel="shortcut icon" href="./static/favicon.ico" />
  <link type="text/css" rel="stylesheet" href="./static/style.css" />
  <script src="//imasdk.googleapis.com/js/sdkloader/ima3.js"></script>
  <script type="text/javascript" src="https://vsp.viscovery.com/visSDK/lib/js/visSDK.{version_number}.js"></script>
</head>
<body>
  <div id="container">
    <div id="content">
      <video id="content-player" class="player-layout" preload="metadata" width="854" height="480" playsinline controls>
        <source src="http://viscovery-vsp-dev.s3.amazonaws.com/sdkdemo/Videos/sillicon%20valley%20S1%20trailer.mp4"></source>
      </video>
    </div>
    <div id="adContainer"></div>
  </div>
  <div class="wording">
    <p>news content for outstream expanding testing : news content for outstream expanding testing</p>
    <p>news content for outstream expanding testing : news content for outstream expanding testing</p>
    <p>news content for outstream expanding testing : news content for outstream expanding testing</p>
    <p>news content for outstream expanding testing : news content for outstream expanding testing</p>
  </div>
</body>
```

<b>style.css</b>
```css
* {
  padding: 0;
  margin: 0;
  border: 0;
}

#container {
  position: relative;
  width: 854px;
  height: 480px;
}

#content, #adContainer {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 854px;
  height: 480px;
  z-index: 0;
}

#content-player {
  width: 854px;
  height: 480px;
  overflow: hidden;
}
```

### SDK initialize

Intialize the SDK with the video domNode and adContainer domNode after the html body tag

```javascript
viscoveryAd.init(Argument1, Argument2, Argument3)
```

<b>Argument1</b> is the html vidoe tag player. Here select it through #content-player and indicates it to <b>Argument1</b>.

```javascript
viscoveryAd.init('#content-player', ...)
```

<b>Argument2</b> is the html adContainer div. Here select it through #adContainer and indicates it to <b>Argument2</b>.

```javascript
viscoveryAd.init('#content-player', '#adContainer', ...)
```

<b>Argument3</b> is an object which required the unique parameter for the publisher


<table>
  <tr>
    <td>key</td>
    <td>description</td>
    <td>required</td>
  </tr>
  <tr>
    <td>api_key</td>
    <td>the applied api key for the currentr publisher</td>
    <td>Y</td>
  </tr>
  <tr>
    <td>video_url</td>
    <td>the video content source url</td>
    <td>Y</td>
  </tr>
  <tr>
    <td>video_id</td>
    <td>the video unique id (confirmed with the contract publishers)</td>
    <td>optional</td>
  </tr>
  <tr>
    <td>debug_mode</td>
    <td>switch of debug mode</td>
    <td>Y</td>
  </tr>
   <tr>
    <td>playerControl</td>
    <td>the video content source url</td>
    <td>Y</td>
  </tr>
</table>


#### playerControl parameter 

for player integration parameter, bind the current using player interface

 - play: *(function)* player play method
 - pause: *(function)* player pause method
 - isSeeking: *(boolean)* player is on seeking method
 - currentTime: *(Number)* play current playback time value
 - isPaused: *(boolean)* player is paused status

### whole SDK init sample
```html 
<script type="text/javascript">
  window.onload = function() {
    viscoveryAd.init('#content-player', '#adContainer', {
      api_key: '<api_key>', // the applied api key of viscoveryAd
      video_url: 'https://video.site.url/<video_id>', // video encode content url
      debug_mode: 0, // 1 for open, 0 for close
      playerControl: {
      }
    });
  }
</script>
```

## Extension

### DailyMotion initialize

for dailyMotion initialize sample, do as the follow

<b>body player part</b>
```html
<div id="container">
  <div id="content">
    <div id="player"></div>
  </div>
  <div id="adContainer"></div>
</div>
```

<b>player and sdk init part</b>
```javascript
    var player = DM.player(document.getElementById("player"), {
      video: '<dailymotion video id>',
      width: '<video width>',
      height: '<video height>',
      params: {
        autoplay: false,
        mute: false,
        controls: true,
      }
    });

    window.onload = function() {
      document.querySelector('#adContainer').style['z-index'] = '-1';
      /* init viscoveryAd sdk after dailymotion preroll ad done */
      player.addEventListener('video_start', function(event) {
        document.querySelector('#adContainer').style['z-index'] = '0';

        viscoveryAd.init('#player', '#adContainer', {
          api_key: '<api_key>', // <api_key> : the applied api key of viscoveryAd
          video_url: "https://video.site.url/<video_id>", // <video_url> : video encoded content url
          debug_mode: 0, // 1 for open, 0 for close
          playerControl: {
            currentTime: player.currentTime,
          }
        });
      });
  }
```



