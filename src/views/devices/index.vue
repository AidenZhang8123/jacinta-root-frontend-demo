<template>
  <el-main>
    <div class="myprojectbox">
      <div class="myproject">
        <div class="mytite">设备列表</div>
      </div>
    </div>
    <div class="app-container">
      <el-dialog
  title="提示"
  :visible.sync="dialogVisible"
  width="30%"
  :before-close="handleClose">
  <el-form :model="adddeviceform">
    <el-form-item label="nodeID" :label-width="formLabelWidth">
      <el-input v-model="adddeviceform.nodeid" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="IMSI" :label-width="formLabelWidth">
      <el-input v-model="adddeviceform.imsi" autocomplete="off"></el-input>
    </el-form-item>
  </el-form>
  <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
</el-dialog>
<el-row>
  <el-col :span="1"><div class="grid-content">
    <div class="deviceimg"><svg-icon icon-class="chip" /></div>
    </div></el-col>
    <el-col :span="11"><div class="grid-content">
    <div class="infotittle">
            <h4>呱唧室内温度计</h4>
          <p>设备数量：1</p>
          </div>
    </div></el-col>
  <el-col :span="10"><div class="grid-content">
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
  <el-form-item label="">
    <el-input v-model="formInline.nodeId" placeholder="nodeID 查询"></el-input>
  </el-form-item>
  <el-form-item label="">
    <el-input v-model="formInline.deviceImsi" placeholder="IMSI 查询"></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary">查询</el-button>
  </el-form-item>
</el-form>
    </div></el-col>
  <el-col :span="2"><div class="grid-content">
    <el-button @click="dialogVisible = true" style="float:right" type="primary" icon="el-icon-plus">添加设备</el-button>
    </div></el-col>
</el-row>

      <el-table :data="tableData" border style="width: 100%">
        <el-table-column prop="order" label="序号" width="100"/>
        <el-table-column prop="nodeId" label="nodeID"  />
        <el-table-column prop="deviceImsi" label="IMSI" />
        <el-table-column
          prop="deviceState"
          label="设备状态"
          width="180"
          :filters="[{ text: '未知', value: '0' },{ text: '拒绝', value: '1' },{ text: '已加', value: '2' },{ text: '有意', value: '3' }]"
          :filter-method="filterTag"
          filter-placement="bottom-end"
        >
          <template slot-scope="scope">
            <el-tag v-if="scope.row.deviceState === '0'" :type="'danger'" disable-transitions>
              <i class="el-icon-error" /> 未启用
            </el-tag>
            <el-tag v-else-if="scope.row.deviceState === '1'" :type="'success'" disable-transitions>
              <i class="el-icon-success" /> 启用中
            </el-tag>
            <el-tag v-if="scope.row.deviceState === '2'" :type="'info'" disable-transitions>
              <i class="el-icon-question" /> 未知
            </el-tag>
          </template>
        </el-table-column>
        <el-table-column prop="creationTime" label="地址" />
        <el-table-column prop="deviceTimeout" label="客服" />
        <el-table-column prop="deviceUptime" label="日期" width="180" />
      </el-table>
      <div class="pagebox">
        <el-pagination
          :current-page="pageNumber"
          :page-sizes="pageSizes"
          :page-size="PageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="totalCount"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        />
      </div>
    </div>
  </el-main>
</template>
    
<script>
// import { gettable } from '@/api/table'
import axios from "axios";
export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: "success",
        draft: "gray",
        deleted: "danger"
      };
      return statusMap[status];
    },
    weixinFilter(status) {}
  },
  data() {
    return {
      adddeviceform:{
        nodeid:'',
        imsi:''
      },

      //  formLabelWidth: '120px',
      dialogVisible: false,
      deviceApi:'http://www.guaji.com/deviceAPI/234KJASFASDF2344',
      formInline: {
          user: '',
          region: ''
        },
      // 总数据
      tableData: [],
      // 默认显示第几页
      pageNumber: 1,
      // 总条数，根据接口获取数据长度(注意：这里不能为空)
      totalCount: 1,
      // 个数选择器（可修改）
      pageSizes: [10, 20, 30, 40],
      // 默认每页显示的条数（可修改）
      PageSize: 10,
      list2: null,
      listLoading: true
    };
  },
  created() {
    this.getData();
  },
  created: function() {
    this.getData(this.PageSize, this.pageNumber);
  },
  methods: {
    // 将页码，及每页显示的条数以参数传递提交给后台
    getData(n1, n2) {
      axios
        .post(
          "https://www.fastmock.site/mock/13770a7dc728d98faee5bd9a77b7c945/frontend/Openservice/api_devices",
          {
            // 页数
            // pageNumber: 1,
            // 每页显示的条数
            pageSize: n1,
            // 显示第几页
            pageNumber: n2
          },
          { emulateJSON: true },
          {
            headers: {
              "Content-Type": "application/x-www-form-urlencoded;charset=utf-8"
            }
          }
        )
        .then(response => {
          console.log(response);
          // 将数据赋值给tableData
          this.tableData = response.data.devices;
          // 将数据的长度赋值给totalCount
          this.totalCount = response.data.pageCount;
        });
    },
    // 分页
    // 每页显示的条数
    handleSizeChange(val) {
      // 改变每页显示的条数
      this.PageSize = val;
      // 点击每页显示的条数时，显示第一页
      this.getData(val, 1);
      // 注意：在改变每页显示的条数时，要将页码显示到第一页
      this.pageNumber = 1;
    },
    // 显示第几页
    handleCurrentChange(val2) {
      // 改变默认的页数
      this.pageNumber = val2;
      // 切换页码时，要获取每页显示的条数
      // this.getData(this.PageSize, val2 * this.pageSize);
      this.getData(this.PageSize, this.pageNumber);
      // console.log(`当前页: ${val2}`);
    },
    filterTag(value, row) {
      return row.deviceState === value;
    }
  }
};
</script>
<style lang="scss" scoped>
$mainwdith: 1280px;
$contentheight: 100%;
.app-container {
  width: $mainwdith;
  background-color: #fff;
  margin: 0 auto;
  padding: 30px;
  margin-top:20px;
  box-shadow: 1px 1px 1px #eee;
  .deviceimg{
    font-size: 42px;
  }
  .infotittle{
    position: relative;
      top:-15px;
      left: 5px;
      margin-bottom: -25px;
    h4{      
      font-size: 18px;
      color:#73abcc;
      position: relative;
      top:-5px;
    }
    p{
      font-size: 12px;
      color: #555;
      position: relative;
      top: -20px;
    }
  }
}
.pagebox {
  margin-top: 10px;
}
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
</style>
