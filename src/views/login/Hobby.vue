<template>
  <div class="hobby-page">
    <img class="bg" src="@/assets/img/bg_hobby.svg" alt="hobby" />
    <div class="hobby-form">
      <b>选择你的兴趣爱好：</b>
      <van-button
        plain
        round
        type="info"
        v-for="hobby in hobbyList"
        :key="hobby"
        >{{ hobby }}
      </van-button>
      <p @click="getHobby">换一批</p>
    </div>

    <van-button class="btn-enter" block round type="info" @click="enter"
      >进入</van-button
    >
    <p class="btn-skip" block @click="skip">下次再说</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      hobbyList: [
        "篮球",
        "羽毛球",
        "乒乓球",
        "网球",
        "瑜伽",
        "滑板",
        "狼人杀",
        "剧本杀",
        "三国杀",
        "飞行棋",
        "象棋",
      ],
    };
  },
  methods: {
    getHobby() {},
    enter() {},
    skip() {
      this.$router.push("/");
    },
  },
};
</script>
<style lang="less" scoped>
.hobby-page {
  .bg {
    position: absolute;
    bottom: 0;
    left: 5%;
    width: 90%;
    z-index: -1;
  }
  .hobby-form {
    margin: 2rem 1rem 0 1rem;
    text-align: center;
    b {
      text-align: left;
      display: block;
      font-weight: 500;
      font-size: 20px;
      color: #797979;
      margin-bottom: 1rem;
    }
    .van-button {
      height: 0.8rem;
      margin: 0 0.25rem;
    }
    p {
      display: block;
      width: 30%;
      cursor: pointer;
      margin: 0.5rem auto 1rem;
      font-size: 14px;
      color: #1989fa;
    }
  }
  .btn-enter {
    width: 80%;
    margin: 0 auto;
  }
  .btn-skip {
    display: block;
    width: 30%;
    margin: 0.25rem auto;
    cursor: pointer;
    text-align: center;
    font-size: 14px;
    color: #797979;
  }
}
</style>