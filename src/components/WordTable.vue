<template>
  <div id="tableContainer">
    <el-table :data="displayedData" style="width: 100%">
      <el-table-column prop="id" label="Id"/>
      <el-table-column prop="word" label="Word"/>
      <el-table-column prop="pronunciation" label="Pronunciation"/>
      <el-table-column prop="definition" label="Definition"/>
      <!--      <el-table-column prop="isLearned" label="isLearned"/>-->
    </el-table>
    <div class="example-pagination-block">
      <div class="example-demonstration"></div>
      <el-pagination
          layout="prev, pager, next"
          :total="tableData.length"
          :page-size="pageSize"
          :pager-count="pagerCount"
          @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import bus from "@/Util/EventBus";

export default {
  data() {
    return {
      tableData: [],
      pageSize: 16,
      pagerCount: 10,
      currentPage: 1,
    };
  },
  computed: {
    displayedData() {
      const start = (this.currentPage - 1) * this.pageSize;
      const end = start + this.pageSize;
      return this.tableData.slice(start, end);
    },
  },
  methods: {
    handleCurrentChange(val) {
      this.currentPage = val;
    },
    async fetchWords() {
      try {
        const response = await axios.get('http://localhost:8080/word');
        this.tableData = response.data.data;
        this.tableData.forEach((item) => {
          if (item.isLearned === 1)
            item.isLearned = '已学习';
          else
            item.isLearned = '未学习';
        });
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.fetchWords();
  },
  created() {
    bus.on('indexChange', () => {
      this.fetchWords();
    })
  },
}
</script>

<style>
#tableContainer {
  width: 100%;
  height: 100vh;
}

.example-pagination-block {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.example-demonstration {
  margin-right: 20px;
}
</style>
