<template>
  <div>
    <ul>
      Gendet
      <li>
        <input type="radio" v-model="gender" value="Nam" />
        <label for="">Nam</label>
      </li>
      <li>
        <input type="radio" v-model="gender" value="Nữ" />
        <label for="">Nữ</label>
      </li>
      <li>
        <input type="radio" v-model="gender" value="Khác" />
        <label for="">Khác</label>
      </li>
    </ul>

    <p>Giá trị giới tính được chọn: {{ gender }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const gender = ref("Nam");
</script>

<style></style>
