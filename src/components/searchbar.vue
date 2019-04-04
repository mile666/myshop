<template>
 <!-- 搜索条 -->
  <div class="search-bar">
    <div class="search-input">
      <navigator url='/pages/search/main'>
        <icon type='search' color='#999'/>
        <!-- <input placeholder="搜索"/> -->
        <span>搜索</span>
      </navigator>
    </div>
   </div>
</template>
<style scoped lang='scss'>
/*scoped:表示局部作用域*/
/* // $color: #Eb4450; */
.search-bar {
  /* // background-color: $color; */
  background-color: #Eb4450;
  padding: 20rpx;
  .search-input {
    background-color: #fff;
    text-align: center;
  }
}
</style>
