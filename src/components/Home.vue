<template>
  <section class="wrapper" :class="{'margin': isMargin}">
    <article class="top-wrap">
      <div class="logo">
        <img src="../assets/img/logo.png">
      </div>
    </article>
    <article class="banner-wrap" :class="{'error': slides.length === 0}">
      <swiper :options="swiperOption" ref="mySwiper">
        <!-- slides -->
        <swiper-slide v-for="(slide, index) in slides" :key="index">
          <a :href="slide.indexActionUrl"><img :src=slide.indexPageUrl></a>
        </swiper-slide>
        <!-- Optional controls -->
        <div class="swiper-pagination"  slot="pagination"></div>
      </swiper>
    </article>
    <article class="body-wrap">
      <div class="text-wrap"></div>
      <div class="panel-wrap choose-wrap">
        <div class="title">为什么选择运立方TMS?</div>
        <div class="body">
          <div class="left">
            <div class="item">
              <div class="ico free"></div>
              <div class="text">
                <p>免费SAAS</p>
                <p>TMS</p>
              </div>
            </div>
            <div class="item">
              <div class="ico app"></div>
              <div class="text">
                <p>移动应用</p>
                <p>协同</p>
              </div>
            </div>
          </div>
          <div class="right">
            <div class="item">
              <div class="ico share"></div>
              <div class="text">
                <p>共享优质</p>
                <p>服务</p>
              </div>
            </div>
            <div class="item">
              <div class="ico contact"></div>
              <div class="text">
                <p>平台资源</p>
                <p>互联</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="panel-wrap service-wrap">
        <div class="title">我们的十大服务</div>
        <div class="body">
          <div class="left">
            <div class="label">共享客服服务</div>
            <div class="label">线上扫码收款</div>
            <div class="label">轨迹实时定位</div>
            <div class="label">定制物流物料</div>
            <div class="label">专业税务筹划</div>
          </div>
          <div class="right">
            <div class="label">共享品牌设计</div>
            <div class="label">客户通知短信</div>
            <div class="label">终身免费调车</div>
            <div class="label">定制化托运单</div>
            <div class="label">低息无押贷款</div>
          </div>
        </div>
      </div>
      <div class="panel-wrap product-wrap">
        <div class="title">我们的产品</div>
        <div class="body">
          <div class="layer layer-tms">
            <div class="title-wrap">
              <p>运立方移动端</p>
              <div class="line"></div>
            </div>
            <div class="desc-wrap">
              <ul>
                <li> 网点开单</li>
                <li> 派送管理</li>
                <li> 签收管理</li>
                <li> 车辆管理</li>
              </ul>
            </div>
            <div class="pic"></div>
            <div class="btn" @click="tmsDownload()">下载运立方移动端</div>
          </div>
          <div class="layer layer-driver">
            <div class="title-wrap">
              <p>车宝（司机版）</p>
              <div class="line"></div>
            </div>
            <div class="desc-wrap">
              <ul>
                <li> 海量货源</li>
                <li> 自主找货</li>
                <li> 定金保障</li>
                <li> 拒绝放空</li>
              </ul>
            </div>
            <div class="pic"></div>
            <div class="btn" @click="driverDownload()">下载车宝</div>
          </div>
        </div>
      </div>
    </article>
    <article class="bottom-wrap">
      <p>400-890-0856</p>
      <p>工作时间  09:00 - 20:00</p>
      <p><a href="http://www.yunlifang.com/static/mobile/apply.html">免费开通</a><a @click="agentDownload()">下载承运商版</a></p>
      <p>www.yunlifang.com</p>
    </article>
    <article class="plus-wrap">
      <div class="left"><a href="./about.html">关于我们</a></div>
      <div class="middle"><a target="_blank" href="http://p.qiao.baidu.com/im/index?siteid=9883058&ucid=22131070">在线客服</a></div>
      <div class="right"><a href="mailto:bd@eunke.com">商务合作</a></div>
    </article>
    <apply @close="toFull"></apply>
  </section>
</template>

<script>

import Apply from '../components/Apply'
import 'swiper/dist/css/swiper.css'
import { swiper, swiperSlide } from 'vue-awesome-swiper'


export default {
  name: 'Home',
  data () {
    return {
      isMargin: true,
      swiperOption: {
        pagination: {
          el: '.swiper-pagination',
          clickable :true                                 //点击小圆点会切换轮播图
        },
        autoplay: {
          delay: 3000,                                    //时间间隔
          disableOnInteraction: false                     //操作swiper后是否停止自动轮播
        }
      },
      slides : []
    }
  },
  computed: {
    swiper() {
      return this.$refs.mySwiper.swiper
    }
  },
  mounted(){
    this.$nextTick(() => {
      //获取轮播图
      let bannerApi = 'http://' + window.location.host + '/donkey/inner/api/banner/m_site/list';
      this.$http.get(bannerApi).then((response) => {

        if(response.data.code === 0)
        {
          this.slides = response.data.data.adList;
        }

      }).catch(() => {

      });

      //跳转下载
      let jumpUrl = window.location.href;
      jumpUrl = window.location.search.substr(1);
      let urlArr = jumpUrl.split('&');
      let urlObj = {};
      urlArr.forEach((val) => {
        let keyword = val.split('=');
        urlObj[keyword[0]] = keyword[1];
      });

      if (this.judgeSystem().isAndroid) {
        urlObj.device = 2;
      } else if (this.judgeSystem().isIOS) {
        urlObj.device = 1;
      }

      //let jumpDownloadApi = '/donkey/inner/api/jumpAgreement';
      let jumpDownloadApi = 'http://' + window.location.host + '/donkey/inner/api/jumpAgreement';

      this.$http.get(jumpDownloadApi,{
        params : urlObj
      }).then((response) => {

          let dataObj = response.data.data.appJumpAgreement;
          let _url = dataObj.linkProtocol;
          let conf = confirm("您确定要打开链接吗？");

          if(conf === true)
          {
            window.location.href = _url;
          }

      }).catch(() => {

      });

    })

  },
  methods : {
    toFull(){
      this.isMargin = false;
    },
    /** 判断访问的系统 **/
    judgeSystem() {
      let inBrowser = typeof window !== 'undefined' && Object.prototype.toString.call(window) !== '[object Object]';
      let UA = inBrowser && window.navigator.userAgent.toLowerCase();
      let isIE = UA && UA.indexOf('trident') > 0;
      let isIE9 = UA && UA.indexOf('msie 9.0') > 0;
      let isAndroid = UA && UA.indexOf('android') > 0;
      let isSymbian = UA && UA.indexOf('SymbianOS') > 0;
      let isWindowsPhone = UA && UA.indexOf('Windows Phone') > 0;
      let isIOS = UA && /iphone|ipad|ipod|ios/.test(UA);

      let browser = {
        isIE: isIE,
        isIE9: isIE9,
        isAndroid: isAndroid,
        isIOS: isIOS,
        isSymbian: isSymbian,
        isWindowsPhone: isWindowsPhone
      };
      return browser;
    },
    getQueryString(name) {
      let reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
      let r = window.location.search.substr(1).match(reg);
      if (r !== null) return unescape(r[2]);
      return null;
    },
    tmsDownload(){

      let _androidUrl = "http://donkey.loji.com/inner/android/cargo";
      let _iosUrl = "https://itunes.apple.com/cn/app/xiao-mao-lu-wu-liu-huo-zhu/id898641448?mt=8";

      if (navigator.userAgent.match(/(micromessenger)/i)) {
        // 如果是微信内置浏览器则打开腾讯应用宝，避开拦截
        _androidUrl = "http://a.app.qq.com/o/simple.jsp?pkgname=com.eunke.burro_cargo";
        window.location.href = _androidUrl;
      } else if (navigator.userAgent.match(/(Android|MiuiBrowser)/i)) {
        window.location.href = _androidUrl;
      } else if (navigator.userAgent.match(/(iPhone|iPod|ios|iPad)/i)) {
        window.location.href = _iosUrl;
      }


    },
    driverDownload(){

      let _androidUrl = "http://donkey.loji.com/inner/android/driver";
      let _iosUrl = "https://itunes.apple.com/cn/app/罗计车宝/id964835843?mt=8";

      if (navigator.userAgent.match(/(micromessenger)/i)) {
        // 如果是微信内置浏览器则打开腾讯应用宝，避开拦截(渠道例外）
        if ("sms" === this.getQueryString("from")) {
          alert('由于微信内置浏览器限制，请用其他浏览器打开。');
          return false;
        } else {
          _androidUrl = "http://a.app.qq.com/o/simple.jsp?pkgname=com.eunke.burro_driver";
          window.location.href = _androidUrl;
        }
      } else if (navigator.userAgent.match(/(Android|MiuiBrowser)/i)) {
        // 来源于短信的链接进入m站进行下载时，下载司机端短信渠道包
        if ("sms" === this.getQueryString("from")) {
          _androidUrl = "http://donkey.loji.com/inner/android/driverDownload/gfduanxin";
          window.location.href = _androidUrl;
        }
        window.location.href = _androidUrl;
      } else if (navigator.userAgent.match(/(iPhone|iPod|ios|iPad)/i)) {
        window.location.href = _iosUrl;
      }


    },
    agentDownload(){

      let _androidUrl = "http://donkey.loji.com/inner/android/broker";
      let _iosUrl = "http://dwz.cn/6axxK9";

      if (navigator.userAgent.match(/(micromessenger)/i)) {
        // 如果是微信内置浏览器则打开腾讯应用宝，避开拦截
        _androidUrl = "http://a.app.qq.com/o/simple.jsp?pkgname=com.eunke.broker";
        window.location.href = _androidUrl;
      } else if (navigator.userAgent.match(/(Android|MiuiBrowser)/i)) {
        window.location.href = _androidUrl;
      } else if (navigator.userAgent.match(/(iPhone|iPod|ios|iPad)/i)) {
        window.location.href = _iosUrl;
      }


    }
  },
  components : {
    Apply, swiper, swiperSlide
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

$t: #FC7634;
.wrapper {
  letter-spacing: 2px;
  color: #fff;
  .top-wrap {
    height: 94px;
    .logo {
      height: 94px;
      padding-left: 30px;
      display: table-cell;
      vertical-align: middle;
      img {
        height: 61px;
      }
    }
  }
  .banner-wrap {
    //height: 360px;
    img {
      width: 100%;
    }
  }
  .error {
    height: 360px;
    background: #eee;
  }
  .body-wrap {
    .text-wrap {
      height: 78px;
      background: url("../assets/img/text.png") no-repeat;
      background-size: 100% 100%;
    }
    .panel-wrap {
      .title {
        padding: 16px 0;
        font-size: 34px;
        background: $t;
        text-align: center;
      }
      .body {
        padding: 60px 30px;
      }
    }
    .choose-wrap {
      .body {
        display: flex;
        display: -webkit-flex;
        justify-content: center;
        background: url("../assets/img/choose-bg.png") no-repeat;
        background-size: 100% 100%;
        .left {
          margin-right: 35px;
        }
        .right {
          margin-left: 35px;
        }
        .item {
          width: 200px;
          height: 250px;
          border-radius: 10px;
          margin-bottom: 60px;
          box-shadow: 0 8px 16px 0 rgba(204,204,204,.5);
          background: #fff;
          position: relative;
          &:last-child {
            margin-bottom: 0;
          }
          .ico {
            width: 120px;
            height: 120px;
            margin: 0 auto;
            position: relative;
            top: 20px;
          }
          .free {
            background: url("../assets/img/free.png") no-repeat;
            background-size: 100% 100%;
          }
          .app {
            background: url("../assets/img/app.png") no-repeat;
            background-size: 100% 100%;
          }
          .share {
            background: url("../assets/img/share.png") no-repeat;
            background-size: 100% 100%;
          }
          .contact {
            background: url("../assets/img/contact.png") no-repeat;
            background-size: 100% 100%;
          }
          .text {
            position: absolute;
            width: 100%;
            bottom: 20px;
            p {
              font-size: 30px;
              text-align: center;
              color: #000;
            }
          }
        }
      }
    }
    .service-wrap {
      .body {
        display: flex;
        display: -webkit-flex;
        justify-content: center;
        background: url("../assets/img/service-bg.png") no-repeat;
        background-size: 100% 100%;
        .left {
          margin-right: 23px;
        }
        .right {
          margin-left: 23px;
        }
        .label {
          width: 300px;
          padding: 12px 0;
          margin-bottom: 50px;
          font-size: 30px;
          border-radius: 100px;
          box-shadow: 0 8px 16px 0 rgba(204,204,204,.5);
          color: $t;
          text-align: center;
          background: #fff;
          &:last-child {
            margin-bottom: 0;
          }
        }
      }
    }
    .product-wrap {
      background: #F3F3F3;
      .layer {
        height: 842px;
        border-radius: 8px;
        box-shadow:0 8px 16px 0 rgba(204,204,204,.50);
        margin-bottom: 60px;
        padding: 40px;
        position: relative;
        background: #fff;
        &:last-child {
          margin-bottom: 0;
        }
      }
      .layer-tms {
        .title-wrap {
          position: absolute;
          top: 70px;
          left: 70px;
          p {
            font-size: 38px;
            margin-bottom: 6px;
            color: $t;
          }
          .line {
            width: 170px;
            height: 7px;
            background: $t;
          }
        }
        .desc-wrap {
          position: absolute;
          top: 280px;
          left: 40px;
          ul {
            li {
              color: #000;
              font-size: 34px;
              margin-bottom: 56px;
              &:before {
                content: "\02022";
                color: $t;
              }
            }
          }
        }
        .pic {
          position: absolute;
          width: 396px;
          height: 577px;
          top: 134px;
          right: 30px;
          background: url("../assets/img/tms-app.png") no-repeat;
          background-size: 100% 100%;
        }
        .btn {
          position: absolute;
          left: 0;
          right: 0;
          margin: auto;
          width: 360px;
          padding: 18px 0;
          bottom: 56px;
          font-size: 34px;
          border-radius: 100px;
          box-shadow: 0 8px 16px 0 rgba(204,204,204,.5);
          background: $t;
          text-align: center;
        }
      }
      .layer-driver {
        .title-wrap {
          position: absolute;
          top: 70px;
          right: 124px;
          p {
            font-size: 38px;
            margin-bottom: 6px;
            color: $t;
          }
          .line {
            width: 170px;
            height: 7px;
            background: $t;
          }
        }
        .desc-wrap {
          position: absolute;
          top: 280px;
          right: 64px;
          ul {
            li {
              color: #000;
              font-size: 34px;
              margin-bottom: 56px;
              &:before {
                content: "\02022";
                color: $t;
              }
            }
          }
        }
        .pic {
          position: absolute;
          width: 396px;
          height: 577px;
          top: 152px;
          left: 10px;
          background: url("../assets/img/driver-app.png") no-repeat;
          background-size: 100% 100%;
        }
        .btn {
          position: absolute;
          left: 0;
          right: 0;
          margin: auto;
          width: 360px;
          padding: 18px 0;
          bottom: 56px;
          font-size: 34px;
          border-radius: 100px;
          box-shadow: 0 8px 16px 0 rgba(204,204,204,.5);
          background: $t;
          text-align: center;
        }
      }
    }
  }
  .bottom-wrap {
    height: 360px;
    padding: 62px 0;
    text-align: center;
    background: $t;
    p {
      font-size: 40px;
      margin-bottom: 8px;
      &:last-child {
        margin-bottom: 0;
      }
      a {
        text-decoration: underline;
        margin: 0 22px;
      }
    }
  }
  .plus-wrap {
    display: flex;
    display: -webkit-flex;
    justify-content: space-between;
    height: 80px;
    background: #FFA919;
    padding: 20px 0;
    div {
      width: 100%;
      border-right: 1px solid #fff;
      text-align: center;
      font-size: 30px;
      &:last-child {
        border-right: none;
      }
    }
  }
}
.margin {
  margin-bottom: 140px;
}
</style>
