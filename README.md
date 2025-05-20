# Reflection 10
---

## Tutorial 2

![server](server.png)
![client1](client1.png)
![client2](client2.png)
![client3.png](client3.png)

This illustrates a typical broadcast-style chat server architecture. Upon connection, each client is greeted with a welcome message from the server. The server operates by receiving messages from any connected client, recording them in its logs, and then distributing them to every connected client simultaneously.

The implementation uses Tokio's broadcast channel mechanism to efficiently send messages to all participants. This is why each message appears across all client terminals tagged with the "From server:" label. The system creates a group chat environment where every participant can see all communications - when one client sends a message, it gets relayed through the server to every other connected client, ensuring complete visibility of the conversation for all participants.

In essence, the server acts as a central hub that captures and redistributes all messages, making sure that no client misses any part of the ongoing conversation.