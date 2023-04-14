<template>
  <div class="wrapper">
    <div class="avatar-container" style="text-align: center; margin-top: 20px; margin-bottom: 20px">
      <img src="../assets/avatar.png" style="border-radius:100%; width:70px;" alt="">
      <p style="color: #555; font-size: 16px; padding: 0; margin: 0;">Ванюшка</p>
    </div>
    <div class="messages-field">
      <div  v-for="message in messages" :key="message" class="message" :class='{"incoming": message.type === "incoming", "outgoing": message.type === "outgoing"}'>

        <div v-if="!message.clickable" class="message-text-container">{{message.text}}</div>
        <a v-else-if="message.clickable" target=”_blank” href="https://music.vk.com/promocode" class="message-text-container message-clickable">{{message.text}}</a>

        <div class="clear"></div>
      </div>
    </div>

    <div class="white-space" style="height: 100px"></div>

    <div class="input-container">
      <input @keypress.enter="onSendButtonClick"  v-model="input" placeholder="Сообщение" type="text">
      <button @click="onSendButtonClick"></button>
    </div>
  </div>

</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
const cryptojs = require('crypto-js');

export default {
  name: 'HomeView',
  data() {
    return {
      input: '',
      encrypted: 'U2FsdGVkX1916rD51zSdwOqATHoMs0WvhfMKQqd8BZw=',
      messages: [
        { type: 'incoming', text: "Привет, Ксбша ;) У меня для тебя есть небольшой сюрприз... Но, чтобы полчить его, введи пароль 🤭" },
        { type: 'incoming', text: "Возможно он как-то связан с вопросами, которые я сегодня задавал 🤔." },
        { type: 'incoming', text: "Удачки ;)" }
      ]
    }
  },

  methods: {
    send(type, text, clickable = false) {


      if(clickable) {
        this.messages = this.messages.concat([{type, text, clickable: true}])
        return;
      } 

      this.messages = this.messages.concat([{type, text}])
      if(text === '/подсказка') return

      if(type === 'outgoing') {
        let decrypted = cryptojs.AES.decrypt(this.encrypted, text.toLowerCase()).toString(
          cryptojs.enc.Utf8
        );

        console.log(decrypted)

        if(decrypted) {
          setTimeout(() => {
            this.send('incoming', "ВЕРНО! прими от меня небольшой подгон 🤪🤪🤪 (это промокод на активацию подписка VK Music на 3 месяца):")
            this.send('incoming', decrypted)
            this.send('incoming', 'Ссылка на активацию (клик)', true);
          }, 1000)
        } else {
          setTimeout(() => {
            this.send('incoming', 'Неверно! чтобы воспользоваться подсказкой, введи команду /подсказка')
          }, 1000)
        }
      }

      this.$nextTick(() => {
        window.scrollTo(0, document.body.scrollHeight)
      })
    },

    onSendButtonClick() {
      if (this.input === '') return;
      this.send('outgoing', this.input)
      if(this.input === '/подсказка') {
        setTimeout(() => {
          this.send('incoming', "подсказка: цвет + 😺 + число (без пробелов)")
        }, 1000)
      }
      this.input = '';
    }
  }
}
</script>

<style scoped>
  @keyframes show{
    from {
      transform: translate(0, 20px);
      opacity: 0;
    }

    to {
      transform: translate(0, 0);
      opacity: 1; 
    }
  }

  .wrapper {
    max-width: 600px;
    margin: 0 auto;
    /*padding: 10px;*/
  }


  .messages-field {
    padding:  10px;
  }
  

  .input-container {
    display:  flex;
    background-color: #fff;
    padding:  10px 0;
    width:  100%;
    max-width: 600px;
    position: fixed;
    bottom: 0px;
    padding:  10px;
  }

  .message {
    animation: show 0.2s;
  }

  .message-text-container {
    padding: 15px 25px;
    border-radius: 40px;
    margin-bottom:  10px;
    background-color: #3781f5;
    max-width:  80%;
    overflow-wrap: break-word;
    color:  white;
    display: inline-block;
    word-wrap: wrap;
  }

  .outgoing .message-text-container {
    float:  right;
    background-color: #ebebf3;
    color:  #444;
  }

  .input-container input {
    flex-grow: 1;
  }

  .message-clickable {
    background-color: #8241eb;
  }
</style>
