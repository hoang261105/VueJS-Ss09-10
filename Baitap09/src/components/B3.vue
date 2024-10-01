<template>
  <div>
    <input type="date" v-model="date" />
    <p>Giá trị ngày: {{ date }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const date = ref("");
</script>

<style></style>
