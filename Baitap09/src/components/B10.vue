<template>
  <div>
    <h2>Đăng nhập</h2>
    <form action="">
      <div>
        <label for="">Tên đăng nhập</label><br />
        <input type="text" v-model="inputValue.fullName" />
      </div>
      <div>
        <label for="">Mật khẩu</label><br />
        <input type="password" v-model="inputValue.password" />
      </div>
      <button type="button" @click="handleLogin">Đăng nhập</button>
    </form>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const user = reactive(JSON.parse(localStorage.getItem("user")) || []);
const inputValue = reactive({
  fullName: "",
  password: "",
});

const handleLogin = () => {
  const userLogin = user.find(
    (item) =>
      item.fullName === inputValue.fullName &&
      item.password === inputValue.password
  );
  if (userLogin) {
    alert("Đăng nhập thành công");
    inputValue.fullName = "";
    inputValue.password = "";
  } else {
    alert("Đăng nhập thất bại");
  }
};
</script>

<style></style>
