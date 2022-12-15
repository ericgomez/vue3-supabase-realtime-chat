<script>
import { ref, onMounted } from 'vue';
import ChatLoginModal from '@/components/Chat/ChatLoginModal.vue';
import ChatMessages from '@/components/Chat/ChatMessages.vue';
import ChatForm from '@/components/Chat/ChatForm.vue';

export default {
  components: {
    ChatLoginModal,
    ChatMessages,
    ChatForm,
  },
  setup() {
    const username = ref('');
    const isLogged = ref('');
    const messages = ref([]);
    const message = ref('');

    const login = (e) => {
      e.preventDefault();

      if (username.value.length) {
        window.localStorage.setItem('username', username.value);
        isLogged.value = true;
      } else {
        isLogged.value = false;
      }
    };

    const sendMessage = async (e) => {
      e.preventDefault();
    };

    onMounted(() => {
      username.value = window.localStorage.getItem('username');
      isLogged.value = Boolean(username.value);
    });

    return {
      username,
      isLogged,
      messages,
      message,
      login,
      sendMessage,
    };
  },
};
</script>

<template>
  <div class="flex-1 p:2 sm:p-6 justify-between flex flex-col h-screen">
    <chat-login-modal v-if="!isLogged" v-model:username="username" @onSubmit="login" />

    <chat-messages v-if="isLogged" :messages="messages" />

    <chat-form v-if="isLogged" v-model:message="message" @onSendMessage="sendMessage" />
  </div>
</template>
