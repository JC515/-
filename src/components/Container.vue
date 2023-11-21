<template>
  <div class="common-layout">
    <el-container class="full-height">
      <el-header height="100px" style="background-color: #79bbff">
        <h1>词汇宇宙</h1>
      </el-header>
      <el-container class="full-height">
        <el-aside width="200px" style="background-color: #a0cfff">
          <Menu></Menu>
        </el-aside>
        <el-main style="background-color: #d9ecff">
          <Word></Word>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script setup>
import Menu from "@/components/SideMenu.vue";
import Word from "@/components/WordLearn.vue";
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
