<template>
  <div class="content-body">
    <van-nav-bar title="商品详情" left-arrow class="header-dom my-goods-bar">
      <van-icon name="cart" slot="right" @click="showBase = true" />
    </van-nav-bar>
    <div class="detail-header-bg">
      <div id="head" class="header-dom">
        <div class="mui-flex">
          <div class="left-btns">
            <div class="icon-link back-link">
              <van-icon name="arrow-left" class="header-icon" />
            </div>
          </div>
          <ul class="cell">
          </ul>
          <div class="right-btns">
            <div class="icon-link cart-link" @click="showBase = true">
              <van-icon name="cart" class="header-icon" />
            </div>    
          </div>
        </div>
        <div id="J_smartjump" class="smart-jump"></div>
      </div>
    </div>

    <!-- swiper -->
    <swiper :options="swiperOption">
      <swiper-slide v-for="thumb in goods.thumb" :key="thumb">
        <img :src="thumb">
      </swiper-slide>

      <div class="swiper-pagination" slot="pagination"></div>
    </swiper>

    <div class="goods-header">

      <van-cell-group>
        <van-cell>
          <template slot="title">
            <div class="my-item-title">
              <van-tag type="danger">{{ goods.tag }}</van-tag>
              <span class="my-title">{{ goods.title }}</span>
              <div class="my-title-mini">{{ goods.subtitle }}</div>
            </div>

            <div class="my-item-title">
              <span class="my-title-price">
                <span class="my-price-symbol">¥</span>{{ formatPrice(goods.price) }}
              </span>
              <!-- <span class="weui-badge">{{ goods.pricetag }}</span> -->
              <van-tag plain type="danger">{{ goods.pricetag }}</van-tag>
            </div>

            <div class="my-item-title">
              <span class="my-title-mini">专柜价:
                <span class="my-title-mini-line">{{ formatMarketPrice(goods.marketprice) }}</span>
              </span>
            </div>

            <van-row class="my-item-title my-title-mini">
              <van-col span="10">运费：{{ goods.express }}</van-col>
              <van-col span="8">剩余：{{ goods.remain }}</van-col>
              <van-col offset="3" span="3" class="my-right">
                <van-button size="mini" type="danger" plain>
                  <van-icon name="like-o" />收藏</van-button>
              </van-col>
            </van-row>

            <van-row gutter="2" class="my-item-title my-title-mini">
              <van-col span="6">
                <van-icon name="passed" color="red" />官方授权</van-col>
              <van-col span="6">
                <van-icon name="passed" color="red" />正品保证</van-col>
              <van-col span="6">
                <van-icon name="passed" color="red" />七天退换</van-col>
              <!-- <van-col span="6"><van-icon name="passed" color="red" />材质保真</van-col> -->
            </van-row>
          </template>
        </van-cell>

      </van-cell-group>

      <van-cell-group>
        <van-cell :border="false" is-link @click="show4 = true">
          <template slot="title">
            <div class="coupon-body">
              <div class="coupon-body-title">优惠</div>

              <div class="coupon-body-box mui-flex">
                <div class="ic-box">
                  <img src="//img.alicdn.com/tfs/TB1XjtERXXXXXbPXFXXXXXXXXXX-180-36.png_320x320Q50s50.jpg_.webp" alt="购物券图标"
                    style="display: inline-block;">
                </div>
                <div class="my-title-minis cell">领取店铺优惠券，单次购物满100元可用。</div>
              </div>

              <div class="coupon-body-box mui-flex">
                <div class="ic-box">
                  <img src="//img.alicdn.com/tfs/TB1WrHnpsj_B1NjSZFHXXaDWpXa-302-36.png_320x320Q50s50.jpg_.webp" alt="购物券图标"
                    style="display: inline-block;">
                </div>
                <div class="my-title-minis cell">领取购物津贴每满300减30元。</div>
              </div>
            </div>
          </template>
        </van-cell>

        <van-cell clickable @click="show1 = true">
          <template slot="title">
            <span class="my-title-mini">促销</span>
            <van-tag plain type="danger">{{ goods.selltag }}</van-tag>
            <span class="my-title-minis">{{ goods.onsell }}</span>
          </template>
        </van-cell>

      </van-cell-group>

      <!-- <van-cell-group>

        <van-cell :value="$t('领取')" is-link>
        <van-cell :border="false" is-link @click="AshowCoupon = true" block>
          <template slot="title">
            <div class="coupon-body">
              <div class="coupon-body-title">优惠</div>

              <div class="coupon-body-box mui-flex">
                <div class="ic-box">
                  <img src="//img.alicdn.com/tfs/TB1XjtERXXXXXbPXFXXXXXXXXXX-180-36.png_320x320Q50s50.jpg_.webp" alt="购物券图标"
                    style="display: inline-block;">
                </div>
                <div class="my-title-minis">领取优惠券</div>
              </div>

              <div class="coupon-body-box mui-flex">
                <div class="ic-box">
                  <img src="//img.alicdn.com/tfs/TB1WrHnpsj_B1NjSZFHXXaDWpXa-302-36.png_320x320Q50s50.jpg_.webp" alt="购物券图标"
                    style="display: inline-block;">
                </div>
                <div class="my-title-minis">领津贴每满300减30领津贴每满300减30</div>
              </div>
            </div>
          </template>
        </van-cell>

        <van-cell clickable>
          <template slot="title">
            <span class="my-title-mini">促销</span>
            <van-tag plain type="danger">{{ goods.selltag }}</van-tag>
            <span class="my-title-minis">{{ goods.onsell }}</span>
          </template>
        </van-cell>

      </van-cell-group> -->

      <van-cell-group>
        <!-- <van-cell :value="goods.selltag" is-link> -->
        <van-cell :border="false" is-link @click="showBase = true">
          <template slot="title">
            <span class="my-title-mini">选择</span>
            <span class="my-title-minis">请选择 颜色分类 尺码 </span>
          </template>
        </van-cell>

        <van-cell is-link @click="show6 = true">
          <template slot="title">
            <span class="my-title-mini">参数</span>
            <span class="my-title-minis">材质 风格...</span>
          </template>
        </van-cell>
      </van-cell-group>

      <van-cell-group>

        <van-cell is-link @click="show4 = true">
          <template slot="title">
            <div class="detail-shop-logo">
              <img src="//img.alicdn.com/imgextra//d0/5d/TB1Ade.fpYM8KJjSZFuSuwf7FXa.jpg_120x120Q50s50.jpg_.webp"
                aria-label="店铺标志">
            </div>
            <div class="van-cell-text">
              <span>小米官方旗舰店</span>
              <!-- <van-tag class="goods-tag" type="danger">官方</van-tag> -->
            </div>
          </template>
          <van-icon slot="right-icon" color="red" name="points-mall" class="van-cell__right-icon" />
        </van-cell>
        <van-cell title="门店地址" icon="location" is-link @click="show5 = true" />
      </van-cell-group>

    </div>

    <van-cell-group>
      <van-popup v-model="show5" position="right">
        <div class="my-popup-body">
          <div class="van-actionsheet__header van-hairline--bottom">
            门店详情
            <van-icon name="close" @click="show5 = false" />
          </div>

          <swiper :options="swiperOption">
            <swiper-slide v-for="thumb in goods.thumb" :key="thumb">
              <img :src="thumb">
            </swiper-slide>
            <div class="swiper-pagination" slot="pagination"></div>
          </swiper>

          <van-cell-group class="my-popup-main">
            <van-cell title="营业时间：8:00-21:00" icon="clock" />
            <van-cell title="联系电话：400888888" icon="phone" is-link url="tel:400888888" />
            <van-cell title="门店详细地址136444门店详细地址" icon="location" />
          </van-cell-group>
          <van-cell-group>
            <van-cell is-link @click="show6 = true">
              <template slot="title">
                <span class="my-title-minis">地图</span>
              </template>
            </van-cell>

          </van-cell-group>
        </div>

        <van-goods-action>
          <van-goods-action-big-btn primary @click="show5 = false">
            确定
          </van-goods-action-big-btn>
        </van-goods-action>
      </van-popup>

      <van-actionsheet v-model="show2" :actions="actions" @select="onSelect">

        <div class="van-sku-actions">
          <van-button type="primary" bottom-action @click="onPointClicked">确定</van-button>
        </div>
      </van-actionsheet>

      <van-actionsheet v-model="show3" title="支持以下配送方式">
        <van-cell title="线下门店123" icon="location" />
        <van-cell title="线下门店" icon="location" is-link @click="sorry" />
      </van-actionsheet>

      <van-popup v-model="show1" position="bottom">
        <div class="my-popup-body">
          <div class="van-actionsheet__header van-hairline--bottom">
            优惠信息
            <van-icon name="close" @click="show1 = false" />
          </div>
          <van-cell>
            <template slot="title">
              <span>满200减20，满300减50元。</span>
            </template>
          </van-cell>
        </div>

        <van-goods-action>
          <van-goods-action-big-btn primary @click="show1 = false">
            确定
          </van-goods-action-big-btn>
        </van-goods-action>
      </van-popup>

      <van-popup v-model="show6" position="bottom">
        <div class="my-popup-body">
          <div class="van-actionsheet__header van-hairline--bottom">
            基本信息
            <van-icon name="close" @click="show6 = false" />
          </div>
          <div class="my-popup-table">
            <table>
              <tbody>
                <tr>
                  <th>材质</th>
                  <td>涤纶 </td>
                </tr>
                <tr>
                  <th>材质成分</th>
                  <td>聚酯纤维100% </td>
                </tr>
                <tr>
                  <th>销售渠道类型</th>
                  <td>商场同款(线上线下都销售) </td>
                </tr>
                <tr>
                  <th>风格</th>
                  <td>街头 </td>
                </tr>
                <tr>
                  <th>货号</th>
                  <td>03046024800-24 </td>
                </tr>
                <tr>
                  <th>服装版型</th>
                  <td>直筒 </td>
                </tr>
                <tr>
                  <th>厚薄</th>
                  <td>常规 </td>
                </tr>
                <tr>
                  <th>品牌</th>
                  <td>ZARA </td>
                </tr>
                <tr>
                  <th>组合形式</th>
                  <td>单件 </td>
                </tr>
                <tr>
                  <th>衣长</th>
                  <td>常规 </td>
                </tr>
                <tr>
                  <th>袖长</th>
                  <td>长袖 </td>
                </tr>
                <tr>
                  <th>领子</th>
                  <td>其他 </td>
                </tr>
                <tr>
                  <th>袖型</th>
                  <td>常规 </td>
                </tr>
                <tr>
                  <th>衣门襟</th>
                  <td>拉链 </td>
                </tr>
                <tr>
                  <th>图案</th>
                  <td>纯色 </td>
                </tr>
                <tr>
                  <th>流行元素/工艺</th>
                  <td>拉链 </td>
                </tr>
                <tr>
                  <th>面料</th>
                  <td>涤纶 </td>
                </tr>
                <tr>
                  <th>成分含量</th>
                  <td>96%及以上 </td>
                </tr>
                <tr>
                  <th>适用年龄</th>
                  <td>25-29周岁 </td>
                </tr>
                <tr>
                  <th>年份季节</th>
                  <td>2018年春季 </td>
                </tr>
                <tr>
                  <th>颜色分类</th>
                  <td>黑色 </td>
                </tr>
                <tr>
                  <th>尺码</th>
                  <td>XS (160/80A) S (165/84A) M (170/88A) L (175/96A) XL (175/100A) XXL (180/108A) </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <van-goods-action>
          <van-goods-action-big-btn primary @click="show6 = false">
            确定
          </van-goods-action-big-btn>
        </van-goods-action>
      </van-popup>

      <van-popup v-model="show4" position="bottom">
        <div class="my-popup-body">
          <h3 class="my-sheet-title">店铺优惠券</h3>
          <van-cell-group>
            <div class="my-popup-main">

              <van-cell :border="false" clickable @click="show2 = true">
                <template slot="title">
                  <div class="coupon-box" style="background:#FFF1F1;  color:#FF615E; ">
                    <div class="mui-flex">
                      <div class="coupon-main cell">
                        <div class="my-tb">
                          <div class="tc">
                            <div class="c-main"> <span class="my-price-symbol">￥</span><span>200</span> </div>
                            <div class="sub">满800使用</div>
                            <div class="sub van-ellipsis">有效期 2018.09.09-2018.09.10</div>
                          </div>
                        </div>
                      </div>
                      <div class="coupon-split ">
                        <div class="line" style="border-color:#FFCDCD;opacity: 1;"></div>
                      </div>
                      <div class="coupon-operator cell ">
                        <div class="my-tb">
                          <div class="tc">
                            <div>立即领取</div>
                            <div>123132</div>
                            <div class="cs">13564987</div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </template>
              </van-cell>

              <van-cell :border="false" clickable @click="show2 = true">
                <template slot="title">
                  <div class="coupon-box" style="background:#F24B4B; color:#FFFFFF;">
                    <div class="mui-flex">
                      <div class="coupon-main cell">
                        <div class="my-tb">
                          <div class="tc">
                            <div class="c-main"> <span style="font-size: 20px;">购物优惠券</span> </div>
                            <div class="sub">100%中奖 最高面值100元</div>
                            <div class="sub van-ellipsis">有效期：7天</div>
                          </div>
                        </div>
                      </div>
                      <div class="coupon-split ">
                        <div class="line" style="border-color:#FFFFFF;opacity: 1;"></div>
                      </div>
                      <div class="coupon-operator cell ">
                        <div class="my-tb">
                          <div class="tc">
                            <div>立即领取</div>
                            <div>兑换</div>
                            <div class="cs">您有5次兑换机会</div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </template>
              </van-cell>

              <van-cell :border="false" clickable @click="show2 = true">
                <template slot="title">
                  <div class="coupon-box" style="background:#FFF1F1;  color:#FF615E; ">
                    <div class="mui-flex">
                      <div class="coupon-main cell">
                        <div class="my-tb">
                          <div class="tc">
                            <div class="c-main"> <span class="my-price-symbol">￥</span><span>200</span> </div>
                            <div class="sub">满800使用</div>
                            <div class="sub van-ellipsis">有效期 2018.09.09-2018.09.10</div>
                          </div>
                        </div>
                      </div>
                      <div class="coupon-split ">
                        <div class="line" style="border-color:#FFCDCD;opacity: 1;"></div>
                      </div>
                      <div class="coupon-operator cell ">
                        <div class="my-tb">
                          <div class="tc">
                            <div>立即领取</div>
                            <div></div>
                            <div class="cs"></div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </template>
              </van-cell>

              <van-cell :border="false" clickable @click="show2 = true">
                <template slot="title">
                  <div class="coupon-box" style="background:#FFF1F1;  color:#FF615E; ">
                    <div class="mui-flex">
                      <div class="coupon-main cell">
                        <div class="my-tb">
                          <div class="tc">
                            <div class="c-main"> <span class="my-price-symbol">￥</span><span>200</span> </div>
                            <div class="sub">满800使用</div>
                            <div class="sub van-ellipsis">有效期 2018.09.09-2018.09.10</div>
                          </div>
                        </div>
                      </div>
                      <div class="coupon-split ">
                        <div class="line" style="border-color:#FFCDCD;opacity: 1;"></div>
                      </div>
                      <div class="coupon-operator cell ">
                        <div class="my-tb">
                          <div class="tc">
                            <div>立即领取</div>
                            <div></div>
                            <div class="cs"></div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </template>
              </van-cell>

            </div>
          </van-cell-group>
        </div>

        <van-goods-action>
          <van-goods-action-big-btn primary @click="show4 = false">
            确定
          </van-goods-action-big-btn>
        </van-goods-action>
      </van-popup>

      <van-sku v-model="showCustomAction" :sku="skuData.sku" :goods="skuData.goods_info">
        <!-- 自定义 sku-header-price -->
        <template slot="sku-header-price" slot-scope="props">
          <div class="van-sku__goods-price">
            <span class="van-sku__price-symbol">￥</span><span class="van-sku__price-num">{{ props.price }}</span>
          </div>
        </template>
        <!-- 自定义 sku actions -->
        <template slot="sku-actions" slot-scope="props">
          <div class="van-sku-actions">
            <van-button bottom-action @click="onPointClicked">积分兑换</van-button>
            <!-- 直接触发 sku 内部事件，通过内部事件执行 onBuyClicked 回调 -->
            <van-button type="primary" bottom-action @click="props.skuEventBus.$emit('sku:buy')">买买买</van-button>
          </div>
        </template>
      </van-sku>

    </van-cell-group>

    <van-cell-group>
      <van-cell title="商品详情" />
      <img v-for="img in goods.thumb" v-lazy="img">
    </van-cell-group>

    <van-goods-action>
      <van-goods-action-mini-btn icon="shop" @click="sorry">
        首页
      </van-goods-action-mini-btn>
      <van-goods-action-mini-btn icon="like-o" @click="showSuccessToast">
        收藏
      </van-goods-action-mini-btn>
      <van-goods-action-mini-btn icon="cart" info="5" @click="onClickCart">
        购物车
      </van-goods-action-mini-btn>
      <van-sku v-model="showBase" :sku="skuData.sku" :goods="skuData.goods_info" :hide-stock="skuData.sku.hide_stock"
        :quota="skuData.quota" :quota-used="skuData.quota_used" reset-stepper-on-hide reset-selected-sku-on-hide
        disable-stepper-input :close-on-click-overlay="closeOnClickOverlay" :custom-sku-validator="customSkuValidator"
        @buy-clicked="onBuyClicked" @add-cart="onAddCartClicked" />
      <van-goods-action-big-btn @click="showBase = true" block>{{ $t('加入购物车') }}</van-goods-action-big-btn>
      <van-goods-action-big-btn primary @click="showCustomAction = true">
        立即购买
      </van-goods-action-big-btn>
    </van-goods-action>

  </div>
</template>

<script>
  import skuData from "./data";

  export default {
    data() {
      this.skuData = skuData;
      return {
        showBase: false,
        showCustomAction: false,
        showCustom: false,
        showStepper: false,
        show1: false,
        show2: false,
        show3: false,
        show4: false,
        show5: false,
        show6: false,
        closeOnClickOverlay: true,
        initialSku: {
          s1: "30349",
          s2: "1193"
        },
        customSkuValidator: component => {
          return `请选择颜色和尺码`;
        },
        customStepperConfig: {
          quotaText: "单次限购100件",
          stockFormatter: stock => `剩余${stock}间`,
          handleOverLimit: data => {
            const {
              action,
              limitType,
              quota
            } = data;

            if (action === "minus") {
              this.$toast("至少选择一件商品");
            } else if (action === "plus") {
              if (limitType === LIMIT_TYPE.QUOTA_LIMIT) {
                this.$toast(`限购${quota}件`);
              } else {
                this.$toast("库存不够了~~");
              }
            }
          }
        },
        // messageConfig: {
        //   uploadImg: (file, img) => {
        //     return new Promise(resolve => {
        //       setTimeout(() => resolve(img), 1000);
        //     });
        //   },
        //   uploadMaxSize: 3
        // },
        goods: {
          title: "ZARA新款 女装 带饰纹理小香风西装外套",
          price: 19980,
          marketprice: 29660,
          subtitle: "质量保证 优质面料 精细做工 柔软透气",
          tag: "新款",
          selltag: "满减",
          onsell: "满200减20，满300减50元。",
          pricetag: "新品预售",
          express: "免运费",
          remain: 19,
          thumb: [
            "http://140.143.20.247:89/attachment/images/1/2018/06/Lrzv6vs9m5rvrMszv53s96OYYzZsrVVu.jpg",
            "http://140.143.20.247:89/attachment/images/1/2018/06/CzGG00oGGDRUTgADZGoJyj6RjXoo9x00.jpg",
            "http://140.143.20.247:89/attachment/images/1/2018/06/m0E78Nei9a1eqH396gUQ333jpiQ6EaQe.jpg",
            "http://140.143.20.247:89/attachment/images/1/2018/06/d13PQJiyIP3OXjH6ij1QhjQpxhXJpHQL.jpg",
            "http://140.143.20.247:89/attachment/images/1/2018/06/zp5XElx20R9yQL292oEP36O2w67Y6wPE.jpg"
          ]
        },
        swiperOption: {
          pagination: {
            el: ".swiper-pagination",
            type: "fraction"
          }
        }
      };
    },

    // 上拉菜单js
    computed: {
      actions() {
        return [{
            name: this.$t("option")
          },
          {
            name: this.$t("option"),
            subname: this.$t("description")
          },
          {
            loading: true
          },
          {
            name: this.$t("disabledOption"),
            disabled: true
          }
        ];
      }
    },

    mounted () {//给window添加一个滚动滚动监听事件 头部导航所用
      window.addEventListener('scroll', this.handleScroll)
    },

    //商品头部价格
    methods: {

      handleScroll () { //改变元素#searchBar的top值 头部导航所用
        var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
        // var offsetTop = document.querySelector('.detail-header-bg').offsetTop;

        if(scrollTop<=80){
          // offsetTop = 300 - Number(scrollTop);
          document.querySelector('.detail-header-bg').style.display = "block";
          document.querySelector('.my-goods-bar').style.display = "none";
          document.querySelector('.header-dom').style.zIndex = '0';
        }else{
          document.querySelector('.detail-header-bg').style.display = "none"; //让元素隐藏
          document.querySelector('.header-dom').style.zIndex = '3';
          document.querySelector('.my-goods-bar').style.display = "block";
        }
      },

      onSelect(item) {
        this.show1 = false;
        this.show2 = false;
        this.show3 = false;
        this.show4 = false;
        this.show5 = false;
        this.show6 = false;
        this.$toast(item.name);
      },

      onCancel() {
        this.$toast("cancel");
      },
      formatPrice() {
        return (this.goods.price / 100).toFixed(1);
      },

      formatMarketPrice() {
        return "¥" + (this.goods.marketprice / 100).toFixed(1);
      },

      onClickCart() {
        this.$router.push("cart");
      },

      showSuccessToast() {
        this.$toast.success(this.$t('收藏成功'));
      },

      sorry() {
        Toast("暂无后续逻辑~");
      },

      // 网页被卷去的高
      getScrollTop() {
        var scrollTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop;
        return scrollTop;
      }
    },
    
    destroyed () {//离开该页面需要移除这个监听的事件 头部导航所用
      window.removeEventListener('scroll', this.handleScroll)
    }
  };

</script>

<style lang="scss" scoped>
  // 头部导航
  .my-goods-bar {
    display: none;
  }

  .detail-header-bg {
    height: 44px;
    margin-top: -44px;
  }

  .header-dom {
    position: fixed;
    z-index: 2;
    left: 0;
    right: 0;
    top: 0;
    line-height: 44px;
    height: 44px;
    overflow: visible;

    .left-btns {
      padding: 7px 0 7px 10px;
      display: -webkit-box;
      display: -webkit-flex;
      display: -ms-flexbox;
      display: flex;
      -webkit-flex-wrap: wrap;
      -ms-flex-wrap: wrap;
      flex-wrap: wrap;

      a {
        margin-right: 5px;
      }
    }

    .icon-link {
      position: relative;
      height: 30px;
      width: 30px;
      display: block;
      -webkit-transition: color ease .2s, background-color ease .2s, fill ease .2s;
      transition: color ease .2s, background-color ease .2s, fill ease .2s;
      border-radius: 50%;
      background-color: rgba(0, 0, 0, .4);

    }

    .right-btns {
      padding: 7px 10px 7px 0;
      display: flex;
      -webkit-flex-wrap: wrap;
      -ms-flex-wrap: wrap;
      flex-wrap: wrap;

      a {
        margin-left: 5px;
      }
    }

    .header-icon {
      font-size: 18px;
      color: #fff;
      position: absolute;
      height: 18px;
      width: 18px;
      margin: 6px;
      text-align: center;
      fill: currentColor;
    }

  }

</style>
