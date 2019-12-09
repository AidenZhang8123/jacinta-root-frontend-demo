<template>
  <el-container>
    <el-header>
      <div class="header">
        <el-row>
          <el-col :span="14">
            <div class="grid-content">
              <img src="../components/logo.svg" class="logo" />
              <div class="title">呱唧物联</div>
            </div>
          </el-col>
          <el-col :span="8">
            <div class="grid-content">
              <el-menu
                :default-active="activeIndex2"
                class="el-menu-demo"
                mode="horizontal"
                @select="handleSelect"
                background-color="#2a3648"
                text-color="#fff"
                active-text-color="#FDBF00"
              >
                <el-menu-item index="1">
                  <i class="el-icon-menu" />我的产品
                </el-menu-item>
                <el-menu-item index="2">
                  <i class="el-icon-document" />使用文档
                </el-menu-item>
                <el-menu-item index="3">
                  <i class="el-icon-question" />问题与建议
                </el-menu-item>
              </el-menu>
            </div>
          </el-col>
          <el-col :span="2">
            <div class="grid-content">
              <el-dropdown class="avatar-container">
                <div class="avatar-wrapper">
                  <img :src="avatar+'?imageView2/1/w/80/h/80'" class="user-avatar" />
                  <i class="el-icon-caret-bottom" />
                </div>
                <el-dropdown-menu slot="dropdown" class="user-dropdown">
                  <router-link to="/">
                    <el-dropdown-item>Home</el-dropdown-item>
                  </router-link>
                  <a target="_blank" href="https://github.com/PanJiaChen/vue-admin-template/">
                    <el-dropdown-item>Github</el-dropdown-item>
                  </a>
                  <a target="_blank" href="https://panjiachen.github.io/vue-element-admin-site/#/">
                    <el-dropdown-item>Docs</el-dropdown-item>
                  </a>
                  <el-dropdown-item divided>
                    <span style="display:block;" @click="logout">Log Out</span>
                  </el-dropdown-item>
                </el-dropdown-menu>
              </el-dropdown>
            </div>
          </el-col>
        </el-row>
      </div>
    </el-header>
    <el-main>
      <div class="myprojectbox">
        <div class="myproject">
          <div class="mytite">我的产品</div>
          <el-button type="primary" round class="addbt" @click="dialogFormVisible = true">
            <i class="el-icon-plus" />添加产品
          </el-button>
        </div>

        <el-dialog title="添加产品" :visible.sync="dialogFormVisible">
          <el-form
            :model="ruleForm"
            :rules="rules"
            ref="ruleForm"
            label-width="120px"
            class="demo-ruleForm"
          >
            <el-form-item label="产品名称" prop="name">
              <el-input v-model="ruleForm.name"></el-input>
            </el-form-item>
            <el-form-item label="选择模型" prop="region">
              <el-select v-model="ruleForm.region" placeholder="请选择一个数据模型">
                <el-option label="通用模型" value="shanghai"></el-option>
                <el-option label="温湿度计" value="beijing"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="活动形式" prop="desc">
              <el-input type="textarea" v-model="ruleForm.desc" class="textareainput"></el-input>
            </el-form-item>
            <el-form-item label="选择一个图标">
              <el-radio-group v-model="ruleForm.icon">
                <el-radio label="0">
                  <svg-icon icon-class="chip" />
                </el-radio>
                <el-radio label="1">
                  <svg-icon icon-class="thermometer" />
                </el-radio>
                <el-radio label="2">
                  <svg-icon icon-class="watch" />
                </el-radio>
                <el-radio label="3">
                  <svg-icon icon-class="monitor" />
                </el-radio>
                <el-radio label="4">
                  <svg-icon icon-class="gas" />
                </el-radio>
                <el-radio label="5">
                  <svg-icon icon-class="satellite" />
                </el-radio>
                <el-radio label="6">
                  <svg-icon icon-class="switcher" />
                </el-radio>
                <el-radio label="7">
                  <svg-icon icon-class="usb" />
                </el-radio>
              </el-radio-group>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="submitForm('ruleForm')">确 定</el-button>
          </div>
        </el-dialog>
      </div>
      <ul class="contentbox">
        <li v-for="productlist in productOpenlist">
          <h2 v-if=" productlist.productIcon  === '0'">
            <svg-icon icon-class="chip" />
          </h2>
          <h2 v-if=" productlist.productIcon  === '1'">
            <svg-icon icon-class="thermometer" />
          </h2>
          <h2 v-if=" productlist.productIcon  === '2'">
            <svg-icon icon-class="watch" />
          </h2>
          <h2 v-if=" productlist.productIcon  === '3'">
            <svg-icon icon-class="monitor" />
          </h2>
          <h2 v-if=" productlist.productIcon  === '4'">
            <svg-icon icon-class="gas" />
          </h2>
          <h2 v-if=" productlist.productIcon  === '5'">
            <svg-icon icon-class="satellite" />
          </h2>
          <h2 v-if=" productlist.productIcon  === '6'">
            <svg-icon icon-class="switcher" />
          </h2>
          <h2 v-if=" productlist.productIcon  === '7'">
            <svg-icon icon-class="usb" />
          </h2>
          <h3>{{ productlist.productOpenName }}</h3>
          <p>
            设备数量：
            <span>{{productlist.productCounts}}</span>
          </p>
          <p class="projectdoc">{{ productlist.productOpenDescription }}</p>
          <div class="coverbox">
            <div class="buttonbox">
              <el-tooltip class="item" effect="dark" content="点击进入管理设备" placement="left">
                <el-button type="success" icon="el-icon-edit" circle></el-button>
              </el-tooltip>
            </div>
          </div>
        </li>
      </ul>
    </el-main>
    <el-footer>©2019 All rights reserved Jacinta 呱唧物联 易用iot平台</el-footer>
  </el-container>
</template>
<script>
import axios from "axios";
import { mapGetters } from "vuex";
import Breadcrumb from "@/components/Breadcrumb";
import Hamburger from "@/components/Hamburger";
export default {
  data() {
    return {
      productOpenlist: [
        {
          productOpenName: "",
          productOpenDescription: "",
          productCounts: "",
          productIcon: ""
        }
      ],
      dialogFormVisible: false,

      ruleForm: {
        name: "",
        region: "",
        date1: "",
        date2: "",
        desc: "",
        icon: ""
      },
      rules: {
        name: [
          { required: true, message: "请输入产品名称", trigger: "blur" },
          { min: 3, max: 50, message: "长度在 3 到 50 个字符", trigger: "blur" }
        ],
        region: [{ required: true, message: "请选择模型", trigger: "change" }],
        desc: [{ required: true, message: "请填写产品说明", trigger: "blur" }]
      }
    };
  },
  created() {
    axios({
      method: "get",
      url:
        "https://www.fastmock.site/mock/13770a7dc728d98faee5bd9a77b7c945/frontend/OpenService/api_productopen_list"
    }).then(res => {
      console.log(res.data.productOpenlist),
        (this.productOpenlist = res.data.productOpenlist);
    });
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          console.log(this.ruleForm);
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    async logout() {
      await this.$store.dispatch("user/logout");
      this.$router.push(`/login?redirect=${this.$route.fullPath}`);
    }
  },
  components: {
    Breadcrumb,
    Hamburger
  },
  computed: {
    ...mapGetters(["sidebar", "avatar"])
  }
};
</script>

<style lang="scss" scoped>
$mainwdith: 1280px;
$contentheight: 100%;

.demo-ruleForm {
  padding: 20px;
  .svg-icon {
    font-size: 48px;
    margin: 10px;
    position: relative;
    top: 20px;
  }
}
.el-container {
  height: $contentheight;
}
.el-header {
  background-color: #2a3648;
  .header {
    width: $mainwdith;
    height: 40px;
    margin: 0 auto;
    color: #fff;
    .logo {
      width: 40px;
      float: left;
      margin-top: 15px;
    }
    .title {
      margin: 18px 0 0 10px;
      float: left;
      font-size: 22px;
    }
  }
}

.el-main {
  height: $contentheight;
  padding: 0;
  background-color: #f5f6fa;
  padding-bottom: 20px;
  .myprojectbox {
    background-color: #fff;
    height: 70px;
    border-bottom: 1px #efeef3 solid;
    //   box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    .myproject {
      width: $mainwdith;
      margin: 0 auto;
      line-height: 70px;
      font-size: 20px;
      .mytite {
        float: left;
      }
      .addbt {
        float: right;
        margin: 15px;
        background-color: #7db6d7;
        border: none;
      }
      .addbt:hover {
        background-color: #73abcc;
      }
    }
  }
  .contentbox {
    width: $mainwdith;
    margin: 0 auto;
    padding-bottom: 120px;
    text-align: left;
    background-color: #999;
    padding: 0;
    li {
      color: #2a3648;
      list-style: none;
      float: left;
      //   border-radius: 5px;
      background-color: #fff;
      width: $mainwdith/4-20;
      padding: 10px 30px;
      margin-right: 20px;
      margin-top: 20px;
      box-shadow: 1px 1px 1px #eee;
      height: 280px;
      position: relative;
      .coverbox {
        display: none;
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        .buttonbox {
          width: 100%;
          height: 50px;
          position: absolute;
          bottom: 0;
        }
        .el-button {
          position: relative;
          top: -10px;
          right: 20px;
        }
      }
      h2 {
        font-size: 68px;
        margin: 5px;
      }
      span {
        font-size: 22px;
        color: #b1d8e5;
      }
      h3 {
        font-size: 18px;
      }
      p {
        font-size: 14px;
        display: block;
        color: #666;
      }
      .projectdoc {
        height: 10px;
        color: #999;
      }
      .el-button {
        float: right;
        position: relative;
        bottom: -10px;
      }
    }
    li:hover {
      position: relative;
      top: -2px;
      right: -2px;
      box-shadow: 5px 5px 10px #ddd;
      //   cursor: pointer;
    }
    li:hover .coverbox {
      display: block;
    }
  }
}
.avatar-container {
  margin-right: 30px;
  float: right;

  .avatar-wrapper {
    margin-top: 10px;
    position: relative;

    .user-avatar {
      cursor: pointer;
      width: 40px;
      height: 40px;
      border-radius: 10px;
    }

    .el-icon-caret-bottom {
      cursor: pointer;
      position: absolute;
      right: -20px;
      top: 25px;
      font-size: 12px;
    }
  }
}
body > .el-container {
  margin-bottom: 40px;
  background-color: #fdfaf3;
}
.el-footer {
  text-align: center;
  height: 60px;
  font-size: 12px;
  color: #666;
  clear: both;
  background-color: #fff;
  border-top: 1px #efeef3 solid;
  line-height: 60px;
}
</style>
