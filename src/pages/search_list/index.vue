<template>
  <div>
    <!-- 搜索条 -->
    <!-- <search-bar></search-bar> -->
    <div class="search">
      <div class="search-input">
        <icon type='search' size='16' color='#999' />
        {{keyword}}
      </div>
    </div>
    <!-- 选项卡 -->
    <div class="tabs">
      <div @click='tabHandle(index)' :class='{active: currentIndex === index}' :key='index' v-for='(item, index) in tabNames' class="tab-item">
        {{item}}
      </div>
    </div>
    <!-- 商品的列表 -->
    <div class="goods-list">
      <navigator :url="'/pages/goods_detail/main?goods_id=' + item.goods_id" class='goods-item' :key='index' v-for='(item, index) in list'>
        <img :src="item.goods_small_logo" mode="aspectFill" />
        <div class="goods-right">
          <h4>{{item.goods_name}}</h4>
          <div class="price">
            <span>¥</span>{{item.goods_price}}
          </div>
        </div>
      </navigator>
    </div>
    <!-- 提示没有更多数据 -->
    <div class="more" v-if='hasMore'>
      没有更多数据了
    </div>
  </div>
</template>
<script>
import SearchBar from '../../components/searchbar'
import request from '../../utils/request.js'
export default {
  data () {
    return {
      tabNames: ['综合', '销量', '价格'],
      keyword: '',
      currentIndex: 0,
      list: [],
      pagenum: 1,
      total: 0,
      // 保证接口调用完成之后才可以再次调用接口,如果接口正在获取数据,那么在这个过程中是不允许再次触发接口调用(和节流类似)
      isLoading: false,
      // 用于判断有没有更多数据,默认false(有更多数据)
      hasMore: false
    }
  },
  methods: {
    tabHandle (index) {
      // 修改当前tab的索引
      this.currentIndex = index
    },
    async loadData () {
      // 如果没有更多数据,就应该禁止发送请求调用接口
      // if (this.hasMore) {
      //   return
      // }
      // 是否已经加载完成
      // 本次接口调用是否已经加载完成
      if (this.isLoading && this.hasMore) {
        return
      }
      // 作用: 禁止再次触发接口调用
      this.isLoading = true
      // 通用的列表加载方法
      // 根据关键字加载匹配的商品列表数据
      // 调用后台接口获取数据
      let res = await request('goods/search', 'get', {
        // query: param.query
        query: this.keyword,
        pagenum: this.pagenum
      })
      // console.log(res)
      let {message} = res.data
      // 需要把新加载的一页数据添加到list中
      let goods = [...this.list, ...message.goods]
      this.list = goods
      this.pagenum = parseInt(message.pagenum)
      this.total = message.total
      // 判断是否还有更多数据
      // console.log(this.list.length, this.total)
      if (this.list.length >= this.total) {
        // 没有更多数据
        this.hasMore = true
      }
      // 加载完成数据之后,让页码加1
      // this.pagenum = this.pagenum + 1  //当成字符串处理了,需要转换
      // this.pagenum = parseInt(this.pagenum) + 1
      this.pagenum = this.pagenum + 1
      // 接口数据返回之后,才允许再次触发请求
      this.isLoading = false
    }
  },
  components: {
    'search-bar': SearchBar
  },
  async onLoad (param) {
    // 小程序的生命周期函数
    // 参数query表示路径传递过来的参数
    // console.log(query)
    this.keyword = param.query
    // 调用接口
    // 页面初次展示的时候,调用loadData加载数据
    this.loadData()
  },
  // onShow (param) {
  //   // console.log(this.keyword)
  //   console.log(this.hasMore,this.isLoading)
  // },
  // onHide () {
  //   // this.hasMore = false
  // },
  onReachBottom () {
    // 滚动条触底的时候触发该方法
    // console.log(111)
    this.loadData()
  }
}
</script>
<style scoped lang="scss">
@import 'main.scss'
</style>
