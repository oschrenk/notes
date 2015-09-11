# HTML 5 Video

## canplaythrough

[Chrome doesn't handle `canplaythrough`](https://code.google.com/p/chromium/issues/detail?id=73609)

I tried to implement my own crude version but when using `progress` event, the video did not loop anymore.

```html
<div id="videobox" style="display: none;" >
  <video autoplay loop poster="<%= static_host %>/video/vertical/<%= vertical %>-poster.jpg" id="bgvid">
    <source src="<%= static_host %>/video/vertical/<%= vertical %>-vid.mp4" type="video/mp4">
    <source src="<%= static_host %>/video/vertical/<%= vertical %>-vid.webm" type="video/webm">
    <source src="<%= static_host %>/video/vertical/<%= vertical %>-vid.ogv" type="video/ogv">
  </video>
</div>
```

```javascript
    <script type="text/javascript">
      var v = document.getElementById("bgvid");
      v.addEventListener('progress', function(e) {

          console.log("Fooo");
        if (this.buffered.length > 0) {
          var total = this.duration;
          var end =   this.buffered.end(0);
          var loaded = end/total;
          if (loaded > 0.6) {
            document.getElementById("videobox").style.display = 'block';
          };
          console.log(loaded);
        };
      }, false);
    </script>
```
