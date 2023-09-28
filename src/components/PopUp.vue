<template>
  <div
    v-if="showingPopup"
    class="flex flex-col items-center justify-center w-screen h-screen fixed inset-0 z-50 bg-black/50"
  >
    <div
      class="flex flex-col items-center justify-center bg-white text-black p-4 gap-2 w-96"
    >
      <h1 class="capitalize text-center">
        a task was created <br />
        for:
        <p class="text-emerald-600 inline underline">{{ user }}</p>
        <br />from
        <p class="text-emerald-600 inline underline">{{ date }}</p>
      </h1>
      <button
        class="bg-blue-400 text-white font-bold px-2 py-0.5 rounded-xl capitalize"
        @click="exit()"
      >
        quit
      </button>
    </div>
  </div>
</template>
<script>
export default {
  name: "PopUp",
  props: {
    user: String,
    date: String,
    showingPopup: Boolean,
    exit: Function,
  },
};
</script>
