# Simulate events

# Convert canvas to image

This is a trip, I can't believe this is possible.  
<canvas id="canvas" width="100" height="100"></canvas>

<script type="text/javascript">

//var canvas = document.getElementById("canvas");
//var dataURL = canvas.toDataURL();
alert("HELLO");

</script>
# Make AJAX requests for HTML files and display in IFRAME

Can then inject your own JS

# Getting HTTP headers 


You cannot obtain the initial page request with JS, however you can obtain response headers through an AJAX call.
The following is a workaround to obtain the HTTP headers from the currect page. This code snippet just sends an AJAX request for the current from which it then has access to header. 
From [Stackoverflow] (http://stackoverflow.com/a/4881836/494762)
```javascript
var req = new XMLHttpRequest();
req.open('GET', document.location, false);
req.send(null);
var headers = req.getAllResponseHeaders().toLowerCase();
alert(headers);
```
http://stackoverflow.com/questions/220231/accessing-the-web-pages-http-headers-in-javascript
