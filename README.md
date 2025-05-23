# Reflection 10
---

## Tutorial 2

### Initial Experiment

![server](server.png)
![client1](client1.png)
![client2](client2.png)
![client3.png](client3.png)

This illustrates a typical broadcast-style chat server architecture. Upon connection, each client is greeted with a welcome message from the server. The server operates by receiving messages from any connected client, recording them in its logs, and then distributing them to every connected client simultaneously.

The implementation uses Tokio's broadcast channel mechanism to efficiently send messages to all participants. This is why each message appears across all client terminals tagged with the "From server:" label. The system creates a group chat environment where every participant can see all communications - when one client sends a message, it gets relayed through the server to every other connected client, ensuring complete visibility of the conversation for all participants.

In essence, the server acts as a central hub that captures and redistributes all messages, making sure that no client misses any part of the ongoing conversation.

### Modify Port

#### Server Port 2000, Client Port 8080

![server](server_ver2.png)
![client1](client1_ver2.png)
![client2](client2_ver2.png)
![client3.png](client3_ver2.png)

In the first scenario, connections failed because there was a port mismatch - the clients were attempting to establish connections on port 8080, but the server was actively listening on port 2000. Since network connections require both sides to use the same port for communication, this mismatch prevented any successful connections.

#### Server Port 8080, Client Port 8080

![server](server_ver3.png)
![client1](client1_ver3.png)
![client2](client2_ver3.png)
![client3.png](client3_ver3.png)

In the second scenario, successful connections were established because the server and all clients were configured to use the same port (8080). When both the server and clients targeted the identical port number, they could successfully establish communication channels.

This demonstrates a core networking concept: for client-server communication to work, the server must be actively listening on the precise port that clients are trying to connect to. Without this port alignment, the connection attempts will fail because the server isn't monitoring the port where clients are knocking.

### Change info

![server](server_ver4.png)
![client1](client1_ver4.png)
![client2](client2_ver4.png)
![client3.png](client3_ver4.png)

By updating the handle_connection function to incorporate {addr:?} in the broadcast message format (bcast_tx.send(format!("{addr:?}: {text:?}"))?;), each transmitted message now includes identification details showing which client originated it. This solution uses the socket address (combining IP address and port number) as a unique marker for each client connection, enabling all participants to see the sender of every message.

The decision to implement this change on the server side rather than modifying how clients display messages was strategic. Centralizing this logic at the message distribution level ensures that all clients receive consistently formatted messages without requiring individual clients to process or decode raw message data.

This design choice also preserves the server's architectural role as the central message handler in the chat system. By keeping the formatting responsibility with the server, client applications remain streamlined and can focus primarily on user interaction and message display, rather than having to handle message parsing and formatting tasks.