<script>
import { ref, onMounted } from 'vue';
import ChatLoginModal from './components/Chat/ChatLoginModal.vue';

export default {
  components: {
    ChatLoginModal,
  },
  setup() {
    const username = ref('');
    const isLogged = ref('');

    const login = (e) => {
      e.preventDefault();

      if (username.value.length) {
        window.localStorage.setItem('username', username.value);
        isLogged.value = true;
      } else {
        isLogged.value = false;
      }
    };

    onMounted(() => {
      username.value = window.localStorage.getItem('username');
      isLogged.value = Boolean(username.value);
    });

    return {
      username,
      isLogged,
      login,
    };
  },
};
</script>

<template>
  <chat-login-modal v-if="!isLogged" v-model:username="username" @onSubmit="login" />
</template>
