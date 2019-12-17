<template>
  <div class="contain-box">
    <div class="table-box">
      <el-form
        label-position="right"
        label-width="100px"
        :model="formLabelSearch"
        :inline="true"
        @submit.native.prevent
      >
        <el-form-item label="mac地址">
          <el-input
            placeholder="请输入mac地址"
            v-model="formLabelSearch.macAddress"
            @keyup.enter.native="handleSearch"
          ></el-input>
        </el-form-item>
        <el-form-item label="快递柜名称">
          <el-input
            placeholder="请输入名称"
            v-model="formLabelSearch.cabinetName"
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
        <el-button
          type="primary"
          @click="handleExport"
        >导出数据</el-button>
      </div>
      <el-table
        :data="userList"
        border
        style="width: 100%"
      >
        <el-table-column
          prop="macAddress"
          label="快递柜mac地址"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="cabinetName"
          label="快递柜名称"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="servingAdr"
          label="投放地址"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="shopName"
          label="投放店铺名称"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="serialNumber"
          label="格口编号"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="type"
          label="格口类型"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="userStatus"
          label="格口使用状态"
          align="center"
          show-overflow-tooltip
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
            >打开</el-button>
          </template>
        </el-table-column>
      </el-table>
    
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :page-sizes="[10, 20]"
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
    var checkAddress = (rule, value, callback) => {
      if (!value) {
        return callback(new Error("地址不能为空"));
      } else {
        callback();
      }
      setTimeout(() => {
        if (this.errorMsg) {
          console.log("tag", "");
          return callback(new Error(this.errorMsg));
        } else {
          callback();
        }
      }, 10);
    };
    return {
      total: 1,
      page: 1,
      userList: [],
      formLabelSearch: {
        pageNumber: 1,
        pageSize: 10
      },
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
        .post("/management/pigeonholes/list", this.$qs.stringify(this.formLabelSearch))
        .then(res => {
          if (res.status == 200) {
            console.log(res);
            this.userList = res.data.data;
            this.total = res.data.totalCount;
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
              "/management/cabinet-info",
              this.$qs.stringify(this.formLabelAdd)
            )
            .then(res => {
              console.log(res);
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
      this.$confirm("此操作将打开格口门, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$axios
          .post(`/management/pigeonholes/openPigeonholes`,this.$qs.stringify({macAddress:rows.macAddress,SerialNumber:rows.SerialNumber}))
          .then(res => {
            if (res.status == 200) {
              this.$message.success("打开成功");
              this.getUserList(1, 10);
            }
          });
      });
    },

    handleDelete(index, rows) {
      console.log(rows);
      this.$confirm("此操作将永久删除该数据, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$axios
          .get(`/management/sysUser/deleteSysUser?id=${rows.id}`)
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
      window.location.href = `http://192.168.50.165:8501/management/pigeonholes/createExcel`;
    },
    //分页
    handleSizeChange(val) {
      this.formLabelSearch.rows = val;
      this.getUserList(
        this.formLabelSearch.pageNumber,
        this.formLabelSearch.pageSize
      );
    },
    handleCurrentChange(val) {
      this.formLabelSearch.pageNumber = val;
      this.getUserList(
        this.formLabelSearch.pageNumber,
        this.formLabelSearch.pageSize
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