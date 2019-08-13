<template>
  <div class="bubble_chat_wrapper" :style="{[position]: 0}">
    <transition name="slide">
      <div class="bubble_chat_wrapper__rolled_up" v-show="!windowMode">
        <div class="messages_list">
          <div
            class="messages_list__item"
            v-for="(msgItem, index) in activeMessages"
            @click="openChat"
            :key="index + '_bubbles'"
            :class="{'hiding-out': index === 0 && messageUpdating, 
                        'showing-in' : index === 2 && messageUpdating,
                        'shake-it': index === 1 && messageUpdating}"
          >
            <div class="messages_list__item___sender">
              <img
                v-if="msgItem[avatarLinkField]"
                :src="msgItem[avatarLinkField]"
                :alt="msgItem[senderNameField]"
              />
              <span v-if="!msgItem[avatarLinkField]">
                  {{msgItem[senderNameField] ? msgItem[senderNameField].charAt(0).toUpperCase() : '?' }}
                </span>
            </div>
            <div class="messages_list__item___message">
              <span class="name">{{msgItem[senderNameField]}}</span>
              <span>{{msgItem[textField]}}</span>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <transition name="slide">
      <div class="bubble_chat_wrapper__rolled_down" v-show="windowMode">
        <div class="bubble_chat_wrapper__rolled_down__list" ref="scrollWrapper">
          <div class="scroll">
            <div v-for="(msgObj, index) in fullMesagesList" :key="index" :class="{'mine': msgObj.isMine}">
                <div>
                    <img v-if="msgObj[avatarLinkField]" :src="msgObj[avatarLinkField]" alt="avatar" />
                    <span class="no_image" v-if="!msgObj[avatarLinkField]">
                        {{msgObj[senderNameField] ? msgObj[senderNameField].charAt(0).toUpperCase() : '?' }}
                    </span>
                    <span class="name">{{msgObj[senderNameField]}}</span>
                    <span class="msg">{{msgObj[textField]}}</span>
                </div>
            </div>
          </div>
        </div>
        <div class="bubble_chat_wrapper__rolled_down__input">
          <textarea 
            v-model="newMessageText"
            placeholder="Use 'Shift + Enter' to send" 
            v-on:keyup.enter.shift="sendMessage"
          ></textarea>
        </div>
      </div>
    </transition>

    <div class="bubble_chat_wrapper__control" @click="openChat">
      <span class="new_mesages" v-show="!windowMode && unredMessagesCount > 0">{{unredMessagesCount}}</span>
      <svg
        v-if="!windowMode"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        version="1.1"
        id="Capa_1"
        x="0px"
        y="0px"
        viewBox="0 0 524.184 524.184"
        style="enable-background:new 0 0 524.184 524.184;fill: #fff;-webkit-transform: scale(.6);-ms-transform: scale(.6);transform: scale(.6);"
        xml:space="preserve"
      >
        <g>
          <path
            d="M483.606,75.31H40.542C18.34,75.31,0,93.65,0,115.852v292.48c0,22.201,18.34,40.542,40.542,40.542h443.064   c22.201,0,40.542-18.34,40.542-40.542v-292.48C525.113,93.65,506.772,75.31,483.606,75.31z M332.057,273.193L500.981,142.88   v261.591L332.057,273.193z M41.507,99.442h442.098c7.722,0,14.479,5.792,16.41,12.549L276.07,285.741   c-8.688,6.757-23.167,6.757-30.889,0L24.132,113.921C26.063,106.199,32.82,99.442,41.507,99.442z M24.132,404.471V144.81   l166.994,129.348L24.132,404.471z M41.507,426.672c-1.931,0-3.861,0-4.826-0.965l174.716-136.105l19.306,15.444   c8.688,6.757,19.306,9.653,29.924,9.653c10.618,0,22.201-2.896,29.924-9.653l21.236-16.41l176.646,137.07   c-1.931,0.965-2.896,0.965-4.826,0.965H41.507z"
          />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
        </g>
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
      </svg>

      <svg
        v-if="windowMode"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        version="1.1"
        id="Capa_1"
        x="0px"
        y="0px"
        viewBox="0 0 47.971 47.971"
        style="enable-background:new 0 0 47.971 47.971;fill: #fff;
    -webkit-transform: scale(.3);
        -ms-transform: scale(.3);
            transform: scale(.3);"
        xml:space="preserve"
      >
        <g>
          <path
            d="M28.228,23.986L47.092,5.122c1.172-1.171,1.172-3.071,0-4.242c-1.172-1.172-3.07-1.172-4.242,0L23.986,19.744L5.121,0.88   c-1.172-1.172-3.07-1.172-4.242,0c-1.172,1.171-1.172,3.071,0,4.242l18.865,18.864L0.879,42.85c-1.172,1.171-1.172,3.071,0,4.242   C1.465,47.677,2.233,47.97,3,47.97s1.535-0.293,2.121-0.879l18.865-18.864L42.85,47.091c0.586,0.586,1.354,0.879,2.121,0.879   s1.535-0.293,2.121-0.879c1.172-1.171,1.172-3.071,0-4.242L28.228,23.986z"
          />
        </g>
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
        <g />
      </svg>
    </div>

  </div>
</template>

<script>
export default {
  name: "BubbleChat",
  props: {
    position: {
      type: String,
      default: "right",
      validator: value => value === "right" || value === "left"
    },
    messages: {
      type: Array,
      default: () => [],
    },
    textField: {
      type: String,
      default: 'text'
    },
    senderNameField: {
      type: String,
      default: 'name'
    },
    avatarLinkField: {
      type: String
    }
  },
  data() {
    return {
      messageUpdating: false,
      windowMode: false,
      newMessageText: '',
      fullMesagesList: [],
      checkedMessages: 0
    };
  },
  computed: {
    notMineMessages() {
      return this.fullMesagesList.filter((msg) => !msg.isMine).length
    },
    unredMessagesCount() {
      return this.notMineMessages - this.checkedMessages
    },
    activeMessages() {
      if (this.windowMode) return

      return this.fullMesagesList.length > 3
        ? [...this.fullMesagesList.slice(Math.max(this.fullMesagesList.length - 3, 1))].reverse()
        : [...this.fullMesagesList].reverse()
    }
  },
  methods: {
    sendMessage() {
        const msg = Object.assign({}, {
                isMine: true,
                [this.$props.textField]: this.newMessageText, 
                [this.$props.senderNameField]: 'me'
            }
        )
        this.fullMesagesList.push(msg)
        this.newMessageText = ''
    },
    scrollDownChat() {
        if(this.windowMode) {
            setTimeout(() => {
              this.$refs.scrollWrapper.scrollTop = this.$refs.scrollWrapper.scrollHeight
            }, 100)
        }
    },
    openChat() {
      this.checkedMessages = this.notMineMessages
      this.windowMode = !this.windowMode;
      this.scrollDownChat()
    }
  },
  created() {
    this.fullMesagesList = this.$props.messages;
  },
  watch: {
    'fullMesagesList': function(e) {
      this.messageUpdating = true;
      this.$emit('chatWasUpdated', {chatState: this.fullMesagesList })
      setTimeout(() => {
        this.messageUpdating = false;
      }, 200);
      this.scrollDownChat()
    }
  },
};
</script>

<style>

@-webkit-keyframes bubble-show-up {
    from {
        -webkit-transform: translate(100%, 100%);
                transform: translate(100%, 100%);
        opacity: 0;
    }

    to {
        -webkit-transform: translate(0%, 0%);
                transform: translate(0%, 0%);
        opacity: 1;
    }
}
@keyframes bubble-show-up {
    from {
        -webkit-transform: translate(100%, 100%);
                transform: translate(100%, 100%);
        opacity: 0;
    }

    to {
        -webkit-transform: translate(0%, 0%);
                transform: translate(0%, 0%);
        opacity: 1;
    }
}



.slide {
  -webkit-backface-visibility: hidden;
          backface-visibility: hidden;
  z-index: 1;
}

/* moving */
.slide-move {
  -webkit-transition: all 300ms ease-in-out 25ms;
  -o-transition: all 300ms ease-in-out 25ms;
  transition: all 300ms ease-in-out 25ms;
}

/* appearing */
.slide-enter-active {
  -webkit-transition: all 200ms ease-out;
  -o-transition: all 200ms ease-out;
  transition: all 200ms ease-out;
}

/* disappearing */
.slide-leave-active {
  -webkit-transition: all 100ms ease-in;
  -o-transition: all 100ms ease-in;
  transition: all 100ms ease-in;
  position: absolute;
  z-index: 0;
}

/* appear at / disappear to */
.slide-enter,
.slide-leave-to {
  opacity: 0;
}

@-webkit-keyframes hideMessage {
  from {
    opacity: 1;
    bottom: 100px;
    bottom: 85px;
  }
  to {
    opacity: 0;
    bottom: 130px;
    bottom: 285px;
  }
}

@keyframes hideMessage {
  from {
    opacity: 1;
    bottom: 100px;
    bottom: 85px;
  }
  to {
    opacity: 0;
    bottom: 130px;
    bottom: 285px;
  }
}

@-webkit-keyframes shaking {
  from {
    background-color: rgba(255, 244, 149, 0.2);
    opacity: 0;
  }
  to {
    background-color: rgba(255, 255, 255, 1);
    opacity: 1;
  }
}

@keyframes shaking {
  from {
    background-color: rgba(255, 244, 149, 0.2);
    opacity: 0;
  }
  to {
    background-color: rgba(255, 255, 255, 1);
    opacity: 1;
  }
}

@-webkit-keyframes showMessage {
  from {
    opacity: 0;
    bottom: -50px;
    right: -300px;
  }
  to {
    opacity: 1;
    bottom: 0px;
    right: 0px;
  }
}

@keyframes showMessage {
  from {
    opacity: 0;
    bottom: -50px;
    right: -300px;
  }
  to {
    opacity: 1;
    bottom: 0px;
    right: 0px;
  }
}

.shake-it {
  -webkit-animation: shaking 0.2s ease-in;
          animation: shaking 0.2s ease-in;
}

.hiding-out {
  -webkit-animation: hideMessage 0.2s ease-out;
          animation: hideMessage 0.2s ease-out;
}

.showing-in {
  -webkit-animation: showMessage 0.2s ease-in;
          animation: showMessage 0.2s ease-in;
}

html,
body {
  min-width: 100%;
  min-height: 100%;
  margin: 0;
  padding: 0;
}

.bubble_chat_wrapper,
.bubble_chat_wrapper * {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
}

.bubble_chat_wrapper {
  position: fixed;
  padding: 5px;
  z-index: 16777271;
  bottom: 0;
  right: 0px;
  width: 320px;
  height: 400px;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: reverse;
      -ms-flex-direction: column-reverse;
          flex-direction: column-reverse;
  -webkit-box-align: end;
      -ms-flex-align: end;
          align-items: flex-end;
}

/* bubble mode */
.bubble_chat_wrapper .bubble_chat_wrapper__rolled_up {
  position: fixed;
  bottom: 60px;
}

.bubble_chat_wrapper .bubble_chat_wrapper__rolled_up .messages_list {
  width: 320px;
  height: 200px;
  position: relative;
}

.bubble_chat_wrapper .bubble_chat_wrapper__rolled_up .messages_list__item {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: space-between;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  min-width: 100%;
  width: 100%;
  padding: 5px 10px;
  background-color: #d6d6d6;
  max-width: 100%;
  border-radius: 10px;
  background-color: #fff;
  -webkit-box-shadow: 0 0 20px rgba(208, 208, 208, 0.3);    
  box-shadow: 0 0 20px rgba(208, 208, 208, 0.3);
  height: 100px;
  -webkit-transition: 0.2s;
  -o-transition: 0.2s;
  transition: 0.2s;
  z-index: 1;
  position: absolute;
  bottom: 0;
  overflow: hidden;
}
.bubble_chat_wrapper .bubble_chat_wrapper__rolled_up .messages_list__item::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    background-image: -webkit-gradient(linear, left top, left bottom, color-stop(75%, transparent), color-stop(80%, rgba(255, 255, 255, .5)), color-stop(98%, rgb(255, 255, 255, 1)));
    background-image: -o-linear-gradient(top, transparent 75%, rgba(255, 255, 255, .5) 80%, rgb(255, 255, 255, 1) 98%);
    background-image: linear-gradient(180deg, transparent 75%, rgba(255, 255, 255, .5) 80%, rgb(255, 255, 255, 1) 98%);
    z-index: 10;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item:nth-of-type(3) {
  bottom: 85px;
  z-index: 1;
  -webkit-transform: scale(0.8);
      -ms-transform: scale(0.8);
          transform: scale(0.8);
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item:nth-of-type(2) {
  bottom: 45px;
  -webkit-transform: scale(0.9);
      -ms-transform: scale(0.9);
          transform: scale(0.9);
    z-index: 2;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item:nth-of-type(1) {
  bottom: 0;
  -webkit-transform: scale(1);
      -ms-transform: scale(1);
          transform: scale(1);
    z-index: 3;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item:hover {
  -webkit-box-shadow: 0 0 20px rgba(208, 208, 208, 0.8);    
  box-shadow: 0 0 20px rgba(208, 208, 208, 0.8);
  -webkit-transition: 0.2s;
  -o-transition: 0.2s;
  transition: 0.2s;
  cursor: pointer;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item:nth-of-type(2):hover {
  -webkit-transform: scale(0.95) translate(0, -25px);
      -ms-transform: scale(0.95) translate(0, -25px);
          transform: scale(0.95) translate(0, -25px);
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item:nth-of-type(3):hover {
  -webkit-transform: scale(0.85) translate(0, -30px);
      -ms-transform: scale(0.85) translate(0, -30px);
          transform: scale(0.85) translate(0, -30px);
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item
  .messages_list__item___sender {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: center;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-flex: 1;
  -ms-flex: 1;
  flex: 1;
  overflow: hidden;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item
  .messages_list__item___sender
  > img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  -o-object-fit: cover;
  object-fit: cover;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item
  .messages_list__item___sender
  > span {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: #98afea;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
          justify-content: center;
  color: #fff;
  font-size: 30px;
  font-weight: 100;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item
  .messages_list__item___message {
  padding-left: 5px;
  -webkit-box-flex: 4;
  -ms-flex: 4;
  flex: 4;
  font-size: 13px;
  letter-spacing: 0.3px;
  min-height: 100%;
  height: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  -webkit-box-align: start;
  -ms-flex-align: start;
  align-items: flex-start;
  -webkit-box-pack: start;
  -ms-flex-pack: start;
  justify-content: flex-start;
  position: relative;
  z-index: 1;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item
  .messages_list__item___message
  span {
  overflow: hidden;
  -o-text-overflow: ellipsis;
     text-overflow: ellipsis;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item
  .messages_list__item___message
  span.name {
  display: block;
  font-weight: 600;
  font-size: 12px;
  width: 100%;
  text-align: right;
  padding-right: 10px;
  margin-bottom: 2px;
  padding-bottom: 5px;
  letter-spacing: 0.6px;
  text-transform: capitalize;
  position: relative;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item
  .messages_list__item___message
  span.name::after {
  content: "";
  position: absolute;
  width: 100%;
  bottom: 0;
  right: 0;
  height: 1px;
  background-image: -o-linear-gradient(45deg, transparent, #e2e2e2);
  background-image: linear-gradient(45deg, transparent, #e2e2e2);
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_up
  .messages_list__item
  .messages_list__item___message
  span:last-of-type {
  max-height: 100%;
}

/* window mode */
.bubble_chat_wrapper .bubble_chat_wrapper__rolled_down {
  width: 320px;
  height: 400px;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
  position: fixed;
  bottom: 60px;
  padding: 2px;
  background-color: #abc3ff;
  border-radius: 5px;
  -webkit-box-pack: justify;
      -ms-flex-pack: justify;
          justify-content: space-between;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list {
  height: calc(100% - 52px);
  display: block;
  background-color: #fff;
  overflow: hidden;
  overflow-y: auto;
  padding: 10px 0 10px 10px;
  position: relative;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
  -webkit-box-pack: end;
      -ms-flex-pack: end;
          justify-content: flex-end;
  -webkit-box-align: end;
      -ms-flex-align: end;
          align-items: flex-end;
  padding-right: 15px;
      min-height: 100%;
    -webkit-box-pack: end;
    -ms-flex-pack: end;
            justify-content: flex-end;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down::before {
    content: '';
    position: absolute;
    width: calc(100% - 4px);
    height: 100%;
    top: 2px;
    left: 2px;
    background-image: -webkit-gradient(linear, left top, left bottom, color-stop(3%, #fff), color-stop(10%, rgba(255, 255, 255, 0.3)), color-stop(20%, transparent), to(transparent));
    background-image: -o-linear-gradient(top, #fff 3%, rgba(255, 255, 255, 0.3) 10%, transparent 20%, transparent 100%);
    background-image: linear-gradient(-180deg, #fff 3%, rgba(255, 255, 255, 0.3) 10%, transparent 20%, transparent 100%);
    z-index: 10;
    border-radius: 4px;
    pointer-events: none!important;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div {
    border-radius: 10px;
    width: 90%;
    -webkit-box-shadow: 0 0 3px 0px rgba(0,0,0,.3);
            box-shadow: 0 0 3px 0px rgba(0,0,0,.3);
    padding: 10px;
    margin: 8px 0;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    position: relative;
    -webkit-animation: .2s bubble-show-up ease-in-out;
        animation: .2s bubble-show-up ease-in-out;
  }

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div.mine {
    background-color: #e5edff;
    -ms-flex-item-align: start;
        align-self: flex-start;
  }

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div.mine img,
.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div.mine span.no_image,
.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div.mine span.name  {
    display: none;
  }

  .bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div > div {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-align: start;
    -ms-flex-align: start;
            align-items: flex-start;
    overflow: hidden;
    text-overflow: ellipsis;
  }

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div img,
.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div span.no_image {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    position: absolute;
    left: -25px;
    bottom: 5px;
    background-color: #808080;
    font-size: 10px;
    color: #fff;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
    -webkit-box-pack: center;
        -ms-flex-pack: center;
            justify-content: center;
  }

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div span.msg {
      font-size: 13px;
      color: #2c2c3e;
      letter-spacing: .3px;
      white-space: pre-line;
      width: 100%;
      text-align: left;
      overflow: hidden;
      text-overflow: ellipsis;
 }

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__list .scroll > div span.name {
    font-size: 13px;
    font-weight: 500;
    text-transform: capitalize;
    color: #696969;
    margin-bottom: 5px;
    text-align: left;
  }

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__input {
  height: 50px;
  display: block;
  background-color: #fff;
}

.bubble_chat_wrapper
  .bubble_chat_wrapper__rolled_down
  .bubble_chat_wrapper__rolled_down__input
  textarea {
  width: 100%;
  height: 100%;
  resize: none;
  outline: none;
  padding: 5px 10px 10px;
  font-size: 12px;
  border: none;
}

/* control  */
.bubble_chat_wrapper .bubble_chat_wrapper__control {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #4f7dec;
  display: block;
  -webkit-box-shadow: 0 0 10px rgba(44, 142, 255, 0.5);
          box-shadow: 0 0 10px rgba(44, 142, 255, 0.5);
  cursor: pointer;
  -webkit-transition: 0.4s;
  -o-transition: 0.4s;
  transition: 0.4s;
  position: relative;
}
.bubble_chat_wrapper .bubble_chat_wrapper__control span.new_mesages {
  position: absolute;
    width: 20px;
    height: 20px;
    display: block;
    border-radius: 50%;
    background-color: #e05f1f;
    color: #fff;
    display: flex;
    align-items: center;
    font-size: 12px;
    justify-content: center;
    left: -5px;
    top: 0px;
    z-index: 10;
}
.bubble_chat_wrapper .bubble_chat_wrapper__control:hover {
  -webkit-transition: 0.4s;
  -o-transition: 0.4s;
  transition: 0.4s;
  -webkit-box-shadow: 0 0 10px rgba(44, 142, 255, 1);
          box-shadow: 0 0 10px rgba(44, 142, 255, 1);
}
</style>
