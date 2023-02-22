<template>
  <div id="app">
    <!-- 卡片 -->
    <el-card class="login-card">
      <!-- 表单容器 -->
      <el-form
        style="margin-top: 50px"
        :model="loginForm"
        :rules="loginRules"
        ref="loginForm"
      >
        <!-- 表单项 -->
        <el-form-item prop="mobile">
          <!-- 输入框 -->
          <el-input
            placeholder="请输入账号"
            v-model="loginForm.mobile"
          ></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            placeholder="请输入密码"
            v-model="loginForm.password"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" style="width: 100%" @click="login"
            >登录</el-button
          >
          <el-button type="primary" @click="test">测试async</el-button>
          <el-button type="primary" @click="test1">测试async1</el-button>
          <el-button type="primary" @click="test2">测试async1</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    const checkMobile = function (rule, value, callback) {
      value.charAt(2) === "9"
        ? callback()
        : callback(new Error('"第三位数必须是9"'));
    };
    return {
      loginForm: {
        mobile: "",
        password: "",
      },
      loginRules: {
        mobile: [
          { required: true, message: "手机号不能为空", trigger: "blur" },
          {
            pattern: /^1[3-9]\d{9}/,
            message: "号码不符合规则",
            trigger: "blur",
          },
          { trigger: "blur", validator: checkMobile },
        ],
        password: [
          { required: true, message: "密码不能为空", trigger: "blur" },
          {
            min: 6,
            max: 16,
            trigger: "blur",
            message: "密码必须为6-16位的数字和字母组合",
          },
        ],
      },
    };
  },
  methods: {
    login() {
      this.$refs.loginForm.validate((isOk) => {
        if (isOk) {
          console.log("通过");
        } else {
          console.log("失败");
        }
      });
    },
    async test() {
      const result = await new Promise(function (resolve) {
        setTimeout(function () {
          resolve(100);
        }, 5000);
      });
      alert(result);
    },
    async test1() {
      await this.test();
      alert(1);
    },
    async test2() {
      try {
        const result = await new Promise(function (reject) {
          setTimeout(function () {
            reject(100);
          }, 5000);
        })
        alert(result)
      } catch (error) {
        alert(error);
      }
    }
  },
};
</script>

<style >
#app {
  width: 100%;
  height: 100vh;
  background-color: pink;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-card {
  width: 400px;
  height: 300px;
}
</style>