<template>
  <div>
    <div class="box">
      <breadcrumb id="breadcrumb-container" class="breadcrumb-container" />
    </div>
    <div class="inquire">
      <div class="box-name">
        <el-form :inline="true" :model="formInline" class="demo-form-inline">
          <el-form-item label="菜品名称">
            <el-input v-model="formInline.user" placeholder="菜品名称"></el-input>
          </el-form-item>
          <el-form-item label="商品分类">
            <el-select v-model="formInline.region" placeholder="请选择商品分类">
              <el-option label="区域一" value="shanghai"></el-option>
              <el-option label="区域二" value="beijing"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit">查询</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
    <div class="content">
      <el-row class="content_top">
        <el-col :span="4" style="line-height: 34px;">商品数量: 125件</el-col>
        <el-col :offset="18" :span="2">
          <el-button type="primary" color="#7764ca" @click="handleCreate">新增</el-button>
        </el-col>
      </el-row>
      <el-table :key="tableKey" :data="list" border fit highlight-current-row style="width: 100%;">
        <!-- @sort-change="sortChange" -->

        <el-table-column label="编号" prop="id" sortable="custom" align="center" style="width: 10%;">
          <!-- :class-name="getSortClass('id')" -->
          <template slot-scope="{row}">
            <span>{{ row.id }}</span>
          </template>
        </el-table-column>
        <el-table-column label="菜品图片" style="width: 10%;" align="center">
          <template slot-scope="{row}">
            <span >{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="菜品名称" style="width: 10%;" align="center">
          <template slot-scope="{row}">
            <span >{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="熟食抢购" style="width: 8%;" align="center">
          <template slot-scope="{row}">
            <span >{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="价格" style="width: 8%;" align="center">
          <template slot-scope="{row}">
            <span >{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="数量" style="width: 8%;" align="center">
          <template slot-scope="{row}">
            <span >{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="排序" style="width: 8%;" align="center">
          <template slot-scope="{row}">
            <span >{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="Actions" align="center" width="228px" class-name="small-padding fixed-width">
        <template slot-scope="{row,$index}">
          <el-button type="primary" size="mini" @click="handleUpdate(row)">
            编辑
          </el-button>
          <el-button size="mini" @click="handleLook(row,'draft')">
            查看
          </el-button>
          <el-button  size="mini" type="danger" @click="handleDelete(row,$index)">
            删除
          </el-button>
        </template>
      </el-table-column>
      </el-table>
      <pagination v-show="total>0" :total="total"  :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />
    </div>
   
    <!-- v-loading="listLoading" -->
    <!-- <div class="content">
     

    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />
</div>-->
    <el-dialog :visible.sync="dialogPvVisible" title="Reading statistics">
      <el-table :data="pvData" border fit highlight-current-row style="width: 100%">
        <el-table-column prop="key" label="Channel" />
        <el-table-column prop="pv" label="Pv" />
      </el-table>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogPvVisible = false">Confirm</el-button>
      </span>
    </el-dialog>
    
  </div>
</template>

<script>
import NewDishes from './NewDishes';
import Breadcrumb from "@/components/Breadcrumb";
import { path } from '@/utils/routing'
import {
  fetchList,
  fetchPv,
  createArticle,
  updateArticle,
} from "@/api/article";
import waves from "@/directive/waves"; // waves directive
import { parseTime } from "@/utils";
import Pagination from "@/components/Pagination"; // secondary package based on el-pagination


export default {
  name: "Dishes",
  components: { Breadcrumb, Pagination, NewDishes },
  directives: { waves },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: "success",
        draft: "info",
        deleted: "danger",
      };
      return statusMap[status];
    },
    typeFilter(type) {
      return calendarTypeKeyValue[type];
    },
  },
  data() {
    return {
      formInline: {
        user: "",
        region: "",
      },
      tableKey: 0,
      list: [
        {
          id: 1,
          reviewer:123,

        },
      ],
      total: 100,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 20,
        importance: undefined,
        title: undefined,
        type: undefined,
        sort: "+id",
      },
      // importanceOptions: [1, 2, 3],
      // sortOptions: [
      //   { label: "ID Ascending", key: "+id" },
      //   { label: "ID Descending", key: "-id" },
      // ],
      // showReviewer: false,
      // temp: {
      //   id: undefined,
      //   importance: 1,
      //   remark: "",
      //   timestamp: new Date(),
      //   title: "",
      //   type: "",
      //   status: "published",
      // },
      // dialogFormVisible: false,
      // dialogStatus: "",
      
      dialogPvVisible: false,
      pvData: [],
      
      downloadLoading: false,
    };
  },
  methods: {
    onSubmit() {
      console.log("submit!");
    },
    getList() {
      this.listLoading = true;
      fetchList(this.listQuery).then((response) => {
        this.list = response.data.items;
        this.total = response.data.total;
        this.listLoading = false;

        // Just to simulate the time of the request
        setTimeout(() => {}, 1.5 * 1000);
      });
    },
    handleFilter() {
      this.listQuery.page = 1;
      this.getList();
    },
    handleModifyStatus(row, status) {
      this.$message({
        message: "操作Success",
        type: "success",
      });
      row.status = status;
    },
    sortChange(data) {
      const { prop, order } = data;
      if (prop === "id") {
        this.sortByID(order);
      }
    },
    sortByID(order) {
      if (order === "ascending") {
        this.listQuery.sort = "+id";
      } else {
        this.listQuery.sort = "-id";
      }
      this.handleFilter();
    },
    handleCreate() {
      this.$router.push({
        path:path.DISHES.ADD
      })
    },
    handleUpdate(row) {   //编辑
      this.$router.push({
        path:path.DISHES.ADD,
        query: {
          row: JSON.stringify(row),
          Status: 'create'
        }
      })
      // this.temp = Object.assign({}, row); // copy obj
      // this.$refs.newDishes.handleUpdate(row)
    },
    handleLook(){   //查看
    },
    handleDelete(row, index) { //删除
      this.$notify({
        title: "Success",
        message: "Delete Successfully",
        type: "success",
        duration: 2000,
      });
      this.list.splice(index, 1);
    },
    handleFetchPv(pv) {
      fetchPv(pv).then((response) => {
        this.pvData = response.data.pvData;
        this.dialogPvVisible = true;
      });
    },
    handleDownload() {
      this.downloadLoading = true;
      import("@/vendor/Export2Excel").then((excel) => {
        const tHeader = ["timestamp", "title", "type", "importance", "status"];
        const filterVal = [
          "timestamp",
          "title",
          "type",
          "importance",
          "status",
        ];
        const data = this.formatJson(filterVal);
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: "table-list",
        });
        this.downloadLoading = false;
      });
    },
    formatJson(filterVal) {
      return this.list.map((v) =>
        filterVal.map((j) => {
          if (j === "timestamp") {
            return parseTime(v[j]);
          } else {
            return v[j];
          }
        })
      );
    },
    getSortClass: function (key) {
      const sort = this.listQuery.sort;
      return sort === `+${key}` ? "ascending" : "descending";
    },
  },
  computed: {},
  created() {},
};
</script>
<style lang="scss" scoped>
.box {
  width: 98%;
  height: 45px;
  background-color: #e9eff5;
  margin: 10px 0px 0px 0px;
  margin-left: 1%;
}
.content {
  width: 98%;
  background-color: #e9eff5;
  margin: 10px 0px 0px 0px;
  margin-left: 1%;
  .content_top{
    padding: 10px 0 10px 10px;
    color: #7764ca;
    background: #fff;
    .el-button--primary{
      background:#7764ca;
    }
  }
}
.inquire {
  width: 98%;
  height: 60px;
  background-color: #fff;
  margin-left: 1%;
  .box-name {
    display: inline-block;
    height: 39px;
    margin-top: 10px;
    margin-left: 29px;
  }
  .el-form-item {
    margin-bottom: 0;
  }
}
</style>
