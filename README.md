Running the interactive 360 video player
=======
First, clone the repo. 

Next, you'll need a simple webserver. At a minimum it should support partial range requests (e.g., to support seek by loading only part of the video at a time). You can see if a server supports partial range requests by following these instructions: <https://developer.mozilla.org/en-US/docs/Web/HTTP/Range_requests#Checking_if_a_server_supports_partial_requests>. Node's http-server does support partial range requests, but Python's SimpleHTTPServer does not. To run the interactive video player using Node's http-server, cd into the repo directory, then type "npm install http-server -g" to install the server, and then type "http-server" to run the server. 

However, http-server does not support POST requests. If you would like to record and save data from viewing sessions (e.g., head orientation and interactions), you'll need a different server (a local Apache web server works great) so that save_results.php can save the viewing session to a filepath. 

API calls
=======
You can use our video player that implements fixed-orientation, viewpoint-oriented, and active reorientation as above. 

If you would like to edit this player to add additional functionality, you may want to use the player's API. This player is based on Google's VR View, but adds some functionality for changing video playback based on interaction and head orientation. 

These API calls in this repo were implemented in the original Google VRView player (for additional API info see here: <https://developers.google.com/vr/concepts/vrview-web>): 
* vrview = new VRView.Player("ID", {video: 'link/to/video.mp4', is_stereo: false}) -- Creates the vrview player
* vrview.play() -- Plays the video
* vrview.pause() -- Pauses the video 

New API calls for this project, these calls will work for desktop and cardboard/mobile: 
* vrview.setOrientation(_y-axis-rotation-in-radians_) -- Sets current camera orientation by rotating camera around y axis 
* vrview.getOrientation() -- Gets the current camera orientation about the y axis (offset by -pi/2). This doesn't actually get the orientation but instead calls an event that you can listen for
* vrview.currentTime() -- Gets the current time in the video. Again, this doesn't actually return the time, it just calls an event that you can listen for
* vrview.seek(_seconds_) -- Seeks to number of seconds within the video
* vrview.record("startRecording") -- Starts recording interactions (e.g., play, pause, seek) and camera orientation
* vrview.record(_filename_) -- Stops recording and saves recording to the file _filename_

To support these API new calls, I've edited the following files in the original API (an old version found here: ): build/embed.js, build/vrview.js, build/three.js
