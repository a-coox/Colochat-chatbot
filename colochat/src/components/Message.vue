<template>
  <div 
    class="message" 
    :class="{ botText: isBot }">

    <div class="messageContainer">
      <p class="messageText">
        {{ messageData.text }}
      </p>
    </div>

    <div 
      v-if="isLastMessage && messageData.options"
      class="responseContainer" 
      @click.stop.prevent="responseClick">
      <button
        class="responseOption"
        v-for="(responseOption, i) in messageData.options"
        :key="i">{{ responseOption }}</button>
    </div>
  </div>
</template>

<script>
import SOURCES from '../services/Sources.js';

export default {
  name: 'Message',

  props: {
    messageData: {
      default: () => { },
      type: Object
    },
    isLastMessage: {
      default: false,
      type: Boolean
    }
  },

  computed: {
    isBot() {
      return this.messageData.source === SOURCES.SOURCE_AGENT;
    }
  },

  methods: {
    responseClick(event) {
      // User has chosen this object, send it back to server
      this.$emit('response-choice', event.srcElement.innerText);
      console.log(event.srcElement.innerText);
    }
  }
};
</script>

<style lang="scss" scoped>
.message {
  text-align: right;

  &.botText {
    text-align: left;

    .messageContainer {
      background-color: rgb(241, 87, 138);
    }
  }

  .messageContainer {
    display: inline-block;
    border-radius: 20px;
    padding: 8px 12px;
    max-width: 55%;
    margin: 7px 0;
    background-color: rgb(136, 129, 145);
    font-size: 1.1em;
    font-family: "Open Sans", sans-serif;

    .messageText {
      color: white;
      text-align: left;
      word-wrap: break-word;
    }
  }

  .responseContainer {
    .responseOption {
      padding: 7px 10px;
      margin: 2px 4px;
      border: 1px solid grey;
      border-radius: 15px;
      background-color: transparent;
      font-size: 1.1em;
      outline: none;
      transition: background-color 0.15s ease;

      &:hover {
        cursor: pointer;
        background-color: rgb(232, 233, 238);
      }
    }
  }

  @media only screen and (max-width: 1000px) {
    .messageContainer {
      font-size: 1em !important;
      max-width: 80%;
    }

    .responseContainer {
      .responseOption {
        font-size: 1em !important;
      }
    }
  }
}
</style>

