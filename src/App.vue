<template>
  <div class="w-screen h-screen m-0 p-0">
    <div class="w-screen h-[8vh] flex bg-gray-300 justify-start items-center">
      <nav class="px-4">
        <RouterLink to="/" class="text-black p-2 font-semibold hover:p-2 ">Home</RouterLink>
        <RouterLink to="/about" class="text-black p-2 font-semibold hover:p-2 ">About</RouterLink>
      </nav>
    </div>

    <div class="w-screen h-[92vh] bg-gray-100">
      <RouterView />
    </div>
  </div>
</template>
<script>
import { RouterLink, RouterView } from 'vue-router'
</script>

<style scoped>
/* nav a{
    color: #444;
    padding: 3px;
  } */
</style>
