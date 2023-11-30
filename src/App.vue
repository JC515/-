<template>
  <div>
    <div v-if="!isLogIn">
      <!--    <div v-if="false">-->
      <LogIn @SuccessfulLogIn="handleLoginSuccess"></LogIn>
    </div>
    <div v-else>
      <Container :userId="nowUserId"></Container>
    </div>
  </div>

  <!--  <component1></component1>-->
  <!--  <Component2></Component2>-->

</template>

<script>
import Container from "@/components/Container.vue";
import LogIn from "@/components/LogIn.vue";
import {ElNotification} from "element-plus";
import Component1 from "@/Test/Component1.vue";
import Component2 from "@/Test/Component2.vue";

export default {
  components: {
    Component2,
    Component1,
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
