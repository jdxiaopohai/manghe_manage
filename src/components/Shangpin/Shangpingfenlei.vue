<template>
  <div class="index">
    <div class="nav1">
      <div class="tit1">商品分类</div>
    </div>
    <div class="nav2">
      <!-- <div class="myForm">
        <el-form :inline="true" :model="searchForm" class="demo-form-inline">
          <el-form-item label="商品分类：">
            <el-select
              size="small"
              v-model="searchForm.pid"
              placeholder="商品分类"
            >
              <el-option label="顶级分类" value="0"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item size="small" label="状态：">
            <el-select v-model="searchForm.status" placeholder="状态">
              <el-option label="区域一" value="shanghai"></el-option>
              <el-option label="区域二" value="beijing"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item size="small" label="分类名称：">
            <el-input
              v-model="searchForm.keyword"
              placeholder="请输入"
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button size="small" type="primary" @click="searchOnSubmit"
              >查询</el-button
            >
          </el-form-item>
        </el-form>
      </div>-->
      <div class="tit1">
        <el-button @click="AddFenlei" size="small" type="primary" icon="el-icon-plus">添加分类</el-button>
      </div>
      <div class="myTable">
        <vxe-table
          align="center"
          :data="tableData"
        >
          <vxe-table-column field="id" width='100' title="ID"></vxe-table-column>
          <vxe-table-column field="name" title="分类名称"></vxe-table-column>
          <vxe-table-column field="weight" title="比重"></vxe-table-column>
          <!-- <vxe-table-column field="is_show" title="状态(是否显示)">
            <template slot-scope="scope">
              <el-switch
                @change="changeKG(scope.row)"
                v-model="scope.row.is_showKG"
              >
              </el-switch>
            </template>
          </vxe-table-column>-->
          <vxe-table-column title="操作" width="180">
            <template slot-scope="scope">
              <div class="flex">
                <el-button size="small" type="text" @click="tabEdit(scope.row)">编辑</el-button>
                <el-button size="small" type="text" @click="tabDel(scope.row)">删除</el-button>
              </div>
            </template>
          </vxe-table-column>
        </vxe-table>
      </div>
    </div>
    <!-- 添加分类 -->
    <el-dialog
      title="添加分类"
      :visible.sync="addDialogVisible"
      width="700px"
      :before-close="addHandleClose"
    >
      <div class="myAddForm">
        <el-form
          :model="addForm"
          :rules="rules"
          ref="addForm"
          label-width="100px"
          class="demo-addForm"
        >
          <!-- <el-row>
            <el-col :span="20">
              <el-form-item label="父级">
                <el-select size="small" v-model="addForm.pid" placeholder="请选择">
                  <el-option label="顶级分类" value="0"></el-option>
                  <el-option
                    v-for="item in tableData"
                    :key="item.id"
                    :label="item.cate_name"
                    :value="item.id"
                  ></el-option>
                </el-select>
                <el-cascader v-model="addForm.pid" size="small" :options="tableData" :props="props"></el-cascader>
              </el-form-item>
            </el-col>
          </el-row> -->
          <el-row>
            <el-col :span="20">
              <el-form-item label="分类名称" prop="category_name">
                <el-input size="small" placeholder="请输入分类名称" v-model="addForm.category_name"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="20">
              <el-form-item label="比重" prop="weight">
                <el-input size="small" placeholder="请输入分类比重" v-model="addForm.weight"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <!-- <el-row>
            <el-col :span="20">
              <el-form-item label="分类图标">
                <div @click="companyList('tb')" class="myImg">
                  <el-image :src="addForm.pic" fit="fill" style="width: 60px; height: 60px">
                    <div slot="error" class="image-slot">
                      <i class="el-icon-picture-outline"></i>
                    </div>
                  </el-image>
                  <div @click.stop="delImg('tb')" class="closeBtn">
                    <el-button circle>×</el-button>
                  </div>
                </div>
              </el-form-item>
            </el-col>
          </el-row> -->

          <!-- <el-row>
            <el-col :span="12">
              <el-form-item label="状态" prop="is_show">
                <el-radio-group v-model="addForm.is_show">
                  <el-radio label="显示"></el-radio>1
                  <el-radio label="隐藏"></el-radio>0
                </el-radio-group>
              </el-form-item>
            </el-col>
          </el-row>-->
          <el-row>
            <el-col :span="20">
              <el-form-item>
                <el-button size="small" type="primary" @click="AddOnSubmit('addForm')">提交</el-button>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
      </div>
    </el-dialog>
    <input
      type="file"
      name="companyLogo"
      id="file0"
      class="displayN"
      multiple="multiple"
      @change="companyLogo($event)"
      ref="fileInputList"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchForm: {
        pid: "",
        status: "",
        keyword: ""
      },
      tableData: [],
      addDialogVisible: false,
      addForm: {
        category_name: "",
        weight:'',
      },
      rules: {
        category_name: [
          { required: true, message: "请输入分类名称", trigger: "blur" }
        ],
        is_show: [{ required: true, message: "请选择状态", trigger: "change" }]
      },
      imgStatus: "",
      imgFile: "",
      id: "",
      isAdd: true,
      props: {
        checkStrictly: true,
        value: "id",
        label: "category_name"
      }
    };
  },
  created() {
    this.getData();
  },
  methods: {
    async getData() {
      const res = await this.$api.addMallCategory();
      console.log(res);
      this.tableData = res.data;
      // this.tableData.forEach(ele => {
      //   ele.is_showKG = ele.is_show == "1" ? true : false;
      //   if (ele.children) {
      //     ele.children.forEach(item => {
      //       item.is_showKG = item.is_show == "1" ? true : false;
      //     });
      //   }
      // });
    },
    // 开关（显示/隐藏）
    async changeKG(row) {
      console.log(row);
      const res = await this.$api.categorySave({
        ...row,
        is_show: row.is_showKG == true ? "1" : "0"
      });
      if (res.code == 200) {
        this.$message({
          message: res.msg,
          type: "success"
        });
        this.addDialogVisible = false;
        this.getData();
      }
    },
    searchOnSubmit() {
      console.log(this.searchForm);
    },
    AddFenlei() {
      this.isAdd = true;
      for (const key in this.addForm) {
        this.$set(this.addForm, key, "");
      }
      this.addDialogVisible = true;
    },
    addHandleClose() {
      this.addDialogVisible = false;
    },
    tabEdit(row) {
      console.log(row);
      this.id = row.id;
      this.isAdd = false;
      this.addDialogVisible = true;
      // row.is_show = row.is_show == "0" ? "隐藏" : "显示";
      // row.pid = row.pid == "0" ? "顶级菜单" : row.pid;
      this.addForm.category_name = row.name;
      this.addForm.weight = row.weight;
    },
    async tabDel(row) {
      console.log(row);
      const res = await this.$api.deleteCategory({id:row.id});
      if (res.status == 200) {
        this.$message({
          message: res.msg,
          type: "success"
        });
        setTimeout(() => {
          this.getData();
        }, 500);
      } else {
        this.$message.error(res.msg);
      }
    },
    AddOnSubmit(formName) {
      this.addForm.is_show = this.addForm.is_show == "显示" ? 1 : 0;
      console.log(this.addForm);
      this.$refs[formName].validate(async valid => {
        if (valid) {
          if (this.isAdd) {
            const res = await this.$api.updateCategory({
              name: this.addForm.category_name,
              weight: this.addForm.weight,
            });
            if (res.status == 200) {
              this.$message({
                message: res.msg,
                type: "success"
              });
              this.addDialogVisible = false;
              this.getData();
            }
          } else {
            const res = await this.$api.updateCategory(
              {
                id:this.id,
                name: this.addForm.category_name,
                weight: this.addForm.weight,
              },
            );
            if (res.status == 200) {
              this.$message({
                message: res.msg,
                type: "success"
              });
              this.addDialogVisible = false;
              this.getData();
            }
          }
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    // 删除图片
    delImg(val) {
      if (val == "tb") {
        this.$set(this.addForm, "pic", "");
      } else if (this.imgStatus == "dt") {
        this.$set(this.addForm, "big_pic", "");
      }
    },
    // 上传图片
    companyList(val) {
      this.imgStatus = val;
      this.$refs.fileInputList.click();
    },
    async companyLogo(event) {
      const that = this;
      var file = event.target.files[0];
      var fileSize = file.size; //文件大小
      var filetType = file.type; //文件类型
      //创建文件读取对象
      // console.log(file);
      if (fileSize <= 10240 * 1024) {
        if (
          filetType == "image/png" ||
          filetType == "image/jpeg" ||
          filetType == "image/gif"
        ) {
          this.imgFile = new FormData();
          this.imgFile.append("image", file);
          sessionStorage.setItem("img", 123);
          const res = await that.$api.upload_pic(this.imgFile);
          console.log(`${this.$url}${res.data.path}`);
          this.$set(this.addForm, "pic", `${this.$url}${res.data.path}`);
          that.$refs.fileInputList.value = "";
        } else {
          this.$message.error("图片格式不正确");
        }
      } else {
        this.$message.error("图片大小不正确");
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.index {
}
.nav1 {
  margin: 0 -18px;
  background-color: #fff;
  padding: 20px 32px 0 40px;
  .tit1 {
    color: #17233d;
    font-weight: 500;
    font-size: 20px;
    padding-bottom: 20px;
  }
}
.nav2 {
  margin: 18px 0;
  background: #fff;
  border-radius: 6px;
  padding: 24px;
  .myForm {
    /deep/ .el-form-item__label {
      font-size: 12px;
    }
    /deep/ .el-form-item {
      margin-right: 30px;
      margin-bottom: 0;
      vertical-align: middle;
    }
  }
  .tit1 {
    margin-top: 10px;
  }
  .myTable {
    margin-top: 10px;
    /deep/ .vxe-table--render-default .vxe-body--column {
      line-height: 0px;
      vertical-align: middle;
    }
    /deep/ .vxe-cell--label {
      font-size: 12px;
    }
    /deep/ .vxe-cell--title {
      font-size: 12px;
    }
    .flex {
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}
.myAddForm {
  /deep/ .el-select {
    width: 100%;
  }
  /deep/ .el-form-item__label {
    font-size: 12px;
    width: 130px !important;
  }
  /deep/ .el-form-item__content {
    margin-left: 130px !important;
  }
  /deep/ .el-radio__label {
    font-size: 12px;
  }
  /deep/ .el-button {
    width: 100%;
  }
  .myImg {
    position: relative;
    width: 60px;
    height: 60px;
    display: inline-block;
    margin-right: 12px;
    .closeBtn {
      position: absolute;
      top: -6px;
      right: -4px;
      width: 20px;
      height: 20px;
      .el-button {
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
      }
    }
    /deep/ .image-slot {
      border: 1px solid #ddd;
      border-radius: 4px;
      background-color: #fafafa;
      width: 58px;
      height: 58px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      .el-icon-picture-outline {
        font-size: 20px;
      }
    }
  }
}
.displayN {
  display: none;
}
</style>