<template>
  <div class="common-layout">
    <el-container class="full-height">
      <el-header height="100px" style="background-color: #79bbff">
        <h1>词汇宇宙</h1>
      </el-header>
      <el-container class="full-height">
        <el-aside width="200px" style="background-color: #a0cfff">
          <Menu @menuSelect="handleMenuSelect"></Menu>
        </el-aside>
        <el-main style="background-color: #d9ecff">
          <div id="page1" v-if="index === 1">
            <p>个人中心</p>
          </div>
          <div id="page2" v-else-if="index === 2">
            <WordLearn></WordLearn>
          </div>
          <div id="page3" v-else-if="index === 3">
            <p>词汇测验</p>
          </div>
          <div id="page4" v-else-if="index === 4">
            <p>设置</p>
          </div>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import Menu from "@/components/SideMenu.vue";
import WordLearn from "@/components/WordLearn.vue";

export default {
  components: {
    Menu,
    WordLearn,
  },
  data() {
    return {
      index: 2,
    };
  },
  methods: {
    handleMenuSelect(index) {
      index = parseInt(index, 10); // or use +index;
      console.log("接受界面切换请求", index);
      this.index = index;
    },
  },
};
</script>

<style>
h1 {
  font-size: 2em;
  font-style: italic;
  color: #000000;
}

.full-height {
  height: 100%;
}

body, html {
  min-height: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>
