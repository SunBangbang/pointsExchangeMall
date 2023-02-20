<template>
  <view @click="isMore = false">
    <view class="goods-head" :style="'background:rgba(255,255,255,' + PageScrollTop / 100 + ')'">
      <!-- 返回 -->
      <view class="back" @click="onBack">
        <view class="back-one" :class="{ action: PageScrollTop > 120 }">
          <text></text>
        </view>
      </view>
      <!-- tab切换 -->
      <view class="head-tab" v-if="PageScrollTop > 120">
        <view class="tab" :class="{'action':TabShow===0}" @click="onTab(0)">
          <text>商品</text>
          <text class="line"></text>
        </view>
        <view class="tab" :class="{'action':TabShow===1}" @click="onTab(1)">
          <text>评价</text>
          <text class="line"></text>
        </view>
        <view class="tab" :class="{'action':TabShow===2}" @click="onTab(2)">
          <text>详情</text>
          <text class="line"></text>
        </view>
      </view>
      <!-- 分享更多 -->
      <view class="share-more">
        <view class="share-more-one" :class="{ action: PageScrollTop > 120 }">
          <view class="list">
            <text class="iconfont icon-share"></text>
          </view>
          <view class="list" @click.stop="isMore = !isMore">
            <text class="iconfont icon-diandian"></text>
          </view>
        </view>
        <view class="mroe-list" v-show="isMore">
          <navigator open-type="switchTab" url="/pages/index/index" class="list">
            <view class="icon">
              <text class="iconfont icon-home"></text>
            </view>
            <view class="title">
              <text>首页</text>
            </view>
          </navigator>
        </view>
      </view>
    </view>
    <!-- banner，标题 -->
    <view class="banner-title">
      <!-- banner -->
      <view class="banner">
        <swiper class="screen-swiper round-dot" indicator-dots="true" circular="true" autoplay="true" interval="5000"
                duration="500">
          <swiper-item v-for="(item, index) in info.imgs" :key="index">
            <image :src="item" mode="aspectFill"></image>
            <!-- <video src="{{item.url}}" autoplay loop muted show-play-btn="{{false}}" controls="{{false
            }}" objectFit="cover" wx:if="{{item.type == 'video'}}"></video> -->
          </swiper-item>
        </swiper>
      </view>
      <!-- 价格 -->
      <view class="price-info" v-show="type==0">
        <view class="price">
          <view class="pp1">
			  <text class="pp1a">{{info.integral}}{{main_name}}</text>
			  <text class="pp1b">+{{info.price}}元</text>
			  <text class="pp1c">¥{{info.old_price}}</text>
		  </view>
		  <view class="pp2">
			  兑换热度{{info.sale_num}}万+
		  </view>
        </view>
        <view class="info">
          <view class="list">
            <image src="/static/xsdh.png" mode="heightFix"></image>
          </view>
          <view class="list">
            <text>{{main_name}}已省¥{{info.old_price - info.price}}</text>
          </view>
        </view>
      </view>
      <!-- 标题 -->
      <view class="goods-title">
		  <view class="imgg">
			  可{{main_name}}抵扣
		  </view>
        <text>{{info.title}}</text>
      </view>
      <!-- 开通会员 -->
      <view class="dredge-vip">
        <view class="title">
          <image src="/static/yhqs.png" mode="heightFix"></image>
          <text>
            现在购买立时省
            <text class="col">{{info.old_price - info.price}}</text>
            元
          </text>
        </view>
        <view class="dredge" @click="showbuy">
          <text>立即</text>
          <text>抢购</text>
        </view>
      </view>
    </view>
    <!-- 优惠积分 -->
    <view class="goods-discounts">
      <view class="list" @click="$refs['GoodsServe'].show()">
        <view class="title">服务</view>
        <view class="content">
          <view class="serve">
            <text class="iconfont icon-baozheng"></text>
            <text>退款保证</text>
          </view>
          <view class="serve">
            <text class="iconfont icon-baozheng"></text>
            <text>物流配送</text>
          </view>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
    </view>
    <!-- 属性规格 -->
    <view class="goods-discounts">
      <view class="list" @click="showbuy">
        <view class="title">已选</view>
        <view class="content">
          <text>{{info.sku_arr[0]}}</text>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
      <view class="list">
        <view class="title">运费</view>
        <view class="content">
          <text>免运费</text>
        </view>
        <view class="more"><!-- <text class="iconfont icon-more"></text> --></view>
      </view>
    </view>
    <!-- 评价 -->
   <view class="evaluate-data" ref="evaluate">
      <view class="title-more">
        <view class="title">
          <text>评价</text>
          <text class="num">{{info.comments}}</text>
        </view>
        <view class="more">
          <text class="iconfont icon-more"></text>
        </view>
      </view>
      <view class="evaluate-list">
		  <swiper class="swiper" style="height: 480rpx;" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
			<swiper-item v-for="(item,index) in comments" @click="imgpre(index)">
				<view class="swiper-item uni-bg-red">
					<view class="user-info">
					  <view class="thumb">
					    <image :src="item.headimg" mode=""></image>
					  </view>
					  <view class="nickname-grade">
					    <view class="nickname">
					      <text>{{item.nickname}}</text>
					    </view>
					    <view class="grade">
					      <text class="cuIcon-favorfill lg text-gray"></text>
						  <text class="cuIcon-favorfill lg text-gray"></text>
						  <text class="cuIcon-favorfill lg text-gray"></text>
						  <text class="cuIcon-favorfill lg text-gray"></text>
						  <text class="cuIcon-favorfill lg text-gray"></text>
					    </view>
					  </view>
					</view>
					<view class="content">
					  <view class="character">
					    <text class="two-omit">{{item.infos}}</text>
					  </view>
					  <view class="thumb-list">
					    <view class="list" v-for="(itm,inde) in item.imgslist">
					      <image :src="itm" mode="heightFix" ></image>
					    </view>
					   
					  </view>
					</view>
					
				</view>
			</swiper-item>
		</swiper>
        
       
      </view>
    </view> 
    <!-- 排行榜 -->
    <view class="ranking-list">
      <view class="ranking-title">
        <view class="title">排行榜</view>
      </view>
      <view class="goods-list">
        <view class="list" v-for="(item, index) in goodsList" :key="index" @click="gopage('/pages/detail/index?good_id='+item.id)">
          <view class="thumb">
            <image :src="item.imgurl"></image>
          </view>
          <view class="title">
            <text class="two-omit">{{item.title}}</text>
          </view>
          <view class="price">
            <text>￥{{item.price}}</text>
          </view>
        </view>
      </view>
    </view>
    <!-- 商品介绍 -->
    <view class="products-introduction" ref="products">
      <view class="title">
        <text>商品介绍</text>
      </view>
      <view class="content" v-html="info.infos"></view>
    </view>
    <!-- 底部 -->
    <view class="page-footer">
      <view class="footer-fn">
        
        <view class="list" @click="gopage('/pages/index/index')">
          <text class="iconfont icon-home"></text>
          <text>首页</text>
        </view>
		<view class="list" @click="gopage('/pages/kefu/index')">
		  <text class="iconfont icon-kefu"></text>
		  <text>联系客服</text>
		</view>
      </view>
      <view class="footer-buy">
        <view class="buy-at" @click="showbuy">
          <text>立即兑换</text>
        </view>
      </view>
    </view>
    <!-- 服务弹窗 -->
    <goods-serve ref="GoodsServe"></goods-serve>
    <!-- 属性规格 -->
    <view class="cu-modal bottom-modal" :class="{'show':isShow}" @click="hide">
      <view class="cu-dialog" @click.stop="showbuy">
    		<view class="goods-data">
    			<view class="thumb">
    				<image :src="info.imgurl" mode=""></image>
    			</view>
    			<view class="item">
    				<view class="title">
    					<text>{{info.title}}</text>
    				</view>
    				<view class="price">
    					<text class="pp1a">{{info.integral}}{{main_name}}</text>
					    <text class="pp1b">+{{info.price}}元</text>
					    <text class="pp1c">¥{{info.old_price}}</text>
    				</view>
    				<view class="inventory">
    					<text>库存：{{info.num}}</text>
    				</view>
    			</view>
    		</view>
    		<view class="attr-size">
    			<view class="attr-list">
    				<view class="title">
    					<text>规格</text>
    				</view>
					<view class="size-list">
						<div class="list" v-for="(value,idx) in info.sku_arr" 
						:class="{'action':skunow === value}" @click.stop="changesku(value)">
							<text>{{value}}</text>
						</div>
					</view>
    			</view>
    			<view class="attr-number">
    				<view class="tit">数量</view>
    				<view class="number">
    					<text class="iconfont icon-jian" @click.stop="less"></text>
    					<input type="number" maxlength="8" v-model="buynum">
    					<text class="iconfont icon-jia" @click.stop="add"></text>
    				</view>
    			</view>
    		</view>
			<view class="address-input">
				<view class="list-input">
					<view class="title">
						<text>收货人</text>
					</view>
					<view class="content">
						<input type="text" placeholder="请填写收货人姓名" v-model="name">
					</view>
				</view>
				<view class="list-input">
					<view class="title">
						<text>手机号</text>
					</view>
					<view class="content">
						<input type="tel" placeholder="请填写收货人手机号" v-model="phone">
					</view>
				</view>
				<view class="list-input">
					<view class="title">
						<text>所在地区</text>
					</view>
					<view class="content">
						<pick-regions :defaultRegion="defaultRegionCode" @getRegion="handleGetRegion">
						    <text>{{regionName||'点击选择省市区'}}</text>
						</pick-regions>
					</view>
				</view>
				<view class="list-textarea">
					<view class="title">
						<text>详细地址</text>
					</view>
					<view class="content">
						<textarea type="text" placeholder="街道/楼牌号等"  v-model="address"/>
					</view>
				</view>
			</view>
    		<view class="attr-btn">
    			<view class="add-buy" @click="tobuy">立即兑换</view>
    		</view>
    	</view>
    </view>
	
	
  </view>
</template>

<script>
import GoodsServe from '../../components/GoodsServe/GoodsServe.vue';
import GoodsCoupon from '../../components/GoodsCoupon/GoodsCoupon.vue';
import GoodsAttr from '../../components/GoodsAttr/GoodsAttr.vue';
import pickRegions from '@/components/pick-regions/pick-regions.vue'

export default {
  components: {
    GoodsServe,
    GoodsCoupon,
    GoodsAttr,
	pickRegions
  },
  data() {
    return {
	  main_name:'',
	  isShow:false,
      TabShow: 0,
      isMore: false,
      AttentionShow: 0,
	  info:[],
	  goods_id:0,
      swiperList: [],
      web_content:'',
      PageScrollTop: 0,
	  type: 0,
	  goodsList:[],
	  skunow:"",
	  buynum:1,
	  region:[],
	  defaultRegion:['北京市','市辖区','东城区'],
	  defaultRegionCode:'110101',
	  name:'',
	  phone:'',
	  mainaddress:'',
	  address:'',
	  comments:[],
	  indicatorDots: true,
	  autoplay: true,
	  interval: 20000,
	  duration: 500
    };
  },
	onLoad(params) {
		this.type = params.type||0;
		this.goods_id = params.good_id;
		this.name = uni.getStorageSync('name');
		this.phone = uni.getStorageSync('phone');
		this.address = uni.getStorageSync('address');
		this.getcomments();
		this.getrank();
		this.getchannel();
	},
	onShow() {
		this.loadData();
		
		
	},
	onPageScroll(e) {
		this.PageScrollTop = e.scrollTop;
	},
	computed:{
	    regionName(){
			console.log('1');
	        // 转为字符串
	        return this.region.map(item=>item.name).join(' ')
	    }
	},
  methods: {
	  imgpre(index){
		  let that = this;
		  console.log("qqq",index)
		  let imgarr = that.comments[index].imgslist;
		  console.log("qqq",imgarr)
		  uni.previewImage({
		    urls: imgarr,
		    current: 0,
		    success(res) {
		      console.log(`previewImage调用成功`);
		    },
		    fail(res) {
		      console.log(`previewImage调用失败`);
		    },
		  });
	  },
	  getcomments(){
	  	let that = this;
	  	that.req({
	  		url: 'getcomments',
	  		data: {},
	  		Loading: true,
	  		success: function(res) {
	  			console.log(res)
	  			if(res.code == 1){
	  				that.comments = res.data;
	  			}
	  		}
	  	})
	  },
	  getchannel(){
	  	let that = this;
	  	that.req({
	  		url: 'getchannel',
	  		data: {code:uni.getStorageSync('channel_code')},
	  		Loading: true,
	  		success: function(res) {
	  			console.log(res)
	  			if(res.code == 1){
	  				uni.setStorageSync('main_name',res.data);
	  				that.main_name = res.data;
	  			}
	  		}
	  	})
	  },
	 // 获取选择的地区
	 handleGetRegion(region){
	 	console.log("log",region);
	     this.region = region
	 },
	  tobuy(){
		  let that = this;
		  if(that.skunow == ''){
			  uni.showToast({
			  	title: "请选择规格",
			  	icon: 'none',
			  	duration: 2000
			  });
			  return;
		  }
		  if(that.name == ''){
			  uni.showToast({
				title: "请填写收货人姓名",
				icon: 'none',
				duration: 2000
			  });
			  return;
		  }
		  if(that.phone == ''){
			  uni.showToast({
				title: "请填写手机号",
				icon: 'none',
				duration: 2000
			  });
			  return;
		  }
		  if(that.regionName == ''){
				  uni.showToast({
					title: "请选择所在地区",
					icon: 'none',
					duration: 2000
				  });
				  return;
		  }
		  if(that.address == ''){
				  uni.showToast({
					title: "请填写详细地址",
					icon: 'none',
					duration: 2000
				  });
				  return;
		  }
		  uni.setStorageSync('name',that.name);
		  uni.setStorageSync('phone',that.phone);
		  uni.setStorageSync('address',that.address);
		  that.req({
		  	url: 'addorder',
		  	data: {goods_id:that.goods_id,skunow:that.skunow,buynum:that.buynum,name:that.name,phone:that.phone,regionName:that.regionName,address:that.address,channel_code:uni.getStorageSync('channel_code')},
		  	Loading: true,
		  	success: function(res) {
		  		console.log(res)
				if(res.code == 1){
					uni.navigateTo({
						url: '/pages/detail/payinfo?order_num='+res.data
					})
				}else{
					uni.showToast({
						title: res.msg,
						icon: 'none',
						duration: 2000
					})
				}
		  	}
		  })
		  /*  */
	  },
	  less(){
	  	 let ying = parseInt(this.buynum - 1)
	  	 if(ying <= 0){
	  		 uni.showToast({
	  		 	title: '购买数量不能为0',
	  		 	icon: 'none',
	  		 	duration: 2000
	  		 })
	  		 ying = 1;
	  	 }
	  	 this.buynum = ying;
	  },
	  add(){
	  	let ying = parseInt(this.buynum + 1)
	  	if(ying <= 0){
	  		 uni.showToast({
	  			title: '购买数量不能为0',
	  			icon: 'none',
	  			duration: 2000
	  		 })
	  		 ying = 1;
	  	}
	  	this.buynum = ying;
	  },
	  changesku(sku){
		  this.skunow = sku;
	  },
	  hide(){
		this.isShow = false;  
	  },
	  showbuy(){
		  this.isShow = true;
	  },
	loadData() {
		let that = this;
		that.req({
			url: 'goodsdetail',
			data: {goods_id:that.goods_id},
			Loading: true,
			success: function(res) {
				console.log(res)
				that.info = res.data;
				that.skunow = res.data.sku_arr[0]
			}
		})
	},
	getrank() {
		let that = this;
		that.req({
			url: 'getrank',
			data: {},
			Loading: true,
			success: function(res) {
				console.log(res)
				  that.goodsList = res.data;
			}
		})
	},
    /**
     * 返回
     */
    onBack() {
      uni.navigateBack();
    },
    /**
     * tab
     */
    onTab(type) {
      this.TabShow = type;
      switch (type) {
        case 0:
          uni.pageScrollTo({
          	scrollTop: 0,
          	duration: 300
          });
          break;
        case 1:
          uni.createSelectorQuery().select(".evaluate-data").boundingClientRect((data) => { //data - 各种参数
            uni.pageScrollTo({
							scrollTop: this.PageScrollTop + data.top -50,
							duration: 300
						});
          }).exec()
          break;
        case 2:
          uni.createSelectorQuery().select(".products-introduction").boundingClientRect((data) => { //data - 各种参数
            uni.pageScrollTo({
            	scrollTop: this.PageScrollTop + data.top - 50,
            	duration: 300
            });
          }).exec()
          break;
      }
    },
    

  }
};
</script>

<style scoped lang="scss">
@import 'index.scss';
</style>
