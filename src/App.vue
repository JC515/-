<template>
  <div>
    <div v-if="!isLogIn">
      <!--    <div v-if="false">-->
      <LogIn @Successfully_logged_in="handleLoginSuccess"></LogIn>
    </div>
    <div v-else>
      <Container :userId="nowUserId"></Container>
    </div>
  </div>
</template>

<script>
import Container from "@/components/Container.vue";
import LogIn from "@/components/LogIn.vue";
import {ElNotification} from "element-plus";

export default {
  components: {
    Container,
    LogIn,
  },
  data() {
    return {
      isLogIn: false,
      nowUserId: ''
    };
  },
  methods: {
    handleLoginSuccess(id) {
      this.isLogIn = true;
      this.nowUserId = id;
    },
  },
  watch: {
    isLogIn: function () {
      ElNotification({
        message: '登录成功！',
        type: 'success',
        duration: 2000,
      })
    },
  },
};
</script>

<style scoped>
</style>
