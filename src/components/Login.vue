<template>
  <div class="login-container">
    <div class="login-box">
      <!-- 头像 -->
      <div class="avatar-box">
        <img src="../assets/logo.png" alt="" />
      </div>
      <!-- 登录表单 -->
      <el-form
        ref="loginFormRef"
        class="login-form"
        :model="loginForm"
        :rules="loginFormRules"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="iconfont icon-users"
          >
          </el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            type="password"
            prefix-icon="iconfont icon-3702mima"
          >
          </el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="reset">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //登录表单
      loginForm: {
        username: "admin",
        password: "123456"
      },
      //校验规则
      loginFormRules: {
        username: [
          { required: true, message: "请输入登录名称", trigger: "blur" },
          { min: 3, max: 6, message: "长度在 3 到 6 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 15, message: "长度在 6 到 15 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return;
        const { data: res } = await this.$http.post("login", this.loginForm);
        if (res.meta.status != 200) return this.$message.error("登录失败");
        this.$message.success("登录成功");
        window.sessionStorage.setItem("token", res.data.token);
        this.$router.push("/home");
      });
    },
    reset() {
      this.$refs.loginFormRef.resetFields();
    }
  }
};
</script>

<style lang="less" scoped>
.login-container {
  height: 100%;

  background-color: #2b4b6b;
}

.login-box {
  position: absolute;
  top: 50%;
  left: 50%;

  border-radius: 5px;
  width: 450px;
  height: 300px;

  background: #fff;

  transform: translate(-50%, -50%);

  .avatar-box {
    position: absolute;
    left: 50%;

    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    width: 130px;
    height: 130px;

    background-color: #fff;

    box-shadow: 0 0 10px #ddd;

    transform: translate(-50%, -50%);

    img {
      border-radius: 50%;
      width: 100%;
      height: 100%;

      background-color: #eee;
    }
  }
}

.login-form {
  position: absolute;
  bottom: 0;

  box-sizing: border-box;
  padding: 0 20px;
  width: 100%;
}

.btns {
  display: flex;

  justify-content: flex-end;
}
</style>
