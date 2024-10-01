<template>
  <div>
    <h2>Đăng ký tài khoản</h2>
    <form action="">
      <div>
        <label for="">Họ và tên</label><br />
        <input type="text" v-model="inputValue.fullName" />
      </div>
      <div>
        <label for="">Email</label> <br />
        <input type="text" v-model="inputValue.email" />
      </div>
      <div>
        <label for="">Mật khẩu</label> <br />
        <input type="password" v-model="inputValue.password" />
      </div>
      <div>
        <label for="">Xác nhận mật khẩu</label><br />
        <input type="password" v-model="inputValue.confirmPassword" />
      </div>
      <button @click="handleAdd" type="button">Đăng ký</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

const inputValue = ref({
  fullName: "",
  email: "",
  password: "",
  confirmPassword: "",
});
const handleAdd = () => {
  console.log(inputValue.value);
};
</script>

<style></style>
