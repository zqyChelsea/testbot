This project is a QQ bot based on LLOneBot

* Node version: 22.2.0

* [LLOneBot](https://github.com/LLOneBot/LLOneBot) version: 3.26.4 

Use pnpm for management, installation method:

```bash
npm i -g pnpm
pnpm i
```

Modify configuration in /config/LLOneBotConfig.js

```js
export default {
    mode: "http", // Mode: http(http request + event reporting), WebSocket support in the future
    httpServer: {
        url: "http://127.0.0.1", // Http service address
        port: 3110 // Port
    },
    httpSubmit: { // http reporting port
        port: 31111, // Request port
        urlPath: "/bot/submit" // Request path
    },
    webSocket: {
        
    }
}
```

File naming conventions

- Multiple words use PascalCase
- Single words use lowercase (unless it's a proper noun)

Coding guidelines

- Functions should not modify object parameters, if needed, copy the object first then use it

## HTTP Mode Flow

Server -> Receives message -> Analyzes message -> Publishes message to corresponding category

Processor -> Subscribes to message -> Processes

Subscription content: 

* [Reported messages](https://docs.go-cqhttp.org/event/#%E6%89%80%E6%9C%89%E4%B8%8A%E6%8A%A5)
* [Message types](https://docs.go-cqhttp.org/cqcode/#%E8%BD%AC%E4%B9%89)

```js
{
    ...msg, // msg details see reported messages
    content: "content", // Detect text message content (commands)
    atMe: true, // Whether @ mentions self
}
```

### Writing Plugin Steps

* Create a new plugin package in src/plugins/, write a main program js file
* Modify src/plugins/config.js, add the main program js file to plugins

```js
// test-plugin/app.js
import MsgSubUtil from '../../util/MsgSubUtil.js'

MsgSubUtil.subPrefixR("#", (msg) => {
    return "Received command: " + msg.content;
})
```

```js
export default {
    plugins: ["./test-plugin/app.js"]
}
```

## TODO

* Parameter validation
* Subscription timeout validation
