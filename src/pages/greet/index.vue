<template>
    <div class="greet">
        <!-- <image class="head" src="../../static/images/heart-animation.gif"/> -->
        <scroll-view scroll-y
            class="box"
            @scrolltolower="loadMore">
            <div class="item" v-for="(item, index) in userList" :key="index">
                <image :src="item.user.avatarUrl"/>
                <p>{{item.user.nickName}}</p>
            </div>
        </scroll-view>
        <p class="count">已收到{{userList.length}}位好友送来的祝福</p>
        <div class="bottom">
            <button class="left" lang="zh_CN" open-type="getUserInfo" @getuserinfo="sendGreet">送上祝福</button>
            <button class="right" open-type="share">分享喜悦</button>
        </div>
    </div>
</template>

<script>
import tools from 'common/js/h_tools'
import cloud from '@/service/cloud'
export default {
  name: 'Greet',
  data () {
    return {
      isMore: true,
      page: 0,
      userList: [],
      openId: '',
      userInfo: ''
    }
  },
  onLoad () {
    console.log('greet >>> onLoad')
    const that = this
    that.getUserList()
  },
  methods: {
    sendGreet (e) {
      const that = this
      if (e.target.errMsg === 'getUserInfo:ok') {
        wx.getUserInfo({
          success: function (res) {
            that.userInfo = res.userInfo
            that.getOpenId()
          }
        })
      }
    },
    addUser () {
      const that = this
      const db = wx.cloud.database()
      const user = db.collection('usergreet')
      user.add({
        data: {
          user: that.userInfo
        }
      }).then(res => {
        // that.getUserList()
        let user = {
          user: that.userInfo
        }
        that.userList = that.userList.concat(user)
        tools.showToast('感谢您的祝福~')
      })
    },

    getOpenId () {
      const that = this
      wx.cloud.callFunction({
        name: 'user',
        data: {}
      }).then(res => {
        that.openId = res.result.openid
        that.getIsExist()
      })
    },
    // 查询用户是否存在
    getIsExist () {
      const that = this
      const db = wx.cloud.database()
      const user = db.collection('usergreet')
      user.where({
        _openid: that.openId
      }).get().then(res => {
        if (res.data.length === 0) {
          that.addUser()
        } else {
          tools.showToast('您已经送过祝福了~')
        }
      })
    },

    loadMore () {
      if (!this.isMore) {
        return false
      }
      cloud.get('usergreet', this.page).then((res) => {
        if (res.errMsg === 'collection.get:ok') {
          if (!res.data || res.data.length <= 0) {
            this.isMore = false
          } else {
            if (res.data.length <= 10) {
              this.isMore = false
            }
            this.userList = [...this.userList, ...res.data]
            wx.hideNavigationBarLoading()
            this.page++
          }
        }
      })
    },
    getUserList () {
      const that = this
      wx.showNavigationBarLoading()
      cloud.get('usergreet', this.page).then((res) => {
        if (res.errMsg === 'collection.get:ok') {
          that.userList = res.data
          wx.hideNavigationBarLoading()
          that.page++
        }
      })
    }
  },
  onShareAppMessage (res) { // 监听分享
    // console.log(res)
    return {
      title: '送上您的祝福',
      path: '/pages/index/main',
      imageUrl: '../../static/logo-share.png'
    }
  }
}
</script>

<style lang="stylus" scoped>
.greet
    width 100%
    height 100%
    .head
        height 150rpx
        width 200rpx
        margin 0 auto
    .box
        height 800rpx
        width 690rpx
        margin 50rpx auto
        margin-left 30rpx
        background #fff
        border-radius 16rpx
        box-shadow 0 0 10rpx rgba(0, 0, 0, 0.1)
        display flex
        justify-content center
        align-items flex-start
        flex-wrap wrap
        .item
            width 175rpx
            display flex
            flex-direction column
            justify-content flex-start
            align-items center
            padding 25rpx
            float left
            image
                width 100rpx
                height 100rpx
                border-radius 50rpx
            p
                height 50rpx
                line-height 50rpx
                font-size 24rpx
                color #444
                width 100rpx
                overflow hidden
                text-overflow ellipsis
                white-space nowrap
                text-align center
    .bottom
        height 140rpx
        position fixed
        bottom 0
        left 0
        background rgba(255, 255, 255, 0.5)
        display flex
        justify-content center
        align-items center
        width 100%
        .left
            height 80rpx
            line-height 80rpx
            font-size 28rpx
            width 300rpx
            color #fff
            background #2CA6F9
            margin-right 20rpx
        .right
            height 80rpx
            line-height 80rpx
            font-size 28rpx
            color #fff
            width 300rpx
            background #81d185
    .count
        height 60rpx
        line-height 60rpx
        background rgba(255, 255, 255, 0.5)
        position fixed
        bottom 140rpx
        left 0
        font-size 28rpx
        color #444
        text-align center
        width 100%
</style>
