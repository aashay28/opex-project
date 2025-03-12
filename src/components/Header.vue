<template>
  <header
    class="bg-white shadow-sm px-6 py-3 flex items-center justify-between"
  >
    <div class="flex items-center space-x-8">
      <span class="text-xl font-bold text-gray-800">OPEX</span>
      <nav class="hidden md:flex space-x-6">
        <a href="#" class="text-gray-600 hover:text-black font-medium">Home</a>
        <a href="#" class="text-gray-900 font-medium border-b-2 border-black"
          >Finances</a
        >
        <a href="#" class="text-gray-600 hover:text-black font-medium"
          >Workflows</a
        >
        <a href="#" class="text-gray-600 hover:text-black font-medium"
          >Settings</a
        >
      </nav>
    </div>

    <div class="flex items-center space-x-4">
      <div class="hidden md:flex items-center space-x-2">
        <img
          src="https://i.pravatar.cc/300"
          alt="User Avatar"
          class="w-8 h-8 rounded-full"
        />
        <div>
          <p class="text-sm font-semibold">Aashay Shah</p>
          <p class="text-xs text-gray-500">aashay@gmail.com</p>
        </div>
      </div>

      <button
        @click="menuOpen = !menuOpen"
        class="md:hidden p-3 text-gray-800 text-3xl font-bold"
      >
        ☰
      </button>
    </div>
  </header>

  <div
    v-if="menuOpen"
    class="fixed inset-0 bg-white z-50 flex flex-col items-center justify-center space-y-6 text-xl font-medium transition-all duration-300"
  >
    <button @click="menuOpen = false" class="absolute top-5 right-5 text-2xl">
      ✖
    </button>
    <a href="#" class="text-gray-600 hover:text-black" @click="menuOpen = false"
      >Home</a
    >
    <a
      href="#"
      class="text-gray-900 border-b-2 border-black"
      @click="menuOpen = false"
      >Finances</a
    >
    <a href="#" class="text-gray-600 hover:text-black" @click="menuOpen = false"
      >Workflows</a
    >
    <a href="#" class="text-gray-600 hover:text-black" @click="menuOpen = false"
      >Settings</a
    >
    <div class="flex flex-col items-center">
      <img
        src="https://i.pravatar.cc/300"
        alt="User Avatar"
        class="w-10 h-10 rounded-full"
      />
      <p class="text-sm font-semibold">Aashay Shah</p>
      <p class="text-xs text-gray-500">aashay@gmail.com</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const menuOpen = ref(false);
</script>
