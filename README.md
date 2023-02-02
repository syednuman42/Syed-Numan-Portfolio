<html>
  <head>
  </head>
  <body onload="init();">
    <h1>V12: Aadhaar Masking / Selfie Recapture Detection Live Demo</h1>
   Click on the Start WebCam, then proceed to take photo.
     <p>
    <button onclick="startWebcam();">Start WebCam</button>
    <button onclick="snapshot2();">Selfie Recapture Detection</button> 
    <button onclick="snapshot();">Aadhaar Masking</button>      
    </p>
    
    <video onclick="snapshot(this);" width=300 height=300 id="video" controls autoplay></video>

  
   <pre>              Input Frame                              Detection Output </pre>

<canvas  id="myCanvas" width="300" height="300"></canvas>  
<img id="img" src="" height=300 width=300 />

  </body>
  <script>

      //--------------------
      // GET USER MEDIA CODE
      //--------------------
          navigator.getUserMedia = ( navigator.getUserMedia ||
                             navigator.webkitGetUserMedia ||
                             navigator.mozGetUserMedia ||
                             navigator.msGetUserMedia);

      var video;
      var webcamStream;
            
      function startWebcam() {
        if (navigator.getUserMedia) {
           navigator.getUserMedia (

              // constraints
              {audio: false,
               video: true
                 
              },

              // successCallback
              function(localMediaStream) {
                  video = document.querySelector('video');
                 video.srcObject=localMediaStream;
                 webcamStream = localMediaStream;
              },

              // errorCallback
              function(err) {
                 console.log("The following error occured: " + err);
              }
           );
        } else {
           console.log("getUserMedia not supported");
        }  
      }
            
      function stopWebcam() {
          webcamStream.stop();
      }
      //---------------------
      // TAKE A SNAPSHOT CODE
      //---------------------
      var canvas, ctx;

      function init() {
        // Get the canvas and obtain a context for
        // drawing in it
        canvas = document.getElementById("myCanvas");
        ctx = canvas.getContext('2d');
      }

      async function snapshot() {
        import api_key from './apikey.js'
        console.log(api_key)
         // Draws current image from the video element into the canvas
        canvas.getContext('2d').drawImage(video, 0, 0, 300,300);   
        img = canvas.toDataURL("image/jpeg").split(';base64,')[1];
        // console.log(img);
        datatosend = {'project':1,'byte_image':img};
        let result = await fetch("https://y1xv8eaws6.execute-api.ap-south-1.amazonaws.com/default/portfolio2", {                      
            method: "post",  
            mode : "cors",
             headers: {
                'x-api-key': api_key,
                'content-type': 'application/json'},
            body: JSON.stringify(datatosend)
             })
        .then(response=>response.json())

document.getElementById("img").src = result
      }

async function snapshot2() {
        import api_key from './apikey.js'
        console.log(api_key)
         // Draws current image from the video element into the canvas
        canvas.getContext('2d').drawImage(video, 0, 0, 300,300);   
        img = canvas.toDataURL("image/jpeg").split(';base64,')[1];
        // console.log(img);
        datatosend = {'project':2,'byte_image':img};
        let result = await fetch("https://y1xv8eaws6.execute-api.ap-south-1.amazonaws.com/default/portfolio2", {                      
            method: "post",  
            mode : "cors",
             headers: {
                'x-api-key': api_key,
                'content-type': 'application/json'},
            body: JSON.stringify(datatosend)
             })
        .then(response=>response.json())

document.getElementById("img").src = result
      }



  </script>
</html>

