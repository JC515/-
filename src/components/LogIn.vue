<template>
  <div class="container">
    <div>
      <span>账号：</span>
      <span>
        <el-input
            v-model="id"
            placeholder="请输入账号"
            clearable
            size="large"
            maxlength="20"
            show-word-limit
            style="width: 20% ;height: 50px"
        />
      </span>
    </div>
    <br>
    <div>
      <span>密码：</span>
      <span>
        <el-input
            v-model="password"
            type="password"
            placeholder="请输入密码"
            show-password
            clearable
            size="large"
            maxlength="30"
            style="width: 20% ;height: 50px"
        />
      </span>
    </div>
    <br>
    <div class="centered-button">
      <Button @click="enter" id="enterButton"></Button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const id = ref('');
const password = ref('');

const enter = () => {
  if (id.value === '' || password.value === '') {
    alert('账号或密码不能为空');
    return;
  }
  console.log('Username:', id.value);
  console.log('Password:', password.value);

  const postData = {
    id: id.value,
    password: password.value
  };
  loginVerification(postData);
};

const loginVerification = async (postData) => {
  try {
    const response = await axios.post('http://localhost:8080/login', postData);
    if (response.data.data === 'success') {
      console.log('success');
    } else {
      console.log('fail');
    }
  } catch (error) {
    console.error(error);
  }
};
</script>

<style scoped>
.container {
  text-align: center;
  margin-top: 20%;
}

.centered-button {
  display: inline-block;
}
</style>
