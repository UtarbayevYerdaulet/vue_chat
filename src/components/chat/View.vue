<template>
  <div class="wrapper">
    <div 
    class="messageList"
    id="messageList"
    v-chat-scroll="{always: false, smooth: true, scrollonremoved:true, smoothonremoved: false}"
    >
      <div 
        v-for="message in chat.history"
        :class="{messageWrapper: true, fromMe: login === message.from, notFromMe: login !== message.from}"
        :key="message.id">
        <div class="from">
          <p class="login_from"> 
            {{ message.from }}
          </p>
        </div>
        <div class="text">
          <p class="text_from">
            {{ message.text }}
          </p>
        </div>
        <div class="timestamp">
          <p class="from_timestamp">
            {{ getDataHandler(message.timestamp) }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EventBus from '@/EventBus'

export default {
  name: 'View.vue',
  data: () => ({

  }),
  props: {
    chat: {
      type: Object,
      default: {
        history: []
      }
    },
    login: {
      type: String,
      default: ''
    }
  },
  computed: {

  },
  methods: {
    getDataHandler: function (timestamp) {
      const currentData = new Date(timestamp)
      const hours = String(currentData.getHours())
      const minuts = String(currentData.getMinutes())
      const handledHours = hours.length < 2 ? '0' + hours : hours
      const handledMinuts = minuts.length < 2 ? '0' + minuts : minuts
      return `${handledHours} : ${handledMinuts}`
    },
    updateScroll: function () {
      const messageList = document.querySelector('#messageList')
      if (messageList.scrollYOffset < messageList.scrollHeight) {
        console.log({messageList});
        messageList.scrollTo(0, messageList.scrollHeight)
      }      
    }
  },
  async mounted () {
    setInterval(() => {
      this.updateScroll()
    }, 1000);


    // EventBus.$on('updateScroll', () => {
    //   this.updateScroll()
    // })


    // const dateNow = new Date()
    // console.log(dateNow);
    // console.log({dateNow});
    // console.log(typeof(dateNow));
  }
}
</script>

<style lang="less" scoped>
  .wrapper {
    width: 95%;
    height: 90vh;
    .messageList {
      width: 100%;
      height: 100%;
      // display: flex;
      // flex-direction: column;
      // justify-content: flex-end;
      // align-items: center;
      overflow-y: auto;
      .messageWrapper {
        max-width: 60%;
        height: auto;
        min-height: 40px;
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        padding: 20px;
        margin-bottom: 20px;     
        * {
          width: 100%;
        }  
        .from {
          .login_from {
            font-size: 1.1em;
            text-align: end;
          }
        }
        .text {
          .text_from {
            font-size: 16px;
          }
        }
        .timestamp {
          .from_timestamp {
            font-size: .9em;
            color: rgba(120, 113, 113, 0.9);
            text-align: right;
          }   
        }
      }
      .fromMe {
        background-color: rgba(50, 192, 50, .8);
        text-align: right;
        align-self: flex-end;

          p {
            margin: 0;
            padding: 0;
            font-weight: 900;
            text-align: end;
          }
      }
      .notFromMe {
        background-color: #c9c9c9;
        text-align: left;
        align-self: flex-start;

          p {
            margin: 0;
            padding: 0;
            font-weight: 900;
          }
      }
    }
  }
</style>