# Client-Server Architecture Using Python

A multi-threaded client-server chat system in Python: one server handles several clients at once over TCP sockets, supporting both broadcast messages to everyone and private messages to one named user.

Originally built for a Computer Networks Lab course project (PCCEC692) at IEM Kolkata, submitted under a five-person group requirement. This repo credits the two people who actually wrote and tested the code: **Somya Singh** and **Lawaz Islam**. The report's own file metadata lists Somya Singh as the author, and the terminal evidence in the report (reproduced in the PDF here) shows the demo running from her machine with the two of us as the only active users during the test.

## How it works

- The server creates a socket, binds it to a host and port, and listens for incoming connections.
- Each client connects, sends a username, and receives a welcome message.
- Messages are framed with a fixed-length header (`HEADER = 10`) stating the payload length, so the receiver always knows exactly how many bytes to read next.
- A message starting with `@username` is routed privately to that one client; anything else is broadcast to everyone connected.
- The server uses `select.select()` to watch all client sockets at once without blocking, so it can serve multiple clients concurrently from a single thread. The client uses two threads, one for receiving, one for sending, so you can type and receive messages at the same time.

## Run it

Open one terminal for the server, and one terminal per client (three in the demo below).

```bash
# terminal 1
python server.py

# terminal 2, 3, 4...
python client.py
# then enter the server's IP (printed by server.py) and a username
```

Type a message and press Enter to broadcast it to everyone connected. Type `@username your message` to send privately to one person. Type `!DISCONNECT` to leave.

## My contribution

I worked on this with Somya Singh. Between us we designed the message framing protocol (the fixed-length header scheme), implemented the server's multi-client handling with `select`, and built the client's dual-thread send/receive loop.

Full report, including the original test screenshots: [`Client_Server_Report.pdf`](Client_Server_Report.pdf) in this repo.
