<script>
import { ref, onMounted, onUnmounted } from 'vue';
import ChatLoginModal from '@/components/Chat/ChatLoginModal.vue';
import ChatMessages from '@/components/Chat/ChatMessages.vue';
import ChatForm from '@/components/Chat/ChatForm.vue';
import { supabase } from '@/utils/supabase';

export default {
  components: {
    ChatLoginModal,
    ChatMessages,
    ChatForm,
  },
  setup() {
    const subscriptionChat = ref(undefined);
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

    const getMessages = async () => {
      try {
        let { data, error } = await supabase.from('messages').select(`id, username, message`).order('id', {
          ascending: true,
        });

        if (error) throw error;

        messages.value = [];

        if (data.length) {
          data.forEach((message) => {
            messages.value.push({
              username: username.value === message.username ? 'Yo' : message.username,
              message: message.message,
            });
          });
        }
      } catch (e) {
        alert(e.message);
      }
    };

    const sendMessage = async (e) => {
      e.preventDefault();

      await supabase.from('messages').insert({
        username: username.value,
        message: message.value,
      });

      // clean input form message after send message
      message.value = '';
    };

    const subscribeChat = async (e) => {
      subscriptionChat.value = await supabase
        .from('messages')
        .on('INSERT', (message) => {
          if (message.new) {
            const _message = message.new; // rename to avoid variable collision
            messages.value.push({
              username: username.value === _message.username ? 'Yo' : _message.username,
              message: _message.message,
            });
          }
        })
        .subscribe();
    };

    onMounted(async () => {
      username.value = window.localStorage.getItem('username');
      isLogged.value = Boolean(username.value);

      await getMessages();
      await subscribeChat();
    });

    onUnmounted(() => {
      // unsubscribe when component is unmounted
      subscriptionChat.value = undefined;
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
