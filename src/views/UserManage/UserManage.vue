<template>
  <div class="manage">
    <el-dialog
      :title="operateType === 'add' ? '添加用户' : '更新用户'"
      :visible.sync="dialogVisible"
      width="30%"
    >
      <form-cmpt :formLabel="operateFormLabel" :form="operateForm"></form-cmpt>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="confim">确 定</el-button>
      </span>
    </el-dialog>

    <div class="manage-header">
      <el-button type="primary" class="addbtn">添加</el-button>
      <form-cmpt inline :formLabel="formLabel" :form="searchFrom">
        <el-button type="primary">搜索</el-button>
      </form-cmpt>
    </div>
    <div class="manage-content">
      <table-cmpt
        :tableData="tableData"
        :tableLabel="tableLabel"
        :config="config"
        @changePage="getList"
        @edit="editUser"
        @delete="deleteUser"
      ></table-cmpt>
    </div>
  </div>
</template>

<script type="text/javascript">
import FormCmpt from "../../components/FormCmpt";
import TableCmpt from "../../components/TableCmpt";
export default {
  components: {
    FormCmpt,
    TableCmpt
  },
  data() {
    return {
      searchFrom: {
        keyword: ""
      },
      formLabel: [
        {
          model: "keyword",
          label: "",
          options: []
        }
      ],
      config: {
        page: 1,
        total: 30,
        loading: false
      },
      tableData: [],
      tableLabel: [
        {
          prop: "name",
          label: "姓名"
        },
        {
          prop: "age",
          label: "年龄"
        },
        {
          prop: "sexLabel",
          label: "性别"
        },
        {
          prop: "birth",
          label: "出生日期",
          width: 200
        },
        {
          prop: "addr",
          label: "地址",
          width: 300
        }
      ],
      dialogVisible: false,
      operateType: "add",
      operateForm: {
        name: "",
        addr: "",
        age: "",
        birth: "",
        sex: ""
      },
      operateFormLabel: [
        {
          model: "name",
          label: "姓名"
        },
        {
          model: "age",
          label: "年龄"
        },
        {
          model: "sex",
          label: "性别",
          type: "select",
          opts: [
            {
              label: "男",
              value: 1
            },
            {
              label: "女",
              value: 0
            }
          ]
        },
        {
          model: "birth",
          label: "出生日期",
          type: "date"
        },
        {
          model: "addr",
          label: "地址"
        }
      ]
    };
  },
  created() {
    this.getList();
  },
  methods: {
    getList() {
      this.config.loading = true;
      this.$http
        .get("/user/getUser", {
          params: {
            page: this.config.page
          }
        })
        .then(res => {
          console.log(res);
          this.tableData = res.data.list.map(item => {
            item.sexLabel = item.sex === 0 ? "女" : "男";
            return item;
          });
          this.config.total = res.data.count;
          this.config.loading = false;
        });
    },
    editUser(row) {
      console.log(row);
      this.operateType = "edit";
      this.operateForm = row;
      this.dialogVisible = true;
    },
    confim() {
      if (this.operateType === "edit") {
        this.$http.post("/user/edit", this.operateForm).then(res => {
          this.dialogVisible = false;
          this.getList();
        });
      }
    },
    deleteUser(row) {
      this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          let id = row.id;
          this.$http.get("/user/del", { params: { id } }).then(res => {
            this.getList();
          });
          this.$message({
            type: "success",
            message: "删除成功!"
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    }
  }
};
</script>

<style lang="scss" scoped>
@import "@/assets/scss/common.scss";
</style>
