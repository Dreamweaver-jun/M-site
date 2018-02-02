<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style lang="scss">

/** reset START **/
p, ul, li, body, a,a:after, a:focus, a:visited, a:hover, a:active, article, aside, footer, header, hgroup, main, nav, section {
  margin: 0;
  padding: 0;
  color: #fff;
  text-decoration: none;
  box-sizing: border-box;
  list-style: none;
  font-family: Helvetica;
}
/** reset END **/

$t: #FC7634;

/** 修改swiper样式 START **/
.banner-wrap>.swiper-container-horizontal>.swiper-pagination-bullets .swiper-pagination-bullet {
  width: 20px;
  height: 20px;
}
.banner-wrap>.swiper-container-horizontal>.swiper-pagination-bullets .swiper-pagination-bullet-active {
  background: $t;
}
/** 修改swiper样式 END **/
</style>
