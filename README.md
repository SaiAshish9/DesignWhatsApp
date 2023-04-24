https://www.youtube.com/watch?v=vvhC64hQZMk


```
Key Features:

1. Group Messaging

Atmost 200 people can enter a single group

2. Sent + Delivered + Read Receipts
3. Online / Last Seen
4. Image / Video Sharing
5. Chats are temporary / permanent

If we delete the app and the friend also deletes the app, those chat messages are lost forever.
This is done to provide privacy to the users.

1. Group Messaging

a. One to one chat

Single Point Of Failure is important when it comes to whatsapp. 

```

 <img width="753" alt="Screenshot 2023-04-25 at 12 15 15 AM" src="https://user-images.githubusercontent.com/43849911/234087630-86df7f20-a411-4a95-a8ad-1fda6897575c.png">

```
Lets say we want to send messages from user A via gateway 1 to user B via gateway 2. We can have the user to gateway mapping like A to 1.

But this will be expensive as maintaining a TCP connection is itself takes some memory.

Issues:

1. We want to increase the maximum number of connections we want to store in a single box
2. And that information will be duplicated in all three gateway servers via caching mechansim which will be handling it
or there can be a database handling this transient information. There can be a lot of updates going on there. There's a lot of 
coupling present at the system.


We want to make a dumb connection.
```
