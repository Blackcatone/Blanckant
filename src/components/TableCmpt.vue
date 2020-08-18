<template>
  <div class="common-table">
    <el-table :data="tableData" border stripe height="90%" v-loading="config.loading">
      <el-table-column label="序号" width="85">
        <template slot-scope="scope">
          <span style="margin-left:20px">{{(config.page - 1) * 20 + scope.$index + 1}}</span>
        </template>
      </el-table-column>
      <el-table-column
        show-overflow-tooltip
        v-for="item in tableLabel"
        :key="item.prop"
        :label="item.label"
        :width="item.width ? item.width : null"
      >
        <template slot-scope="scope">
          <span style>{{scope.row[item.prop]}}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="150">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      layout="prev, pager, next"
      :total="config.total"
      :current-page.sync="config.page"
      @current-change="changePage"
      class="pagers"
    ></el-pagination>
  </div>
</template>

<script type="text/javascript">
export default {
  props: {
    tableData: Array,
    tableLabel: Array,
    config: Object
  },
  data() {
    return {};
  },
  methods: {
    handleEdit(row) {
      this.$emit("edit", row);
    },
    handleDelete(row) {
      this.$emit("delete", row);
    },
    changePage(page) {
      this.$emit("changePage", page);
    },
  }
};
</script>

<style lang="scss" scoped>
.common-table {
  height: calc(100% - 62px);
  background-color: #fff;
  .pagers {
    // text-align: center;
    position: absolute;
    bottom: 20px;
    right: 20px;
  }
}
</style>
