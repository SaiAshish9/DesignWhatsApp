https://www.youtube.com/watch?v=vvhC64hQZMk

https://www.enjoyalgorithms.com/blog/design-whatsapp

https://bytebytego.com/courses/system-design-interview/design-a-chat-system

https://www.theverge.com/2016/4/12/11415198/facebook-messenger-whatsapp-number-messages-vs-sms-f8-2016

```
Key Features:

1. Group Messaging

Atmost 200 people can enter a single group

2. Sent + Delivered + Read Receipts
3. Online / Last Seen
4. Image / Video Sharing
5. Chats are temporary / permanent

If we delete the app and the friend also deletes the app, those 
chat messages are lost forever.
This is done to provide privacy to the users.

1. Group Messaging

a. One to one chat

Single Point Of Failure is important when it comes to whatsapp. 

```

 <img width="753" alt="Screenshot 2023-04-25 at 12 15 15 AM" src="https://user-images.githubusercontent.com/43849911/234087630-86df7f20-a411-4a95-a8ad-1fda6897575c.png">

```
Lets say we want to send messages from user A via gateway 1 to user 
B via gateway 2. We can have the user to gateway mapping like A to 1.

But this will be expensive as maintaining a TCP connection is
itself takes some memory.

Issues:

1. We want to increase the maximum number of connections we want 
to store in a single box
2. And that information will be duplicated in all three gateway 
servers via caching mechansim which will be handling it
or there can be a database handling this transient information. 
There can be a lot of updates going on there. There's a lot of 
coupling present at the system.


We want to make a dumb connection.

We can have a session microservices for storing the connections info.
```

<img width="688" alt="Screenshot 2023-04-25 at 12 25 15 AM" src="https://user-images.githubusercontent.com/43849911/234089816-8503a9c1-0191-4596-ac86-70f6f619fe11.png">

```
Session microservice will have that information via a DB or cache storage. 
Hence, single point of Failure is covered here.

Gateway will simply send the message from user and the metadata to the session service.

In this way A can send the message to B.
```

<img width="1061" alt="Screenshot 2023-04-25 at 10 09 52 PM" src="https://user-images.githubusercontent.com/43849911/234345026-fffa2241-05ae-4c1c-88da-57dbecd49a8d.png">

```
At every minute B can ask the gateway for new messages using long polling.

We can implement this over TCP with the help of websockets (wss).
They allow P2P communication, A can send to B and vice versa.

Once the message is sent, user A should be notified that the message has been delivered.

We can have another db for chat messages, once the message is delivered user A will be notified and sent mark 
will appear with the help of gateway 1 and socket server.

Gateways can be used to update the "received" and "sent" statuses.
```

<img width="1085" alt="Screenshot 2023-04-25 at 10 16 35 PM" src="https://user-images.githubusercontent.com/43849911/234346530-aed3f8af-8e01-4f12-b965-ef61fb85abda.png">

```

```

<img width="1064" alt="Screenshot 2023-04-25 at 10 33 36 PM" src="https://user-images.githubusercontent.com/43849911/234350397-095bed58-42a6-46ff-8e36-494c7032077e.png">

```
 3. Online / Last Seen:
 
Second feature is the last seen or is the person online right now.

Lets say B wants to know when A was online the last time.

Whenever A does something, last seen timestamp needs to be updated.

We can have a lastSeen microservice which will do the user activity tracking.

When they send a message to the gateway, lastSeen will be updated.

Two types of messages client will be sending via gateway. One of those two is
User Activity messages and second one is app messages. And accordingly microservice
will update the db.
```

<img width="1280" alt="Screenshot 2023-04-25 at 11 13 08 PM" src="https://user-images.githubusercontent.com/43849911/234358832-5335c094-7302-4639-bec3-5b17ad17d3a8.png">


```
1. Group Messaging

It will be really hard for the session service to handle the group messaging logic, that's why
we can decouple information for who's existing in each group at the group service with the help of route id.

There can be millions of messages getting delivered at each group, hence we need to limit the
number of people at each group as it will be difficult to handle such load. (let's say 200).

We can directly send the electronic message from the user application itself, there's no work
needs to be done at the gateway. Message can later be parsed using a parser & unparser 
microservice.

There can be group id to user id mapping present at the group service level. And that will be a one to
many mapping.

And to lot of duplication in the mappings we've, we can make use of consistent hashing.

It helps to reduce memory split across servers and allows to route request to the right box
based on the group id. If we've requests routed on the group id, we can easily identify the users
of that group.

That takes care of the routing mechanism we've.

In case the group service fails, like we send the message to the box and it faile, we can retry.

We can only retry if we know what request needs to be sent next.

One of the mechanisms for this is the message queue.

Once we give message to the message queue, it ensures that the message will be sent.
May be now, 10s later or 15s later. Those are configurable options and also how many times
we are going to retry , all of these are configurable in the message queue.

If the message queue fails even after 5 retries , it can tell that it's failed and notification
will be sent all the way to the client.

Group receipts will be expensive as everyone needs to view the message.

For Group / Chat Messaging we need :
1. Idempotency
2. Retrial
3. Ordering

Facebook messenger de-priortizes the un-important messages incase there's a huge event
to keep the system health good.

Things like last seen can be ignored during diwali because of the load. RateLimiting
principles comes into play here.

```

```
4. Image / Video Sharing

https://www.youtube.com/watch?v=tndzLznxq40&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=8

```
