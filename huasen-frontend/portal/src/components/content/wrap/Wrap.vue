<!--
 * @Autor: huasenjio
 * @Date: 2022-07-31 21:51:50
 * @LastEditors: huizhang43 huizhang43@iflytek.com
 * @LastEditTime: 2023-12-26 16:23:44
 * @Description: 框架组件
-->
<template>
  <div class="hs-wrap">
    <div class="wrap-body">
      <WrapLeft v-show="showWrapLeft && !simpleMode"></WrapLeft>
      <WrapRight v-show="!simpleMode"></WrapRight>
      <WrapSidebar v-show="showWrapSidebar && !simpleMode"></WrapSidebar>
      <WrapSign></WrapSign>
      <WrapPerson></WrapPerson>
      <WrapSimple v-show="simpleMode"></WrapSimple>
    </div>
    <div v-if="showWrapFooter" v-html="appConfig.site.footerHtml" class="wrap-footer"></div>
  </div>
</template>
<script>
import { mapState } from 'vuex';

import navs from '@/config/nav.config.js';

import WrapLeft from './left/WrapLeft.vue';
import WrapRight from './right/WrapRight.vue';
import WrapSidebar from './sidebar/WrapSidebar.vue';
import WrapSign from './sign/WrapSign.vue';
import WrapPerson from './person/WrapPerson.vue';
import WrapSimple from './simple/WrapSimple.vue';

export default {
  name: 'HsWrap',

  components: { WrapLeft, WrapRight, WrapSidebar, WrapSign, WrapPerson, WrapSimple },

  data() {
    return {
      navs,
    };
  },

  computed: {
    ...mapState(['showWrapLeft', 'showWrapConnect', 'showWrapSidebar', 'user', 'appConfig']),
    simpleMode() {
      return this.user.config.simpleMode;
    },
    showWrapFooter() {
      return this.appConfig.site.footerHtml;
    },
  },
};
</script>

<style lang="scss" scoped>
.hs-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  .wrap-body {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    overflow: hidden;
  }
  .wrap-footer {
    width: 100%;
    height: 20px;
    text-align: center;
    line-height: 20px;
    font-size: 12px;
    border-top: 1px solid var(--gray-50);
    background-color: var(--gray-0);
    z-index: 3;
    a {
      color: var(--gray-600);
    }
  }
}
</style>
