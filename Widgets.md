# Beeper Widgets

Beeper Widgets are webpages that live in the sidebar of Beeper Desktop. They can access data from a chat in Beeper, such as messages and a list of users. They can also take actions, such as sending messages on the user's behalf.

![summarizer.png](media%2Fsummarizer.png)

Since Beeper is built on top of the Matrix protocol, and Beeper Desktop is a (distant) fork of Element, we were able to use the [Matrix widget api](https://github.com/beeper/matrix-widget-api) and Nordeck's [Matrix Widget Toolkit](https://github.com/beeper/matrix-widget-toolkit) as the foundation for Beeper Widgets. We've added a few improvements and created several widgets. One major difference is that in Beeper widgets are available globally alongside each of your chats, and are not shared with everyone in the chat.

Widgets currently are only supported in Beeper Desktop.

## Currently available widgets

Built a cool widget for Beeper? Create a pull request to add your widget to this readme!

| Name | Description | Created By | Link | Code |
| ---- | ----------- | ---------- | ---- | ---- |
| Example Widget | Showcases what you can do with widgets. | Beeper | https://example.beeper.vercel.app | https://github.com/beeper/widget-example |
| Summarizer | Summarizes all unread messages in chat using [Anthropic Claude](https://docs.anthropic.com/claude/reference/getting-started-with-the-api) AI. | Beeper | https://summarizer.beeper.vercel.app | https://github.com/beeper/widget-summarizer |
| Do It | It's like a button you can press that guesses what you need (information, guidance, etc) and gives it to you. Uses the OpenAI API. | Beeper | https://do-it.beeper.vercel.app | https://github.com/beeper/widget-do-it |

## What can Beeper Widgets do?

| Widget Capabilities                           |
|-----------------------------------------------|
| Read + send messages in a chat                |
| Read + send reactions in a chat               |
| Delete messages in a chat                     |
| Read the list of participants                 |
| See which message was last-read by the user   |
| Change the name of a chat (Matrix chats only) |

### Permissions
Widgets must request permission to take action in your chats. Each capability is requested separately, so users know up-front which capabilities they are granting to each widget. For example, you can create a widget that only has read access to a chat (eg Summarizer), or only send access (eg pre-canned message sender).  

### With the Beeper Widget API, you can build widgets that could do things like:
- Send pre-canned messages
- Keep track of notes per contact
- Show info about everyone in the chat, using a data source like Clearbit
- Show the Twitter/Instagram feed of everyone in a chat
- Display your calendar
- Add things from chat to your to-do list

## Install a Beeper Widget

1. Click the settings gear at the top-left of Beeper Desktop, then press "Settings"
2. In the sidebar, click "Labs" and enable "Show widgets in chat view"
3. In one of your chats, click "i" on the top-right, then press "Add widget". Enter the URL of the widget and give it a name.
- If you'd just like to try out a widget, type in `https://example.beeper.vercel.app` for the demo widget. Otherwise, type in the URL of the widget you're trying to install.
4. That's all! Click your widget in the sidebar to open it.

https://github.com/beeper/sdk-docs/assets/61036090/e6eb4f78-1931-45a0-9b5e-f5685a2aaccf

## Build your own widget in 5 minutes

Follow these instructions to create a boilerplate example widget using [NextJS](https://nextjs.org/) that you can then customize.

1. Install dependencies
   1. If you haven't already, install the latest LTS or Current version of Node.js. 
   2. Install `yarn`: `npm install --global yarn`
2. Clone the repo. Type the following into your terminal:
    ```bash
    npx create-next-app --example https://github.com/beeper/widget-example --use-yarn --app
    ```
   1. You'll be prompted "Ok to proceed? (y)". Type "y" then press enter
   2. It'll ask for your project name. Choose any name you want. This will be the name of the folder it creates on your computer.
   3. Wait for the packages to install. Then, in your terminal, `cd` into the folder you just created, then type `yarn dev`.
3. In Beeper Desktop, search for your "Note to self" chat and open it up so that you can test out your widget. Alternatively, you can also make your own Matrix chat by clicking "Start New Chat" -> "Create group chat" -> "Beeper (Matrix)" and name it something like 'Widget test'.
4. Follow the instructions in [Install a Beeper Widget](#install-a-beeper-widget), and type `http://localhost:3000` as the URL.
5. Wait a couple of seconds for NextJS to compile the page. You'll then see the example widget! Click through it to explore some of the functionality, and open it in your code editor to customize and create your own widget.

### Notes for development

- Your example project has the foundational code for building Beeper Widgets. We recommend building your widget directly on the codebase.
- The codebase is in [NextJS](https://nextjs.org/), with inbuilt support for [Tailwind CSS](https://tailwindcss.com/) and [CSS Modules](https://nextjs.org/docs/app/building-your-application/styling/css-modules). You can also use [Sass](https://nextjs.org/docs/app/building-your-application/styling/sass).
  - If you haven't used NextJS, just know that you can write normal React code. NextJS is a layer on top that improves the developer experience through various features.
  - You must use React, but you don't need to use NextJS. If there's another framework you prefer, use the code inside of the example project as a reference.
- `app/page.tsx` is rendered when you open your widget.
- In order to access data inside of a chat, you'll need to add code inside your widget to request permissions. In `page.tsx`, there's an example of that; just modify the `capabilities` array to suit your needs.
- We recommend [Vercel](https://vercel.com/) for deploying your widget so others can use it. Push your code to a GitHub repo, create an app in Vercel, and add the widget in Beeper Desktop using the Vercel URL.

# API Documentation

## Background Info on Matrix

The Matrix chat protocol consists of `events`. Events represent data inside a chat, such as a message, reaction, or list of participants. Each chat in Matrix is called a `room`. Events are split into two types: room events and state events.

In Beeper, the list of chat messages in a room is called a `timeline`. Room events represent events inside of that timeline that happen one after another. For example, messages being sent, messages being deleted, or reactions being applied. State events represent the current state of a room, for example, the title and the list of participants. State events have a single current value.

Room account data is JSON data stored inside of each Beeper account, inaccessible by other users. Inside of room account data, Beeper stores the indicator of which message has been last read, and whether the chat has been marked done, and whether it has been marked unread.

## Widget API Info

The code from the example widget will have already set up the fundamentals, so most of the API-related code you'll write will be based on `useWidgetApi` from [@beeper/matrix-widget-toolkit-react](https://github.com/beeper/matrix-widget-toolkit/tree/main/packages/react).

You'll create an instance of a `WidgetApi` by calling `useWidgetApi()`, and can then call various methods on it.

Methods called on a WidgetApi instance return promises, so they should be called using either async/await or .then().


In code, it looks like this:
```javascript
import { useWidgetApi } from "@beeper/matrix-widget-toolkit-react";

// Use inside of a React component
const widgetApi = useWidgetApi();
```


For example:

```javascript
import { useWidgetApi } from "@beeper/matrix-widget-toolkit-react";
import { useEffect, useState } from "react";
import { RoomEvent } from "@beeper/matrix-widget-toolkit-api";

export default function Home() {

    const [message, setMessage] = useState("")
    const widgetApi = useWidgetApi()
    
    async function fetchData(event: any) {
        event.preventDefault();
        setMessage("");

        await widgetApi.sendRoomEvent('m.room.message', {
            msgtype: 'm.text',
            body: message,
        });
    }
    
    return (
        <form onSubmit={fetchData}>
            <input value={message} onChange={(event) => setMessage(event.target.value)} />
        </form>
    );
}
```

### Types Info (for TypeScript)

`RoomEvent`: `import { RoomEvent } from "@beeper/matrix-widget-toolkit-api";`

```javascript
RoomEvent<T>:

{
    type: string;
    sender: string;
    event_id: string;
    room_id: string;
    origin_server_ts: number;
    content: <T>;
}
```

`StateEvent`: `import { StateEvent } from "@beeper/matrix-widget-toolkit-api"`

```javascript
StateEvent<T>:

{
    type: string;
    sender: string;
    event_id: string;
    room_id: string;
    origin_server_ts: number;
    state_key: string;
    content: T;
}
```

`RoomAccountData`: `import { RoomAccountData } from "@beeper/matrix-widget-toolkit-api";`

```javascript
RoomAccountData<T>:

{
    type: string;
    room_id: string;
    content: T;
}
```

## Room Events

### Receiving

```javascript
const events: RoomEvent<any>[] = await widgetApi.receiveRoomEvents(eventType, {
    messageType?: string;
    limit?: number;
    roomIds?: string[] | Symbols.AnyRoom;
    since?: string | undefined;
});
```

eventType (string): the "m.type" key in the JSON representing the event you want to get. Each room event inside of a Matrix client has its own "m.type" key. Examples:

- messages: "m.room.message"
- reaction: "m.reaction"
- roomIds: array of ids of other rooms to get events in. Symbols.AnyRoom is "*". Not needed if you only want events from the room currently open

You'll need to request permissions for the corresponding "eventType", like so:
```javascript
WidgetEventCapability.forRoomEvent(
    EventDirection.Receive,
    'm.room.message' // replace with the eventType you requested
),
```

Response format:
```json
[
    {
        "content": {
            ...
        },
        "origin_server_ts": 1689883910958,
        "room_id": "!jqIPEzQnHoxVKgBgVM:beeper.com",
        "sender": "@griffin:beeper.com",
        "type": "m.reaction",
        "unsigned": {
            "age": 1228375,
            "transaction_id": "m1689883910662.65",
            "com.beeper.hs.order": 720911906
        },
        "event_id": "$jbXMaSmOJvgJa7oFaMjlr8RxHIc39hnfnDxqnCiaTd4",
        "user_id": "@griffin:beeper.com",
        "age": 1228375
    }
]
```

`content` varies depending on the `eventType`. Examples:

m.room.message:
```json
"content": {
    "msgtype": "m.text",
    "body": "hello there",
    "com.beeper.linkpreviews": [],
    "com.beeper.origin_client_type": "desktop",
    "_isEmojiBody": null,
    "com.beeper.origin_client_ts": 1689883904990,
    "com.beeper.origin_client_version": "3.66.1"
},
```

m.reaction:
```json
"content": {
    "com.beeper.origin_client_ts": 1689883910662,
    "com.beeper.origin_client_type": "desktop",
    "com.beeper.origin_client_version": "3.66.1",
    "com.beeper.reaction.shortcode": ":heart:",
    "m.relates_to": {
        "event_id": "$ul_WqBU0XMWTyS773XAZtX2KPRjRVyt6ba4xhxDmf7A",
        "key": "❤️",
        "rel_type": "m.annotation"
    }
},
```

`since` (string) is an optional parameter used when fetching `m.room.message` events. It fetches only messages sent after a certain message, which you specify. Pass in the message's eventId as a string. For example, to get only unread messages, get `m.fully_read` from room account data, then set that as the `since` parameter.

### Sending

```javascript
const events: RoomEvent<any>[] = await widgetApi.receiveRoomEvents(eventType, {
    messageType?: string;
    limit?: number;
    roomIds?: string[] | Symbols.AnyRoom;
    since?: string | undefined;
});

await widgetApi.sendRoomEvent(eventType: string, content: {
  ...  
});
```

The `content` parameter varies depending on the eventType. For example:

```javascript
await widgetApi.sendRoomEvent('m.room.message', {
    msgtype: 'm.text',
    body: "Hello everyone!",
});
```

```javascript
await widgetApi.sendRoomEvent('m.room.redaction', {
    redacts: "$ul_WqBU0XMWTyS773XAZtX2KPRjRVyt6ba4xhxDmf7A" // this is the eventId
});
```

To send an event in another room, specify the roomId in `roomIds[]`. Not needed if you want to send an event in the currently-viewed room.

As usual, you'll need to request permissions:
```javascript
WidgetEventCapability.forRoomEvent(
    EventDirection.Send,
    'm.room.redaction' // replace with the eventType you're using
)
```
## State Events

### Receiving

```javascript
const events: StateEvent<any>[] = await widgetApi.receiveStateEvents(eventType, {
    stateKey, // Optional: string
    roomIds, // Optional: string[] | Symbols.AnyRoom
});
```

eventType (string): the "m.type" key of the JSON representing the state event you want. Each state event inside of a Matrix client has its own "m.type". Examples:

- room members: "m.room.member"
- room name: "m.room.name"

stateKey: to get the value of something (for example, the room name) at a certain point in time, provide the stateKey (as a string) corresponding to the state event. For example, if someone changed the room name but you'd like the previous one, provide the previous state key.

roomIds: an array of other rooms to get state events from. Not needed for getting data from the current room. Use Symbols.AnyRoom (which is "*") to get the state event from all of the user's rooms.

### Sending

This is most useful if you're in a Matrix chat (eg. if you're in a Beeper-Beeper chat room). If you're using Beeper to chat on another network, your changes might not show there (for example, changing a room name in a WhatsApp chat using a Widget doesn't change it in WhatsApp, but rather only inside your Beeper client). 

```javascript
await widgetApi.sendStateEvent(eventType, content, {
    stateKey, // Optional: string
    roomId, // Optional: string
});
```

## Room Account Data

The primary use for room account data when developing Beeper Widgets is probably getting the `m.fully_read` indicator. This is a string representing the most recent message that the user has seen. 

Room account data can currently only be read from; you can't write to it.

```javascript
const data = await widgetApi.receiveRoomAccountData(eventType, {
    roomIds // Optional: string[] | Symbols.AnyRoom;
});
```

For example, to get only unread messages:

```javascript
const fullyReadData: RoomAccountData<any>[] = await widgetApi.receiveRoomAccountData('m.fully_read');
const fullyRead: string | undefined = fullyReadData[0].content.event_id;
roomEvents = await widgetApi.receiveRoomEvents('m.room.message', {limit: limit, since: fullyRead});
```

Permissions:

```javascript
WidgetEventCapability.forRoomAccountData(
    EventDirection.Receive,
    'm.fully_read' // Replace with the eventType you're using
)
```

## Permissions for other rooms

To request permissions for data in all rooms, use `org.matrix.msc2762.timeline:*` as a capability request. 

For example:

```javascript
<MuiCapabilitiesGuard
    capabilities={[
        WidgetEventCapability.forStateEvent(
            EventDirection.Receive,
            'm.room.name'
        ),
        WidgetEventCapability.forRoomEvent(
            EventDirection.Receive,
            'm.room.message'
        ),
        'org.matrix.msc2762.timeline:*',
    ]}
>
```

The user will be asked whether they want to grant the widget access to all rooms, if you specify this parameter. 

**Most Beeper Widgets won't need this capability.** People can open normal Beeper Widgets within every room. This should only be used if you want to use data from multiple rooms at once in your widget.

# Credits

Thanks to [Nordeck](https://github.com/nordeck) for their wonderful library [matrix-widget-toolkit](https://github.com/nordeck/matrix-widget-toolkit) and Matrix for [matrix-widget-api](https://github.com/matrix-org/matrix-widget-api). Beeper Widgets are built on these libraries, plus some additional modifications for extra functionality.
