<template>
  <div>
    <div class="h-12 flex justify-center items-center text-lg ... ">指令执行情况</div>
    <el-table
        :data="tableData"

        highlight-current-row
        @current-change="handleCurrentChange"
        height="360px"
        ref="table"
        empty-text="暂无已执行过的指令">
      <el-table-column
          prop="index"
          label=""
          width="50">
      </el-table-column>
      <el-table-column
          prop="place"
          label="地址"
          width="100">
      </el-table-column>
      <el-table-column
          prop="page"
          label="页数"
          width="80">
      </el-table-column>
      <el-table-column
          prop="loss"
          label="缺页"
          width="80">
      </el-table-column>
      <el-table-column
          prop="outPage"
          label="换出页"
          width="80">
      </el-table-column>
      <el-table-column
          prop="inPage"
          label="换入页"
          width="80">
      </el-table-column>
    </el-table>
  </div>

</template>

<script>
export default {
  name: "implement",
  data: () => ({
    tableData: [],
    currentRow: null
  }),
  watch: {},
  methods: {
    addData(newData) {
      this.tableData.push(newData);
    },
    handleCurrentChange(val) {
      this.currentRow = val;
    },
    setCurrent() {
      this.$refs.table.setCurrentRow(this.tableData[this.tableData.length - 1]);
    },
    reset() {
      this.tableData = []
    }
  }
}
</script>


<style scoped>

</style>