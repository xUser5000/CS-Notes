# Explanation
- Channels wraps Django’s native asynchronous view support, allowing Django projects to handle not only HTTP, but protocols that require long-running connections too - WebSockets, MQTT, chatbots, amateur radio, and more.
- It does this while preserving Django’s synchronous and easy-to-use nature, allowing you to choose how you write your code - synchronous in a style like Django views, fully asynchronous, or a mixture of both. On top of this, it provides integrations with Django’s auth system, session system, and more, making it easier than ever to extend your HTTP-only project to other protocols.
- Channels also bundles this event-driven architecture with _channel layers_, a system that allows you to easily communicate between processes, and separate your project into different processes.

# Scopes and Events
- The _scope_ is a set of details about a single incoming connection - such as the path a web request was made from, or the originating IP address of a WebSocket, or the user messaging a chatbot. The scope persists throughout the connection.
- For HTTP, the scope just lasts a single request. For WebSockets, it lasts for the lifetime of the socket (but changes if the socket closes and reconnects). For other protocols, it varies based on how the protocol’s ASGI spec is written; for example, it’s likely that a chatbot protocol would keep one scope open for the entirety of a user’s conversation with the bot, even if the underlying chat protocol is stateless.
- During the lifetime of this _scope_, a series of _events_ occur. These represent user interactions - making a HTTP request, for example, or sending a WebSocket frame. Your Channels or ASGI applications will be **instantiated once per scope**, and then be fed the stream of _events_ happening within that scope to decide what action to take.

# Consumers
- A consumer is the basic unit of Channels code. We call it a _consumer_ as it _consumes events_, but you can think of it as its own tiny little application. When a request or new socket comes in, Channels will follow its routing table - we’ll look at that in a bit - find the right consumer for that incoming connection, and start up a copy of it.
- This means that, unlike Django views, consumers are long-running. They can also be short-running - after all, HTTP requests can also be served by consumers - but they’re built around the idea of living for a little while (they live for the duration of a _scope_, as we described above).
- A basic consumer looks like this:

```python
class ChatConsumer(WebsocketConsumer):

    def connect(self):
        self.username = "Anonymous"
        self.accept()
        self.send(text_data="[Welcome %s!]" % self.username)

    def receive(self, *, text_data):
        if text_data.startswith("/name"):
            self.username = text_data[5:].strip()
            self.send(text_data="[set your username to %s]" % self.username)
        else:
            self.send(text_data=self.username + ": " + text_data)

    def disconnect(self, message):
        pass
```

# Channel Layers
- A channel layer is a kind of communication system. It allows multiple consumer instances to talk with each other, and with other parts of Django.
- A channel layer provides the following abstractions:
	- A **channel** is a mailbox where messages can be sent to. Each channel has a name. Anyone who has the name of a channel can send a message to the channel.
	- A **group** is a group of related channels. A group has a name. Anyone who has the name of a group can add/remove a channel to the group by name and send a message to all channels in the group. It is not possible to enumerate what channels are in a particular group.
- Every consumer instance has an automatically generated unique channel name, and so can be communicated with via a channel layer.
- Example Code:
```python
import json

from asgiref.sync import async_to_sync
from channels.generic.websocket import WebsocketConsumer

class ChatConsumer(WebsocketConsumer):
    def connect(self):
        self.room_name = self.scope["url_route"]["kwargs"]["room_name"]
        self.room_group_name = f"chat_{self.room_name}"

        # Join room group
        async_to_sync(self.channel_layer.group_add)(
            self.room_group_name, self.channel_name
        )

        self.accept()

    def disconnect(self, close_code):
        # Leave room group
        async_to_sync(self.channel_layer.group_discard)(
            self.room_group_name, self.channel_name
        )

    # Receive message from WebSocket
    def receive(self, text_data):
        text_data_json = json.loads(text_data)
        message = text_data_json["message"]

        # Send message to room group
        async_to_sync(self.channel_layer.group_send)(
            self.room_group_name, {"type": "chat.message", "message": message}
        )

    # Receive message from room group
    def chat_message(self, event):
        message = event["message"]

        # Send message to WebSocket
        self.send(text_data=json.dumps({"message": message}))
```

- Every consumer has a [scope](https://channels.readthedocs.io/en/latest/topics/consumers.html#scope) that contains information about its connection, including in particular any positional or keyword arguments from the URL route and the currently authenticated user if any.
- Group names are restricted to ASCII alphanumerics, hyphens, and periods only and are limited to a maximum length of 100 in the default backend. Since this code constructs a group name directly from the room name, it will fail if the room name contains any characters that aren’t valid in a group name or exceeds the length limit.
- An event has a special `'type'` key corresponding to the name of the method that should be invoked on consumers that receive the event. This translation is done by replacing `.` with `_`, thus in this example, `chat.message` calls the `chat_message` method.

# Sources
- [Introduction - Channels](https://channels.readthedocs.io/en/latest/introduction.html)