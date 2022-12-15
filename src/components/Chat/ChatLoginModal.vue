<script>
export default {
  name: 'ChatLoginModal',
  props: ['username'],
  emits: ['onSubmit', 'update:username'],
};
</script>

<template>
  <div class="h-screen w-full flex justify-center items-center bg-gradient-to-tr from-blue-900 to-blue-500">
    <div
      class="bg-image w-full sm:w-1/2 md:w-9/12 lg:w-1/2 mx-3 md:mx-5 lg:mx-0 shadow-md flex flex-col md:flex-row items-center rounded z-10 overflow-hidden bg-center bg-cover bg-blue-600"
    >
      <div class="w-full flex flex-col items-center bg-white py-5 md:py-8 px-4">
        <h3 class="mb-4 font-bold text-3xl flex items-center text-blue-500">Acceso chat</h3>
        <form @submit="$emit('onSubmit', $event)" class="px-3 flex flex-col justify-center items-center w-full gap-3">
          <input
            @input="$emit('update:username', $event.target.value)"
            placeholder="Introduce tu nombre"
            class="px-4 py-2 w-full rounded border border-gray-300 shadow-sm text-base placeholder-gray-500 placeholder-opacity-50 focus:outline-none focus:border-blue-500"
          />
          <button
            class="flex justify-center items-center bg-blue-500 hover:bg-blue-600 text-white focus:outline-none focus:ring rounded px-3 py-1"
          >
            Acceder
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
