<template>
  <div class="container">
    <!-- 地址信息的展示 -->
    <div class="cart-top" v-if='address'>
      <div class="receive">
        <div class="name">收货人: {{address.username}}</div>
        <div class="phonen-number">{{address.telNumber}}</div>
      </div>
      <div class="address">收货地址:{{detaiAddress}}</div>
      <img src="../../../static/images/cart_border@2x.png" class="address-bar" mode='aspectFill'>
    </div>
    <!-- 新增收货人信息 -->
    <div class="add_addresss" v-else @click='getAddressInfo'>
      <text>新增收货人</text>
      <span></span>
    </div>
    <div class="list-title">优购生活馆</div>
    <!-- 商品列表信息 -->
    <div class="ware-list">
      <div :key="item.goods_id" v-for="item in products" class="ware-item">
        <!-- 左侧的按钮checkbox -->
        <div class="choice-button">
          <icon @click='changeItemCheckbox(item.goods_id)' :color="item.checked?'red':'#eee'" type="success" size='18'/>
        </div>
        <!-- 右侧商品信息 -->
        <div class="ware-content">
          <!-- 左侧图片 -->
          <navigator class="ware-image">
            <img :src="item.goods_small_logo" mode='aspectFill'>
          </navigator>
          <!-- 右侧商品信息 -->
          <div class="ware-info">
            <!-- 商品名称 -->
            <navigator class='ware-title'>
              {{item.goods_name}}
            </navigator>
            <!-- 商品的价格和数量变更 -->
            <div class="ware-info-btn">
              <!-- 商品价格 -->
              <div class="ware-price">
                <span>¥</span>
                {{item.goods_price}}
              </div>
              <!-- 数量变更 -->
              <div class="calculate">
                <div class="rect">-</div>
                <div class="number">{{item.num}}</div>
                <div class="rect">+</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- 底部菜单 -->
    <div class="cart-total">
      <!-- 左侧CheckBox -->
      <div class="total-button">
        <icon @click='selectAll' :color="isAll?'red':'#eee'" type='success' size='18'/>全选
      </div>
      <!-- 中间的价格 -->
      <div class="total-center">
        <div class="total-price">合计:
          <text class="colorRed">
            <text>¥</text>
          </text>
        </div>
        <div class="price-tips">包含运费</div>
      </div>
      <!-- 右侧结算按钮 -->
      <div class="accounts">
        结算
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      products: [],
      address: null,
      isAll: false
    }
  },
  computed: {
    // 计算属性
    // 基于现有的属性,而产生新的数据
    detaiAddress () {
      let {provinceName, cityName, countyName, detailInfo} = this.address
      return `${provinceName}${cityName}${countyName}${detailInfo}`
    }
  },
  methods: {
    selectAll () {
      // 实现所有商品的全部选中或者全部取消
      // 实现思路: 把products中所有商品的checked属性全部修改一遍
      // 控制全选按钮的样式
      this.isAll = !this.isAll
      let products = [...this.products]
      // 修改所有的商品的选中状态
      products.forEach(item => {
        item.checked = this.isAll
      })
      this.products = products
    },
    changeItemCheckbox (id) {
      // 控制每件商品的选中与否:本质上就是控制每件商品的checked属性值
      // console.log(id)
      // 根据id去修改相应商品的checked(保证该值在true和false之间进行切换)
      // some的作用:终止的条件是return true,查询数组中有没有匹配条件的元素,有一个符合的返回就是true,可以利用它做遍历.
      // 不推荐修改原来的数据,推荐使用一个新的数据,然后再覆盖原来的数据
      let products = [...this.products]
      products.some(item => {
        if (item.goods_id === id) {
          // console.log('----')
          // 表示找到了要选中的商品
          item.checked = !item.checked
          // 终止遍历
          return true
        }
      })
      this.products = products
    },
    getAddressInfo () {
      // 获取地址信息
      let that = this
      mpvue.chooseAddress({
        success (res) {
          // console.log(res)
          that.address = res
          // 同时存储在本地存储中
          mpvue.setStorageSync('myAddress', res)
        }
      })
    },
    getCartData () {
      // 获取购物车数据
      let cdata = mpvue.getStorageSync('mycart') || {}
      // console.log(cdata)
      // 把cdata对象转化为数组
      let products = []
      for (let key in cdata) {
        // 可以给没意见商品添加一个属性checked
        // checked属性的作用: 控制商品是否选中;false是不选中
        cdata[key].checked = false
        // console.log(cdata[key])
        products.push(cdata[key])
      }
      console.log(products)
      this.products = products
    }
  },
  onLoad () {
    // 从本地存储中获取购物车商品信息
    this.getCartData()
    // 页面加载成功后,从本地存储中获取地址信息
    this.address = mpvue.getStorageSync('myAddress')
  }
}
</script>
<style scoped lang="scss">
@import 'main.scss'
</style>
