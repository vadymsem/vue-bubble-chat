# vue-bubble-chat

Amesome & simple vue chat component

## Installation

```bash
npm install --save vue-bubble-chat
```

## Usage

```vue
import Vue from "vue";
import BubbleChat from "vue-bubble-chat";

Vue.use(BubbleChat);
```

-----------------------------------------------------------------------
### Params
**:position** - { type: string, default: 'right' }

*horizontal position on your page, receives 'left' or 'right' values*

**:messages** - { type: array, default: [] }

*objects array with all your chat messages*

**:text-field** - { type: string }

*name of key which is equel to your message*

**:sender-name-field** - { type: string }

*name of key which is equel to user's name*

**:avatar-link-field** - { type: string }

*name of key which is equel to user's avatar link; by default it will be simple circle with the first letter of user's name*

-----------------------------------------------------------------------

### Events

**@onMessageSent**

*return an message which just was sent from chat textarea*

**@onMessageReceive**

*return an object like {messages: /here all array of the chat messages/}*

-----------------------------------------------------------------------

## Example
```vue
<template>
   <bubble-chat
     :position="chatPosition"
     :messages="messagesList"
     :text-field="'message'"
     :sender-name-field="'username'"
     :avatar-link-field="'iconUrl'"
   >
   </bubble-chat>
</template>

<script>
 ...
 data() {
   return {
     chatPosition: 'right',
     messagesList: [
      {
          username: 'John Doe',
          message: 'Hey guys! How are you?'
          iconUrl: 'https://www.link-to-john-doe-pic.jpg'
      }
     ]
    }
 }

...
<script>
```

### One more thing
-----------------------------------------------------------------------
To have an opportunity of identification own messages, you can add one more key to *messagesList* named *isMine* which should be Boolean. It will make that message in chat with light blue background and left aligned.

Enjoy!

## License
[MIT](https://choosealicense.com/licenses/mit/)