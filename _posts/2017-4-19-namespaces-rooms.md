---
layout: post
title: "Namespaces and Rooms"
date: 2017-4-19
categories:
description: 
permalink: /namespaces-rooms/

---

As a way to manage data connections, we can introduce separation between communication channels using custom Namespaces. Within each namespace, you can also define Rooms, or arbitrary channels that sockets can join and leave.



<br>

<span class="underlined">**Namespaces**</span>

Socket.IO allows you to **Namespace** your sockets, which essentially means assigning different endpoints or paths.

This is a useful feature to minimize the number of resources (TCP connections) and at the same time separate concerns within your application by introducing separation between communication channels. Multiple namespaces actually share the same WebSockets connection thus saving us socket ports on the server.

Namespaces are created on the server side. But they are joined by clients by sending a request to the server.

#### Default Namespace 

The root namespace `/` is the default namespace which is joined by clients if a namespace is not specified by the client when they connect to the server. All connections to the server using the socket object client side are made to the default namespace. So far we've been using the default, like this:

`var socket = io();`

This will connect the client to the default namespace. All events on this namespace connection will be handled by the io object on the server. All the previous examples were utilizing default namespaces to communicate with the server and back.

#### Custom Namespace 

We can create our own custom namespaces. To set up a custom namespace, we can call the of function on the server-side:

<pre>
// attach Socket.io to our HTTP server
var io = require('socket.io').listen(httpServer);

var nsp = io.of('/my-namespace');
nsp.on('connection', function(socket){
	console.log('someone connected');
});
nsp.emit('hi', 'everyone!');
</pre>

<br>

Then, on the client side, we tell tell Socket.IO client to connect to that namespace:

`var socket = io('/my-namespace');`

<br>

<span class="underlined">**Rooms**</span>

Rooms are subchannels of the namespaces. **Rooms are purely a server-side construct and the client knows nothing about them.**

You can't join a room with socket io from the client side, it should happens in the server side. To tackle this you need to emit a socket with the room you want to join as data , and in the server you listen to this socket and call socket.join() with the name or the id of the room that has been sent.

Here is a [helpful example](https://gist.github.com/crtr0/2896891) of using rooms with socket.io

<br>

<span class="underlined">**Other Resources**</span>


More discussion about [namespaces vs. rooms](http://stackoverflow.com/questions/10930286/socket-io-rooms-or-namespacing) on github 

See [socket.io](https://socket.io/docs/rooms-and-namespaces/) documentation for more about this

