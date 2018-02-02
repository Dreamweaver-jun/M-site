<template>
  <section class="wrapper" v-show="show">
      <article class="left">
          <div class="app-logo"></div>
          <div class="text">运立方TMS</div>
      </article>
      <article class="right">
        <a class="btn" href="http://www.yunlifang.com/static/mobile/apply.html">申请开通</a>
      </article>
      <div class="close" @click="close()"></div>
  </section>
</template>

<script>
export default {
  name: 'Apply',
  data () {
    return {
      show: true
    }
  },
  methods : {
    close(){
      this.show = false;
      this.$emit('close')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

$t: #FC7634;
.wrapper {
  position: fixed;
  bottom: 0;
  display: flex;
  width: 100%;
  height: 140px;
  background: #fff;
  justify-content: space-between;
  align-items: center;
  padding: 0 60px;
  box-shadow: 0 -2px 5px 0 #F4EEEB;
  .left {
    display: flex;
    justify-content: space-between;
    .app-logo {
      width: 100px;
      height: 100px;
      background: url("../assets/img/app-logo.png") no-repeat;
      background-size: 100% 100%;
    }
    .text {
      color: #000;
      font-size: 40px;
      margin-left: 40px;
      display: flex;
      align-items: center;
    }
  }
  .right {
    .btn {
      background: $t;
      font-size: 30px;
      border-radius: 10px;
      padding: 26px 16px;
    }
  }
  .close {
    position: absolute;
    top: 10px;
    right: 10px;
    width: 24px;
    height: 24px;
    background: url("../assets/img/close.png") no-repeat;
    background-size: 100% 100%;
  }
}
</style>
