<template>
    <div class="index">
      <image mode="" class="inv" src="../../static/images/inv.png"/>
      <div class="bg-swiper" :animation="animationData">
          <!-- <index-swiper :list="list"></index-swiper> -->
          <image src="../../static/index.png" class="logo"/>
      </div>
      
      <div class="bg_music" v-if="isPlay" @tap="audioPlay">
          <image src="../../static/images/music_icon.png" class="musicImg music_icon"/>
          <!-- image src="../../static/images/music_play.png" class="music_play pauseImg"/> -->
      </div>
      <div class="bg_music" v-else @tap="audioPlay">
          <image src="../../static/images/music_icon.png" class="musicImg"/>
          <!-- <image src="../../static/images/music_play.png" class="music_play playImg"/> -->
      </div>
      <div class="info">
          <div class="content">
              <h1>Mr.刘 & Miss.崔</h1>
              <p>谨定于 2020年11月22日 （星期日）中午12:00</p>
              <p>农历 十月初八 中午十二点整 举办婚礼</p>
              <p @click="doNavigation">地址：河北省邯郸市永年区西阳城乡邓上村</p>
              <image src="../../static/images/we.png" class="img_footer"/>
          </div>
      </div>
    </div>
</template>

<script>
import IndexSwiper from 'components/indexSwiper'
import tools from 'common/js/h_tools'
import cloud from '@/service/cloud'
const audioCtx = wx.createInnerAudioContext()
export default {
  name: 'Index',
  components: {
    IndexSwiper
  },
  data () {
    return {
      isPlay: true,
      list: ['../../static/hc.png']
    }
  },
  onLoad () {
    const that = this
    that.getList()
  },
  onShow () {
    const that = this
    that.isPlay = true
    // that.getMusicUrl()
  },

  methods: {
    audioPlay () {
      const that = this
      if (that.isPlay) {
        audioCtx.pause()
        that.isPlay = false
        tools.showToast('您已暂停音乐播放~')
      } else {
        audioCtx.play()
        that.isPlay = true
        tools.showToast('背景音乐已开启~')
      }
    },
    // 获取首页图片和音乐地址
    getList () {
      // const that = this
      wx.showNavigationBarLoading()
      cloud.get('weddingInvite').then((res) => {
        if (res.errMsg === 'collection.get:ok') {
          // that.list = res.data[0].banner
          let musicUrl = res.data[0].music
          audioCtx.src = musicUrl
          audioCtx.loop = true
          audioCtx.autoplay = true
          audioCtx.play()
          wx.hideNavigationBarLoading()
        }
      })
    },
    doNavigation: function () {
      wx.navigateTo({
        url: '../map/nav/main'
      })
    }
  },

  onShareAppMessage: function (res) {
    return {
      title: '送上您的祝福',
      path: '/pages/index/main',
      imageUrl: '../../static/logo.png'
    }
  }
}
</script>

<style scoped lang="stylus">
@-webkit-keyframes musicRotate
  from
    -webkit-transformb rotate(0deg)
  to
    -webkit-transform rotate(360deg)
@-webkit-keyframes musicStop
  from
    -webkit-transform rotate(20deg)
  to
    -webkit-transform rotate(0deg)
@-webkit-keyframes musicStart
  from
    -webkit-transform rotate(0deg)
  to
    -webkit-transform rotate(20deg)
@-webkit-keyframes infoAnimation
  0%
    -webkit-transform scale(1) translate(0, 0)
  50%
    -webkit-transform scale(.9) translate(5px, 5px)
  100%
    -webkit-transform scale(1) translate(0, 0)
.index
  height 100%
  position relative
  .img
    width 100%
    height 100%
  .bg-swiper
    display:flex;
    height: 100%;
    justify-content: center;
    align-items:center;
    .logo
      animation infoAnimation 2s linear infinite
  .inv
    position absolute
    top 50rpx
    left 89rpx
    width 572rpx
    height 69rpx
    z-index 9
  .bg_music
    position fixed
    right 0
    top 150rpx
    width 85rpx
    z-index 99
    display flex
    justify-content flex-start
    align-items flex-start
    .musicImg
      width 60rpx
      height 60rpx
    .music_icon
      animation musicRotate 3s linear infinite
    .music_play
      width 28rpx
      height 60rpx
      margin-left -10rpx
      transform-origin top
      -webkit-transform rotate(20deg)
    .playImg
      animation musicStop 1s linear forwards
    .pauseImg
      animation musicStart 1s linear forwards
  .info
    width 630rpx
    background rgba(255, 255, 255, 0.75)
    z-index 9
    position fixed
    bottom 40rpx
    left 50rpx
    padding 10rpx
    .content
      width 626rpx
      border 1rpx solid #000
      display flex
      flex-direction column
      justify-content flex-start
      align-items center
      position relative
      padding-bottom 30rpx
      h1
        font-size 50rpx
        height 100rpx
        line-height 100rpx
      p
        font-size 24rpx
        height 60rpx
        line-height 60rpx
      .img_footer
        position absolute
        bottom 0
        left 53rpx
        z-index 99
        height 14rpx
        width 520rpx
</style>
