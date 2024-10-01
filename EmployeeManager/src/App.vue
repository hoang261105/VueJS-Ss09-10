<template>
  <div>
    <div class="w-[80%] m-auto mt-4 h-[100vh]">
      <main class="main">
        <header class="d-flex justify-content-between mb-3">
          <h3>Nhân viên</h3>
          <button class="btn btn-primary" @click="handleOpen">
            Thêm mới nhân viên
          </button>
        </header>
        <div class="d-flex align-items-center justify-content-end gap-2 mb-3">
          <input
            style="width: 350px"
            type="text"
            class="form-control"
            placeholder="Tìm kiếm theo email"
            v-model="searchItem"
          />
          <i class="fa-solid fa-arrows-rotate" title="Refresh"></i>
        </div>
        <!-- Danh sách nhân viên -->
        <table class="table table-bordered table-hover table-striped">
          <thead>
            <tr>
              <th>STT</th>
              <th>Họ và tên</th>
              <th>Ngày sinh</th>
              <th>Email</th>
              <th>Địa chỉ</th>
              <th>Trạng thái</th>
              <th colspan="2">Chức năng</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(employee, index) in filteredEmployees"
              :key="employee.id"
            >
              <td>{{ index + 1 }}</td>
              <td>{{ employee.fullName }}</td>
              <td>{{ employee.birthDay }}</td>
              <td>{{ employee.email }}</td>
              <td>{{ employee.address }}</td>
              <td>
                <div
                  style="display: flex; align-items: center; gap: 8px"
                  v-if="employee.status === true"
                >
                  <div class="status status-active"></div>
                  <span> Đang hoạt động</span>
                </div>
                <div
                  style="display: flex; align-items: center; gap: 8px"
                  v-else
                >
                  <div
                    class="status status-active"
                    style="background: red"
                  ></div>
                  <span> Bị chặn</span>
                </div>
              </td>
              <td>
                <button
                  v-if="employee.status === true"
                  class="button button-block"
                  @click="handleBlockModal(employee.id)"
                >
                  Chặn
                </button>
                <button
                  v-else
                  class="button button-block"
                  style="background: red"
                  @click="handleUnBlockModal(employee.id)"
                >
                  Bỏ Chặn
                </button>
              </td>
              <td>
                <button
                  class="button button-edit"
                  @click="handleShowEdit(employee)"
                >
                  Sửa
                </button>
              </td>
              <td>
                <button
                  class="button button-delete"
                  @click="handleShowDelete(employee.id)"
                >
                  Xóa
                </button>
              </td>
            </tr>
          </tbody>
        </table>
        <footer class="d-flex justify-content-end align-items-center gap-3">
          <select class="form-select">
            <option selected>Hiển thị 10 bản ghi trên trang</option>
            <option>Hiển thị 20 bản ghi trên trang</option>
            <option>Hiển thị 50 bản ghi trên trang</option>
            <option>Hiển thị 100 bản ghi trên trang</option>
          </select>
          <ul class="pagination">
            <li class="page-item">
              <a class="page-link" href="#">Previous</a>
            </li>
            <li class="page-item"><a class="page-link" href="#">1</a></li>
            <li class="page-item"><a class="page-link" href="#">2</a></li>
            <li class="page-item"><a class="page-link" href="#">3</a></li>
            <li class="page-item"><a class="page-link" href="#">Next</a></li>
          </ul>
        </footer>
      </main>
    </div>

    <!-- Form thêm mới nhân viên -->
    <div class="overlay" v-if="isOpen">
      <form class="form">
        <div class="d-flex justify-content-between align-items-center">
          <h4>Thêm nhân viên</h4>
          <i class="fa-solid fa-xmark" @click="handleClose"></i>
        </div>
        <div>
          <label class="form-label" for="userName">Họ và tên</label>
          <input
            id="userName"
            type="email"
            class="form-control"
            v-model="inputValue.fullName"
          />
          <p style="color: red">{{ error.fullName }}</p>
        </div>
        <div>
          <label class="form-label" for="dateOfBirth">Ngày sinh</label>
          <input
            id="dateOfBirth"
            type="date"
            class="form-control"
            v-model="inputValue.birthDay"
          />
          <p style="color: red">{{ error.birthDay }}</p>
        </div>
        <div>
          <label class="form-label" for="email">Email</label>
          <input
            id="email"
            type="text"
            class="form-control"
            v-model="inputValue.email"
          />
          <p style="color: red">{{ error.email }}</p>
        </div>
        <div>
          <label class="form-label" for="address">Địa chỉ</label>
          <input
            id="address"
            type="text"
            class="form-control"
            v-model="inputValue.address"
          />
        </div>
        <div>
          <button
            class="w-100 btn btn-primary"
            type="button"
            @click="handleAdd"
          >
            Thêm mới
          </button>
        </div>
      </form>
    </div>

    <!-- Form sửa nhân viên -->
    <div class="overlay" v-if="isOpenEdit">
      <form class="form">
        <div class="d-flex justify-content-between align-items-center">
          <h4>Sửa nhân viên</h4>
          <i class="fa-solid fa-xmark" @click="handleCloseEdit"></i>
        </div>
        <div>
          <label class="form-label" for="userName">Họ và tên</label>
          <input
            id="userName"
            type="text"
            class="form-control"
            v-model="employeeEdit.fullName"
          />
          <p style="color: red">{{ error.fullName }}</p>
        </div>
        <div>
          <label class="form-label" for="dateOfBirth">Ngày sinh</label>
          <input
            id="dateOfBirth"
            class="form-control"
            v-model="employeeEdit.birthDay"
            type="date"
          />
          <p style="color: red">{{ error.birthDay }}</p>
        </div>
        <div>
          <label class="form-label" for="email">Email</label>
          <input
            id="email"
            type="text"
            class="form-control"
            v-model="employeeEdit.email"
          />
          <p style="color: red">{{ error.email }}</p>
        </div>
        <div>
          <label class="form-label" for="address">Địa chỉ</label>
          <input
            id="address"
            type="text"
            class="form-control"
            v-model="employeeEdit.address"
          />
        </div>
        <div>
          <button
            class="w-100 btn btn-primary"
            type="button"
            @click="handleUpdate"
          >
            Cập nhật
          </button>
        </div>
      </form>
    </div>

    <!-- Modal xác nhận chặn tài khoản -->
    <div class="overlay" v-if="isBlockModal">
      <div class="modal-custom">
        <div class="modal-title">
          <h4>Cảnh báo</h4>
          <i class="fa-solid fa-xmark"></i>
        </div>
        <div class="modal-body-custom">
          <span>Bạn có chắc chắn muốn chặn tài khoản này?</span>
        </div>
        <div class="modal-footer-custom">
          <button class="btn btn-light" @click="handleCloseBlockModal">
            Hủy
          </button>
          <button class="btn btn-danger" @click="handleBlock">Xác nhận</button>
        </div>
      </div>
    </div>

    <!-- Modal xác nhận bỏ chặn tài khoản -->
    <div class="overlay" v-if="isUnBlockModal">
      <div class="modal-custom">
        <div class="modal-title">
          <h4>Cảnh báo</h4>
          <i class="fa-solid fa-xmark"></i>
        </div>
        <div class="modal-body-custom">
          <span>Bạn có chắc chắn muốn bỏ chặn tài khoản này?</span>
        </div>
        <div class="modal-footer-custom">
          <button class="btn btn-light" @click="handleCloseUnBlockModal">
            Hủy
          </button>
          <button class="btn btn-danger" @click="handleUnBlock">
            Xác nhận
          </button>
        </div>
      </div>
    </div>

    <!-- Modal xác nhận xóa tài khoản -->
    <div class="overlay" v-if="isDelete">
      <div class="modal-custom">
        <div class="modal-title">
          <h4>Cảnh báo</h4>
          <i class="fa-solid fa-xmark" @click="handleCloseDelete"></i>
        </div>
        <div class="modal-body-custom">
          <span>Bạn có chắc chắn muốn xóa tài khoản này?</span>
        </div>
        <div class="modal-footer-custom">
          <button class="btn btn-light" @click="handleCloseDelete">Hủy</button>
          <button class="btn btn-danger" @click="handleDelete">Xác nhận</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { format } from "date-fns";
import { computed, reactive, ref } from "vue";
function validateEmail(email) {
  return String(email)
    .toLowerCase()
    .match(
      /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
    );
}

const isOpen = ref(false);
const isDelete = ref(false);
const selectIdDelete = ref(null);
const searchItem = ref("");
const isBlockModal = ref(false);
const isOpenEdit = ref(false);
const isUnBlockModal = ref(false);
const employeeEdit = ref(null);
const selectedUserId = ref(null);
const listEmployee = reactive(
  JSON.parse(localStorage.getItem("employees")) || []
);
const inputValue = reactive({
  fullName: "",
  email: "",
  birthDay: "",
  address: "",
});
const error = reactive({
  fullName: "",
  email: "",
  birthDay: "",
});
const handleOpen = () => {
  isOpen.value = true;
};

const handleClose = () => {
  isOpen.value = false;
};

const saveToLocal = (key, value) => {
  localStorage.setItem(key, JSON.stringify(value));
};

// Hàm thêm nhân viên
const handleAdd = () => {
  const existEmail = listEmployee.find(
    (item) => item.email === inputValue.email
  );
  if (!inputValue.fullName) {
    error.fullName = "Vui lòng nhập họ tên";
  } else {
    error.fullName = "";
  }

  if (!inputValue.email) {
    error.email = "Vui lòng nhập email";
  } else if (!validateEmail(inputValue.email)) {
    error.email = "Email không hợp lệ";
  } else if (existEmail) {
    error.email = "Email đã tồn tại";
  } else {
    error.email = "";
  }

  const birthDay = new Date(inputValue.birthDay);
  const currentDate = new Date();

  if (!inputValue.birthDay) {
    error.birthDay = "Vui lòng nhập ngày sinh";
  } else if (birthDay >= currentDate) {
    error.birthDay = "Ngày sinh không hợp lệ";
  } else {
    error.birthDay = "";
  }

  if (!error.fullName && !error.email && !error.birthDay) {
    const newEmployee = {
      id: Math.ceil(Math.random() * 10000),
      fullName: inputValue.fullName,
      status: true,
      email: inputValue.email,
      birthDay: inputValue.birthDay,
      address: inputValue.address,
    };
    listEmployee.push(newEmployee);
    saveToLocal("employees", listEmployee);
    isOpen.value = false;
    inputValue.fullName = "";
    inputValue.email = "";
    inputValue.birthDay = "";
    inputValue.address = "";
  }
};

// Hàm chặn và bỏ chặn
const handleBlockModal = (userId) => {
  selectedUserId.value = userId;
  isBlockModal.value = true;
};

const handleCloseBlockModal = () => {
  isBlockModal.value = false;
};

const handleBlock = () => {
  const index = listEmployee.findIndex(
    (employee) => employee.id === selectedUserId.value
  );
  if (index !== -1) {
    listEmployee[index].status = !listEmployee[index].status;
    saveToLocal("employees", listEmployee);
    isBlockModal.value = false;
  }
};

const handleUnBlockModal = (userId) => {
  selectedUserId.value = userId;
  isUnBlockModal.value = true;
};

const handleCloseUnBlockModal = () => {
  isUnBlockModal.value = false;
};

const handleUnBlock = () => {
  const index = listEmployee.findIndex(
    (employee) => employee.id === selectedUserId.value
  );
  if (index !== -1) {
    listEmployee[index].status = !listEmployee[index].status;
    saveToLocal("employees", listEmployee);
    isUnBlockModal.value = false;
  }
};

// Hàm sửa nhân viên
const handleShowEdit = (employee) => {
  isOpenEdit.value = true;
  employeeEdit.value = employee;
};

const handleCloseEdit = () => {
  isOpenEdit.value = false;
};

const handleUpdate = () => {
  const existEmail = listEmployee.find(
    (item) =>
      item.email === employeeEdit.value.email &&
      item.id !== employeeEdit.value.id
  );
  if (!employeeEdit.value.fullName) {
    error.fullName = "Vui lòng nhập họ tên";
  } else {
    error.fullName = "";
  }

  if (!employeeEdit.value.email) {
    error.email = "Vui lòng nhập email";
  } else if (!validateEmail(employeeEdit.value.email)) {
    error.email = "Email không hợp lệ";
  } else if (existEmail) {
    error.email = "Email đã tồn tại";
  } else {
    error.email = "";
  }

  const birthDay = new Date(employeeEdit.value.birthDay);
  const currentDate = new Date();

  if (!employeeEdit.value.birthDay) {
    error.birthDay = "Vui lòng nhập ngày sinh";
  } else if (birthDay >= currentDate) {
    error.birthDay = "Ngày sinh không hợp lệ";
  } else {
    error.birthDay = "";
  }

  if (!error.fullName && !error.email && !error.birthDay) {
    const index = listEmployee.findIndex(
      (employee) => employee.id === employeeEdit.value.id
    );
    console.log(index);
    if (index !== -1) {
      listEmployee[index] = { ...employeeEdit.value };
      saveToLocal("employees", listEmployee);
      isOpenEdit.value = false;
    }
  }
};

// Hàm tìm kiếm theo email
const filteredEmployees = computed(() => {
  return listEmployee.filter((employee) =>
    employee.email.toLowerCase().includes(searchItem.value.toLowerCase())
  );
});

// Hàm xóa nhân viên
const handleShowDelete = (id) => {
  selectIdDelete.value = id;
  isDelete.value = true;
};

const handleCloseDelete = () => {
  isDelete.value = false;
};

const handleDelete = () => {
  // Lọc danh sách nhân viên và giữ lại những nhân viên không có id khớp với nhân viên cần xóa
  listEmployee = listEmployee.filter(
    (employee) => employee.id !== selectIdDelete.value
  );

  // Lưu lại danh sách đã cập nhật vào localStorage
  saveToLocal("employees", listEmployee);

  // Đóng modal xác nhận xóa
  isDelete.value = false;
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-size: 14px;
  font-family: sans-serif;
}

.main {
  /* height: 100vh; */
  margin: 50px 100px;
}

.fa-arrows-rotate {
  font-size: 20px;
  cursor: pointer;
  color: gray;
}

.fa-arrows-rotate:hover {
  color: rgb(184, 180, 180);
  transform: rotate(20deg);
  transition: all 0.5s linear;
}

.button {
  padding: 4px 12px;
  line-height: 30px;
  border-radius: 4px;
  color: #fff;
  cursor: pointer;
}

.button-edit {
  background-color: orange;
}

.button-delete {
  background-color: red;
}

.button-block {
  background-color: green;
}

td:first-child,
td:nth-child(6),
td:nth-child(7) {
  text-align: center;
}

.form-select {
  width: 270px !important;
}

.overlay {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.form {
  background-color: #fff;
  width: 500px;
  padding: 20px 24px;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.form-label {
  font-weight: 600;
  margin-bottom: 8px;
}

.fa-xmark {
  font-size: 20px;
  cursor: pointer;
}

.error {
  color: red !important;
}

.status-container {
  border: 1px solid #dadada;
  padding: 4px 8px;
  border-radius: 4px;
}

.status {
  height: 10px;
  width: 10px;
  border-radius: 50%;
  border: 1px solid transparent;
}

.status-active {
  background-color: green;
}

.status-stop {
  background-color: red;
}

.modal-custom {
  background-color: #fff;
  padding: 20px 24px;
  border-radius: 4px;
  width: 400px;
}

.modal-title {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.modal-body-custom {
  padding: 20px 0;
}

.modal-footer-custom {
  display: flex;
  justify-content: end;
  gap: 8px;
}

.pagination {
  margin-bottom: 0;
}
</style>
