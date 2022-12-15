<script>
export default {
  name: 'ChatMessages',
  props: ['messages'],
};
</script>

<template>
  <div id="messages">
    <div v-for="message in messages" :key="message.id" class="mb-1">
      <div class="flex items-end">
        <div class="flex flex-col space-y-2 text-xs max-w-xs mx-2 order-2 items-start">
          <div>
            <span class="mr-1 rounded-full bg-indigo-800 text-white text-center p-2 order-1">
              {{ message.username }}
            </span>
            <span class="px-4 py-2 rounded-lg inline-block rounded-bl-none bg-gray-300 text-gray-600">
              {{ message.message }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
