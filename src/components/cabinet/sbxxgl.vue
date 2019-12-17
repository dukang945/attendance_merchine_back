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
        <el-form-item label="mac地址">
          <el-input
            placeholder="请输入mac地址"
            v-model="formLabelSearch.mac"
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
          prop="cabinetName"
          label="设备名称"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="cabinetType"
          label="设备型号"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="servingAdr"
          label="投放地址"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="servingTime"
          label="投放时间"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="installName"
          label="安装人姓名"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="installPhone"
          label="安装人电话"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="shopName"
          label="投放店铺名称"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="shopAdress"
          label="投放店铺地址"
          align="center"
          show-overflow-tooltip
        ></el-table-column>
        <el-table-column
          prop="shopLeader"
          label="店铺负责人姓名"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="leaderPhone"
          label="店铺负责人电话"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="productionDate"
          label="货柜生产日期"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="firmwareVersion"
          label="货柜固件版本"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="macAddress"
          label="货柜唯一Mac地址"
          align="center"
        ></el-table-column>
        <el-table-column
          label="操作"
          align="center"
          width="250"
        >
          <template slot-scope="scope">
            <el-button size="mini">查看</el-button>
            <el-button
              size="mini"
              @click="handleEdit(scope.row)"
            >编辑</el-button>
            <el-button size="mini">删除</el-button>
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
            label="唯一Mac地址："
            prop="macAddress"
          >
            <el-input v-model="formLabelAdd.macAddress"></el-input>
          </el-form-item>
          <el-form-item
            label="设备名称："
            prop="cabinetName"
          >
            <el-input v-model="formLabelAdd.cabinetName"></el-input>
          </el-form-item>
          <el-form-item
            label="设备型号："
            prop="cabinetType"
          >
            <el-input v-model="formLabelAdd.cabinetType"></el-input>
          </el-form-item>
          <el-form-item
            label="生产日期："
            prop="productionDate"
          >
            <el-date-picker
              v-model="formLabelAdd.productionDate"
              type="datetime"
              placeholder="选择日期时间"
            >
            </el-date-picker>
          </el-form-item>
          <el-form-item
            label="固件版本："
            prop="firmwareVersion"
          >
            <el-input v-model="formLabelAdd.firmwareVersion"></el-input>
          </el-form-item>
          <el-form-item
            label="投放时间："
            prop="servingTime"
          >
            <el-date-picker
              v-model="formLabelAdd.servingTime"
              type="datetime"
              placeholder="选择日期时间"
            >
            </el-date-picker>
          </el-form-item>
          <el-form-item
            label="投放地点："
            prop="servingAdr"
          >
            <el-input v-model="formLabelAdd.servingAdr"></el-input>
          </el-form-item>
          <el-form-item
            label="店铺名称："
            prop="shopName"
          >
            <el-input v-model="formLabelAdd.shopName"></el-input>
          </el-form-item>
          <el-form-item
            label="店铺地址："
            prop="shopAdress"
          >
            <el-input v-model="formLabelAdd.shopAdress"></el-input>
            <el-tag size="mini">经度：{{formLabelAdd.longtitude}}</el-tag>
            <el-tag size="mini">纬度：{{formLabelAdd.latitude}}</el-tag>
          </el-form-item>
          <el-form-item
            label="负责人姓名："
            prop="shopLeader"
          >
            <el-input v-model="formLabelAdd.shopLeader"></el-input>
          </el-form-item>
          <el-form-item
            label="负责人联系电话："
            prop="leaderPhone"
          >
            <el-input v-model="formLabelAdd.leaderPhone"></el-input>
          </el-form-item>
          <el-form-item
            label="安装人姓名："
            prop="installName"
          >
            <el-input v-model="formLabelAdd.installName"></el-input>
          </el-form-item>
          <el-form-item
            label="安装人手机号："
            prop="installPhone"
          >
            <el-input v-model="formLabelAdd.installPhone"></el-input>
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
            label="唯一Mac地址："
            prop="macAddress"
          >
            <el-input v-model="formLabelEdit.macAddress"></el-input>
          </el-form-item>
          <el-form-item
            label="设备名称："
            prop="cabinetName"
          >
            <el-input v-model="formLabelEdit.cabinetName"></el-input>
          </el-form-item>
          <el-form-item
            label="设备型号："
            prop="cabinetType"
          >
            <el-input v-model="formLabelEdit.cabinetType"></el-input>
          </el-form-item>
          <el-form-item
            label="生产日期："
            prop="productionDate"
          >
            <el-date-picker
              v-model="formLabelEdit.productionDate"
              type="datetime"
              placeholder="选择日期时间"
            >
            </el-date-picker>
          </el-form-item>
          <el-form-item
            label="固件版本："
            prop="firmwareVersion"
          >
            <el-input v-model="formLabelEdit.firmwareVersion"></el-input>
          </el-form-item>
          <el-form-item
            label="投放时间："
            prop="servingTime"
          >
            <el-date-picker
              v-model="formLabelEdit.servingTime"
              type="datetime"
              placeholder="选择日期时间"
            >
            </el-date-picker>
          </el-form-item>
          <el-form-item
            label="投放地点："
            prop="servingAdr"
          >
            <el-input v-model="formLabelEdit.servingAdr"></el-input>
          </el-form-item>
          <el-form-item
            label="店铺名称："
            prop="shopName"
          >
            <el-input v-model="formLabelEdit.shopName"></el-input>
          </el-form-item>
          <el-form-item
            label="店铺地址："
            prop="shopAdress"
          >
            <el-input v-model="formLabelEdit.shopAdress"></el-input>
            <el-tag size="mini">经度：{{formLabelEdit.longtitude}}</el-tag>
            <el-tag size="mini">纬度：{{formLabelEdit.latitude}}</el-tag>
          </el-form-item>
          <el-form-item
            label="负责人姓名："
            prop="shopLeader"
          >
            <el-input v-model="formLabelEdit.shopLeader"></el-input>
          </el-form-item>
          <el-form-item
            label="负责人联系电话："
            prop="leaderPhone"
          >
            <el-input v-model="formLabelEdit.leaderPhone"></el-input>
          </el-form-item>
          <el-form-item
            label="安装人姓名："
            prop="installName"
          >
            <el-input v-model="formLabelEdit.installName"></el-input>
          </el-form-item>
          <el-form-item
            label="安装人手机号："
            prop="installPhone"
          >
            <el-input v-model="formLabelEdit.installPhone"></el-input>
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
        pageNumber: 1,
        pageSize: 10
      },
      formLabelAdd: {},
      formLabelEdit: {},
      rules: {
        macAddress: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        cabinetName: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        cabinetType: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        productionDate: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        firmwareVersion: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        servingTime: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        servingAdr: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        shopName: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        leaderPhone: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        installName: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        shopLeader: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        installPhone: [
          { required: true, message: "请输入完整信息", trigger: "blur" }
        ],
        shopAdress: [
          { required: true, validator: checkAddress, trigger: "blur" }
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
  computed: {
    shopAdress() {
      return this.formLabelAdd.shopAdress;
    },
    macAddress() {
      return this.formLabelAdd.macAddress;
    }
  },
  watch: {
    shopAdress(val) {
      this.getLocation(val);
    },
    macAddress(val) {
      this.$axios
        .get(`/management/cabinet-info/find/mac?mac=${val}`)
        .then(res => {
          console.log(res);
          if (res.data.data) {
            this.$message.warning("mac地址已注册！");
          }
        });
    }
  },
  methods: {
    // 高德地图获取经纬度
    getLocation(address) {
      //   var geocoder = new AMap.Geocoder({
      //       city: "010", //城市设为北京，默认：“全国”
      //   });
      //   console.log(geocoder)
      //   var self = this;
      //   geocoder.getAddress(address, function(status, result) {
      //         console.log(result);

      //   });
      this.$axios
        .get(
          `https://restapi.amap.com/v3/geocode/geo?address=${
            this.shopAdress
          }&key=2a22e943ec56f0c4a7c62c9d8f344c37`
        )
        .then(res => {
          console.log(res);
          if (res.data.info == "OK") {
            if (res.data.geocodes[0]) {
              this.$set(
                this.formLabelAdd,
                "latitude",
                res.data.geocodes[0].location.split(",")[1]
              );
              this.$set(
                this.formLabelAdd,
                "longtitude",
                res.data.geocodes[0].location.split(",")[0]
              );
              this.errorMsg = "";
            } else {
              this.errorMsg = "请输入有效地址";
              console.log(this.errorMsg);
            }
          }
        });
    },
    //获取列表数据
    getUserList(page, rows) {
      if (this.time) {
        this.formLabelSearch.start = this.time[0];
        this.formLabelSearch.end = this.time[1];
        // this.formLabelSearch.time = null;
      }
      this.$axios
        .get("/management/cabinet-info/list", { params: this.formLabelSearch })
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
      this.idx = rows.id;
      this.dialogEditVisible = true;
      this.$axios.get(`/management/cabinet-info?id=${rows.id}`).then(res => {
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
              `/management/cabinet-info?id=${this.idx}`,
              this.$qs.stringify(this.formLabelEdit)
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
      window.location.href = `http://192.168.50.165:8501/management/cabinet-info/createExcel?start=${
        this.time[0]
      }&end=${this.time[1]}&mac=${this.formLabelSearch.mac?this.formLabelSearch.mac:''}`;
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