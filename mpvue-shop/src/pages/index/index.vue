<template>
  <div class="index">
    <!--  头部的搜索 -->
    <div class="search">
      <div @click="toMappage">{{cityName}}</div>
      <div>
        <input type="text" placeholder="搜索商品" />
        <span class="icon"></span>
      </div>
    </div>
  </div>
</template>

<script>
import amapFile from '../../utils/amap-wx.js'
import {mapState, mapMutations } from 'vuex'

export default {
  data() {
    return {
    };
  },
  computed: {
    ...mapState(['cityName'])
  },
  methods: {
    ...mapMutations(['update']),
    toMappage() {
      // 通过wx.getSetting先查询一下用户是否授权 “scoped.record”
      let _this = this;
      wx.getSetting({
        success: res => {
          console.log("res", res);
          // 如果没有授权，打开设置
          if (!res.authSetting['scope.userLocation']) {
            wx.openSetting({
              success: res => {
                // 获取授权位置信息
                _this.getCityName();
              }
            });
          } else {
            wx.navigateTo({ url: "/pages/mappage/main" });
          }
        },
        fail: err => {
          console.log("err", err);
        },
        complete: () => {}
      });
    },
    getCityName () {
      let _this = this
      var myAmapFun = new amapFile.AMapWX({key:'5acec1739973e0b1923a3944b41781f4'});
      myAmapFun.getRegeo({
        success: function (data) {
          // 成功回调
          console.log('data', data)
          // ........
        },
        fail: function (info) {
          // 失败回调
          console.log('info', info)
          this.update({cityName: '北京'})
        }
      })
    },
  }
};
</script>

<style lang='less' scoped>
@import "./style.less";
</style>
