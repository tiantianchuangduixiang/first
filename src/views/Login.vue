<template>
  <div class="login fillcontain">
    <div class="login_page">
      <el-form :model="loginForm" :rules="rules" ref="loginForm">
        <p class="title">
          <span>系统登录</span>
        </p>
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            placeholder="用户名"
            prefix-icon="el-icon-user"
          ></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            type="password"
            placeholder="密码"
            v-model="loginForm.password"
            prefix-icon="el-icon-goods"
            show-password
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button
            type="primary"
            @click="submitForm('loginForm')"
            class="submit_btn"
            v-loading="loading"
            >登陆
          </el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  name: "login",
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
      },
      loading: false,
    };
  },
  methods: {
    submitForm(formName) {
      this.loading = true;
      this.$http({
        method: "post",
        url: "/api/login",
        data: {
          userName: this.loginForm.username,
          passWord: this.loginForm.password,
        },
      }).then((res) => {
        if (res.data == true) {
          this.$message("登陆成功");
          setTimeout(() => {
            // 加载延迟
            this.loading = false;

            // 跳转页面
            this.$router.push({ path: "/mainPage" });

            // 页面缓存
            sessionStorage.setItem("user", this.numberValidateForm);
          }, 3000);
        } else {
          this.loading = false;
          this.$message("登陆失败");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },
};
</script>
<style scoped>
.login {
  background-color: #1f222b;
}
.login_page {
  border: 1px red solid;
  background-color: white;
  width: 400px;
  height: 250px;
  margin: 0 auto;
  position: absolute;
  left: 700px;
  top: 280px;
  padding: 50px;
}
.submit_btn {
  width: 100%;
  font-size: 16px;
  margin-top: 40px;
}
.title {
  font-size: 18px;
  margin-left: 160px;
  margin-bottom: 40px;
}
</style>