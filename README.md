# Remove Watched Videos from Youtube Watch Later playlist
Simple script to paste in console to remove all watched videos from Watch Later playlist (bypassing 502 error). Updated on 04/07/2023.

1. Go to your [Watch Later playlist](https://www.youtube.com/playlist?list=WL)
2. Open Developer Console pressing **F12** or **CTRL + SHIFT + I**
3. Paste the following code in the console:

```javascript
setInterval(function () {
  watchedVideo = document.querySelector('ytd-thumbnail-overlay-resume-playback-renderer').closest('#content')
  watchedVideoActionMenu = watchedVideo.nextElementSibling
  watchedVideoActionMenu.querySelector('#primary button[aria-label="Action menu"]').click();
  var things = document.evaluate(
    '//span[contains(text(),"Watch Later")]',
    document,
    null,
    XPathResult.ORDERED_NODE_SNAPSHOT_TYPE,
    null
  );
  for (var i = 0; i < things.snapshotLength; i++) {
    things.snapshotItem(i).click();
  }
}, 1000);
```
