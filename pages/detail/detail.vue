<template>
	<view>	
		<!-- 商品详情大图 -->
		<swiper-image :resdata="banners" height="750" preview></swiper-image>
		<!-- 基础详情 -->
		<base-info :detail="detail" :show-price="showPrice"></base-info>
		<!-- 滚动商品特性 w170*h110 -->
		<!-- <scroll-attrs :baseAttrs="baseAttrs"></scroll-attrs> -->
		<!--  分享 -->
		<view class="share-section" @click="share">
			<text class="tit">分享可领100减10红包</text>
			<view class="share-btn">
				立即分享
				<text class="iconfont icon-you"></text>
			</view>
		</view>
		<!-- 属性选择 --> 
		<view class="p-2">
			<view class="rounded border bg-light-secondary">
				<uni-list-item @click="show('attr')"
				v-if="detail.sku_type === 1">
					<view class="d-flex">
						<text class="mr-2 text-muted">已选</text>
						<text>{{checkedSkus}}</text>
					</view>
				</uni-list-item>
				<uni-list-item >
					<view class="d-flex">
						<text class="mr-2 text-muted">促销</text>
						<view class="text-muted font d-flex a-center mr-2">满100减10</view>
						<view class="text-muted font d-flex a-center mr-2">满500减50</view>
					</view>
				</uni-list-item>
				<uni-list-item extraWidth="15%">
					<view class="d-flex">
						<text class="mr-2 text-muted">服务</text>
						<view class="text-muted font d-flex a-center mr-2">
							<view class="iconfont icon-finish main-text-color"></view>
							七天无理由退货
						</view>
						<view class="text-muted font d-flex a-center mr-2">
							<view class="iconfont icon-finish main-text-color"></view>
							假一赔十
						</view>
					</view>
				</uni-list-item>
			</view>
		</view>
		<!-- 评价 -->
		<view class="eva-section">
			<view class="e-header border-bottom border-light-secondary">
				<text class="tit">评价</text>
				<text>(86)</text>
				<text class="tip" @click="open">更多</text>
				<text class="iconfont icon-you" @click="open"></text>
			</view> 
			<view class="eva-box" v-for="i in 2" :key="i">
				<image class="portrait" src="http://img3.imgtn.bdimg.com/it/u=1150341365,1327279810&fm=26&gp=0.jpg" mode="aspectFill"></image>
				<view class="right">
					<text class="name">Leo yo</text>
					<text class="con">商品收到了，79元两件，质量不错，试了一下有点瘦，但是加个外罩很漂亮，我很喜欢</text>
					<view class="bot">
						<text class="attr">购买类型：XL 红色</text>
						<text class="time">2019-04-01 19:21</text>
					</view>
				</view>
			</view>
		</view>
		
		<divider />
		<!-- 商品详情 -->
		<view class="py-1 detail-desc">
			<view class="d-header">
				<text>图文详情</text>
			</view>
			<u-parse className="uparse" :content="context" @preview="preview" @navigate="navigate" ></u-parse>
		</view>
		<!-- 热门推荐 -->
		<card headTitle="热门推荐" :headTitleWeight="false" :headBorderBottom="false">
			<view class="row j-sb">
				<common-list v-for="(item,index) in hotList" :key="index" :item="item" :index="index">
				</common-list>
			</view>
		</card>
		
		<!-- 底部操作菜单 -->
		<view class="page-bottom">
				<view class="p-b-btn" @tap="ToIndex">
					<text class="iconfont icon-shouye"></text>
					<text>首页</text>
				</view>
				<view class="p-b-btn" @tap="ToCart">
					<text class="iconfont icon-gouwuche"></text>
					<text>购物车</text>
				</view>
				<view class="p-b-btn" :class="{active: favorite}" @click="toFavorite">
					<text class="iconfont icon-xihuan"></text>
					<text>收藏</text>
				</view>
				
			<view class="action-btn-group">
				<view type="primary" class=" action-btn no-border buy-now-btn text-white" @click="buy">立即购买</view>
				<view type="primary" class=" action-btn no-border add-cart-btn text-white" @click="show('attr')">加入购物车</view>
			</view>
		</view>
		
		<!-- 属性筛选框 -->
		<common-popup :popupClass="popup.attr" @hide="hide('attr')">
			<!-- 
			商品信息(275rpx)
			图片 180*180
			-->
			<view class="d-flex a-end" style="height: 175rpx;margin-bottom: 79rpx;">
				<image src="../../static/images/demo/list/1.jpg" mode="widthFix"
				style="height: 180rpx;width: 180rpx;margin-top: -158rpx;" class="border rounded"></image>
				<view class="pl-2">
					<price priceSize="font-lg" unitSize="font">{{showPrice}}</price>
					<text class="d-block pt-2">已选:{{checkedSkus}}</text>
				</view>
			</view>
			<!-- 
			表单部分(660rpx)
			-->
			<scroll-view scroll-y class="w-100" style="height: 660rpx;">
				<card :headTitle="item.title" :headTitleWeight="false" 
				:headBorderBottom="false" :key="index"
				v-for="(item,index) in selects">
					<zcm-radio-group :label="item" 
					:selected.sync='item.selected'></zcm-radio-group>
				</card>
				<view class="d-flex j-sb a-center p-2 border-top border-light-secondary">
					<text>购买数量</text>
					<uni-number-box :min="1" :max="maxStock" :value="detail.num" @change="detail.num = $event"></uni-number-box>
				</view>
			</scroll-view>
			<!-- 
			 按钮(100rpx)
			 -->
			 <view class="main-bg-color text-white font-md d-flex a-center j-center" hover-class="main-bg-hover-color" style="height: 100rpx;margin-top: 49rpx;margin-bottom: 30rpx;" 
			 @tap.stop="addCart">
			 	加入购物车
			 </view>
		</common-popup>
		
	</view>
</template>

<script>
	import swiperImage from "@/components/index/swiper-image.vue"
	import baseInfo from "@/components/detail/base-info.vue"
	import scrollAttrs from "@/components/detail/scroll-attrs.vue"
	import uniListItem from "@/components/uni-ui/uni-list-item/uni-list-item.vue"
	import scrollComments from "@/components/detail/scroll-comments.vue"
	import uParse from "@/components/uni-ui/uParse/src/wxParse.vue"
	import card from "@/components/common/card.vue"
	import commonList from "@/components/common/common-list.vue"
	import bottomBtn from "@/components/detail/bottom-btn.vue"
	import commonPopup from "@/components/common/common-popup.vue"
	import price from "@/components/common/price.vue"
	import zcmRadioGroup from "@/components/common/radio-group.vue"
	import uniNumberBox from "@/components/uni-ui/uni-number-box/uni-number-box.vue"
	import {mapState,mapMutations} from "vuex"
	
	export default {
		components: {
			swiperImage,
			baseInfo,
			scrollAttrs,
			uniListItem,
			scrollComments,
			uParse,
			card,
			commonList,
			bottomBtn,
			commonPopup,
			price,
			zcmRadioGroup,
			uniNumberBox
		},
		data() {
			return {
				selects:[],
				popup:{
					attr:"none",
					express:"none",
					service:"none"
				},
				comments:[],
				banners:[],
				detail:{
					id:20,
					title:"小米MIX3 6GB+128GB",
					desc:"磁动力滑盖全面屏 / 前后旗舰AI双摄 / 四曲面彩色陶瓷机身 / 高效10W无线充电",
					cover:"/static/images/demo/list/1.jpg",
					pprice:3299,
					num:1,
					max:100
				},
				context:"",
				baseAttrs:[],
				hotList:[]
			}
		},
		// 监听页面返回事件
		onBackPress() {
			// 关闭模态框
			for (let key in this.popup) {
				if (this.popup[key] !== 'none') {
					this.hide(key)
					return true
				}
			}
		},
		computed: {
			...mapState({
				pathList:state=>state.path.list
			}),
			// 选中的sku
			checkedSkus(){
				// 拿到选中skus组成字符串
				let checkedSkus = this.selects.map(v=>{
					return v.list[v.selected].name
				})
				return checkedSkus.join(',')
			},
			// 选中skus的索引
			checkedSkusIndex(){
				if (!Array.isArray(this.detail.goodsSkus)) {
					return -1
				}
				let index = this.detail.goodsSkus.findIndex((item)=>{
					return item.skusText === this.checkedSkus
				})
				return index
			},
			// 显示价格
			showPrice(){
				if (this.checkedSkusIndex < 0) {
					return this.detail.min_price || 0.00
				}
				return this.detail.goodsSkus[this.checkedSkusIndex].pprice
			},
			// 最大库存
			maxStock(){
				if (this.detail.sku_type === 0) {
					return this.detail.stock
				}
				if (!Array.isArray(this.detail.goodsSkus)) {
					return 100
				}
				return this.detail.goodsSkus[this.checkedSkusIndex].stock || 100
			}
		},
		onLoad(e) {
			if (e.detail) {
				this.__init(JSON.parse(e.detail))
			}
		},
		methods: {
			...mapMutations([
				'addGoodsToCart'
			]),
			// 初始化页面
			__init(data){
				this.$H.get('/goods/'+data.id).then(res=>{
					// 轮播图
					this.banners = res.goodsBanner.map(v=>{
						return {
							src:v.url
						}
					})
					// 初始化基本信息
					this.detail = res
					this.detail.num = 1
					// 修改页面标题
					uni.setNavigationBarTitle({
						title:res.title
					})
					// 滚动商品属性
					this.baseAttrs = res.goodsAttrs.map(v=>{
						return { 
							icon:"icon-cpu",
							title:v.name,
							desc:v.value,
						}
					})
					// 热门评论
				   this.comments = res.hotComments.map(v=>{
					   var imglist = []
					   
					   for (let k in v.imglist) {
							imglist.push(v.imglist[k].src)
					   }
					   
					   return {
						   id:v.id,
						   user_id:v.user.id,
						   userpic:v.user.avatar,
						   username:v.user.nickname,
						   create_time:v.review_time,
						   goods_num:v.goods_num,
						   context:v.review.data,
						   imglist:v.review.image
					   }
				   })
				   // 商品详情
				   this.context = res.content
				   // 热门推荐
				  this.hotList = res.hotList.map(v=>{
					  return {
						  id:v.id,
						  cover:v.cover,
						  title:v.title,
						  desc:v.desc,
						  oprice:v.min_oprice,
						  pprice:v.min_price
					  }
				  })
				  if (this.detail.sku_type === 1) {
				  	// 商品规格（选项部分）
				  	this.selects = res.goodsSkusCard.map(v=>{
					  let list = v.goodsSkusCardValue.map(v1=>{
						  return {
							  id:v1.id,
							  name:v1.value
						  }
					  })
					  return {
						id:v.id,
						title:v.name,
						selected:0,
						list:list
					  }
				  	})
					console.log(this.selects)
				  	// 商品规格（匹配价格）
				  	this.detail.goodsSkus.forEach(item=>{
					  let arr = []
					  for (let key in item.skus) {
						arr.push(item.skus[key].value)
					  }
					  item.skusText = arr.join(',')
				  	})
				  }
				})
			},
			ToIndex(){
				uni.switchTab({
					url:"../index/index"
				})
			},
			//切换购物车
			ToCart() {
				uni.switchTab({
					url:"../cart/cart"
				})
			},
			//收藏
			toFavorite(){
				this.favorite = !this.favorite;
			},
			// 加入购物车
			addCart(){
				// 组织数据
				let goods = this.detail
				goods['checked'] = false
				goods['attrs'] = this.selects
				// 加入购物车
				this.addGoodsToCart(goods)
				// 隐藏筛选框
				this.hide('attr')
				// 成功提示
				uni.showToast({
					title: '加入成功'
				});
			},
			openCreatePath(){
				uni.navigateTo({
					url: '../user-path-edit/user-path-edit',
				});
				this.hide('express')
			},
			hide(key){
				this.popup[key] = 'hide'
				setTimeout(()=>{
					this.popup[key] = 'none'
				}, 200);
			},
			show(key){
				this.popup[key] = 'show'
			},
			preview(src, e) {
				// do something
				console.log("src: " + src);
			},
			navigate(href, e) {
				// 如允许点击超链接跳转，则应该打开一个新页面，并传入href，由新页面内嵌webview组件负责显示该链接内容
				console.log("href: " + href);
			},
			
			//评论更多
			open(){
				uni.navigateTo({
					url: '/pages/detail-comment/detail-comment?id='+ 25,
					// url: '/pages/detail-comment/detail-comment?id='+this.goods_id,
				});
			},
			buy(){
				uni.navigateTo({
					url: `/pages/order/createOrder`
				})
			},
		}
	}
</script>

<style lang="scss">
.uparse .p{ padding: 0!important; }
.uparse view,.uparse uni-view{ line-height: 0!important; }


/* 分享 */
	.share-section{
		display:flex;
		align-items:center;
		color: $font-color-base;
		background: linear-gradient(left, #fdf5f6, #fbebf6);
		padding: 12rpx 30rpx;
		.share-icon{
			display:flex;
			align-items:center;
			width: 70rpx;
			height: 30rpx;
			line-height: 1;
			border: 1px solid $uni-color-primary;
			border-radius: 4rpx;
			position:relative;
			overflow: hidden;
			font-size: 22rpx;
			color: $uni-color-primary;
			&:after{
				content: '';
				width: 50rpx;
				height: 50rpx;
				border-radius: 50%;
				left: -20rpx;
				top: -12rpx;
				position:absolute;
				background: $uni-color-primary;
			}
		}
		.icon-xingxing{
			position:relative;
			z-index: 1;
			font-size: 24rpx;
			margin-left: 2rpx;
			margin-right: 10rpx;
			color: #fff;
			line-height: 1;
		}
		.tit{
			font-size: $font-base;
			margin-left:10rpx;
		}
		.icon-bangzhu1{
			padding: 10rpx;
			font-size: 30rpx;
			line-height: 1;
		}
		.share-btn{
			flex: 1;
			text-align:right;
			font-size: $font-sm;
			color: $uni-color-primary;
		}
		.icon-you{
			font-size: $font-sm;
			margin-left: 4rpx;
			color: $uni-color-primary;
		}
	}
	
	
	/* 评价 */
	.eva-section{
		display: flex;
		flex-direction: column;
		padding: 20rpx 30rpx;
		background: #fff;
		margin-top: 16rpx;
		.e-header{
			display: flex;
			align-items: center;
			height: 70rpx;
			font-size: $font-sm + 2rpx;
			color: $font-color-light;
			.tit{
				font-size: $font-base + 2rpx;
				color: $font-color-dark;
				margin-right: 4rpx;
			}
			.tip{
				flex: 1;
				text-align: right;
			}
			.icon-you{
				margin-left: 10rpx;
			}
		}
	}
	.eva-box{
		display: flex;
		padding: 20rpx 0;
		.portrait{
			flex-shrink: 0;
			width: 80rpx;
			height: 80rpx;
			border-radius: 100px;
		}
		.right{
			flex: 1;
			display: flex;
			flex-direction: column;
			font-size: $font-base;
			color: $font-color-base;
			padding-left: 26rpx;
			.con{
				font-size: $font-base;
				color: $font-color-dark;
				padding: 20rpx 0;
			}
			.bot{
				display: flex;
				justify-content: space-between;
				font-size: $font-sm;
				color:$font-color-light;
			}
		}
	}
	
	
	.detail-desc{
		background: #fff;
		margin-top: 16rpx;
		.d-header{
			display: flex;
			justify-content: center;
			align-items: center;
			height: 80rpx;
			font-size: $font-base + 2rpx;
			color: $font-color-dark;
			position: relative;
				
			text{
				padding: 0 20rpx;
				background: #fff;
				position: relative;
				z-index: 1;
			}
			&:after{
				position: absolute;
				left: 50%;
				top: 50%;
				transform: translateX(-50%);
				width: 300rpx;
				height: 0rpx;
				content: '';
				border-bottom: 1rpx solid #ccc; 
			}
		}
	}
	
	
	
	/* 底部操作菜单 */
	.page-bottom{
		position:fixed;
		left: 30rpx;
		bottom:30rpx;
		z-index: 95;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 690rpx;
		height: 100rpx;
		background: rgba(255,255,255,.9);
		box-shadow: 0 0 20rpx 0 rgba(0,0,0,.5);
		border-radius: 16rpx;
		
		.p-b-btn{
			display:flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			font-size: $font-sm;
			color: $font-color-base;
			width: 96rpx;
			height: 80rpx;
			.iconfont{
				font-size: 40rpx;
				line-height: 48rpx;
				color: $font-color-light;
			}
			&.active, &.active .iconfont{
				color: $uni-color-primary;
			}
			.icon-fenxiang2{
				font-size: 42rpx;
				transform: translateY(-2rpx);
			}
			.icon-shoucang{
				font-size: 46rpx;
			}
		}
		.action-btn-group{
			display: flex;
			height: 76rpx;
			border-radius: 100px;
			overflow: hidden;
			box-shadow: 0 20rpx 40rpx -16rpx #fa436a;
			box-shadow: 1px 2px 5px rgba(219, 63, 96, 0.4);
			background: linear-gradient(to right, #ffac30,#fa436a,#F56C6C);
			margin-left: 20rpx;
			position:relative;
			&:after{
				content: '';
				position:absolute;
				top: 50%;
				right: 50%;
				transform: translateY(-50%);
				height: 28rpx;
				width: 0;
				border-right: 1px solid rgba(255,255,255,.5);
			}
			.action-btn{
				display:flex;
				align-items: center;
				justify-content: center;
				width: 180rpx;
				height: 100%;
				font-size: $font-base ;
				padding: 0;
				border-radius: 0;
				background: transparent;
				border: none;
			}
		}
	}
</style>
