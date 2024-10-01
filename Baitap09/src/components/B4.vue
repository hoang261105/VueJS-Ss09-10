<template>
  <div>
    <input type="range" v-model="range" />
    <p>Tiến độ hoàn thành: {{ range }}%</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const range = ref(0);
</script>

<style></style>
