<template>
<div class="content">
<div class="login">
    <el-form ref="ruleForm" class="demo-ruleForm" :model="userform" :rules="rules">
      <el-form-item prop="username">
           <!-- 二次验证 取消错误提示给input添加 -->
        <el-input placeholder="请输入用户名"
         v-model="userform.username"
          @focus="Validate('username')">
        </el-input>
      </el-form-item>
      <el-form-item prop="password">

        <el-input
          type="password"
          placeholder="请输入密码"
          v-model="userform.password"
          @focus="Validate('password')"
        ></el-input>
      </el-form-item>
       <el-form-item prop="username">
           <!-- 二次验证 取消错误提示给input添加 -->
        <el-input placeholder="请输入用户名"
         v-model="userform.username"
          @focus="Validate('username')">
        </el-input>
      </el-form-item>
       <el-form-item prop="username">
           <!-- 二次验证 取消错误提示给input添加 -->
        <el-input placeholder="请输入用户名"
         v-model="userform.username"
          @focus="Validate('username')">
        </el-input>
      </el-form-item>
      <p class="p">忘记密码？</p>
      <el-button type="primary" style="width:100%" @click="login">登录</el-button>
    </el-form>
  </div>

  </div>
  
  

</template>

<script>
export default {
  data() {
    return {
      userform: {
        username: "",
        password: "",
      },
      rules: {
        username: [
          { required: true, message: "请输入正确的用户名", trigger: "blur" },
          {
            pattern: /^\w+$/,
            message: "用户名格式错误",
            trigger: "blur",
          },
        ],
        password: [
          { required: true, message: "请输入正确密码", trigger: "blur" },
          { pattern: /^\w+$/, message: "密码格式错误", trigger: "blur" },
        ],
      },
    };
  },
  methods: {
    login() {
        // 对整个表单的验证  满足条件返回true
      this.$refs.ruleForm.validate((result) => {
        console.log(result);
        if (result) {
            // 验证成功够调用 接口
          this.$axios({
            url: "/accounts/login",
            method: "post",
            data: this.userform,
          })
            .then((res) => {
              console.log(res);
              this.$message({
                showClose: true,
                message: "登录成功",
                type: "success",
              });
              this.$router.push('/')
            })
            .catch((ress) => {
              this.$message({
                showClose: true,
                message: "输入错误",
                type: "warning",
              });
            });
        }
      });
    },
    //   鼠标焦点时 去除错误提示
    Validate(aa) {
      this.$refs.ruleForm.clearValidate(aa);
    },
  },
};
</script>

<style lang="less" scoped>
.demo-ruleForm {
  padding: 20px;
  .p {
    text-align: right;
    font-size: 10px;
    color: blue;
    margin-bottom: 10px;
    cursor: pointer;
  }
}
</style>