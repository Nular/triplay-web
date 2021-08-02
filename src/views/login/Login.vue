<template>
  <div class="login-page">
    <img class="bg" src="@/assets/img/bg_login.svg" alt="bg_login" />
    <!-- <van-nav-bar
      class="app-nav-bar"
      title="注册 / 登录"
      left-arrow
      @click-left="$router.back()"
    /> -->
    <img class="logo" src="@/assets/img/logo.png" alt="logo" />
    <van-form
      ref="loginForm"
      @submit="login"
      @falied="onFailed"
      :show-error="false"
      :show-error-message="false"
      validate-first
    >
      <van-cell-group class="login-cell-group" inset>
        <van-field
          v-model="user.mobile"
          :rules="loginRules.moibile"
          placeholder="请输入手机号"
          label="邮箱"
          name="email"
        />
        <van-field
          v-model="user.code"
          clearable
          :rules="loginRules.code"
          placeholder="请输入验证码"
          label="密码"
          name="code"
        >
        </van-field>
      </van-cell-group>
      <div class="login-btn-wrap">
        <van-button block round type="info" class="login-btn">登录</van-button>
      </div>
    </van-form>

    <van-divider>快捷登录</van-divider>
    <van-row>
      <van-col span="6"></van-col>
      <van-col span="6"
        ><img
          class="icon_fastLogin"
          src="@/assets/img/icon_tripPal.png"
          alt="bg_tripPal"
      />
      <p class="text_fastLogin">TripPal</p></van-col>
      <van-col span="6"
        ><img
          class="icon_fastLogin"
          src="@/assets/img/icon_ctrip.png"
          alt="bg_ctrip"
      />
      <p class="text_fastLogin">SSO</p></van-col></van-col>
      <van-col span="6"></van-col>
    </van-row>
  </div>
</template>

<script>
import { loginApi, sendCodeApi } from "@/api/user";
import { loginRules } from "@/utils/validateRules";
export default {
  name: "Login",
  data() {
    return {
      user: {
        mobile: "17090086870",
        code: "246810",
      },
      loginRules,
      isCountDown: false,
      isSendLoad: false,
    };
  },
  methods: {
    // 登录
    async login() {
      this.$toast.loading({
        message: "登录中...",
        forbidClick: true,
        duration: 0,
      });
      try {
        const { data } = await loginApi(this.user);
        if (data.data.token) {
          this.$toast.success("登陆成功");
          this.$store.commit("SET_USER", data.data.token);
          this.$store.commit("SET_REFRESH", data.data.refresh_token);
          // this.$router.push(this.$route.query.redirect || '/')
          this.$router.push("/");
        }
      } catch (error) {
        this.$toast.fail("登录失败，手机号或验证码错误");
      }
    },
    onFailed(error) {
      if (error.errors[0]) {
        this.$toast({
          message: error.errors[0].message,
          position: "top",
        });
      }
    },
    // 发送验证码
    async sendCode() {
      try {
        await this.$refs["loginForm"].validate("mobile");
        this.isSendLoad = true;
        await sendCodeApi(this.user.mobile);
        this.isCountDown = true;
      } catch (err) {
        let message = "";
        console.log(err);
        if (err && err.response && err.response.status === 429) {
          message = "发送太频繁了，请稍后重试";
        } else if (err.name === "mobile") {
          message = err.message;
        } else {
          message = "发送失败，请稍后重试";
        }
        this.$toast({
          message,
          position: "top",
        });
      }
      this.isSendLoad = false;
    },
  },
};
</script>

<style lang="less">
.login-page {
  .bg {
    position: absolute;
    bottom: 0;
    left: 5%;
    width: 90%;
    z-index: -1;
  }
  .logo {
    position: relative;
    margin-top: 25%;
    left: 15%;
    width: 70%;
  }
  .login-cell-group {
    margin: 0.75rem 0.875rem;
  }
  .login-btn-wrap {
    padding: 26px 16px;
    .login-btn {
      width: 80%;
      margin: 0 auto;
      background-color: #1989fa;
      border: none;
      .van-button__text {
        font-size: 15px;
      }
    }
  }
  .van-row {
    text-align: center;
    .icon_fastLogin {
      width: 50%;
    }
    .text_fastLogin{
      font-size: 0.373rem;
      color: #969799;
      margin: 0;
    }
  }
}
</style>