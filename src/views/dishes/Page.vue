<template>
  <div>
    <div class="box">
      <breadcrumb id="breadcrumb-container" class="breadcrumb-container" />
    </div>
    <div class="inquire">
      <div class="box-name">
        <el-form :inline="true" :model="formInline" class="demo-form-inline">
          <el-form-item label="菜品名称">
            <el-input v-model="formInline.user" placeholder="菜品名称" />
          </el-form-item>
          <el-form-item label="商品分类">
            <el-select v-model="formInline.region" placeholder="请选择商品分类">
              <el-option label="区域一" value="shanghai" />
              <el-option label="区域二" value="beijing" />
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit">查询</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
    <div class="content-top">
      <el-row :gutter="20">
        <el-col :span="6" :offset="1">商品数量：  135件</el-col>
        <el-col :span="3" :offset="13">
          <el-button type="primary" size="mini" @click="newDishes">
            新建
          </el-button>
        </el-col>
      </el-row>
    </div>
    <div class="content">
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
            <span>{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="菜品名称" style="width: 10%;" align="center">
          <template slot-scope="{row}">
            <span>{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="熟食抢购" style="width: 8%;" align="center">
          <template slot-scope="{row}">
            <span>{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="价格" style="width: 8%;" align="center">
          <template slot-scope="{row}">
            <span>{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="数量" style="width: 8%;" align="center">
          <template slot-scope="{row}">
            <span>{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="排序" style="width: 8%;" align="center">
          <template slot-scope="{row}">
            <span>{{ row.reviewer }}</span>
          </template>
        </el-table-column>
        <el-table-column label="操作" align="center" width="228px" class-name="small-padding fixed-width">
          <template slot-scope="{row,$index}">
            <el-button type="primary" size="mini" @click="handleUpdate(row)">
              编辑
            </el-button>
            <el-button size="mini" @click="handleLook(row,'draft')">
              查看
            </el-button>
            <el-button size="mini" type="danger" @click="handleDelete(row,$index)">
              删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
      <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />
    </div>
    <!-- v-loading="listLoading" -->
  </div>
</template>

<script>
import Breadcrumb from '@/components/Breadcrumb'
import {
  fetchList,
  fetchPv,
  createArticle,
  updateArticle
} from '@/api/article'
import waves from '@/directive/waves' // waves directive
import { parseTime } from '@/utils'
import Pagination from '@/components/Pagination' // secondary package based on el-pagination

export default {
  /** 菜品管理 */
  name: 'Dishes',
  components: { Breadcrumb, Pagination },
  directives: { waves },
  data() {
    return {
      formInline: {
        user: '',
        region: ''
      },
      tableKey: 0,
      list: [
        {
          id: 1,
          reviewer: 123

        }
      ],
      total: 100,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 20,
        importance: undefined,
        title: undefined,
        type: undefined,
        sort: '+id'
      },
      temp: {
        id: undefined,
        importance: 1,
        remark: '',
        timestamp: new Date(),
        title: '',
        type: '',
        status: 'published'
      }
    }
  },
  computed: {},
  created() {},
  methods: {
    newDishes() {
      console.log(11111)
      this.$router.push('newDishes')
    },
    onSubmit() {
      console.log('submit!')
    },
    getList() {
      this.listLoading = true
      fetchList(this.listQuery).then((response) => {
        this.list = response.data.items
        this.total = response.data.total
        this.listLoading = false

        // Just to simulate the time of the request
        setTimeout(() => {}, 1.5 * 1000)
      })
    },
    handleFilter() {
      this.listQuery.page = 1
      this.getList()
    },
    sortChange(data) {
      const { prop, order } = data
      if (prop === 'id') {
        this.sortByID(order)
      }
    },
    sortByID(order) {
      if (order === 'ascending') {
        this.listQuery.sort = '+id'
      } else {
        this.listQuery.sort = '-id'
      }
      this.handleFilter()
    },
    resetTemp() {
      this.temp = {
        id: undefined,
        importance: 1,
        remark: '',
        timestamp: new Date(),
        title: '',
        status: 'published',
        type: ''
      }
    },
    handleCreate() {
      this.resetTemp()
      this.$nextTick(() => {
        this.$refs['dataForm'].clearValidate()
      })
    },
    handleUpdate(row) { // 编辑
      this.temp = Object.assign({}, row) // copy obj
      this.temp.timestamp = new Date(this.temp.timestamp)
      this.$nextTick(() => {
        this.$refs['dataForm'].clearValidate()
      })
    },
    handleDelete() { // 查看

    },
    handleDelete(row, index) { // 删除
      this.$notify({
        title: 'Success',
        message: 'Delete Successfully',
        type: 'success',
        duration: 2000
      })
      this.list.splice(index, 1)
    },
    getSortClass: function(key) {
      const sort = this.listQuery.sort
      return sort === `+${key}` ? 'ascending' : 'descending'
    }
  }
}
</script>
<style lang="scss" scoped>
.box {
  width: 98%;
  height: 45px;
  background-color: #e9eff5;
  margin: 10px 0px 0px 0px;
  margin-left: 1%;
}
.content-top{
  width: 98%;
  height: 70px;
  background-color: #fff;
  margin: 10px 0px 0px 0px;
  margin-left: 1%;
  line-height: 70px;
}
.content {
  width: 98%;
  background-color: #e9eff5;
  margin: 10px 0px 0px 0px;
  margin-left: 1%;
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
