<template>
  <div>
    <!-- 搜索条 -->
    <search-bar></search-bar>
    <!-- 轮播图 -->
    <swiper indicator-dots='true'>
        <swiper-item :key='item.goods_id' v-for='item in swiper'>
          <image :src="item.image_src" class="slide-image"/>
        </swiper-item>
    </swiper>
    <!-- 菜单 -->
    <div class="menu">
      <div :key='index' v-for='(item, index) in menu' class="menu-item">
        <img :src="item.image_src">
      </div>
    </div>
    <!-- 商品列表 -->
    <div class="floor" :key='index' v-for='(item, index) in floor'>
      <!-- 楼层的头部 -->
      <div class="floor-title">
        <img :src="item.floor_title.image_src" mode="aspectFill">
      </div>
      <div class="floor-content">
        <div class="left">
          <img :src="item.product_list[0].image_src" mode="aspectFill">
        </div>
        <div class="right">
          <!-- 这里v-if的作用是去掉数组中的第一张图片,让i从1开始,跳过0 -->
          <img v-if='i > 0' :key='i' v-for='(img, i) in item.product_list' :src="img.image_src" mode="aspectFill">
        </div>
      </div>
    </div>
    <!-- 回到顶部按钮 -->
    <div class="toTop" @click='toTopHandle' v-if='isShow'>
      ︿
      <p>顶部</p>
    </div>

  </div>
</template>

<script>
import request from '../../utils/request.js'
import SearchBar from '../../components/searchbar'
export default {
  data () {
    return {
      swiper: [],
      menu: [],
      floor: [],
      isShow: false
    }
  },
  methods: {
    // 方法写到methods中
    async queryData (path) {
      // 通用接口调用方法
      let res = await request(path)
      return res.data.message
    },
    // async swiperData () {
    //   // 请求后台接口获取轮播图数据
    //   // let that = this
    //   // mpvue.request({
    //   //   url: 'https://www.zhengzhicheng.cn/api/public/v1/home/swiperdata',
    //   //   success: function (res) {
    //   //     // console.log(res)
    //   //     let {message} = res.data
    //   //     // console.log(message)
    //   //     // console.log(this)
    //   //     that.swiper = message
    //   //   }
    //   // })
    //   // let url = 'home/swiperdata'
    //   // request(url, 'get', {}, {}, () => {
    //   //   console.log(1)
    //   // })
    //   // request(url).then(res => {
    //   //   console.log(res)
    //   // })
    //   // let res = await request('home/swiperdata')
    //   // let {message} = res.data
    //   // console.log(message)
    //   // this.swiper = message
    //   // this.swiper = res.data.message
    //   this.swiper = await this.queryData('home/swiperdata')
    // },
    // async menuData () {
    //   // 请求后台接口获取菜单数据
    //   // let that = this
    //   // mpvue.request({
    //   //   url: 'home/catitems',
    //   //   success: function (res) {
    //   //     // console.log(res)
    //   //     let {message} = res.data
    //   //     // console.log(message)
    //   //     // console.log(this)
    //   //     that.menu = message
    //   //   }
    //   // })
    //   // let res = await request('home/catitems')
    //   // this.menu = res.data.message
    //   this.menu = await this.queryData('home/catitems')
    // },
    // async floorData () {
    //   // 楼层数据
    //   this.floor = await this.queryData('home/floordata')
    // },
    toTopHandle () {
      // 控制回到顶部
      // wx.pageScrollTo也可以,但是不推荐使用
      mpvue.pageScrollTo({
        scrollTop: 0
      })
    },
    async initData () {
      this.swiper = await this.queryData('home/swiperdata')
      this.menu = await this.queryData('home/catitems')
      this.floor = await this.queryData('home/floordata')
    }
  },
  components: {
    'search-bar': SearchBar
  },
  mounted () {
    // 调用接口请求方法
    // this.swiperData()
    // this.menuData()
    // this.floorData()
    this.initData()
  },
  onPullDownRefresh () {
    //  下拉刷新,重新加载页面的数据
    // console.log(123)
    // this.swiperData()
    // this.menuData()
    // this.floorData()
    this.initData()
  },
  onPageScroll (event) {
    // 小程序生命周期函数,监控页面的滚动
    // console.log(123)
    // 如果滚动指定大小,那么久控制显示或隐藏
    // console.log(event.scrollTop)
    this.isShow = event.scrollTop > 50
  }
}
</script>

<style scoped lang='scss'> 
  @import 'main.scss' 
</style>
