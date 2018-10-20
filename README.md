# Video Doorbell, Lab 7

*A lab report by Tal Genkin*


## Part A. HelloYou from the Raspberry Pi

**a. Link to a video of your HelloYou sketch running.**

[Can be seen in Prelab 7 prep](https://github.com/TalGenkin/Interactive--Device-Design/blob/master/Lab7Prep.md)

## Part B. Web Camera

**a. Compare `helloYou/server.js` and `IDD-Fa18-Lab7/pictureServer.js`. What elements had to be added or changed to enable the web camera? (Hint: It might be good to know that there is a UNIX command called `diff` that compares files.)**

[pictureServer and server](https://github.com/TalGenkin/Interactive--Device-Design/blob/master/pictureServerandServer.png)

In pictureServer.js, there is an addition for code for the web camera setup:

/--Additions:
//----------------------------WEBCAM SETUP------------------------------------//
//Default options
var opts = { //These Options define how the webcam is operated.
    //Picture related
    width: 1280, //size
    height: 720,
    quality: 100,
    //Delay to take shot
    delay: 0,
    //Save shots in memory
    saveShots: true,
    // [jpeg, png] support varies
    // Webcam.OutputTypes
    output: "jpeg",
    //Which camera to use
    //Use Webcam.list() for results
    //false for default device
    device: false,
    // [location, buffer, base64]
    // Webcam.CallbackReturnTypes
    callbackReturn: "location",
    //Logging
    verbose: false
};
var Webcam = NodeWebcam.create( opts ); //starting up the webcam
//----------------------------------------------------------------------------//


In addition, there is a code for taking a picture:


//-- Addition: This function is called when the client clicks on the `Take a picture` button.
  socket.on('takePicture', function() {
    /// First, we create a name for the new picture.
    /// The .replace() function removes all special characters from the date.
    /// This way we can use it as the filename.
    var imageName = new Date().toString().replace(/[&\/\\#,+()$~%.'":*?<>{}\s-]/g, '');

    console.log('making a making a picture at'+ imageName); // Second, the name is logged to the console.

    //Third, the picture is  taken and saved to the `public/`` folder
    NodeWebcam.capture('public/'+imageName, opts, function( err, data ) {
    io.emit('newPicture',(imageName+'.jpg')); ///Lastly, the new name is send to the client web browser.
    /// The browser will take this new name and load the picture from the public folder.
  });
  
Something that was changed was the variables defined in the beginning of server.js were const:


const express = require('express'); // web server application
const http = require('http');       // http basics
const app = express();                          // instantiate express server
const server = http.Server(app);        // connects http library to server
const io = require('socket.io')(server);        // connect websocket library to server  // serial library
const serverPort = 8000;            // port (ixe##.local:PORT)


While in pictureServer.js, they weren't:


var express = require('express'); // web server application
var app = express(); // webapp
var http = require('http').Server(app); // connects http library to server
var io = require('socket.io')(http); // connect websocket library to server
var serverPort = 8000;
var SerialPort = require('serialport'); // serial library
var Readline = SerialPort.parsers.Readline; // read serial data as lines


Additional variable:

var NodeWebcam = require( "node-webcam" );// load the webcam module


**b. Include a video of your working video doorbell**

[Picture-camera](https://github.com/TalGenkin/Interactive--Device-Design/blob/master/cameraPicture.png)

[A video of pressing the button in the Arduino and seeing the screen goes white and black, with the picture taken from the wbecam](https://youtu.be/Tw60-grqKYY)

## Part C. Make it your own

**a. Find, install, and try out a node-based library and try to incorporate into your lab. Document your successes and failures (totally okay!) for your writeup. This will help others in class figure out cool new tools and capabilities.**

**b. Upload a video of your working modified project**
