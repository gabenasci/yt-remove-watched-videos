# Remove Watched Videos from Youtube Watch Later playlist
Currently, most Youtube users are unable to remove watched videos from their Watch Later playlist using the 'Remove Watched Videos' button. This becomes a problem once users reach the maximum playlist size of 5000 videos and would otherwise have to manually remove videos from their playlist.

This simple console script will remove all fully watched videos from Watch Later playlist, bypassing the 502 error by removing each video individually.

1. Go to your [Watch Later playlist](https://www.youtube.com/playlist?list=WL)
2. Open Developer Console pressing **F12** or **CTRL + SHIFT + I**
3. Paste the following code in the console and press **ENTER**:

```javascript
setInterval(function () {
  try {
    watchedVideo = document.querySelector(`ytd-thumbnail-overlay-resume-playback-renderer > div.style-scope[style="width: 100%;"]`).closest('#content');
    watchedVideoMenu = watchedVideo.nextElementSibling;
    watchedVideoMenu.querySelector('#primary button[aria-label="Action menu"]').click();
    var things = document.evaluate(
      '//span[contains(text(),"Remove from")]',
      document,
      null,
      XPathResult.ORDERED_NODE_SNAPSHOT_TYPE,
      null
    );
    for (var i = 0; i < things.snapshotLength; i++) {
      things.snapshotItem(i).click();
    }
    console.log("Watched video removed");
  }
  catch(err) {
    if(err.constructor = TypeError){
      console.log("No watched videos loaded on page, scrolling down to load more videos.");
    }
    else{
      console.log(`Error: ${err}`);
    }
  }
  hideActionMenu = document.querySelector('body').click();
  window.scrollBy(0, window.innerHeight);
}, 2000);
```

---

If you would also like to remove partially watched videos from the Watch Later playlist, you can use the following script (does not check for width: 100% on the 'resume-pĺayback' progress bar):

```javascript
setInterval(function () {
  try {
    watchedVideo = document.querySelector(`ytd-thumbnail-overlay-resume-playback-renderer`).closest('#content');
    watchedVideoMenu = watchedVideo.nextElementSibling;
    watchedVideoMenu.querySelector('#primary button[aria-label="Action menu"]').click();
    var things = document.evaluate(
      '//span[contains(text(),"Remove from")]',
      document,
      null,
      XPathResult.ORDERED_NODE_SNAPSHOT_TYPE,
      null
    );
    for (var i = 0; i < things.snapshotLength; i++) {
      things.snapshotItem(i).click();
    }
    console.log("Watched video removed");
  }
  catch(err) {
    if(err.constructor = TypeError){
      console.log("No watched videos loaded on page, scrolling down to load more videos.");
    }
    else{
      console.log(`Error: ${err}`);
    }
  }
  hideActionMenu = document.querySelector('body').click();
  window.scrollBy(0, window.innerHeight);
}, 2000);
```
---
Previously, the script would fail once it had removed all the videos that had been loaded onto the page. With the help of <a href="https://github.com/imogenwren">Imogen Wren</a>, the following scrollBy function was added to the end of the scripts to scroll down the page continually as it removes videos from the playlist. 

```javascript
window.scrollBy(0, window.innerHeight);
```



