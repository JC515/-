<template>
  <div class="container">
    <div>
      <span>账号：</span>
      <span>
        <el-input v-model="id" placeholder="请输入账号" clearable size="large" maxlength="20" show-word-limit
                  style="width: 20% ;height: 50px" ref="idInput"/>
      </span>
    </div>
    <br>
    <div>
      <span>密码：</span>
      <span>
        <el-input v-model="password" type="password" placeholder="请输入密码" show-password clearable size="large"
                  maxlength="30" style="width: 20% ;height: 50px" ref="passwordInput"/>
      </span>
    </div>
    <br>
    <div class="centered-button">
      <el-Button @click="enter" id="enterButton" type="primary">登录</el-Button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      id: '',
      password: ''
    };
  },
  methods: {
    enter() {
      if (!this.id || !this.password) {
        alert('账号或密码不能为空');
        return;
      }

      // 将this.id和this.password转换为字符串
      const requestData = {
        id: String(this.id),
        password: String(this.password)
      };

      axios.post('http://localhost:8080/user/login', requestData)
          .then(response => {
            if (response.data.data === 'success') {
              console.log('success');
              this.$emit('SuccessfulLogIn', this.id);
            } else {
              alert('账号或密码错误')
              this.$refs.idInput.clear();
              this.$refs.passwordInput.clear();
              console.log('fail');
            }
          })
          .catch(error => {
            console.error(error);
          });
    }
  }
}
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
