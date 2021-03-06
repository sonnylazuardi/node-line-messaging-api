# node-line-messaging-api

> Unofficial SDK for Line's Messaging API

```sh
npm install --save node-line-messaging-api
```

# Usage

```js
import Bot, { Messages } from 'node-line-messaging-api'

const SECRET = 'YOURSECRETHERE' // Line@ APP SECRET

const TOKEN = 'YOURTOKENHERE' // Line@ issued TOKEN

const PORT = process.env.PORT || 3002

let bot = new Bot(SECRET, TOKEN, { webhook: { port: PORT } })

// bot webhook succesfully started
bot.on('webhook', w => console.log(`bot listens on port ${w}.`))

// on ANY events
bot.on('events', e => console.dir(e))

// on Message event
bot.on('message', m => console.log(`incoming message: ${m.message}`))

let msgs = new Messages()

msgs.addText('HELLO WORLD!').addText({text: 'harambe4lyf'})

bot.pushMessage('CHANNELXXXXXXXX', msgs.addText('FOO BAR BAZ').commit()) // HELLO WORLD! -- harambe4lyf -- FOO BAR BAZ

```

There are some other events from `examples/` (WIP).

## Events

Listen-able events (WIP):

```js
const _webhook = 'webhook' // emitted when webhook listener is created successfully

const _events = 'events' // emitted on all events, returns an array of events.

const _eventTypes = ['message', 'follow', 'unfollow', 'join', 'leave', 'postback', 'beacon'] // emitted on parsing event types, returns that specific event.

const _messageTypes = ['text', 'image', 'video', 'audio', 'location', 'sticker', 'non-text', 'message-with-content'] // emitted on parsing message types (more specific), returns that specific event (type === 'message').


```


## Webhook

By default, webhook will listen on port `5463`. You should change it if it interferes with something.

## Messages

`Messages` class is a helper in composing your messages.

# API

## LineBot


- `LineBot`

  - `new LineBot(secret, token, options)`

  - `.on(event, function callback (eventContent) {})` // Standard event listener. Events are shown above.

  - `.onText(regexp, function callback (event, match) {})` // Executes callback everytime a message.text matches regexp

  - `.replyMessage(replyToken, messages)` => Promise

  - `.pushMessage(channel, messages)` => Promise

  - `.getContent(messageId)` => Promise

  - `.getProfile(userId)` => Promise

  - `.leaveChannel({groupId, roomId})` => Promise //pick one between groupId or roomId

- `Messages`

  - `new Messages()` // creates an empty array of messages. Maximum 5 messages following LINE's spec.

  - `.addRaw(message)` // message Object following LINE's spec

  - `.addText(message)` // message may be a string or an object with .text property. Chainable.

  - `.addImage({originalUrl, previewUrl})` // `originalUrl` and `previewUrl` must be a HTTPS link to a JPG or PNG image. (size < 1MB, width < 1024px)

  - `.addAudio({originalUrl, duration})` // `originalUrl` must be a HTTPS link to m4a audio. `duration` is in milliseconds.

  - `.addVideo({originalUrl, previewUrl})` // same as `.addImage` but mp4 < 10MB for the originalUrl.

  - `.addLocation({title = 'My Location', address = 'Here\'s the location.', latitude, longitude})` // lat and lon is of `number` type

  - `.addSticker({packageId, stickerId})` // both params are of number type. see https://devdocs.line.me/files/sticker_list.pdf

  - `.addButtons({thumbnailImageUrl, altText, title, text, actions})` // Buttons template message. `actions` must follow Action template. length of `actions` <=4

  - `.addConfirm({altText, text, actions})` // confirmation type. actions max length = 2

  - `.addVideo, etc` (WIP)

  - `.commit()` // returns the payload (array of messages)

# Installation

```
npm install --save node-line-messaging-api
```

or

```
yarn add node-line-messaging-api
```

# Deployment

1. Prepare your cloud host, note IP Address.

2. Provide HTTPS support for the webhook endpoint.

3. Register for an account in business.line.me, choose Messaging API and then dev-trial.

4. Open developers.line.me for your account, note the `APP_SECRET`.

5. Issue a `TOKEN` and note it.

6. Go to server whitelist, add your IP Address.

7. Create your bot, input your `APP_SECRET` and `TOKEN`.

8. Deploy to your cloud host, and wait for events to come in!

# Contributing

**PRs welcome**. Open Issues first. ;)


# License

The MIT License (MIT)

Copyright &copy; 2016 [Muhammad Mustadi](https://mustadi.xyz)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
