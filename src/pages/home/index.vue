<template>
  <div>
    <!-- 搜索条 -->
    <div class="search-bar">
      <div class="search-input">
       <!-- <icon type='search' color='#999'/> -->
       <input placeholder="搜索"/>
      </div>
    </div>
    <!-- 轮播图 -->
    <swiper indicator-dots='true'>
        <swiper-item :key='item.goods_id' v-for='item in swiper'>
          <image :src="item.image_src" class="slide-image" />
        </swiper-item>
    </swiper>
    <!-- 菜单 -->
    <div class="menu">
      <div :key="index" v-for='(item,index) in menu'  class="menu-item">
        <img :src="item.image_src"/>
      </div>
       
    </div>
  </div>
</template>

<script>
import request from '../../utils/request.js'
export default {
  data () {
    return {
      swiper: [],
      menu: []
    }
  },
  methods: {
    async queryData (path) {
      // 通用接口调用接口与
      let res = await request(path)
      return res.data.message
    },
    async swiperData () {
      // 请求后台接口获取轮播图数据
      // let that = this
      // mpvue.request({
      //   url: 'https://www.zhengzhicheng.cn/api/public/v1/home/swiperdata',
      //   success: function (res) {
      //     // console.log(res)
      //     let {message} = res.data
      //     // console.log(message)
      //     // console.log(this)
      //     that.swiper = message
      //   }
      // })
      // let url = 'home/swiperdata'
      // request(url, 'get', {}, {}, () => {
      //   console.log(1)
      // })
      // request(url).then(res => {
      //   console.log(res)
      // })
      // let res = await request('home/swiperdata')
      // let {message} = res.data
      // console.log(message)
      // this.swiper = message
      // this.swiper = res.data.message
      this.swiper = await this.queryData('home/swiperdata')
    },
    async menuData () {
      // 请求后台接口获取菜单数据
      // let that = this
      // mpvue.request({
      //   url: 'home/catitems',
      //   success: function (res) {
      //     // console.log(res)
      //     let {message} = res.data
      //     // console.log(message)
      //     // console.log(this)
      //     that.menu = message
      //   }
      // })
      // let res = await request('home/catitems')
      // this.menu = res.data.message
      this.menu = await this.queryData('home/catitems')
    }
  },
  mounted () {
    // 调用接口请求方法
    this.swiperData()
    this.menuData()
  }
}
</script>

<style scoped> 
/*scoped:表示局部作用域*/
.search-bar {
  background-color: #Eb4450;
  padding: 20rpx;
}
.search-input {
  background-color: #fff;
  text-align: center;
}
.slide-image {
  width: 750rpx;
}
.menu {
  display: flex;
  /* 周围填充间距,平均分配 */
  justify-content: space-around;
}
.menu .menu-item img {
  width: 128rpx;
  height: 140rpx;
}
</style>
