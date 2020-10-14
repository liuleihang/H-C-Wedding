<template>
    <div class="map">
        <image mode="aspectFit" class="head-img" src="../../static/images/t1.png"/>
        <map class="content" id="map" 
          :longitude="longitude" 
          :latitude="latitude" 
          show-location="true" 
          :markers="markers" 
          :scale="scale" 
          @markertap="getLoc"
        ></map> 
        <div class="call">
            <div class="left" @tap="linkHe">
                <image src="../../static/images/he.png"/>
                <span>呼叫新郎</span>
            </div>
            <div class="right" @tap="linkShe">
                <image src="../../static/images/she.png"/>
                <span>呼叫新娘</span>
            </div>
        </div>
        <!-- <image class="footer" src="../../static/images/grren-flower-line.png"/> -->
    </div>
</template>

<script>
/* import QQMapWX from 'common/js/qqmap-wx-jssdk.js'
let qqmapsdk */
export default {
  name: 'Map',
  data () {
    return {
      // qqSdk: '',
      latitude: 36.835479,
      longitude: 114.408542,
      scale: 13,
      markers: [{
        iconPath: '../../static/index.png',
        id: 0,
        latitude: 36.835479,
        longitude: 114.408542,
        width: 50,
        height: 50,
        scale: 13
      }]
    }
  },
  onReady: function (e) {
    // 使用 wx.createMapContext 获取 map 上下文
    this.mapCtx = wx.createMapContext('map')
  },
  onLoad: function () {
    // 实例化API核心类
    /* qqmapsdk = new QQMapWX({
      key: '7UKBZ-2DXK6-LQFSJ-EDPUW-CXSXJ-VBBWC'
    }) */
    // this.getLoc()
  },
  methods: {
    getLoc () {
      let that = this
      wx.getLocation({
        type: 'gcj02', // 默认为wgs84的gps坐标，如果要返回直接给openLocation用的火星坐标，可传入'gcj02'
        success: function (res) {
          console.log('location:', res)
          wx.openLocation({
            latitude: parseFloat(that.latitude),
            longitude: parseFloat(that.longitude),
            address: that.location,
            scale: that.scale
          })
        },
        cancel: function (res) {
          console.log('地图定位失败')
        }
      })
    },
    clickMark () {
      console.log('clickMark >>> ')
      wx.navigateTo({
        url: 'nav/main'
      })
    },
    linkHe () {
      wx.makePhoneCall({
        phoneNumber: '13146075762'
      })
    },

    linkShe () {
      wx.makePhoneCall({
        phoneNumber: '15612099581'
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
.map
    height 100%
    .head-img
      width 100%
      height 180rpx
      margin-top 30rpx
      margin-bottom 30rpx
    .content
      width 690rpx
      margin-left 30rpx
      height 700rpx
      margin-top 50rpx
      margin-bottom 50rpx
      border-radius 16rpx
    .call
      display flex
      justify-content space-around
      align-items center
      width 100%
      margin-bottom 20rpx
      .left, .right
        flex 1
        display flex
        flex-direction column
        justify-content center
        align-items center
        image
          height 80rpx
          width 80rpx
        span
          height 50rpx
          line-height 50rpx
          font-size 24rpx
          color #6B4F4E
    .footer
      height 80rpx
      width 716rpx
      margin-left 17rpx
</style>
