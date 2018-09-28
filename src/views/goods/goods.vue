<template>
<div class="mymall">
  <div class="goods">
    <van-swipe class="goods-swipe" :autoplay="3000">
      <van-swipe-item v-for="thumb in scope.row.thumb" :key="thumb">
        <img :src="thumb" >
      </van-swipe-item>
    </van-swipe>

    <van-cell-group>
      <van-cell>
        <div class="goods-title">{{ scope.row.title }}</div>
        <div class="goods-price">{{ scope.row.price }}</div>
      </van-cell>
      <van-cell class="goods-express">
        <van-col span="10">运费：{{ scope.row.express }}</van-col>
        <van-col span="14">剩余：{{ scope.row.remain }}</van-col>
      </van-cell>
    </van-cell-group>

    <van-cell-group class="goods-cell-group">
      <van-cell value="进入店铺" icon="shop" is-link @click="sorry">
        <template slot="title">
          <span class="van-cell-text">有赞的店</span>
          <van-tag class="goods-tag" type="danger">官方</van-tag>
        </template>
      </van-cell>
      <van-cell title="线下门店" icon="location" is-link @click="sorry" />
    </van-cell-group>

    <van-cell-group class="goods-cell-group">
      <van-cell title="查看商品详情" is-link @click="sorry" />
    </van-cell-group>

    <van-goods-action>
      <van-goods-action-mini-btn icon="chat" @click="sorry">
        客服
      </van-goods-action-mini-btn>
      <van-goods-action-mini-btn icon="cart" @click="onClickCart">
        购物车
      </van-goods-action-mini-btn>
      <van-goods-action-big-btn @click="sorry">
        加入购物车
      </van-goods-action-big-btn>
      <van-goods-action-big-btn primary @click="sorry">
        立即购买
      </van-goods-action-big-btn>
    </van-goods-action>
  </div>
</div>
</template>

<script>
import { fetchList } from '@/api/goods'
export default {


  methods: {
    formatPrice() {
      return '¥' + (this.goods.price / 100).toFixed(2)
    },

    onClickCart() {
      this.$router.push('cart')
    },

    sorry() {
      Toast('暂无后续逻辑~')
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
  @import "src/styles/mymall.scss";
</style>