<template>
  <div>
    <input type="text" v-model="inputValue" /><br />
    <p>Giá trị: {{ inputValue }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const inputValue = ref("");
</script>

<style></style>
