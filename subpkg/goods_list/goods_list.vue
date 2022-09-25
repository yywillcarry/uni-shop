<template>
  <view>
    <view class="goods-list">
     <view v-for="(item, i) in goodsList" :key="i" @click="gotoDetail(item)">
   <my-goods :goods="item"></my-goods>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
data() {
  return {
        // 商品列表的数据
        goodsList: [],
        isloading: false,
        // 总数量，用来实现分页
        total: 0,
    // 请求参数对象
    queryObj: {
      // 查询关键词
      query: '',
      // 商品分类Id
      cid: '',
      // 页码值
      pagenum: 1,
      // 每页显示多少条数据
      pagesize: 10
    }
  }
},
onLoad(options) {
  // 将页面参数转存到 this.queryObj 对象中
  this.queryObj.query = options.query || ''
  this.queryObj.cid = options.cid || ''
  this.getGoodsList()
},
methods: {
  // 获取商品列表数据的方法
  async getGoodsList(cb) {
    this.isloading = true
    // 发起请求
    const { data: res } = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
     this.isloading = false
     cb && cb()
    if (res.meta.status !== 200) return uni.$showMsg()
    // 为数据赋值
  this.goodsList = [...this.goodsList, ...res.message.goods]
  this.total = res.message.total
  },
  gotoDetail(item) {
    uni.navigateTo({
      url: '/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id
    })
  }
},
onReachBottom() {
   if (this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('数据加载完毕！')
  if (this.isloading) return
  this.queryObj.pagenum +=1
  this.getGoodsList()
},
onPullDownRefresh() {
  this.queryObj.pagenum = 1
  this.total = 0
  this.isloading = false
  this.goodsList = []
  this.getGoodsList(() => uni.stopPullDownRefresh())
}
  }
</script>

<style lang="scss">

</style>
