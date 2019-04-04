<template>
  <div>
    <!-- 输入框 -->
    <div class="search">
      <div class="search-content">
        <!-- 搜索输入框 -->
        <div class="search-input">
          <icon type='search' size='16'/> 
          <input @input='inputHandle' v-model='keyword' placeholder="请输入关键字"/>
        </div>
        <button v-if='keyword' class="cancel">取消</button>
        <!-- 搜索结果 -->
        <div v-if='keyword' class="search-modal">
          <div :key='item.goods_id' v-for='item in searchResult' class="search-item">
            {{item.goods_name}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import request from '../../utils/request.js'
export default {
  data () {
    return {
      keyword: '',
      searchResult: [],
      isLoading: false,
      timer: null
    }
  },
  methods: {
    async inputHandle () {
      // 根据输入的关键字,调用后台接口查询匹配的数据
      // 控制请求的频率(节流): 输入多个字符, 但是只发送一次请求
      // 限制请求发出的频率-->函数节流
      // 控制是否发送请求
      if (this.isLoading) {
        // 终止后续代码的执行,终止请求
        return
      }
      // isLoading变成true之后,就阻止了后续请求
      this.isLoading = true
      this.timer = setTimeout(async () => {
        // 关闭之前的定时任务
        clearTimeout(this.timer)
        // request是一个异步的函数
        let res = await request('goods/qsearch', 'get', {
          query: this.keyword
        })
        const {message} = res.data
        this.searchResult = message
        // 重新打开发送请求的开关
        this.isLoading = false
      }, 1000)
    }
  }
}
</script>

<style scoped lang="scss">
@import 'main.scss'
</style>
