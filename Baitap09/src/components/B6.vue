<template>
  <div>
    <ul>
      My hobby
      <li>
        <input type="checkbox" v-model="hobby" value="Đá bóng" />
        <label for="">Đá bóng</label>
      </li>
      <li>
        <input type="checkbox" v-model="hobby" value="Code" />
        <label for="">Code</label>
      </li>
      <li>
        <input type="checkbox" v-model="hobby" value="Cầu lông" />
        <label for="">Cầu lông</label>
      </li>
    </ul>
    <p>Hobby: {{ hobby }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const hobby = ref([]);
</script>

<style></style>
