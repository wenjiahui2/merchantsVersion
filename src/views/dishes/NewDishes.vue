<template>
  <div class="newDishes">
    <div class="box">
      <breadcrumb id="breadcrumb-container" class="breadcrumb-container" />
      <!-- <span>新建菜品</span> -->
    </div>
    <div class="box-from">
      <el-form
        ref="ruleForm"
        :model="ruleForm"
        :rules="rules"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="菜品名称" prop="name">
          <el-input v-model="ruleForm.name" />
        </el-form-item>
        <el-form-item label="菜品分类" prop="region">
          <el-select v-model="ruleForm.region" placeholder="菜品分类" style="width:100%">
            <el-option label="区域一" value="shanghai" />
            <el-option label="区域二" value="beijing" />
          </el-select>
        </el-form-item>
        <el-form-item label="菜品介绍" prop="desc">
          <el-input v-model="ruleForm.desc" type="textarea" :autosize="{ minRows: 2, maxRows: 4}" />
        </el-form-item>
        <el-form-item label="是否抢购" prop="resource">
          <el-radio-group v-model="ruleForm.resource">
            <el-radio label="是" />
            <el-radio label="否" />
          </el-radio-group>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  /** 新增菜品 */
  name: 'NewDishes',
  data() {
    return {
      ruleForm: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      rules: {
        name: [
          { required: true, message: '请输入活动名称', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        region: [
          { required: true, message: '请选择活动区域', trigger: 'change' }
        ],
        date1: [
          { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
        ],
        date2: [
          { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
        ],
        type: [
          { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
        ],
        resource: [
          { required: true, message: '请选择是否抢购', trigger: 'change' }
        ],
        desc: [
          { required: true, message: '请填写活动形式', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
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
  line-height: 45px;
  span{
    margin-left: 20px;
  }
}
.box-from{
  width: 98%;
  margin-left: 1%;
  margin-top: 20px;
  background-color: #fff;
  padding: 20px 0;
}
.demo-ruleForm{
  width: 60%;
}
</style>
