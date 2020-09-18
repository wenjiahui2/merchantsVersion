<template>
  <div class="box">
    <div class="pass">
      <div class="logo">找回密码</div>
      <div class="input">
        <el-form ref="ruleForm" :model="ruleForm" status-icon :rules="rules2" label-width="90px" class="demo-ruleForm">
          <el-form-item label="手机号" prop="phone">
            <el-input v-model="ruleForm.pass" type="tel" autocomplete="off" />
          </el-form-item>
          <el-form-item label="验证码" prop="verify">
            <el-row>
              <el-col :span="16"><el-input v-model="ruleForm.verify" /></el-col>
              <el-col :span="6" :offset="1"><div class="verifystyle">11111</div></el-col>
            </el-row>

          </el-form-item>
          <el-form-item label="密码" prop="verify">
            <el-input v-model="ruleForm.pass" type="password" autocomplete="off" />
          </el-form-item>
          <el-form-item label="确认新密码" prop="checkPass">
            <el-input v-model="ruleForm.checkPass" type="password" autocomplete="off" />
          </el-form-item>
        </el-form>
        <div class="input1">
          <el-button class="yespass" @click.native="submitForm('ruleForm2')">确认</el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Password',
  data() {
    return {
      ruleForm: {
        pass: '',
        checkPass: '',
        name: '',
        verify: ''
      },
      rules2: {
        pass: [{ required: true, message: '请输入活动名称', trigger: 'blur' }],
        name: [{ required: true, message: '请输入活动名称', trigger: 'blur' }]
      }
    }
  },
  methods: {
    submitForm(formName) {
    //   this.$refs[formName].validate(valid => {
    //     if (valid) {
    //       this.$router.push('home')
    //     } else {
    //       console.log("error submit!!");
    //       return false;
    //     }
    //   });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped >
.box{
  width: 100%;
  height: 100%;
  overflow: hidden;
  background:rgba(106, 126, 153, 1);
}
.pass{
  width: 516px;
  height: 516px;
  background-color: #fff;
  margin: 0 auto;
  margin-top: 50px;
  overflow: hidden;
}
.logo{
  width:322px;
  margin: 0 auto;
  margin-top: 30px;
  color: rgba(59, 177, 165, 1);
  font-size: 32px;
  text-align: center;
}

.input{
  width: 335px;
  margin: 0 auto;
  margin-top: 65px;
}

.verifystyle{
    width: 120px;
    height: 40px;
}
.input1{
    width: 332px;
    margin: 0 auto;
    margin-top: 30px;
}
.yespass{
  width: 100%;
  margin: 0 auto;
  background:rgba(26, 188, 156, 1);
  color: #fff;
}
</style>
