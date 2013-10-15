#CoolQueue.io
##A gentlemen's persistent failsafe for Socket.io
License: MIT

So you have this cool website featuring realtime i/o made with your favourite tools and your awesome skills.
But what if this realtime i/o is not so realtime anymore? What if your site goes offline? (which is obviously not your fault!)

###Fear no more bro!
At CodeBuffet I had the same nightmare. But now a remedy is born! And it's free!

CoolQueue.io sits between socket.io, using HTML5 Storage to keep track of anything that **was supposed to be** send to the server. When you come online again, we push it back to the server like nothing every happened **(WOW!)**

It's like Socket.io always had this feature all along!

###I want it!
To install CoolQueue.io, just load coolqueue.io.js and the dependency (jStorage) file like this in your website:

	<script src="<PATH TO>/jstorage.min.js"></script>
	<script src="<PATH TO>/coolqueue.io.js"></script>
	
Or for the smaller minified version (useful for production):

	<script src="<PATH TO>/jstorage.min.js"></script>
	<script src="<PATH TO>/coolqueue.io.min.js"></script>

The next step is to initialize CoolQueue:

	var coolQueue = createCoolQueue();

After you initialize socket.io, but before you define any connect or disconnect events, do the following to make sure CoolQueue intercepts the socket.io requests correctly:

	coolQueue.swallow(socket);
	
When you want to send the queue (if any) do the following:

	// Send the queue (if any)
	coolQueue.sendQueue();

###Complete example
	
	var coolQueue = createCoolQueue();
	coolQueue.swallow(socket);
	
	socket.on('connect', function () {
		// Send the queue (if any) on connection
	    coolQueue.sendQueue();
	});
	
	socket.on('disconnect', function() {
	    
	});
	
	socket.emit("lol", {"I will be saved and sent later if your server goes offline!": "Really? Cool!"});
	
###Custom Properties

Properties can be defined like this:

	var coolQueue = createCoolQueue({
		persistent: true
	});

####persistent
***Default:*** True

Disables HTML5 storage, will only save in RAM (non-pesistent) and queue is just like it was before if you refresh or restart the browser

####queueKey
***Default:*** "my_queue"

Set different queues, easy for managing different socket.io connections

####ignoreKeys
***Default:*** (Empty Array)

This is an array of strings, strings in here will be ignored.
