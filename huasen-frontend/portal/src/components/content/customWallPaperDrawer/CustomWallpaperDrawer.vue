<!--
 * @Autor: huasenjio
 * @Date: 2022-09-19 22:14:21
 * @LastEditors: huizhang43 huizhang43@iflytek.com
 * @LastEditTime: 2023-12-27 17:17:13
 * @Description: 
-->
<template>
  <HsDrawer v-bind="$attrs" v-on="$listeners">
    <div class="custom-wallpaper-drawer">
      <el-collapse class="collapse" v-model="activeName" accordion>
        <!-- 基础配置 -->
        <el-collapse-item name="1">
          <template slot="title">
            <div class="title">基础设置</div>
          </template>
          <div class="collapse-content">
            <section class="other">
              <div>纯色背景</div>
              <ul>
                <li v-for="(item, index) in displayPure" :key="index" :style="{ backgroundColor: item.background }" @click="changeBg(item)"></li>
                <li>
                  <el-color-picker v-model="bg" size="mini" @change="changeBg($event, 'pick')"> </el-color-picker>
                </li>
              </ul>
            </section>
            <section class="other">
              <div>背景模糊度</div>
              <el-slider :show-tooltip="false" class="w-px-380 pl-px-8" :min="0" :max="10" :step="0.5" v-model="sliderFilter" @change="changeFilter"></el-slider>
            </section>
            <section class="other">
              <div>遮罩浓度</div>
              <el-slider :show-tooltip="false" class="w-px-380 pl-px-8" :min="0" :max="1" :step="0.1" v-model="sliderLightness" @change="changeShadow"></el-slider>
            </section>
            <section class="other">
              <div>背景透明度</div>
              <el-slider :show-tooltip="false" class="w-px-380 pl-px-8" :min="0" :max="1" :step="0.1" v-model="bgOpacity" @change="changeBgOpacity"></el-slider>
            </section>
            <section class="other">
              <div>文字灰度</div>
              <el-slider :show-tooltip="false" class="w-px-380 pl-px-8" :min="0" :max="1000" :step="100" v-model="bgColor" @change="changeBgColor"></el-slider>
            </section>
            <section class="other">
              <div>文字颜色</div>
              <ul>
                <li v-for="(item, index) in displayPure" :key="index" :style="{ backgroundColor: item.background }" @click="changeTitle(item)"></li>
                <li>
                  <el-color-picker v-model="bg" size="mini" @change="changeTitle($event, 'pick')"> </el-color-picker>
                </li>
              </ul>
            </section>
          </div>
        </el-collapse-item>
        <!-- 图片背景 -->
        <el-collapse-item name="2">
          <template slot="title">
            <div class="title">图片背景</div>
          </template>
          <div class="collapse-content">
            <ul class="image">
              <li v-for="(item, index) in displayWallpaperImages" :key="index" :style="{ backgroundImage: `url(${item.background})` }" @click="changeBg(item)">
                <div class="setting">
                  立即设置
                </div>
              </li>
            </ul>
          </div>
        </el-collapse-item>
        <!-- 自定义上传 -->
        <el-collapse-item name="3">
          <template slot="title">
            <div class="title">自定义上传</div>
          </template>
          <div class="collapse-content flex justify-center items-center">
            <el-upload class="upload py-px-10" accept=".png,.jpg,.git" :before-upload="beforeUpload" action drag>
              <i class="el-icon-upload"></i>
              <div class="el-upload__text">拖拽上传 · <em>点击上传</em></div>
            </el-upload>
          </div>
        </el-collapse-item>
      </el-collapse>
    </div>
  </HsDrawer>
</template>
<script>
import { mapState, mapMutations } from 'vuex';
import HsDrawer from '@/components/content/drawer/Drawer.vue';

export default {
  name: 'CustomWallpaperDrawer',

  components: { HsDrawer },

  data() {
    return {
      show: false,
      activeName: '1',
      // 背景
      bg: '',
      // 控制封面模糊度
      sliderFilter: 0,
      // 控制封面遮罩浓度
      sliderLightness: 0,
      // 控制背景图的透明度
      bgOpacity: 0,
      // 控制背景文字的灰度
      bgColor: 0,
      titleColor: '',
      // 封面颜色可选纯色系
      pures: [
        {
          color: '#FFFFFF',
          background: '#9CA3AF',
        },
        {
          color: '#FFFFFF',
          background: '#F87171',
        },
        {
          color: '#FFFFFF',
          background: '#FBBF24',
        },
        {
          color: '#FFFFFF',
          background: '#34D399',
        },
        {
          color: '#FFFFFF',
          background: '#60A5FA',
        },
        {
          color: '#FFFFFF',
          background: '#A78BFA',
        },
      ],
      // 壁纸
      wallpaperImages: [
        {
          headerFontColor: '#FFFFFF',
          background: require('@/assets/img/wallpaper/1.jpeg'),
        },
        {
          headerFontColor: '#FFFFFF',
          background: require('@/assets/img/wallpaper/2.jpeg'),
        },
        {
          headerFontColor: '#FFFFFF',
          background: require('@/assets/img/wallpaper/3.jpeg'),
        },
        {
          headerFontColor: '#000000',
          background: require('@/assets/img/wallpaper/4.jpeg'),
        },
        {
          headerFontColor: '#FFFFFF',
          background: require('@/assets/img/wallpaper/5.jpeg'),
        },
        {
          headerFontColor: '#000000',
          background: require('@/assets/img/wallpaper/6.jpeg'),
        },
        // {
        //   headerFontColor: '#000000',
        //   background: require('@/assets/img/wallpaper/6.jpeg'),
        // },
      ],
    };
  },
  computed: {
    ...mapState(['user', 'themeConfig']),
    displayPure() {
      let pureTemp = this.themeConfig.pure ? this.themeConfig.pure : [];
      let temp = this.pures.concat(pureTemp);
      return [...temp];
    },
    displayWallpaperImages() {
      let wallpaperTemp = this.themeConfig.wallpaper ? this.themeConfig.wallpaper : [];
      let temp = this.wallpaperImages.concat(wallpaperTemp);
      return [...temp];
    },
  },
  mounted() {
    this.bg = this.user.config.bg;
    this.sliderFilter = this.user.config.bgFilter;
    this.sliderLightness = this.user.config.bgLightness;
    this.bgOpacity = this.user.config.bgOpacity || 0.65;
    this.bgColor = this.user.config.bgColor || 760;
    this.titleColor = this.user.config.titleColor;
  },
  methods: {
    ...mapMutations(['commitAll']),

    changeFilter(val) {
      this.initCustomStyle({
        user: {
          config: {
            bgFilter: val,
          },
        },
      });
    },

    changeShadow(val) {
      this.initCustomStyle({
        user: {
          config: {
            bgLightness: val,
          },
        },
      });
    },
    changeBgOpacity(val) {
      this.initCustomStyle({
        user: {
          config: {
            bgOpacity: val,
          },
        },
      });
    },
    changeBgColor(val) {
      this.initCustomStyle({
        user: {
          config: {
            bgColor: val,
          },
        },
      });
    },
    // 上传文件转换成base64进行保存
    beforeUpload(file) {
      // 1048576 = 1M
      // 2097152 = 2M
      // 3145728 = 3M
      if (file.size <= 2097152) {
        this.TOOL.getBase64(file, bs64 => {
          this.initCustomStyle({
            user: {
              config: {
                bg: bs64,
              },
            },
          });
        });
      } else {
        this.$tips('error', '图片大小已超过 2MB', null, 2000);
      }
      return false; // 终止上传
    },

    changeBg(val, tag) {
      this.initCustomStyle({
        user: {
          config: {
            bg: tag == 'pick' ? val : val.background,
          },
        },
      });
    },
    changeTitle(val, tag) {
      this.initCustomStyle({
        user: {
          config: {
            titleColor: tag == 'pick' ? val : val.background,
          },
        },
      });
    },

    initCustomStyle(data) {
      this.commitAll(data);
      this.$store.dispatch('snapshoot');
      this.$store.dispatch('initLocalUserInfo');
    },
  },
};
</script>

<style lang="scss" scoped>
.custom-wallpaper-drawer {
  width: 400px;
  margin: 10px auto;
  .collapse {
    .title {
      padding-left: 10px;
      font-size: 14px;
      box-sizing: border-box;
    }
    .collapse-content {
      .pure {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
        li {
          width: 120px;
          height: 60px;
          margin: 10px auto;
          border-radius: 3px;
          display: flex;
          justify-content: center;
          align-items: center;
          cursor: pointer;
        }
      }
      .image {
        display: flex;
        flex-wrap: wrap;
        li {
          flex-shrink: 0;
          flex-grow: 0;
          width: 180px;
          height: 108px;
          margin: 3px 10px;
          border-radius: 3px;
          display: flex;
          justify-content: center;
          align-items: center;
          background-size: cover;
          background-repeat: no-repeat;
          .setting {
            width: 60px;
            height: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 3px;
            background-color: var(--gray-o5);
            cursor: pointer;
          }
        }
      }
      .other {
        padding-left: 10px;
        margin-top: 10px;
        ul {
          display: flex;
          align-items: center;
          flex-wrap: wrap;
          li {
            width: 26px;
            height: 26px;
            margin-right: 8px;
            margin-top: 8px;
            opacity: 0.8;
            border-radius: 4px;
            cursor: pointer;
            &:first-child {
              margin-left: 0px;
            }
          }
        }
      }
    }
  }
}
</style>
