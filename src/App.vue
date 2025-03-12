<template>
  <Header />
  <div class="p-8">
    <nav class="text-gray-600 text-sm mb-4 p-4">
      <ol class="list-reset flex">
        <li>
          <span class="text-blue-600 cursor-pointer hover:underline"
            >Dashboard</span
          >
        </li>
        <li>
          <span class="mx-2">/</span>
        </li>
        <li class="text-gray-800 font-semibold">Finances</li>
      </ol>
    </nav>

    <!-- <h1 class="text-2xl font-semibold text-gray-800">Finances</h1> -->
    <Table />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Table from "./components/Table.vue";
</script>
