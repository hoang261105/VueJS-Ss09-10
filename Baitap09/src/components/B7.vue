<template>
  <div>
    <h2>Thêm mới sản phẩm</h2>
    <form action="">
      <div>
        <label for="">Mã sản phẩm</label><br />
        <input type="text" v-model="inputValue.productCode" />
      </div>
      <div>
        <label for="">Tên sản phẩm</label> <br />
        <input type="text" v-model="inputValue.productName" />
      </div>
      <div>
        <label for="">Giá</label> <br />
        <input type="text" v-model="inputValue.price" />
      </div>
      <div>
        <label for="">Số lương</label><br />
        <input type="number" v-model="inputValue.quantity" />
      </div>
      <button @click="handleAdd" type="button">Thêm mới</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

const inputValue = ref({
  productCode: "",
  productName: "",
  price: "",
  quantity: "",
});

const handleAdd = () => {
  console.log(inputValue.value);
};
</script>

<style></style>
