---
layout: post
title: "Simple Chat"
date: 2017-1-25
categories:
description: 
permalink: /simple-chat/

---

### <span class="underlined">Simple Chat</span>

Here is a full chat example that we can start with. 

This examples uses [Socket.io](http://socket.io/), which we will be using extensively over the course of the semester. Socket.io is a JavaScript library that uses websockets for realtime web applications. It has two parts: a client-side library that runs in the browser, and a server-side library for node.js.

<br>

### <span class="underlined">server.js</span>

	// HTTP Portion
	var http = require('http');
	var fs = require('fs'); // Using the filesystem module
	var httpServer = http.createServer(requestHandler);
	var url = require('url');
	httpServer.listen(8080);

	function requestHandler(req, res) {

		var parsedUrl = url.parse(req.url);
		console.log("The Request is: " + parsedUrl.pathname);
			
		fs.readFile(__dirname + parsedUrl.pathname, 
			// Callback function for reading
			function (err, data) {
				// if there is an error
				if (err) {
					res.writeHead(500);
					return res.end('Error loading ' + parsedUrl.pathname);
				}
				// Otherwise, send the data, the contents of the file
				res.writeHead(200);
				res.end(data);
	  		}
	  	);
	  	
	  	/*
	  	res.writeHead(200);
	  	res.end("Life is wonderful");
	  	*/
	}

	// WebSocket Portion
	// WebSockets work with the HTTP server
	var io = require('socket.io').listen(httpServer);

	// Register a callback function to run when we have an individual connection
	// This is run for each individual user that connects
	io.sockets.on('connection', 
		// We are given a websocket object in our function
		function (socket) {
		
			console.log("We have a new client: " + socket.id);
			
			// When this user emits, client side: socket.emit('otherevent',some data);
			socket.on('chatmessage', function(data) {
				// Data comes in as whatever was sent, including objects
				console.log("Received: 'chatmessage' " + data);
				
				// Send it to all of the clients
				socket.broadcast.emit('chatmessage', data);
			});
			
			
			socket.on('disconnect', function() {
				console.log("Client has disconnected " + socket.id);
			});
		}
	);



<br>

### <span class="underlined">index.html</span>


	<html>
		<head>
			<script type="text/javascript" src="/socket.io/socket.io.js"></script>
			<script type="text/javascript">
			
				var socket = io.connect();
				
				socket.on('connect', function() {
					console.log("Connected");
				});

				// Receive from any event
				socket.on('chatmessage', function (data) {
					console.log(data);
					document.getElementById('messages').innerHTML = "" + data + 
	 + "" + document.getElementById('messages').innerHTML;
				});
				
				var sendmessage = function(message) {
					console.log("chatmessage: " + message);
					socket.emit('chatmessage', message);
				};

		
			</script>	
		</head>
	 <body>
	 <div id="messages">
	 No Messages Yet
	 </div>
	 <input type="text" id="message" name="message">
	 <input type="submit" value="submit" onclick="sendmessage(document.getElementById('message').value);">
	 </body>
	</html>


<br>

### <span class="underlined">Running It</span>

First, upload these files to a dedicated folder on your server (using Cyberduck). 

In order to view our app live, we need to run the Node server. This happens through the command line. Open the Terminal / Command Prompt and connect to your server:

	ssh root@YOUR_IP

You can run the `ls` command to confirm that the folder you uploaded is in there. Using the command line, move into that folder:

	cd directory_you_just_created

The first time you attempt to run your app, you will need to install the socket.io module in that directory via the command line. You will see feedback in the command line letting you know it has been installed. **You only need to do this the first time you upload* 

	npm install socket.io

Finally you can run the server via node.

	node server.js

Assuming you don't get any errors, you should be able to connect via your browser:

**http://YOUR_IP:8080/index.html**

<br>

To stop your server, enter control + 'C'
