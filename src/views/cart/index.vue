<template>
  <div class="tab-cart">
    <van-nav-bar title="购物车" left-arrow>
			<span slot="right" @click="isEditor = !isEditor">{{isEditor ? '完成' : '管理'}}</span>
    </van-nav-bar>
		<!-- <div class="editor_head" v-show="goods.length">
			<van-icon :name="isEditor ? 'success' : 'editor'" />
			<span @click="isEditor = !isEditor">{{isEditor ? '完成' : '编辑'}}</span>
		</div> -->
    <div class="my-popup-main">
    <van-checkbox-group class="card-goods" v-model="checkedGoods">
     	<div v-for="(item, i) in goods" class="card-goods__item">
				<van-checkbox
					:key="item.id"
					:name="item.id"
					:disabled="!item.status"
				>
				</van-checkbox>

        <van-card
          class="my-van-stepper"
          :thumb="item.thumb"
        >
          
					<div class="van-card__row" slot="title">
						<div class="van-card__title">{{item.title}}</div>
						<van-icon name="delete" class="van-card__price" v-if="isEditor" @click="deleteCart(i)" />
					</div>
          <div slot="desc">
            <van-cell :border="false" is-link arrow-direction="down" :value="item.desc" class="my-cart-desc" @click="showStepper = true" />
            <div class="van-card__row">
              <div class="van-sku__goods-price">
                <span class="van-sku__price-symbol">￥</span><span class="van-sku__price-num">{{ item.price }}</span>
              </div>
            <van-stepper
              class="van-card__price"
              v-model="item.num"
              integer
              :min="1"
              :max="20"
              :step="1"
              :default-value="9"
            />
            </div>
          </div>
        </van-card>

				 <!-- <van-card
					:desc="item.desc"
					:num="item.num"
					:thumb="item.thumb"
					>
					<div class="van-card__row" slot="title">
						<div class="van-card__title"><van-tag plain type="danger">海淘</van-tag>{{item.title}}</div>
						<div class="van-card__price">{{item.price | yuan}}</div>
					</div>
					<div slot="footer" v-if="isEditor">
						 <van-stepper v-model="item.num" disableInput/>
					</div>
					<div slot="footer" v-else>生成日期1993-10-54</div>
				 </van-card> -->
				
				<!-- <div class="cart_delete" v-if="isEditor" @click="deleteCart(i)">删除</div> -->
      </div>
    </van-checkbox-group>
    </div>
    
		<!-- <div class="clear_invalid" v-if="goods.length" @click="clearInvalid">
			<van-icon name="lajitong"/>
			清除失效商品
		</div> -->
		
    <is-empty v-if="!goods.length">您的购物车空空如也~</is-empty>
    
    <van-submit-bar
      :price="totalPrice"
      :disabled="!checkedGoods.length"
      :buttonText="submitBarText"
			:loading="isSubmit"
      label="总计"
      @submit="cartSubmit"
     >
    	<van-checkbox v-model="checkedAll" @change="setCheckAll" style="padding: 0 10px;">全选</van-checkbox>
      <span slot="tip" v-if="isEditor && goods.length">        
        <div class="clear_invalid" @click="clearInvalid">
          <van-icon name="lajitong"/>
          清除失效商品
        </div>
      </span>
    </van-submit-bar>

        <van-sku
          v-model="showStepper"
          :sku="skuData.sku"
          :goods="skuData.goods_info"
          :goods-id="skuData.goods_id"
          :hide-stock="skuData.sku.hide_stock"
          :quota="skuData.quota"
          :quota-used="skuData.quota_used"
          :custom-stepper-config="customStepperConfig"
          :message-config="messageConfig"
          @buy-clicked="onBuyClicked"
          @add-cart="onAddCartClicked"
        >        
          <template slot="sku-actions" slot-scope="props">
            <div class="van-sku-actions">
              <van-button type="primary" bottom-action @click="showStepper= false">确定</van-button>
            </div>
          </template>
        </van-sku>
        
  </div>
</template>

<script>
	import {
		Checkbox,
		CheckboxGroup,
		Card,
    SubmitBar,
    Sku,
		Stepper,
	} from 'vant';
	
	import isEmpty from "./cart-empty";
  import skuData from "./data";
	
	export default {
		data() {      
      this.skuData = skuData;
			return {
				isEditor: false,
				checkedAll: false,
				isSubmit: false,
        showStepper: false,
				checkedGoods: [],
				goods: [{
					id: '1',
					title: '进口香蕉进口香蕉进口香蕉进口香蕉进口香蕉进口香蕉进口香蕉进口香蕉进口香蕉',
					desc: '约250g，2根',
					price: 200,
					status: 0,
					num: 1,
					thumb: 'https://img.yzcdn.cn/public_files/2017/10/24/2f9a36046449dafb8608e99990b3c205.jpeg'
				}, {
					id: '2',
					title: '陕西蜜梨',
					desc: '约600g',
					price: 690,
					status: 1,
					num: 3,
					thumb: 'https://img.yzcdn.cn/public_files/2017/10/24/f6aabd6ac5521195e01e8e89ee9fc63f.jpeg'
				}, {
					id: '3',
					title: '美国伽力果',
					desc: '约680g/3个',
					price: 2680,
					status: 1,
					num: 1,
					thumb: 'https://img.yzcdn.cn/public_files/2017/10/24/320454216bbe9e25c7651e1fa51b31fd.jpeg'
        }],
        
        initialSku: {
          s1: '30349',
          s2: '1193'
        },
      customSkuValidator: (component) => {
        return `请选择${skuRow.k}`;
      },
      customStepperConfig: {
        quotaText: '单次限购100件',
        stockFormatter: (stock) => `剩余${stock}间`,
        handleOverLimit: (data) => {
          const { action, limitType, quota } = data;

          if (action === 'minus') {
            this.$toast('至少选择一件商品');
          } else if (action === 'plus') {
            if (limitType === LIMIT_TYPE.QUOTA_LIMIT) {
              this.$toast(`限购${quota}件`);
            } else {
              this.$toast('库存不够了~~');
            }
          }
        }
      },
      messageConfig: {
        uploadImg: (file, img) => {
          return new Promise(resolve => {
            setTimeout(() => resolve(img), 1000);
          });
        },
        uploadMaxSize: 3
      }
			};
		},
		
		activated(){
			this.checkedAll = false;
			this.isEditor = false;
			this.isSubmit = false;
		},

		computed: {
			submitBarText() {
				const count = this.checkedGoods.length;
				return this.isEditor ? "删除" : '结算' + (count ? `(${count})` : '');
			},
			totalPrice() {
				return this.goods.reduce((total, item) => total + (this.checkedGoods.indexOf(item.id) !== -1 ? item.price * item.num : 0), 0);
			},
		},

		methods: {
			cartSubmit(){
				if(this.isEditor){
					this.$dialog.confirm({message: "确定删除所选商品吗?", cancelButtonText: "再想想"}).then(() => {
						this.goods = this.goods.filter( goods => (this.checkedGoods.indexOf(goods.id) == -1));
						this.checkedGoods = [];
						this.checkedAll = false;
						this.isEditor = !this.goods.length;
					})
				}else{
					this.isSubmit = true;
					this.$router.push({name: "placeOrderEntity"})
				}
			},
			formatPrice(price) {
				return (price / 100).toFixed(2);
			},
			setCheckAll(val){
				this.checkedGoods = val ? this.goods.filter(goods => !!goods.status).map(goods => goods.id) : [];
			},
			deleteCart(i){
				this.$dialog.confirm({message: "确定删除所选商品吗", cancelButtonText: "再想想"}).then(() => {
					let goodsId = this.goods.splice(i, 1)[0].id;
					this.$nextTick(() => {
						this.deleteNext(goodsId)
					})
				})
			},
			deleteNext(goodsId){
				this.isEditor = !!this.goods.length;
				this.checkedGoods.forEach((goods, i) => {
					if(goods.id == goodsId){
						this.checkedGoods.splice(i, 1);
						return false;
					}
				})
			},
			clearInvalid(){
				this.$dialog.confirm({message: "确定清除所有失效商品吗?", cancelButtonText: "再想想"}).then(() => {
					this.goods = this.goods.filter(goods => goods.status)
				})
			},
      onBuyClicked(data) {
        this.$toast(JSON.stringify(data));
      },

      onAddCartClicked(data) {
        this.$toast(JSON.stringify(data));
      },

      onPointClicked() {
        this.$toast('积分兑换');
      }
		},

		components: {
			[Card.name]: Card,
			[Stepper.name]: Stepper,
			[isEmpty.name]: isEmpty,
			[Checkbox.name]: Checkbox,
			[SubmitBar.name]: SubmitBar,
			[CheckboxGroup.name]: CheckboxGroup,
		},
	}

</script>


<style lang="scss" scoped>
	
	@import "../../styles/one-border";
  @import "../../styles/var";
  
  // 已选商品信息
  .my-cart-desc {
    padding: 0 5px;
    margin: 5px 0;
    background: #f5f5f5;
  }

  // 数量进步器
  .van-stepper__input {
    width: 40px;
    height: 18px;
  }

  .van-stepper__minus, .van-stepper__plus {
    width: 22px;
    height: 22px;
  }
  
	.tab-cart{
		padding-bottom: 50px;
    box-sizing: border-box;
    background-color: #f2f2f2;
    height: 100vh;
	}
	
	.editor_head{
		@include one-border;
		text-align: right;
		padding: 10px;
		font-size: $font-size-normal;
		background-color: #fff;
	}

	.card-goods {
		background-color: $bg-color;
		.card-goods__item {
			display: flex;
			align-items: center;
			margin-bottom: 10px;
			background-color: #fff;
		}
		.cart_delete{
			line-height: 100px;
			padding: 0 10px;
			color: #fff;
			background-color: $red;
		}
		.card-goods__footer{
			font-size: $font-size-normal;
			color: $font-color-gray;
		}
	}
	
	.clear_invalid{
		width: 120px;
		color: $font-color-gray;
		border: 1px solid $font-color-gray;
		margin: 0 auto;
		text-align: center;
		padding: 5px 3px;
		// margin-top: 20px;
		border-radius: 3px;
	}

</style>
