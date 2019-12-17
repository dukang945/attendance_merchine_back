<template>
  <div class="contain-box">
    <div class="table-box">
      <el-form
        label-position="right"
        label-width="80px"
        :model="formLabelSearch"
        :inline="true"
        @submit.native.prevent
      >
        <el-form-item label="工号">
          <el-input
            placeholder="请输入工号"
            v-model="formLabelSearch.jobNumber"
            @keyup.enter.native="handleSearch"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button
            type="primary"
            @click="handleSearch"
          >查询</el-button>
          <!-- <el-button @click="handleReset">重置</el-button> -->
        </el-form-item>
      </el-form>
    </div>
    <div class="table-box">
      <div class="function">
        <!-- <el-button
          type="primary"
          @click="handleExport"
        >导出数据</el-button> -->
        <el-button
          type="primary"
          @click="dialogAddVisible=true"
        >新增</el-button>
      </div>
      <el-table
        :data="userList"
        border
        style="width: 100%"
      >
        <el-table-column
          prop="jobNumber"
          label="工号"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="realName"
          label="姓名"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="section"
          label="部门"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="position"
          label="职位"
          align="center"
        ></el-table-column>
        <el-table-column
          label="操作"
          align="center"
          width="250"
        >
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.row)"
            >编辑</el-button>
            <el-button size="mini" @click="handleDelete(scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-dialog
        title="新增"
        :visible.sync="dialogAddVisible"
        @open="open"
        :close-on-click-modal="false"
      >
        <el-form
          label-width="140px"
          :model="formLabelAdd"
          :rules="rules"
          ref="formLabelAdd"
        >
          <el-form-item
            label="姓名："
            prop="realName"
          >
            <el-input v-model="formLabelAdd.realName"></el-input>
          </el-form-item>
          <el-form-item
            label="工号："
            prop="jobNumber"
          >
            <el-input v-model="formLabelAdd.jobNumber"></el-input>
          </el-form-item>
          <el-form-item
            label="部门："
            prop="section"
          >
            <el-input v-model="formLabelAdd.section"></el-input>
          </el-form-item>
          <el-form-item
            label="职位："
            prop="position"
          >
            <el-input v-model="formLabelAdd.position"></el-input>
          </el-form-item>
        </el-form>
        <div
          slot="footer"
          class="dialog-footer"
        >
          <el-button @click="dialogAddVisible = false">取 消</el-button>
          <el-button
            type="primary"
            @click="handleAdd('formLabelAdd')"
          >确 定</el-button>
        </div>
      </el-dialog>
      <el-dialog
        title="编辑"
        :visible.sync="dialogEditVisible"
        @open="open"
        :close-on-click-modal="false"
      >
        <el-form
          :model="formLabelEdit"
          label-width="140px"
          ref="formLabelEdit"
          :rules="rules"
        >
          <el-form-item
            label="姓名："
            prop="realName"
          >
            <el-input v-model="formLabelEdit.realName"></el-input>
          </el-form-item>
          <el-form-item
            label="工号："
            prop="jobNumber"
          >
            <el-input v-model="formLabelEdit.jobNumber"></el-input>
          </el-form-item>
          <el-form-item
            label="部门："
            prop="section"
          >
            <el-input v-model="formLabelEdit.section"></el-input>
          </el-form-item>
          <el-form-item
            label="职位："
            prop="position"
          >
            <el-input v-model="formLabelEdit.position"></el-input>
          </el-form-item>
        </el-form>
        <div
          slot="footer"
          class="dialog-footer"
        >
          <el-button @click="dialogEditVisible = false">取 消</el-button>
          <el-button
            type="primary"
            @click="saveEdit('formLabelEdit')"
          >确 定</el-button>
        </div>
      </el-dialog>
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :page-sizes="[10, 20]"
        :current-page="formLabelAlign.page"
        :page-size="10"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      total: 1,
      page: 1,
      rows: 1,
      idx: -1,
      errorMsg: "",
      dialogEditVisible: false,
      dialogAddVisible: false,
      onlineDialogVisible: false,
      userList: [],
      time: [],
      formLabelAlign: {
        page: 1,
        rows: 10
      },
      formLabelSearch: {
        page: 1,
        rows: 10
      },
      formLabelAdd: {},
      formLabelEdit: {},
      rules: {
        position: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        section: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        jobNumber: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        realName: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ]
      }
    };
  },
  mounted() {
    this.getUserList(
      this.formLabelSearch.pageNumber,
      this.formLabelSearch.pageSize
    );
  },
  methods: {
    //获取列表数据
    getUserList(page, rows) {
      this.$axios
        .get("/checking-in/clockInUser/getPage", { params: this.formLabelSearch })
        .then(res => {
          if (res.status == 200) {
            console.log(res);
            this.userList = res.data.data.rows;
            this.total = res.data.data.total;
          } else {
            this.$message({
              type: "warning",
              message: `操作异常!`
            });
          }
        });
    },
    // dialog打开回调
    open() {
      this.formLabelAdd = {};
    },
    //新增
    handleAdd(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$axios
            .post(
              "/checking-in/clockInUser/save",
              this.$qs.stringify(this.formLabelAdd)
            )
            .then(res => {
              console.log(res);
              this.$message.success('新增成功!')
              this.dialogAddVisible = false
              this.formLabelAdd ={}
            });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    //编辑
    handleEdit(rows) {
      console.log(rows);
      this.idx = rows.id;
      this.dialogEditVisible = true;
      this.$axios.get(`/checking-in/clockInUser/getDetails?id=${rows.id}`).then(res => {
        if (res.status == 200) {
          console.log(res, "");
          this.formLabelEdit = res.data.data;
        }
      });
    },
    //确认编辑
    saveEdit(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$axios
            .post(
              `/checking-in/clockInUser/save?id=${this.idx}`,
              this.$qs.stringify({
                  realName:this.formLabelEdit.realName,
                  position:this.formLabelEdit.position,
                  jobNumber:this.formLabelEdit.jobNumber,
                  section:this.formLabelEdit.section,
              })
            )
            .then(res => {
              if (res.status == 200) {
                console.log(res);
                if (res.data.code == 506) {
                  this.$message({
                    type: "warning",
                    message: `账号重复`
                  });
                  return false;
                } else if (res.data.code == 507) {
                  this.$message({
                    type: "warning",
                    message: `手机号重复`
                  });
                  return false;
                }
                this.$message({
                  type: "success",
                  message: `编辑成功!`
                });
                this.dialogEditVisible = false;
                this.getUserList(1, 10);
                this.$refs[formName].resetFields();
                this.formLabelEdit = {};
              }
            });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },

    handleDelete(rows) {
      console.log(rows);
      this.$confirm("此操作将永久删除该数据, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$axios
          .get(`/checking-in/clockInUser/delete?id=${rows.id}`)
          .then(res => {
            if (res.status == 200) {
              this.$message.success("删除成功");
              this.getUserList(1, 10);
            }
          });
      });
    },
    // 查询
    handleSearch() {
      this.getUserList(1, this.formLabelSearch.pageSize);
    },
    //导出数据
    handleExport() {
      window.location.href = `http://192.168.50.165:8501/management/cabinet-info/createExcel?start=${
        this.time[0]
      }&end=${this.time[1]}&mac=${this.formLabelSearch.mac?this.formLabelSearch.mac:''}`;
    },
    //分页
    handleSizeChange(val) {
      this.formLabelSearch.rows = val;
      this.getUserList(
        this.formLabelSearch.page,
        this.formLabelSearch.rows
      );
    },
    handleCurrentChange(val) {
      this.formLabelSearch.page = val;
      this.getUserList(
        this.formLabelSearch.page,
        this.formLabelSearch.rows
      );
    }
  }
};
</script>

<style scoped lang='scss'>
// /deep/ .el-dialog .el-form{
//   width: 800px;
// }
// .left-form{
//   margin-right: 130px;
// }
.black {
  color: #fff;
  opacity: 0;
}
</style>