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
        <el-form-item label="用户名">
          <el-input
            placeholder="请输入姓名"
            v-model="formLabelSearch.userName"
            @keyup.enter.native="handleSearch"
          ></el-input>
        </el-form-item>
        <el-form-item label="快递公司">
          <el-input
            placeholder="请输入快递公司名"
            v-model="formLabelSearch.companyName"
            @keyup.enter.native="handleSearch"
          ></el-input>
        </el-form-item>
        <el-form-item label="时间">
          <el-date-picker
            v-model="time"
            type="daterange"
            range-separator="至"
            value-format='yyyy-MM-dd'
            start-placeholder="开始日期"
            end-placeholder="结束日期"
          >
          </el-date-picker>
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
        <!-- <el-button
          type="primary"
          @click="dialogAddVisible=true"
        >新增</el-button> -->
      </div>
      <el-table
        :data="userList"
        border
        style="width: 100%"
        @filter-change="filterTag"
      >
        <el-table-column
          prop="user_name"
          label="姓名"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="company_name"
          label="所属快递公司"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="phone"
          label="手机号"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="ID_card"
          label="身份证号"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="create_time"
          label="提交时间"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="status"
          label="是否通过审核"
          align="center"
          :filters="[{ text: '审核通过', value: '1' },{ text: '未通过', value: '0' }]"
        >
          <template slot-scope="scope">
            <span
              v-if="scope.row.status==0"
              style="color:#F56C6C"
            >未通过</span>
            <span
              v-if="scope.row.status==1"
              style="color:#67C23A"
            >通过</span>
          </template>
        </el-table-column>
        <el-table-column
          label="操作"
          align="center"
          width="250"
        >
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.row)"
            >查看</el-button>
            <el-button
              size="mini"
              @click="pass(scope.row)"
            >通过</el-button>
            <el-button
              size="mini"
              @click="reject(scope.row)"
            >拒绝</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-dialog
        title="查看详情"
        :visible.sync="dialogEditVisible"
        @open="open"
        :close-on-click-modal="false"
      >
        <el-form
          :model="formLabelEdit"
          label-width="140px"
        >
          <el-form-item
            label="姓名："
            prop="macAddress"
          >
            <span>{{formLabelEdit.userName}}</span>
          </el-form-item>
          <el-form-item
            label="所属快递公司："
            prop="macAddress"
          >
            <span>{{formLabelEdit.companyName}}</span>
          </el-form-item>
          <el-form-item
            label="手机号："
            prop="macAddress"
          >
            <span>{{formLabelEdit.userPhone}}</span>
          </el-form-item>
          <el-form-item
            label="提交时间："
            prop="macAddress"
          >
            <span>{{formLabelEdit.createTime}}</span>
          </el-form-item>
          <el-form-item
            label="工牌图片："
            prop="macAddress"
          >
            <img
              :src="formLabelEdit.workPicAddress"
              alt=""
              width="40%"
              style="display:block;margin-top:15px"
            >
          </el-form-item>
          <el-form-item
            label="审核状态："
            prop="macAddress"
          >
            <span>{{formLabelEdit.status==0?'拒绝':'通过'}}</span>
          </el-form-item>
        </el-form>
        <div
          slot="footer"
          class="dialog-footer"
        >
          <el-button @click="dialogEditVisible = false">取 消</el-button>
          <el-button
            type="primary"
            @click="dialogEditVisible = false"
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
// window.VueAMap.initAMapApiLoader({
//   key: '	2a22e943ec56f0c4a7c62c9d8f344c37',
//   plugin: [... 'Geocoder']
// });

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
        pagepageNo: 1,
        pageSize: 10
      },
      formLabelAdd: {},
      formLabelEdit: {}
    };
  },
  mounted() {
    this.getUserList(
      this.formLabelSearch.pagepageNo,
      this.formLabelSearch.pageSize
    );
  },
  methods: {
    //获取列表数据
    getUserList(page, rows) {
      if(this.time){
        this.formLabelSearch.startTime = this.time[0]
      this.formLabelSearch.endTime = this.time[1]
      }
      this.$axios
        .post(
          "/management/api/expUser/getExpUserCertifiedInfos",
          this.$qs.stringify(this.formLabelSearch)
        )
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
    filterTag(value) {
      this.formLabelSearch.status = Object.values(value)[0][0];
      this.getUserList(1, 10);
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
            .post("/management/cabinet-info", JSON.stringify(this.formLabelAdd))
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
      this.idx = rows.id;
      this.dialogEditVisible = true;
      this.$axios
        .get(
          `/management/api/expUser/getExpUserCertifiedInfoById?id=${rows.id}`
        )
        .then(res => {
          if (res.status == 200) {
            console.log(res, "");
            this.formLabelEdit = res.data.data;
          }
        });
    },
    // 审核
    pass(row) {
      this.$axios
        .post(`/management/api/expUser/passCertify?idList=${row.id}`)
        .then(res => {
          if (res.status == 200) {
            this.$message.success("审核通过！");
            this.getUserList();
          }
        });
    },
    reject(row) {
      this.$axios
        .post(`/management/api/expUser/rejectCertify?idList=${row.id}`)
        .then(res => {
          if (res.status == 200) {
            this.$message.success("审核拒绝！");
            this.getUserList();
          }
        });
    },
    // 查询
    handleSearch() {
      this.getUserList(1, this.formLabelSearch.pageSize);
    },
    //导出数据
    handleExport() {
      window.location.href = `http://192.168.50.165:8501/management/api/expUser/createExcel?userName=${
        this.formLabelSearch.userName?this.formLabelSearch.userName:''
      }&companyName=${this.formLabelSearch.companyName?this.formLabelSearch.companyName:''}&startTime=${this.time[0]}&endTime=${this.time[1]}`;
    },
    //分页
    handleSizeChange(val) {
      this.formLabelSearch.rows = val;
      this.getUserList(
        this.formLabelSearch.pageNo,
        this.formLabelSearch.pageSize
      );
    },
    handleCurrentChange(val) {
      this.formLabelSearch.pageNo = val;
      this.getUserList(
        this.formLabelSearch.pageNo,
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