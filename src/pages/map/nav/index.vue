<template>
  <div class="container">

  </div>
</template>

<script>
export default {
  components: {
  },

  data () {
    return {
      latitude: 36.835479,
      longitude: 114.408542,
      scale: 16
    }
  },
  methods: {
  },
  created () {
  },
  onLoad: function () {
    let that = this
    wx.getLocation({
      type: 'gcj02', // 默认为wgs84的gps坐标，如果要返回直接给openLocation用的火星坐标，可传入'gcj02'
      success: function (res) {
        console.log('location:', res)
        wx.openLocation({
          latitude: parseFloat(that.latitude),
          longitude: parseFloat(that.longitude),
          // address: that.location,
          scale: that.scale
        })
      },
      cancel: function (res) {
        console.log('地图定位失败')
      }
    })
  }
}
</script>

<style>
  /* pages/bless/index.wxss */
  .title {
    text-align: center;
    font-size: 30rpx;
    line-height: 60rpx;

  }
  .hide {
    display: none;
  }

  .show {
    display: block;
  }
  .edit_bg{
    z-index:200;
    left: 90%;
    top: 25%;
    position: absolute;
    text-align: right;
  }
  .friends {
    height: auto;
    min-height: 0;
    overflow: hidden;
    width: 600rpx;
    margin: 0 auto;
    margin-top: 20rpx;
  }
  .load_more_friends_arrow{
    right: 20px;
    position: absolute;
    top: 70px;
    width: 5px;
    height: 15px;
  }
  .friends_view{
    width: 100%;
    display: flex;
    text-align: center
  }
  .face {
    width: 100rpx;
    height: 100rpx;
    float: left;
    margin: 10rpx;
  }
  .face image {
    width: 100rpx;
    height: 100rpx;
    border-radius: 50%;
  }
  .btn_zone {
    padding: 30rpx 50rpx;
    border-top: 1px solid #f1f1f1;
    border-bottom: 1px solid #f1f1f1;
    min-height: 0;
    overflow: hidden;
    margin-top: 20rpx;
    display: flex
  }
  .zan {
    width: 300rpx;
    background: #CD96CD;
    color: white
  }
  .share {
    width: 300rpx;
    margin-left: 20px;
  }
  .bless_img {
    width: 400rpx;
    height: 300rpx;
    margin: 0 auto;
  }

  .bless_img image{
    width: 400rpx;
    height: 300rpx;
  }
  .tips {
    text-align: center;
    font-size: 25rpx;
    margin-top: 20rpx;
    margin-bottom: 20rpx;
    color: #ccc;
  }

  /* pages/chat/index.wxss */
  .container{
    background-color: #f1f2f7;
  }
  page {
    background-color: #f1f2f7;
    padding-bottom: 150rpx;
  }
  .chat_view{
    margin-bottom: 180rpx;
  }
  .chat_list {
    width: 630rpx;
    margin: 0 auto;
    margin-top: 30rpx;
    padding: 30rpx;
    min-height: 0;
    overflow: hidden;
    background-color: #f1f2f7;
    border-radius: 5px;
    box-shadow:  0 0 20px rgba(190,77,43,.2);
  }
  .chat_face {
    width: 100rpx;
    height: 100rpx;
    float: left;
    margin-right: 30rpx;
    margin-top: 30rpx;
  }
  .chat_face image{
    width: 100rpx;
    height: 100rpx;
    border-radius: 50%;
  }
  .chat_words {
    width: 500rpx;
    float: left;
    font-size: 30rpx;
    line-height: 50rpx;
    color: #999;
  }
  .chat_meta {
    width: 100%;
    min-height: 0;
    overflow: hidden;
    margin-bottom: 10rpx;
  }
  .chat_user {
    display: inline-block;
    font-size: 30rpx;
    font-weight: 600;
    color: #666;
    float: left;
  }
  .chat_date {
    display: inline-block;
    font-size: 24rpx;
    color: #ccc;
    float: right;
  }

  .send_msg {
    width: 700rpx;
    padding: 25rpx;
    background-color: #fff;
    min-height: 0;
    overflow: hidden;
    position: fixed;
    bottom: 0;
    left: 0;
    border-top: 1px solid #f1f1f1;
    border-bottom: 1px solid #f1f1f1;

  }
  .send_ipt {
    width: 500rpx;
    padding: 10rpx;
    font-size: 30rpx;
    height: 60rpx;
    line-height: 60rpx;
    float: left;
    background-color: #f5f5f5;
    border-radius: 5px;
  }
  .send_btn {
    width: 150rpx;
    height: 80rpx;
    font-size: 36rpx;
    line-height: 80rpx;
    float: right;
    background: #CD96CD;
    color: white
  }

  .befirst {
    width: 600rpx;
    height: 450rpx;
    margin: 0 auto;
    min-height: 0;
    overflow: hidden;
    margin-top: 200rpx;
  }
  .befirst image{
    width: 600rpx;
    height: 450rpx;
  }
  .text {
    margin-top: 5px;
    width: 100%;
    display: flex;
    justify-content: center;
  }
  .modal-mask {
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    background: #000;
    opacity: 0.5;
    overflow: hidden;
    z-index: 9000;
    color: #fff;
  }
  .modal-dialog {
    width: 540rpx;
    overflow: hidden;
    position: fixed;
    top: 50%;
    left: 0;
    z-index: 9999;
    background: #fff;
    margin: -180rpx 105rpx;
    border-radius: 36rpx;
  }
  .modal-title {
    padding-top: 50rpx;
    font-size: 36rpx;
    color: #030303;
    text-align: center;
  }
  .modal-content {
    padding: 50rpx 32rpx;
  }
  .modal-input {
    display: flex;
    background: #fff;
    border-bottom: 2rpx solid #ddd;
    border-radius: 4rpx;
    font-size: 28rpx;
  }
  .input {
    width: 100%;
    height: 82rpx;
    font-size: 28rpx;
    line-height: 28rpx;
    padding: 0 20rpx;
    box-sizing: border-box;
    color: #333;
  }
  input-holder {
    color: #666;
    font-size: 28rpx;
  }
  .modal-footer {
    display: flex;
    flex-direction: row;
    height: 86rpx;
    border-top: 1px solid #dedede;
    font-size: 34rpx;
    line-height: 86rpx;
  }
  .btn-cancel {
    width: 50%;
    color: #666;
    text-align: center;
    border-right: 1px solid #dedede;
  }
  .btn-confirm {
    width: 50%;
    color: #ec5300;
    text-align: center;
  }
</style>
