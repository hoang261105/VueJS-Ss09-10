<template>
  <div>
    <h2>Đăng ký tài khoản</h2>
    <form action="">
      <div>
        <label for="">Họ và tên</label><br />
        <input type="text" v-model="inputValue.fullName" />
        <p style="color: red">{{ error.fullName }}</p>
      </div>
      <div>
        <label for="">Email</label> <br />
        <input type="text" v-model="inputValue.email" />
        <p style="color: red">{{ error.email }}</p>
      </div>
      <div>
        <label for="">Mật khẩu</label> <br />
        <input type="password" v-model="inputValue.password" />
        <p style="color: red">{{ error.password }}</p>
      </div>
      <div>
        <label for="">Xác nhận mật khẩu</label><br />
        <input type="password" v-model="inputValue.confirmPassword" />
        <p style="color: red">{{ error.confirmPassword }}</p>
      </div>
      <button @click="handleAdd" type="button">Đăng ký</button>
    </form>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const user = reactive(JSON.parse(localStorage.getItem("user")) || []);
const inputValue = reactive({
  fullName: "",
  email: "",
  password: "",
  confirmPassword: "",
});

const error = reactive({
  fullName: "",
  email: "",
  password: "",
  confirmPassword: "",
});

const handleAdd = () => {
  const existEmail = user.find((item) => item.email === inputValue.email);
  if (!inputValue.fullName) {
    error.fullName = "Họ và tên không được để trống";
  } else {
    error.fullName = "";
  }

  if (!inputValue.email) {
    error.email = "Email không được để trống";
  } else if (existEmail) {
    error.email = "Email đã tồn tại";
  } else {
    error.email = "";
  }
  if (!inputValue.password) {
    error.password = "Mật khẩu không được để trống";
  } else {
    error.password = "";
  }
  if (!inputValue.confirmPassword) {
    error.confirmPassword = "Xác nhận mật khẩu không được để trống";
  } else if (inputValue.password !== inputValue.confirmPassword) {
    error.confirmPassword = "Xác nhận mật khẩu không đúng";
  } else {
    error.confirmPassword = "";
  }

  if (
    !error.fullName &&
    !error.email &&
    !error.password &&
    !error.confirmPassword
  ) {
    const newUser = {
      id: Math.ceil(Math.random() * 10000),
      fullName: inputValue.fullName,
      email: inputValue.email,
      password: inputValue.password,
      confirmPassword: inputValue.confirmPassword,
    };
    user.push(newUser);
    localStorage.setItem("user", JSON.stringify(user));

    inputValue.fullName = "";
    inputValue.email = "";
    inputValue.password = "";
    inputValue.confirmPassword = "";
  }
};
</script>

<style></style>
