<template>
  <el-menu
      background-color="#a0cfff"
      active-text-color="#EBEDF0"
      class="el-menu-vertical-demo"
      default-active="2"
      text-color="#000000"
      @select="handleMenuSelect"
  >
    <el-menu-item index="1">
      <el-icon>
        <HomeFilled/>
      </el-icon>
      <span>个人中心</span>
    </el-menu-item>
    <el-menu-item index="2">
      <el-icon>
        <el-icon>
          <Reading/>
        </el-icon>
      </el-icon>
      <span>单词学习</span>
    </el-menu-item>
    <el-menu-item index="3">
      <el-icon>
        <el-icon>
          <Check/>
        </el-icon>
      </el-icon>
      <span>词汇测验</span>
    </el-menu-item>
    <el-menu-item index="4">
      <el-icon>
        <setting/>
      </el-icon>
      <span>设置</span>
    </el-menu-item>
  </el-menu>
</template>

<script lang="ts" setup>
import {Setting, HomeFilled, Reading, Check} from '@element-plus/icons-vue'
</script>

<script lang="ts">
export default {
  methods: {
    handleMenuSelect(index) {
      this.$emit('menuSelect', index)
      console.log("发送界面切换请求", index)
    },
  },
}
</script>
